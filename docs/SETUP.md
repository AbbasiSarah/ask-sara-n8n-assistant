# Setup Guide – Ask Sara (n8n)

> Using this workflow requires written permission from the author (see `LICENSE`).

## Requirements

- n8n (self-hosted or n8n Cloud) with the AI Agent / LangChain nodes
- A Google Gemini API key (free tier works) from Google AI Studio
- A Google account with Google Calendar and Google Sheets

## 1. Import the workflow

1. In n8n, open **Workflows → Create workflow**.
2. Open the **⋯** menu → **Import from file**, then choose `workflow/ask-sara-student-assistant.json`.

## 2. Connect the AI models

1. Open **Google Gemini Chat Model** → **Credential** → create a *Google Gemini (PaLM) API* credential with your API key.
2. Do the same for **Google Gemini Chat Model1** (the fallback model).
3. If a model is retired or overloaded, choose another Gemini model from the **Model** list.

## 3. Connect Google Calendar

1. Open **Get many events in Google Calendar** → create a Google Calendar OAuth2 credential.
2. Replace `YOUR_GOOGLE_CALENDAR_ID` with your calendar (usually your Gmail address), or pick it from the list.
3. Repeat for **Create an event in Google Calendar**, using the **same** account and calendar.

## 4. Connect Google Sheets

1. Create a Google Sheet with a tab named **`ask sara`**.
2. Paste the headers from `docs/bookings-sheet-template.csv` into row 1:
   `Date, Time, Student Name, Email, Course, Reason, Booked At`
3. Open **Append row in sheet in Google Sheets** → create a Google Sheets OAuth2 credential.
4. Replace `YOUR_GOOGLE_SHEET_URL` with your sheet's URL.

> **Important:** the Google account in this credential must have **Editor** access to the sheet. Otherwise the log fails with *"Forbidden – the caller does not have permission"*.

## 5. Customise

Open the **AI Agent** node → **System Message** to change office hours, meeting length, timezone, website, language, or tone.
Update the **HTTP Request** node URL to your own website.

## 6. Test, then publish

1. Click **Open chat** and send: *"Is Monday at 11:00 AM free? Just check, don't book."*
2. Then test a full booking with dummy details and confirm. Check that the event and sheet row appear, then delete them.
3. Click **Publish**. Share the chat URL from the **When chat message received** node with students.

## Troubleshooting

| Problem | Fix |
|---|---|
| Run stays "Running" for minutes | The Gemini model is overloaded (503). Switch model or rely on the fallback. |
| `404 … no longer available to new users` | That Gemini model is retired. Pick a newer one. |
| Calendar "Bad Request" | Check that the After/Before fields contain one expression each, with no extra blank lines. |
| Sheets "Forbidden" | Share the sheet (Editor) with the account used in the Sheets credential. |
| Students still see old behaviour | Click **Publish**. The public chat uses the published version only. |
