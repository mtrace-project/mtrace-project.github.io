---
title: Playwright
cascade:
  type: docs
---

## Configuration
The Playwright trigger is configurable using the `playwright` type and the following arguments:
| Argument          | Description                                                                              | Default value     | Optional  |
| ----------------- | ---------------------------------------------------------------------------------------- | ----------------- | --------- |
| `playwrightPath`  | Path to the Playwright folder relative to the working directory                          | playwright        | Yes       |
| `filePath`        | Path to the file containing the Playwright script relative to the `playwrightPath`       |                   | No        |
| `projects`        | List of browser types to use for the execution of the Playwright script                  |                   | Yes       |
| `traceUrlPattern` | String to filter the endpoints where the *traceparent* header should be included         | **                | Yes       |

### Playwright path
The `playwrightPath` field specifies the path to the Playwright folder relative to the working directory, which can be the directory where **Mtrace** is executed or the directory specified via the `--dir` flag. If not specified, the default value is `playwright`.

### File path
The `filePath` field specifies the path to the file containing the Playwright script relative to the folder specified in the `playwrightPath` field. This field is mandatory and must be provided as a string.

### Projects
The `projects` field specifies the list of browser types to use for the execution of the Playwright script. The supported browsers can be found in the official [Playwright documentation](https://playwright.dev/docs/browsers). If not specified, the Playwright script will be executed with the browsers specified in the `playwright.config.ts` file.

### Trace URL pattern
The `traceUrlPattern` field specifies a string to filter the endpoints where the *traceparent* header should be included. If not specified, the default value is `**`, which indicates that the *traceparent* header will be included in all requests made by the Playwright script. For more information on the syntax of this string and how to filter endpoints, we recommend consulting the official [Playwright documentation](https://playwright.dev/docs/network#glob-url-patterns).

This field is important to avoid including the *traceparent* header in multiple endpoints, as it might not be accepted by all services.

## Playwright script configuration
For the Playwright script to be used as a trigger, you must include the following code snippet in a separate file:
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
This code snippet allows **Mtrace** to generate a *traceparent* and provide it to each individual Playwright script.

Finally, inside the Playwright script used as the trigger, it is essential to import the newly created file via `import '<path-to-script>';`.

## Complete configuration example
1. **Mtrace** test file with the Playwright trigger configured:
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

2. Inside the `config.ts` file in the `playwright/mtrace` folder, the code to insert the *traceparent* header in all requests made by the Playwright script according to the URL pattern is present. The code snippet in question is shown in the [Playwright script configuration](#playwright-script-configuration) section.

3. The `rolldice.spec.ts` file inside `playwright/tests` containing the Playwright script that will be executed as a trigger:
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
Make sure to wait for the server's response inside the Playwright script.   
Otherwise, once the actions on the page are executed, the Playwright script will terminate its execution and might cause errors in the involved spans.
{{% /callout %}}

{{% callout type="warning" %}}
In Playwright, it is possible to execute the same test with multiple browsers through the `--project` flag or the configuration file. However, **Mtrace** can analyze one **trace** at a time. Therefore, if you execute the same test with multiple browsers, **Mtrace** will only analyze the first trace received, ignoring the others. 
{{% /callout %}}
