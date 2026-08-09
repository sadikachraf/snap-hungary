# New COD backend preview test

This branch preserves the production `main` behavior by default. The new Apps Script backend is used only when the page URL explicitly contains both:

- `cod_backend=new-test`
- `cod_endpoint=<URL-encoded Apps Script /exec URL>`
- `test_build=ONLINE-B03`

In `new-test` mode:

- the old Apps Script webhook is not called;
- the COD Command Center API is not called;
- marketing pixels are not loaded, so test traffic does not fire PageView or Purchase events;
- a visible banner identifies the branch build, endpoint, disabled legacy backends,
  disabled SMS, and disabled pixels;
- the normal bundle selector, customer form, and upsell remain in place;
- after the upsell choice, a manual pre-submit card displays the resolved payload;
- nothing is sent until `SEND TO NEW GOOGLE SHEET` is pressed;
- one URL-encoded native browser form request is then sent to the new Apps Script backend;
- the browser then opens the Apps Script submission result in the current tab, so a
  backend validation or technical error cannot be hidden behind the landing-page
  thank-you state.

## Online preview

Push only `codex/new-cod-backend-test` and use the Vercel Preview Deployment created
for that branch. Do not promote the preview or merge the branch into `main`.

Open the preview with the current `/exec` endpoint and exact build marker:

```text
https://VERCEL_PREVIEW_URL/?cod_backend=new-test&cod_endpoint=URL_ENCODED_APPS_SCRIPT_EXEC_URL&utm_source=cod_migration_test&utm_campaign=phase1_online_preview&utm_content=manual_online&test_build=ONLINE-B03
```

The Apps Script project must first have `configurePhase1Test()` run manually, be deployed as a Web App executing as the owner with access set to Anyone, and have its production `/exec` URL saved as the `WEB_APP_URL` Script Property.

Do not use the production landing-page URL for this test, promote this preview, or
merge this branch until the new Sheet row has been verified.
