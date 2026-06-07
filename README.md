# Personal Operating System (POS)

A mobile-first Google Apps Script web app for daily productivity and life management. POS combines habits, repeated routines, one-time tasks, subtasks, weekly planning, journaling, end-day reviews, streaks, and progress reports using Google Sheets as an automatic database.

## Features

- Dashboard with daily progress, streaks, completion rate, and quick actions
- Today page split into Daily, Repeated, and Tasks
- Subtask support, including parent tasks such as Prayer with Fajr, Dhuhr, Asr, Maghrib, and Isha
- Weekly planner for repeated tasks and one-time backlog assignment
- End-day review wizard for incomplete items
- Journal entries with search and date filters
- Reports for habits, task status, categories, and streaks
- Dark mode support
- RTL-friendly layout foundation
- No external frameworks

## Tech Stack

- Google Apps Script standalone project
- HtmlService templates
- HTML
- CSS
- Vanilla JavaScript
- Google Sheets database
- Script Properties for database ID storage

## Project Structure

```text
Code.gs
Index.html
Style.html
Script.html
README.md
```

## Database

The app creates a Google Spreadsheet automatically on first run and stores its ID in Script Properties using:

```js
PropertiesService.getScriptProperties()
```

Sheets created:

- `Tasks`
- `WeeklyPlan`
- `HabitLog`
- `DailyReview`
- `Settings`

## Task Types

- `daily`: appears every day
- `repeated`: appears according to a repeat pattern such as `SUN,TUE,THU`
- `oneTime`: planned manually from the backlog

## Setup

1. Open [Google Apps Script](https://script.google.com).
2. Create a new standalone project.
3. Create these files in the Apps Script editor:
   - `Code.gs`
   - `Index.html`
   - `Style.html`
   - `Script.html`
4. Copy the contents from this repository into the matching files.
5. Save the project.

## Deployment

1. In Apps Script, click **Deploy**.
2. Choose **New deployment**.
3. Select **Web app**.
4. Set **Execute as** to `Me`.
5. Set access based on your preference, usually `Only myself`.
6. Click **Deploy**.
7. Approve permissions.
8. Open the Web App URL.

On first launch, the app will create the Google Sheets database automatically.

## Updating The App

After editing files:

1. Save changes in Apps Script.
2. Go to **Deploy > Manage deployments**.
3. Edit the current deployment.
4. Select **New version**.
5. Deploy again.

## Notes

- Apps Script file names should be `Index`, `Style`, and `Script` for HTML files in the editor.
- `Code.gs` contains backend APIs and database logic.
- `Script.html` contains frontend state and UI behavior.
- `Style.html` contains all app styling.
