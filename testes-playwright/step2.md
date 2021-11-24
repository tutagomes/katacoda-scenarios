`npm init`{{execute}}

`npm i -D @playwright/test`{{execute}}

`export PLAYWRIGHT_SKIP_BROWSER_DOWNLOAD=false`{{execute}}

`npx playwright install`{{execute}}

`mkdir tests`{{execute}}

`touch tests/example.spec.js`{{execute}}

`touch tests/playwright.config.js`{{execute}}

```sh
cat << EOF > /tests/playwright.config.js
// playwright.config.ts
import { PlaywrightTestConfig, devices } from '@playwright/test';

const config: PlaywrightTestConfig = {
  forbidOnly: !!process.env.CI,
  retries: process.env.CI ? 2 : 0,
  use: {
    trace: 'on-first-retry',
  },
  projects: [
    {
      name: 'chromium',
      use: { ...devices['Desktop Chrome'] },
    }
  ],
};
export default config;
EOF
```{{execute}}

`npx playwright test`{{execute}}