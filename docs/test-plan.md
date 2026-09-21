# Test Plan

Manual test cases for the multi-label Gmail → Google Sheets & Drive workflow.
Fill in the **Result** column as you run each case, and add notes on anything unexpected.

**Environment**

- n8n version: `___`
- Run mode: `___` (local / Docker / cloud)
- Test Gmail account: a dedicated test account, not a personal inbox
- Labels used: `label1`, `label2`, `label3`

## How to run

- **Manual run:** in Gmail Trigger, click **Fetch Test Event**, then **Execute workflow**. This processes only the most recent matching email, so change the trigger's Search to `label:labelN` to test each label.
- **Live run:** activate the workflow and send new emails from a second account. Apply labels with Gmail filters so they are present on arrival. The trigger only picks up emails that arrive after activation.

## Test cases

| # | Case | Setup | Expected result | Result |
|---|---|---|---|---|
| 1 | Email without attachment | One email under `label1`, no attachment | One sheet row. Attachment Count is 0 and Drive Folder Path is empty. Nothing uploaded to Drive. | ☐ Pass ☐ Fail |
| 2 | Single attachment | One email under `label1` with `report.pdf` | One sheet row. File appears at `label1/<date>/<sender>/report.pdf` with the original name. | ☐ Pass ☐ Fail |
| 3 | Multiple attachments | One email with three different files | All three files upload to the same sender folder with their original names. | ☐ Pass ☐ Fail |
| 4 | Duplicate filenames in one email | One email with two attachments named identically | Both files upload. Drive keeps them as separate files. | ☐ Pass ☐ Fail |
| 5 | Three labels at once | One email each under `label1`, `label2`, `label3` within one poll interval | Three sheet rows with the correct labels. Three separate label folders. | ☐ Pass ☐ Fail |
| 6 | Same sender, same day, one poll | Two emails from one sender under `label1`, both with attachments | One label folder, one date folder, one sender folder. No duplicate folders. Files from both emails inside. | ☐ Pass ☐ Fail |
| 7 | Same sender, different labels | One email each under `label1` and `label2` from the same sender | Separate `label1/...` and `label2/...` trees, each with its own sender folder. | ☐ Pass ☐ Fail |
| 8 | Email under two labels | One email carrying both `label1` and `label2` | Two sheet rows (one per label) and a copy of the attachments under each label. | ☐ Pass ☐ Fail |
| 9 | Apostrophe in sender name | Sender display name such as `O'Brien` | Folder is created once and found again on the next email. No search error. | ☐ Pass ☐ Fail |
| 10 | Sender with no display name | Email from an address with no name set | Folder is named with the sender's email address. | ☐ Pass ☐ Fail |
| 11 | Unconfigured label | Email under a label not in `TARGET_LABELS` | Skipped. No row and no upload. | ☐ Pass ☐ Fail |
| 12 | Repeat run | Run the same email twice manually | Two sheet rows (expected for manual runs). Existing folders are reused, not recreated. | ☐ Pass ☐ Fail |
| 13 | Live polling | Activate the workflow and send a new email | Row and files appear within one poll interval. | ☐ Pass ☐ Fail |
| 14 | Date boundary | Email received near midnight Asia/Manila time | Date folder matches the local date, not UTC. | ☐ Pass ☐ Fail |

## Notes

<!-- Record failures, fixes made and anything you'd change. This section shows how you debug. -->

| # | Issue found | Cause | Fix |
|---|---|---|---|
|   |   |   |   |
