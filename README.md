# Responsible Data Analytics — competition website

A responsive static website with individual and team registration, plus a Google Apps Script receiver that saves submissions to Google Sheets.

## 1. Create the registration spreadsheet
1. Create a Google Spreadsheet named **Responsible Data Analytics — Registrations**.
2. Open **Extensions → Apps Script** from that spreadsheet.
3. Replace the default Apps Script content with `Code.gs` from this package.
4. Save. In the Apps Script editor, select the `setup` function and click **Run** once; approve the requested permissions. This creates the `Registrations` tab and headers.

## 2. Deploy the receiver
1. In Apps Script choose **Deploy → New deployment → Web app**.
2. Set **Execute as: Me** and **Who has access: Anyone** (if your Workspace policy permits).
3. Deploy, authorize if prompted, and copy the **Web app URL** ending in `/exec`.
4. Paste that URL between the quotes in `config.js` as `window.REGISTRATION_ENDPOINT = 'https://script.google.com/macros/s/.../exec';`.
5. Upload the website files to your hosting provider. Never publish the spreadsheet itself or share editor access with participants.

## 3. Publish the website
**Netlify:** create an account, choose Add new project → Deploy manually, and drag the folder containing `index.html`, `style.css`, `script.js`, and `config.js` into the upload area. Netlify provides a public URL. You can also use GitHub Pages or another static host.

**GitHub Pages:** create a repository, upload the four website files at the repository root, then Settings → Pages → Deploy from branch → main / root. Wait for the public URL.

## 4. Test before sharing
- Open the public website in an incognito/private window and submit a test individual registration.
- Check the spreadsheet `Registrations` tab for a new row.
- Submit a test team registration and verify both team fields.
- Test on a phone. Remove test rows afterward.

## Important implementation notes
- Browser cross-origin restrictions mean this lightweight Apps Script integration sends the request in `no-cors` mode. The browser **cannot confirm whether Google Sheets actually stored the row**. The UI deliberately says “Submission sent,” not “Registration confirmed.” Verify test rows before publishing. For reliable confirmation, use a server-side proxy (e.g. Netlify Function) that checks the Apps Script response, or a dedicated backend.
- The public Apps Script endpoint can receive automated/spam submissions. The form includes a basic honeypot and server validation, but these are not robust anti-spam controls. Consider CAPTCHA, rate limiting through a backend, or restricting the registration window.
- Collect only necessary personal information, restrict spreadsheet access to organisers, publish a privacy notice with a retention/deletion policy, and confirm applicable local privacy requirements before launch.
- The programme and detailed rules/prizes are intentionally marked as awaiting organiser confirmation rather than invented.
- For updates to Apps Script code, deploy a new version via **Deploy → Manage deployments → Edit**.
