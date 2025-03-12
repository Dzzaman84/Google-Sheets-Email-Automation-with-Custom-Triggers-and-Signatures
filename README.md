# Google Sheets Email Automation
## Overview
This script automates email sending based on specific trigger words entered in Column D of a Google Sheet. The recipient's email is taken from Column A of the same row. When a trigger word is detected, the script sends a predefined email message.

## How It Works
1. A user enters one of the predefined trigger words in Column D.
2. The script retrieves the recipient's email from Column A.
3. If the entered word matches a predefined trigger, an automated email is sent.

## Supported Trigger Words & Messages
- **"Are you there"** → "Hello! Just checking in. Let me know if you need anything."
- **"Bye"** → "Goodbye! Have a great day!"
- **"Hello"** → "Hi there! Hope you're doing well."

## Installation & Setup
1. Open your **Google Sheet** and navigate to **Extensions > Apps Script**.
2. Paste the script into the editor.
3. Save and authorize the script when prompted.
4. The script will automatically run whenever an edit is made in Column D.

## Customization Tips
- **Adding More Trigger Words**: Update the `messages` object with additional key-value pairs.
- **Changing Email Subject**: Modify the `"Automated Response"` text in the `sendEmail` function.
- **Modifying Sender Name**: Update the `name` field in `MailApp.sendEmail`.
- **Ensuring Script Functionality**: Make sure the Google Sheets trigger is set to "onEdit" so it runs automatically when changes are made.

## Code Implementation
```javascript
function onEdit(e) {
  var sheet = e.source.getActiveSheet();
  var range = e.range;
  var column = range.getColumn();
  var row = range.getRow();

  if (column === 4) { // Check if edit is in Column D (4th column)
    var email = sheet.getRange(row, 1).getValue(); // Get email from Column A
    var triggerWord = range.getValue();
    
    var messages = {
      "Are you there": "Hello! Just checking in. Let me know if you need anything.",
      "Bye": "Goodbye! Have a great day!",
      "Hello": "Hi there! Hope you're doing well."
    };
    
    if (messages[triggerWord]) {
      sendEmail(email, "Automated Response", messages[triggerWord]);
    }
  }
}

function sendEmail(recipient, subject, body) {
  MailApp.sendEmail({
    to: recipient,
    subject: subject,
    body: body,
    name: "Automated System"
  });
}
