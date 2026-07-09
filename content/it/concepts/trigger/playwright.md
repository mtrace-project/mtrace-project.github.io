---
title: Playwright
cascade:
  type: docs
---

## Configurazione
Il trigger Playwright è configurabile attraverso il type `playwright` e i seguenti argomenti:
| Argomento         | Descrizione                                                                      | Valore di default | Opzionale |
| ----------------- | -------------------------------------------------------------------------------- | ----------------- | --------- |
| `playwrightPath`  | Percorso della cartella di Playwright a partire dalla working directory          | playwright        | Si        |
| `filePath`        | Percorso del file contenente lo script Playwright a partire dal `playwrightPath` |                   | No        |
| `projects`        | Lista dei tipi di browser da utilizzare per l'esecuzione dello script Playwright |                   | Si        |
| `traceUrlPattern` | Stringa per filtrare gli endpoint in cui includere l'header *traceparent*        | **                | Si        |

### Playwright path
Il campo `playwrightPath` specifica il percorso della cartella di Playwright a partire dalla working directory, che può essere la cartella in cui si esegue **Mtrace** o la cartella specificata attraverso la flag `--dir`. Se non specificato, il valore di default è `playwright`.

### File path
Il campo `filePath` specifica il percorso del file contenente lo script Playwright a partire dalla cartella specificata nel campo `playwrightPath`. Questo campo è obbligatorio e deve essere specificato come stringa.

### Projects
Il campo `projects` specifica la lista dei tipi di browser da utilizzare per l'esecuzione dello script Playwright. I browser supportati si possono trovare nella documentazione ufficiale di [Playwright](https://playwright.dev/docs/browsers). Se non specificato, lo script Playwright verrà eseguito con i browser specificati nel file `playwright.config.ts`.

### Trace URL pattern
Il campo `traceUrlPattern` specifica una stringa per filtrare gli endpoint in cui includere l'header *traceparent*. Se non specificato, il valore di default è `**`, il quale indica che l'header *traceparent* verrà incluso in tutte le richieste effettuate dallo script Playwright. Per ulteriori informazioni sulla sintassi di questa stringa e su come filtrare gli endpoint, si consiglia di consultare la documentazione ufficiale di [Playwright](https://playwright.dev/docs/network#glob-url-patterns).

Questo campo è importante per evitare di includere l'header *traceparent* in endpoint multipli, in quanto potrebbe non essere accettato da tutti i servizi.

## Configurazione script Playwright
Affinché lo script Playwright possa essere utilizzato come trigger, è necessario includere il seguente frammento di codice in un file separato:
```javascript
import { test } from '@playwright/test';

test.beforeEach(async ({ page }) => {
    const serverUrl = process.env.MTRACE_PLAYWRIGHT_SERVER_URL;

    if (!serverUrl) {
        return;
    }

    try {
        const response = await fetch(serverUrl);
        if (response.ok) {
            const data = await response.json();
            const traceparent = data.traceparent;
            const traceUrlPattern = data.traceUrlPattern || '**';
            console.log(`[Mtrace] Received traceparent: ${traceparent}, pattern: ${traceUrlPattern}`);

            await page.route(traceUrlPattern, async (route) => {
                const headers = {
                    ...route.request().headers(),
                    'traceparent': traceparent
                };
                await route.continue({ headers });
            });
        } else {
            console.error(`[Mtrace] Server responded with error: ${response.status}`);
        }
    } catch (error) {
        console.error(`[Mtrace] Failed to contact Mtrace server at ${serverUrl}:`, error);
    }
});
```
Questo frammento di codice permette a **Mtrace** di generare un *traceparent* e di fornirlo a ogni singolo script Playwright.

Infine, all'interno dello script Playwright utilizzato come trigger, è fondamentale importare il file appena creato tramite `import '<path-to-script>';`.

## Esempio di configurazione completa
1. File di test **Mtrace** con trigger Playwright configurato:
```yaml
trigger:
  type: "playwright"
  args:
    playwrightPath: "playwright"
    filePath: "tests/rolldice.spec.ts"
    projects:
      - "chromium"
      - "webkit"
    traceUrlPattern: "**/rolldice**"
```

2. All'interno del file `config.ts` nella cartella `playwright/mtrace` è presente il codice per inserire l'header *traceparent* in tutte le richieste effettuate dallo script Playwright secondo l'URL pattern. Lo snippet di codice in questione è riportato nella sezione [Configurazione script Playwright](#configurazione-script-playwright).

3. File `rolldice.spec.ts` all'interno di `playwright/tests` contenente lo script Playwright che verrà eseguito come trigger:
```javascript
import { test, expect } from '@playwright/test';
import '../mtrace/config';

test('roll the dice', async ({ page }) => {
    await page.goto('http://localhost:5173/');
    await expect(page.getByRole('textbox', { name: 'Player Name (Optional)' })).toBeVisible();
    await page.getByRole('textbox', { name: 'Player Name (Optional)' }).click();
    await page.getByRole('textbox', { name: 'Player Name (Optional)' }).fill('Mtrace');
    await expect(page.getByRole('button', { name: 'Roll the Dice' })).toBeVisible();
    await page.getByRole('button', { name: 'Roll the Dice' }).click();
    await expect(page.getByText('Mtrace rolled a')).toBeVisible({ timeout: 10000 });
});
```

{{% callout type="warning" %}}
Assicurati di aspettare la risposta del server all'interno dello script Playwright.   
Altrimenti, una volta eseguite le azioni sulla pagina, lo script Playwright terminerà l'esecuzione e potrebbe mandare in errore le span coinvolte.
{{% /callout %}}

{{% callout type="warning" %}}
In Playwright, è possibile eseguire lo stesso test con più browser attraverso la flag `--project` o il file di configurazione. Tuttavia **Mtrace** può analizzare una **trace** alla volta. Perciò, se si esegue lo stesso test con più browser, **Mtrace** analizzerà solo la prima trace ricevuta, ignorando le altre. 
{{% /callout %}}