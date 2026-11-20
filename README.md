# Ask Sara – AI Student Assistant (n8n)

An AI chat assistant built with **n8n** that answers student questions and books office-hours meetings automatically, checking real calendar availability, asking for confirmation, and logging every booking to Google Sheets.

Built by **Sara Abbasi**, Senior Lecturer (Computer Science & Software Engineering) and ML engineer.

---

## What it does

| Feature | How it works |
|---|---|
| **Answers student questions** | Reads the official website (HTTP Request tool) and answers only from that content, with page links. |
| **Checks availability** | Looks up the real Google Calendar for the requested day before offering any slot. |
| **Books 20-minute meetings** | Creates the Google Calendar event only **after the student explicitly confirms**. |
| **Logs every booking** | Appends one row per confirmed booking to Google Sheets (date, time, name, email, course, reason, timestamp). |
| **Protects academic integrity** | Refuses to do graded assignments, never discloses marks or exam content. |
| **Stays honest** | Never claims a booking or log succeeded unless the tool confirms it, and tells the student exactly what failed. |
| **Bilingual** | Replies in English or Roman Urdu, matching the student. |
| **Reliable** | Uses a main AI model plus an automatic **fallback model** if the first one is busy. |

## Example conversation

> **Student:** Is Monday 23 November at 11:00 AM free?
> **Ask Sara:** Yes, Monday, 23 November 2026, 11:00–11:20 AM is free. To book it, please share your full name, email, course, and reason for the meeting.
> **Student:** Test Student, test.student@example.com, HND Programming, help with Python assignment feedback.
> **Ask Sara:** Please confirm: 23 Nov 2026, 11:00–11:20 AM, Test Student, HND Programming. Shall I book it?
> **Student:** Yes, confirmed.
> **Ask Sara:** Your meeting is booked for Monday, 23 November 2026, 11:00–11:20 AM.

## Architecture

```
Chat trigger (public hosted chat)
        │
        ▼
   AI Agent ── system prompt (rules, booking procedure, tone)
   ├─ Chat model:      Google Gemini (gemini-3.1-flash-lite)
   ├─ Fallback model:  Google Gemini (gemini-3.5-flash-lite)
   ├─ Memory:          Simple Memory (last 5 messages per session)
   └─ Tools:
        ├─ HTTP Request ............ reads the website
        ├─ Google Calendar ......... get events for a day (availability)
        ├─ Google Calendar ......... create a 20-minute event
        └─ Google Sheets ........... append booking row
```

## Office-hours rules (configurable in the system prompt)

- Monday to Thursday, 11:00 AM – 3:00 PM (Asia/Karachi)
- Every meeting is exactly 20 minutes; last slot 2:40 – 3:00 PM
- Name, email, course, reason, and preferred time are collected before booking

## Tech stack

n8n · AI Agent (LangChain nodes) · Google Gemini API · Google Calendar API · Google Sheets API

## Setup

See **[docs/SETUP.md](docs/SETUP.md)** for step-by-step installation.

## Project structure

```
workflow/ask-sara-student-assistant.json   n8n workflow (import this)
docs/SETUP.md                              setup guide
docs/bookings-sheet-template.csv           column headers for the Google Sheet
LICENSE                                    All Rights Reserved
```

## Hire me

Need an AI assistant, booking bot, or n8n automation for your business, school, or clinic?

- Website: https://abbasisarah.github.io/hire/
- Fiverr: https://www.fiverr.com/sarahabb

## License

Copyright © 2026 Sara Abbasi. **All Rights Reserved.**
This code is shared for viewing and portfolio purposes only. You may not copy, modify, distribute, or use it without written permission. See [LICENSE](LICENSE).
