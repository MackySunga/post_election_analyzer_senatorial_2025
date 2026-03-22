# Post Election Analyzer

A Django-based **Model-Template-View (MTV)** web application for **post-election senatorial analysis** using precinct-level election results. The system provides candidate profiles, voter turnout summaries, vote share analysis, province and municipality breakdowns, ranking charts, and choropleth-ready map integration.

## Features

- Senator profile pages
  - Last name
  - First name
  - Party
- Cascading geographic filters
  - Region
  - Province
  - Municipality
- Voter turnout analysis
  - Total number of voters in the selected scope
  - Breakdown by province
  - Breakdown by municipality
- Vote profile analysis
  - Total votes gathered by the selected candidate
  - Candidate percent share within the selected scope
  - Province-level and municipality-level vote summaries
- Top-performing province analysis
  - Top 3 provinces by candidate votes
  - Percent share of votes for that province
  - Percent share of the candidate's votes in the selected region
- Data visualizations
  - Province ranking chart
  - Municipality ranking chart
- Choropleth-ready mapping
  - Province choropleth map
  - Municipality choropleth map
- Import utility for senatorial election result files
  - Supports comma-separated, tab-separated, or semicolon-separated files

## Tech Stack

- **Python**
- **Django**
- **SQLite**
- **Chart.js**
- **Leaflet**
- **GeoJSON**

## Project Structure

```text
Post Election Analyzer/
├── LICENSE
├── NOTICE.txt
├── README.md
├── manage.py
├── db.sqlite3
├── post_election_analyzer/
│   ├── __init__.py
│   ├── asgi.py
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
└── senators/
    ├── __init__.py
    ├── admin.py
    ├── apps.py
    ├── models.py
    ├── services.py
    ├── urls.py
    ├── views.py
    ├── migrations/
    ├── management/
    │   └── commands/
    │       └── import_senatorial_results.py
    ├── static/
    │   └── senators/
    │       ├── css/
    │       ├── js/
    │       └── geo/
    └── templates/
        └── senators/
```

## Expected Dataset Columns

The importer expects the following fields:

```text
Year
Election_Type
Region
Province
Municipality
Barangay
Precinct_Code
Candidate_Name
Position
Party
Votes
Total_Voters
Total_Votes
Percentage
Contest_ID
Candidate_ID
Scraped_At
Source
```

## Setup Guide

### 1. Open the project folder

Make sure you are inside the folder that contains `manage.py`.

```powershell
cd "C:\Users\PC\OneDrive\Documents\Special Project\Post Election Candidate Profile"
dir
```

You should see:

```text
manage.py
post_election_analyzer
senators
```

### 2. Create a virtual environment

```powershell
py -m venv .venv
```

### 3. Install dependencies

If PowerShell blocks script activation, use the virtual environment Python directly:

```powershell
.\.venv\Scripts\python.exe -m pip install --upgrade pip
.\.venv\Scripts\python.exe -m pip install Django==6.0.3
.\.venv\Scripts\python.exe -m django --version
```

### 4. Create the database

```powershell
.\.venv\Scripts\python.exe manage.py makemigrations
.\.venv\Scripts\python.exe manage.py migrate
```

### 5. Import the election dataset

Place your dataset in a convenient folder, for example:

```text
.\data\senatorial_results.csv
```

Then run:

```powershell
.\.venv\Scripts\python.exe manage.py import_senatorial_results ".\data\senatorial_results.csv" --clear
```

### 6. Run the server

```powershell
.\.venv\Scripts\python.exe manage.py runserver
```

Open:

```text
http://127.0.0.1:8000/
```

## GeoJSON Map Setup

To enable the choropleth maps, add these files:

```text
senators/static/senators/geo/philippines_provinces.geojson
senators/static/senators/geo/philippines_municipalities.geojson
```

Without these files:

- the app will still run
- tables and summaries will still work
- the map panels will show placeholders or loading errors

## How the Analysis Works

### Turnout

The total number of voters is computed using **unique precinct codes**, so precinct-level totals are not double-counted across candidate rows.

### Candidate Share

Candidate share is computed as:

```text
Candidate Share = Candidate Votes / Total Votes Across All Candidates × 100
```

### Province Share

For each province:

```text
Province Share = Candidate Votes in Province / Total Votes in Province × 100
```

### Candidate Vote Concentration

For Top 3 Province analysis:

```text
Candidate Vote Concentration = Candidate Votes in Province / Total Candidate Votes in Selected Region × 100
```

## Example Workflow

1. Import the election file.
2. Open the site in your browser.
3. Go to the **Senators** page.
4. Select a candidate.
5. Filter by region, province, and municipality.
6. Review turnout, vote share, charts, and maps.

## Common Issues

### PowerShell blocks activation

Use:

```powershell
.\.venv\Scripts\python.exe manage.py runserver
```

instead of activating with `Activate.ps1`.

### Django not found

Reinstall Django into the virtual environment:

```powershell
.\.venv\Scripts\python.exe -m pip install Django==6.0.3
```

### Import command not found

Make sure this file exists:

```text
senators\management\commands\import_senatorial_results.py
```

### Maps not loading

Make sure the GeoJSON files exist and that their boundary names match your dataset names closely enough for the name-normalization logic.

## License

Post Election Analyzer  
Copyright (C) 2026 Bob Mathew Sunga

This project is licensed under the GNU General Public License, version 3, or, at your option, any later version.

This program is free software: you can redistribute it and/or modify it under the terms of the GNU General Public License as published by the Free Software Foundation, either version 3 of the License, or, at your option, any later version.

This program is distributed in the hope that it will be useful, but WITHOUT ANY WARRANTY, without even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the GNU General Public License for more details.

You should have received a copy of the GNU General Public License along with this program. If not, see <https://www.gnu.org/licenses/>.

## Author

**Bob Mathew Sunga**

## Future Improvements

- Compare two senators side by side
- Barangay-level drilldown
- Export to Portable Document Format (PDF)
- Export to Comma-Separated Values (CSV)
- Dashboard summary cards by party
- Time-series analysis for multi-election datasets
