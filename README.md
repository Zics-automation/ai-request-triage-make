AI Customer Request Triage (Make.com + Gemini)

An automation that reads incoming customer requests, uses AI to classify them and draft a reply, routes them, and holds risky ones for human approval before anything is sent. It includes failure logging and automatic retries.

This is a self-initiated portfolio project. All data in the screenshots is test data.

Case study: https://trusted-cobweb-41e.notion.site/Automation-Portfolio-417482d30def80f683e3ff113d7962de?source=copy_link
Demo video: [paste your video link here]

How it works
  1. A customer submits a Google Form. The response lands in a Google Sheet.
  2. Make.com picks up each new row and sends the message to Gemini, which returns a category (Billing, Support, Sales or Complaint), an       urgency level, the order number mentioned, a draft reply, and a flag for whether human approval is needed.
  3. The results are written back to the same row.
  4. A Router splits the flow:
         Ready to Send: the reply is emailed automatically and the row is marked Sent.
         Needs Approval: the owner receives an email with all the details. The row stays Awaiting Approval.
  5. A second scenario (approval sender) finds rows where the Approval column says Approved, emails the customer the reply, and marks the      row Sent.
     
Error handling
  1. Every AI and email step has an error route that logs the timestamp, scenario, module and error message to an Execution Log sheet.
  2. Failed runs are stored and retried (3 attempts, 5 minutes apart).
  3. The approval scenario only picks up rows that are Approved and still Awaiting Approval, so running it again never sends a duplicate.
  4. A filter stops the approval scenario when a row has no email address.
  5. The AI prompt forbids promising refunds, discounts or deadlines.
     
Tools
Make.com, Google Gemini (Flash model), Google Forms, Google Sheets, Gmail

Repository contents
workflows/     Exported Make.com blueprints (request triage + approval sender)
screenshots/   Scenario maps, sheet, Execution Log and email examples

Sheet structure
Form Responses 1: Timestamp, Name, Email, Order Number, Message, Category, Urgency, Extracted Order No, Draft Reply, Status, Approval, Processed At

Execution Log: Timestamp, Scenario, Module, Error Message

To run it yourself
Create the Google Form and a linked Sheet with the columns above, plus an Execution Log tab.
In Make.com, create a scenario and choose Import Blueprint from the three-dot menu at the bottom of the editor. Import the files from workflows/.
Reconnect your own Google and Gmail accounts, and select your spreadsheet in each Google Sheets module.
Add your Gemini API key from Google AI Studio and choose a Gemini Flash model.
Replace YOUR_SPREADSHEET_ID and you@example.com wherever they appear.
Limitations and next steps
The AI can take the order number from the message text instead of the form field. A future version should show both numbers in the approval email.
Approval is done by typing in the sheet. A one-click approve link would be smoother.
A knowledge-base lookup (RAG) could ground replies in real company policy.

Author
Ifeanyi, automation developer based in Abuja, Nigeria.
