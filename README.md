# Brain Experiment — connected Version 2 candidate

**Status: Implementation candidate, NOT YET DEPLOYED OR END-TO-END TESTED.** The GitHub Pages → Apps Script POST echo test succeeded previously, but this full backend has not been authorized, deployed, or tested with concurrent student devices. Do not use for class until testing is complete.

## 1. Create a private Google Sheet

In your personal Google account, create a new Google Sheet named `Brain Experiment Responses`. In the Sheet, select **Extensions → Apps Script** to create a **sheet-bound** Apps Script project. The prior connection-test script was standalone; this new bound script is recommended for the live data store.

Replace its `Code.gs` with the provided `Code.gs`. Save. Select the function `setup` and click **Run**. Approve the Google Sheets authorization requested by Google. This creates a `Responses` tab and a new session ID.

## 2. Configure instructor passcode

In `Code.gs`, find `setInstructorPasscode()`. Temporarily replace `REPLACE_WITH_A_LONG_UNIQUE_PASSPHRASE` with a unique passphrase of at least 16 characters. Save, select `setInstructorPasscode`, and click **Run**. Then **replace the literal passphrase with the placeholder again, save, and deploy**. The salted SHA-256 hash remains in private Script Properties; the plaintext passphrase should not remain in source code. Do not share your passphrase in chat or commit it to GitHub.

## 3. Deploy the backend

Choose **Deploy → New deployment → Web app**. Set **Execute as: Me** and **Who has access: Anyone**. Copy the new permanent `/exec` URL. This is a *different* script from the connection test. Later backend changes require **Manage deployments → Edit → New version → Deploy**.

## 4. Configure the website

Open `config.js`, replace `window.BRAIN_API` with the NEW `/exec` URL from step 3. Upload the site files **except `Code.gs`** to a public GitHub repository and enable GitHub Pages from `main` / root. The public student URL is `index.html`; the instructor-only entry point is `instructor.html`. Do not link the instructor page from the student page. The instructor page HTML and explanation content are public, but results require the passcode and server-side authorization.

## 5. Test before classroom use

- Open the student URL on two independent browsers/devices and verify different assigned variants; across groups of four, there should be two low/two high anchors and two saved/two lost frames, with independently shuffled orders.
- Complete an experiment, refresh midway and verify the saved draft remains. Submit and confirm that refreshing cannot create a second submission.
- Unlock instructor dashboard with passcode; refresh results and verify data. Test wrong passcode, CSV, 2-4-6 histories, fullscreen, reset, and return visits after reset.
- Test two simultaneous submissions, and all 18 mock submissions with actual browsers before relying on the app.
- Note: Browser local storage and the Sheet contain pseudonymous device identifiers, not names or emails. This does not guarantee full anonymity against all device/network metadata.

## Limitations and review items

- No direct integration test of the full Apps Script backend has been run; Google authorization and live behavior remain unverified.
- Frontend code is publicly readable, including the instructor presentation's answer key; student `index.html` does not load that key. A student could still discover the public instructor page.
- The instructor passcode is verified server-side, but there is no sophisticated brute-force throttling or audit logging. Use a strong unique passphrase and don't reuse account passwords.
- The app relies on device localStorage for draft recovery; clearing browser data or switching devices loses a draft.
- The initial 2-4-6 hypothesis is locked in the UI; backend checks test validity but does not separately attest the chronology of student interactions. This is a classroom exercise, not a tamper-proof research instrument.
