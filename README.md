# Data Room mockup

This folder holds a clickable HTML mockup of a proposed "Data Room" page for CFO Desk. The page uses sample data only. It does not connect to the real app.

Version 3 follows the reviews with Sadaf on 18/11/2026 and 20/11/2026. Version 1 stays in git history (last version 1 commit `e3dff77`).

## Open the mockup

1. Open `index.html` in a browser.

## What the mockup shows

The page has one card for each page of the app. Each card has one or more slots. A slot shows the database (CFO Desk), an arrow, and the Google Sheet that receives the data. The file name under the sheet icon is a link. It opens the Google Sheets home page in a new tab.

| Page | Slots |
|---|---|
| Financial Metrics | 1 |
| Operating Metrics | 1 |
| Financial Modelling | Up to 3: Base, Downside and Upside. "Add case" adds the next one |
| Cash Flow | 1 |
| Cap Table | 1 |
| ARR | 1 |

Each slot has one of three states. Each state has one main action. Live and broken slots also have a small Disconnect button.

| State | Picture | Action |
|---|---|---|
| Not connected | Dashed empty sheet slot | Connect |
| Live | Moving arrow, "Synced 12 min ago" | Sync now |
| Needs reconnect | Arrow with a red break and a diamond, one line of reason | Reconnect |
| Needs reconnect, wrong layout | Same picture, badge "Wrong layout" | Change sheet |

You can do these things in the mockup:

- Use the dashed "Mockup" bar to jump to Empty, Healthy or Broken. The page starts Empty.
- Select Connect. A dialog shows the steps: Continue with Google, Choose spreadsheet, Confirm, Connecting, Connected.
- In Choose spreadsheet, browse a sample Drive. Open a folder to go in. Use the path above the list to go up. Select a sheet to choose it.
- Select "Untitled spreadsheet" as the sheet. Then select Connect. The dialog shows "Sheet layout does not match". The slot does not change.
- Select Sync now on a live slot.
- On Financial Modelling, select "Add case" to add a slot. The button stays disabled until Base is connected.
- After 3 cases, the button stays disabled and shows "Limit is 3 cases."
- Select Reconnect on a broken slot. The dialog asks for Google access again and restarts the feed.
- Select Change sheet on a slot with the badge "Wrong layout". The Broken state has one such slot: Operating Metrics.
- Select Disconnect on a live or broken slot. Then confirm in the dialog. The slot returns to Not connected.
- Filter by Needs attention or Not connected.
- Select a file name. The Google Sheets home page opens in a new tab. The mockup opens no real sheet.

## What changed from version 1

| Removed | Reason |
|---|---|
| Download buttons, Exports section, Exports count | Sadaf: the app writes to a sheet, so nothing needs a download |
| Details panel and refresh history table | Sadaf: too much text |
| Push now and Refresh now | One action remains: Sync now |
| Stale badge, Rows to review filter | Not part of the new design |
| Open page buttons and route labels | Not needed in the new design |
| Xero and Google Doc connection types | Every slot is now a Google Sheet |

## Differences from the real app

Read this before the build. The mockup follows Sadaf's description: every page sends data from the database to a sheet.

| Item | Status |
|---|---|
| Outbound sync (database to sheet) for Financial Metrics, Operating Metrics, Financial Modelling and Cash Flow | Does not exist. Today these pages read FROM a sheet (Xero, QuickBooks or a Google Sheet for Financial Metrics) or from a Google Doc. Cash Flow reads bank statements. Only Cap Table, ARR and the Actuals write-back write to a sheet |
| Actuals write-back | The mockup folds it into the Financial Modelling slots. The app uses the same sheet and writes one tab when the user selects "Write to Sheet". There is no scheduler |
| One Reconnect button | Does not exist. The app has three repair paths: Connect Google, Change spreadsheet and Reconnect Google. The mockup shows Reconnect, and Change sheet for a wrong layout |
| Live status and last sync time for Cap Table and ARR | Not recorded. The app stores only the last write time and the last write error |
| One endpoint that returns every slot for the Data Room | Missing. It needs a new backend route |
| Number of forecast cases | The app has no fixed limit. Sadaf said up to 3 and Maddie said it can be 1, 2 or more than 3. The mockup uses "Add case", as the app does, but it stops at 3 cases |
| Creating a sheet | The app never creates a sheet. The user picks one with the Google file picker. The mockup shows a fake picker |
| Export log (who exported what and when) | Not needed while the page has no downloads |

The connect steps match the app: sign in with Google, pick a spreadsheet with the Google picker, then pick a tab. Cap Table always uses the tab "FP to Sheet". The error lines and the status label "Live" use the wording of the app.

## Sample data notes

- All names, times and the address `founder@example.test` are invented.
- The Google sign-in step shows one button and no password field. The mockup is on a public site.
- The colours and fonts come from `founders-tool-react/src/styles/global.css` in the real app. The sidebar labels come from `Sidebar.tsx`.
- The sheet icon follows the app's `SheetIcon`. The mockup draws it larger. The app has no database icon, so the mockup reuses the sidebar cylinder.
- The arrow animation stops when the user sets "reduce motion".

## Open questions for Sadaf

1. Where does the page sit in the sidebar: Data or Share? The mockup uses Data.
2. Do the page-level downloads (CSV exports, Cash Flow Excel, ARR .xlsx) go away because the sheets replace them, or do they move into the Data Room? Sadaf said "no download option". Maddie thought the export buttons move into the Data Room. The mockup follows Sadaf.
3. Which role can see the page: founders only, team members, or investors too?
4. Does Cash Flow get a destination sheet? The page shows one. The app has none today.
