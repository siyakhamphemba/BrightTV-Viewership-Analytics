BrightTV Viewership Analytics
A customer value management (CVM) analytics case study for BrightTV, a subscription television service. The objective: help BrightTV's CVM team grow the subscription base by uncovering who watches, when they watch, what drives consumption, and how to win back low-engagement periods.

Business Context
BrightTV's CEO set a goal to grow the company's subscription base this financial year. This project analyses raw user profile and viewership transaction data to answer four core questions for the CVM team:

What are the user and usage trends on BrightTV?
What factors influence consumption?
What content should be recommended on low consumption days?
What initiatives would grow BrightTV's user base?
Dataset
Two raw sources, provided in UTC and joined on UserID:

Table	Description	Rows
bright_tv_user_profile	Subscriber demographics — name, gender, race, age, province, social handle	5,376
bright_tv_viewership	One row per viewing session — channel, record date/time, duration	10,001
Approach
1. Data Exploration & Validation Inspected both raw tables, validated the join key, and checked for data quality issues — including a duplicated UserID column in the viewership table and ~175 "ghost" profiles (sessions with no associated demographic data).

2. Data Cleaning Decisions

Issue	Decision	Rationale
Ghost profiles (no name/demographics)	Soft excluded — kept in consumption totals, excluded from demographic breakdowns	Their viewing activity is real and shouldn't be discarded, but they carry no demographic value
Duplicate UserID column	Dropped, kept one reference column	Redundant, identical values
UTC timestamps	Converted to SA time (UTC+2)	SA does not observe daylight saving, so a flat +2 hour offset applies year-round
Duration stored as timestamp	Converted to total minutes	Required for SUM/AVG aggregation
Age	Bucketed into 5 life-stage segments: Kids (0-12), Teenagers (13-17), Youth (18-29), Adults (30-59), Pensioners (60+)	Life-stage groupings are more actionable for content and marketing decisions than raw age
3. SQL Transformation Layer Built a master analytical view (bright_tv_master) in Databricks SQL that joins both tables, applies all cleaning logic above, and extracts date/time dimensions (day, month, year, hour, day of week) needed for trend analysis. See sql/BrightTV_Viewership_Analysis_2026.sql.

4. Analysis 12 analytical queries covering subscriber demographics, peak viewing times, channel performance, and low-consumption day identification — each one feeding directly into the dashboards below.

5. Visualisation Three themed Power BI dashboards, connected to the bright_tv_master view:

Dashboard	Focus	Key Visuals
Demographics	Who is watching	Gender split, age group distribution, province breakdown, race breakdown
Time Trends	When are they watching	Peak viewing hours, day-of-week trends, monthly trends, lowest consumption days
Content & Channels	What are they watching	Top channels by watch time, channel preference by age/gender, consumption by province
Repository Structure
BrightTV-Viewership-Analytics/
├── README.md
├── sql/
│   └── BrightTV_Viewership_Analysis_2026.sql   # Full annotated SQL notebook
├── dashboards/
│   └── (Power BI .pbix files / screenshots)
├── docs/
│   └── (Findings write-up, presentation deck)
└── data/
    └── (Data dictionary / sample extracts, if shared)
Tools Used
Databricks SQL — data cleaning, transformation, and aggregation
Power BI — dashboard visualisation
Microsoft Word — findings documentation and presentation narrative
Key Findings
See docs/ for the full findings write-up.

A summary of headline insights will be added here once the analysis is finalised — e.g. subscriber gender split, peak viewing windows, top/bottom performing channels, and lowest consumption days.

Recommendations
See docs/ for full recommendations tied to each of the four business questions.

Author
Siyakha Ntuli
