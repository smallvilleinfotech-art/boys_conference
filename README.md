# WIN RIGHT 2026: Registration website

Files in this folder:

```
index.html        the whole website (HTML + CSS + JS, no build step)
Code.gs           Google Apps Script backend (paste into Apps Script)
assets/           photos and logos (must sit next to index.html, folder name exactly "assets")
```

## Part 1 – Set up the Google Sheet backend (about 10 minutes)

1. Go to **sheets.google.com** and create a **blank spreadsheet** (sign in with the account that should own the data, e.g. the boys@boystokings.org Google account). Name it `WIN RIGHT 2026 Registrations`.
2. In the sheet, open **Extensions → Apps Script**.
3. Delete everything in the editor, open `Code.gs` from this folder, copy all of it and paste it in. Click **Save** (disk icon).
4. In the function dropdown at the top choose **setup**, then click **Run**. Google asks for permission: click **Review permissions**, choose your account, then **Advanced → Go to (project) → Allow**. (It needs to edit your sheet and send email as you.) A tab called **Registrations** with the 16 column headings now appears.
5. Click **Deploy → New deployment**. Click the gear next to "Select type" and choose **Web app**.
   - Description: `WIN RIGHT registration`
   - **Execute as: Me**
   - **Who has access: Anyone**
   - Click **Deploy** and **copy the Web app URL** (it starts with `https://script.google.com/macros/s/…/exec`).
6. Open `index.html` in any text editor, find the block marked `CONFIG` near the bottom and replace the placeholder:
   ```js
   const APPS_SCRIPT_URL = "PASTE_YOUR_APPS_SCRIPT_WEB_APP_URL_HERE";
   ```
   with your URL (keep the quotation marks). Save.

### Test it (after hosting, Part 2)
- Open `YOUR_URL?action=seats` in a browser. You should see `{"ok":true,"total":500,"registered":0,"left":500,"open":true}`.
- Submit the form with test details and your own email. You should see the success screen with `WR-0001`, a new row in the sheet, a confirmation email to the guardian and a notification to boys@boystokings.org.
- Submit the same boy name and guardian phone again: it must be refused as a duplicate.
- Delete your test rows afterwards, then reset the counter: in Apps Script open **Project Settings → Script Properties** and set `SEQ` to `0` (or run setup() on a fresh sheet).

### Changing the code later (re-deploy)
Editing `Code.gs` does nothing until you re-deploy: **Deploy → Manage deployments → pencil icon → Version: New version → Deploy**. The URL stays the same. (Do not choose "New deployment", which creates a different URL.)

### Protecting the sheet
- Keep the spreadsheet **private**. Share it only with named organisers (Share → add emails as Editor/Viewer). The public only reaches the web app, never the sheet.
- Protect the header row and the ID/Timestamp columns: **Data → Protect sheets and ranges**.
- The "Checked In" column is for event day: type `Yes` as boys arrive. The "Status" column is `Confirmed`; if a boy withdraws, change it to `Cancelled` and his seat is released automatically.
- Download a backup copy before the event (**File → Download → .xlsx**). Registrations contain children's data: delete the sheet or archive it securely once the event is over.
- Google limits a free account to about 100 emails a day through MailApp. For 500 registrations spread over several days this is usually fine; if you expect a rush, use a Google Workspace account (about 1,500 a day) or spread the promotion.

## Part 2 – Host the website

A published claude.ai preview cannot call Google Apps Script, so the form only works once the site is hosted on a normal web address. Choose one:

### Option A – GitHub Pages (free, you said you use GitHub)
1. Create a new **public** repository, e.g. `win-right`.
2. Upload `index.html` and the whole `assets` folder (Add file → Upload files; drag both in).
3. **Settings → Pages → Source: Deploy from a branch → main / (root) → Save.**
4. After a minute your site is live at `https://YOUR-USERNAME.github.io/win-right/`.
5. Optional own address: in Settings → Pages add a custom domain such as `winright.boystokings.org`, then at your domain host add a **CNAME** record pointing `winright` to `YOUR-USERNAME.github.io`.

### Option B – Your main website/domain (cPanel / hosting file manager)
Create a folder such as `/public_html/winright/`, upload `index.html` and the `assets` folder, and visit `boystokings.org/winright/`.

### Option C – WordPress/Elementor (boystokings.org/register style)
Pasting the page into an HTML widget breaks the image paths and fonts and fights the theme. The reliable way: host as in Option A or B, then on a WordPress page add an **HTML widget** with:
```html
<iframe src="https://YOUR-USERNAME.github.io/win-right/" style="width:100%;height:100vh;border:0" title="WIN RIGHT registration"></iframe>
```
or simply link your menu/button to the hosted page.

### Final touches after hosting
- In `index.html` change the `og:image` address to the full `https://…/assets/boys-to-kings-logo.png` (or a flyer image) so WhatsApp/Facebook previews show a picture.

## What I assumed / placeholders to confirm
- **Look and feel** follows your screenshots of boystokings.org/register (teal #23788C, amber #F0B92B, aqua #E8F7F9; Poppins, Montserrat and Nunito fonts). The teal is slightly darker than the live site so white text stays readable.
- **Programme**: only the 8:00am start and 12:30pm close are given, so the programme shows the sequence without invented times.
- **FAQ answers** on what to bring, dress code and guardians are sensible defaults. Edit them to match your rules.
- **Host spotlight** has no biography because none was supplied. Add one in the Host section.
- **Age** is accepted from 12 to 20, and class SS1–SS3.
- **Photos** were cropped around each face automatically. Tunde Onakoya and Uka Eje's photos are small and may look soft; send larger versions if available.
- Speaker names and photos follow the labels you gave. "Adetayo_1" is shown as Tayo Bamiduro (MAX NG).
