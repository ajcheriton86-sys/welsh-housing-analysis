Welsh housing data analysis: what I've done so far
I want to keep track of the difference between running commands and reading data, because they're not the same skill. Running commands is just typing; reading is the thinking I do once the output appears on screen.
How "reading data" actually works
When I run a command, the computer prints output. That output is the answer to one specific question. Reading the data is the thinking I do after the output appears, and it's roughly five steps:
1.	What did I ask for?
2.	What did the computer give back?
3.	What do the items have in common?
4.	Is there structure or pattern within the answer?
5.	What does that mean for my analysis?
For example, when I asked for unique values in the Area column, I got back a list of 26 text strings, all of which were places in Wales. Within that list, there's a structure: one country, three regions, twenty-two local authorities, nested inside each other. That means I shouldn't sum across all twenty-six rows for one period, or I'd triple-count everyone.
Python is just the tool for getting answers; the skill is asking useful questions and noticing what's in the output beyond the surface.
About the dataset
•	Source: StatsWales HOUS0413, "Households for which assistance has been provided by outcome and household type".
•	Coverage: 1 April 2015 onwards (post-Housing (Wales) Act 2014).
•	Size: 45,774 rows × 19 columns.
•	Format: long — each row is one observation.
Meaningful columns (5)
•	Data values — the count.
•	Data description — household category (e.g., "All households").
•	Period — time period.
•	Area — geography.
•	Outcomes — what happened (e.g., "Successful prevention").
Metadata columns (14)
•	_sort — display ordering.
•	_reference — codes; useful for joining to other datasets later.
•	_hierarchy — how values nest within a dimension.
•	Notes — caveats on specific data points.
Geography
There are 26 unique areas across three nested levels: one country, three regional groupings, and twenty-two local authorities. The trap is that these aren't 26 separate populations; they overlap. I need to filter to one geographic level before summing, or I'll triple-count.
Time
There are 33 unique periods, but the reporting structure has changed over time:
•	2015-16 to 2019-20: full quarterly plus annual.
•	2020-21 to 2022-23: annual only (most likely because of COVID-disrupted data collection).
•	2023-24 onwards: half-yearly (April-September) plus annual.
For my 2019-2024 analysis window, this means annual data is the only consistent granularity across the whole period. I can't make quarterly comparisons across the years because most of them don't have quarterly data.
Outcomes (29 categories)
The 29 entries in the Outcomes column aren't a flat list; they're organised around Welsh homelessness law, which mirrors the English Housing Act Part 7 prevention/relief duties under the HRA 2017:
•	Eligibility (2): Ineligible households; Eligible but not homeless or threatened.
•	Section 66 — Prevention duty (8): the "threatened with homelessness within 56 days" duty. Contains a total ("Number of outcomes"), success indicators (Successful / Unsuccessful prevention), and reasons for case closure (Assistance Refused, Non-cooperation, Application Withdrawn, etc.).
•	Section 73 — Relief duty (8): "already homeless, 56 days to relieve", the equivalent list for households already homeless. Same internal structure: total, Successfully / Unsuccessfully relieved, plus closure reasons.
•	Priority need outcome totals (3): Eligible, homeless but not in priority need; Eligible, homeless and in priority need but intentionally so; Eligible, unintentionally homeless and in priority need (Section 75).
•	Section 75 — Main housing duty (7): households unintentionally homeless with priority need — the most serious duty. "Positively discharged" is the success indicator; same closure reasons.
•	Grand totals (2): Total Outcomes; Total prevention / Relief.
That's 2 + 8 + 8 + 3 + 7 + 2 = 29.
The same trap from geography applies here: I shouldn't sum all 29 categories, or I'll double-count. A "Successful prevention" sits inside its Section's "Number of outcomes", which sits inside "Total Outcomes". For demand analysis, I'll need to pick specific categories, most likely the per-section "Number of outcomes" subtotals, since those count the cases the system handled at each duty stage.
Exploration commands learned so far
•	Load pandas: import pandas as pd.
•	Load a CSV: df = pd.read_csv("file.csv").
•	See the first 5 rows: df.head().
•	See dimensions (rows, columns): df.shape.
•	See all column names: df.columns.
•	See unique values in a column: df['column_name'].unique().


