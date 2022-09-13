# My Project 1 : Standardized Test Analysis

<br>

## Problem Statement

***
The College Board (SAT), in partnership with ACT Inc. wishes to explore if

* Test Participation Rates
* Household Median Annual Income
* State Population
* College Enollment Rates

have any relationship with each state's ACT and SAT performance.

We are a team of analysts hired to analyse the provided data, use supporting data from other sources and present our findings to the college boards.

External sources were sourced from:

1. [National Center for Education Statistics](https://nces.ed.gov/)
2. [United States Census Bureau](https://www.census.gov/en.html)

#### **Team Members and Work Definition**

1. Anand Ramchandani (anand308@hotmail.com)
    1. Household Median Annual Income
    2. State Population
2. Emi Chua (yinxian900205@gmail.com)
    1. College Enrollment Rates
3. Maybelle Auw (jewelbelle@gmail.com)
    1. Test Participation Rates
4. Bryan Goh (bryan.gohyc@gmail.com) (me)
    1. SAT VS ACT Scores

<br>

## Sources Used

***

### Provided Sources

* ACT 2017
* ACT_2018
* ACT_2019
* SAT_2017
* SAT_2018
* SAT_2019

### External Sources

* [ACT 2018](https://nces.ed.gov/programs/digest/d18/tables/dt18_226.60.asp)
  * Table shows additional data scores for ACT 2018 subjects
* [ACT_2019](https://nces.ed.gov/programs/digest/d19/tables/dt19_226.60.asp)
  * Table shows additional data scores for ACT 2019 subjects
* [Population 2017 - 2019](https://www.census.gov/data/tables/time-series/demo/popest/2010s-state-total.html)
  * Under Table, sub-section: "Population, Population Change, and Estimated Components of Population Change: April 1, 2010 to July 1, 2019 (NST-EST2019-alldata)"
  * The first dataset was used - file title: 'Annual Estimates of the Resident Population for the Unites States, Regions, States, and Puerto Rico: April 1, 2010 to July 1,, 2019 (NST-EST2019-01)
* [Total Enrollment of Students for the Year 2017](https://nces.ed.gov/programs/digest/d18/tables/dt18_307.20.asp)
* [Total Enrollment of Students for the Year 2018](https://nces.ed.gov/programs/digest/d19/tables/dt19_307.20.asp)
* [Total Enrollment of Students for the Year 2019](https://nces.ed.gov/programs/digest/d20/tables/dt20_307.20.asp?current=yes)
* [Median Household Income for Years 2017 - 2019](https://www.census.gov/data/tables/time-series/demo/income-poverty/historical-income-households.html)
  * Under 'Table H-8. Median Household Income by State, filename = 'Median Household Income by State'
* [ACT/SAT Concordance Tables](https://satsuite.collegeboard.org/higher-ed-professionals/score-reports/score-comparisons/sat-act)
  * Sheet Names:
    * Tables A1 & Table A2 - SAT Total to ACT Composite conversion chart
    * Tables B1 & B2 - SAT Math to ACT Math conversion chart
    * Tables C1 & C2 - SAT EBRW to ACT English+Reading conversion chart
  * Reference: [ACT/SAT Concordance Guidelines](https://satsuite.collegeboard.org/media/pdf/guide-2018-act-sat-concordance.pdf)
* [USA State Abbreviations]("https://www2.census.gov/geo/docs/reference/state.txt")

<br>

## Steps Taken

***

### Date Pre-Processing

1. **ACT & SAT files - Provided data**
   1. All 6 provided datasets (3 from ACT, 3 from SAT) were read
   2. Addition of the 'year' column to help differentiate the datasets
   3. df_act and df_sat were merged together to form df_all
2. **Addition of external datasets**
   1. External ACT 2018 and 2019 datasets read
      1. Drop the columns and rows not related to 2018/2019 respectively
      2. Both datasets are joined together = df_ext
   2. Total Resident Population
      1. 1 file read, rows skipped due to information not relevant
      2. Rows at index 1 - 4 are dropped as the information is not used - data of non-state related information
      3. First column renamed to 'State' and reformatted to be consistent
      4. Row with "United States" as State renamed to "National"
      5. Dataset columns were then rearranged to 'State','Year' and 'Pop' and named df_pop
   3. Median Household Income
      1. 1 file read, skip the top 7 rows, and merge cells are filled with repeated data across the rows
      2. Take out the required columns (2017/2018/2019)
      3. State "United States" renamed to "National
      4. Duplicate states dropped
      5. Dataset is then rearranged to 'State','Year' and 'Income' and named df_income
   4. Student Enrollment
      1. 3 files are read, with the top 3 rows skipped
      2. Dropped rows and columns not related to the year (2017/2018/2019)
      3. State "United States" renamed to "National
      4. Remove commas from enrollment data
      5. Values found with '†' were removed and filled with 0
      6. All 3 datasets were joined together to form df_en
3. **Combination of all data**
   1. All datasets are merged together based on the 'State' and 'Year' columns
      1. df_all
      2. df_ext
      3. df_pop
      4. df_income
      5. df_en

### Data Tidying

1. **Tidying of Data**
   1. Checked for duplicate or missing states - Maine duplicated (removed), National - less by 1 (2018)
   2. Reformatted participation and participation_sat to float without the '%'
   3. Extra letter 'x' found at composite column for Wyoming
   4. Found low score at 'science' and 'math_sat' columns for Maryland
   5. Found that the null values are the 'National' rows
        1. Creation of "National" row for year 2018
        2. Fill in the missing data for "National" for year 2018 and 2019
        3. Fill in the rest of the missing data in "National" SAT columns for all 3 years

### Additional Data Conversion

1. **Conversion of SAT score range to ACT score range**
   1. Read in ACT/SAT Concordance Tables, each dataframe for each tab
      1. df_total_convert - Tables A1 & Table A2
      2. df_math_convert - Tables B1 & B2
      3. df_ebrw_convert - Tables C1 & C2
   2. For all dataframes, rows and columns not relevant to the data were removed.
   3. Respective total_sat, math_sat and ebrw_sat were used in conjunction to create 3 new columns with data in ACT score ranges
      1. total_sat_convert
      2. math_sat_convert
      3. ebrw_sat_convert
   1. eng_read_act column was added = the sum of English ACT + Reading
2. In addition, a Min-max scaling was also used. This was taken from the sklearn's preprocessing library
   1. For a more 'fair' comparisons between the act/sat scores and the participation rates
   2. The dataframe was named df_scaled, and have the columns names ending with '_s'

<br>

### End Result

***

| Features           | Types    | Datasets    | Descriptions|
| ----------------- | ------- | --------- | -----------------|
| state             | object  | ACT & SAT  | List of States                                                                                |
| participation_sat | float   | ACT        | ACT Participation rate of each state in percentage, where 30.0 represents 30.0%               |
| english_act       | float   | ACT        | Average ACT English score for each state                                                          |
| math_act          | float   | ACT        | Average ACT Math score for each state                                                             |
| reading_act       | float   | ACT        | Average ACT Reading score for each state                                                          |
| science_act       | float   | ACT        | Average ACT Science score for each state                                                          |
| composite_act     | float   | ACT        | Average total ACT scores for each state. The Composite score has a minimum of 1 and maximum of 36 |
| year              | object  | Self-added | Year of data reflected|
| participation_sat | float   | SAT        | SAT Participation rate for each state in percentage, where 30.0 represents 30.0%|
| math_sat          | float   | SAT        | Average SAT Math score for each state|
| total_sat         | interger   | SAT        | Total SAT score = Math score + EBRW score|
| ebrw_sat          | float   | SAT        | Average SAT EBRW score for each state|
| pop               | integer | External   | Total estimated population for each state from 2017 to 2019.                                  |
| income            | float   | External   | Total medium income for each state from 2017 to 2019
| enroll            | integer | External   | Total estimated enrollment of students into Universities from 2017 to 2019|
|total_sat_convert|integer|External| Conversion of SAT Total scores to ACT Composite score range|
|math_sat_convert|integer|External| Conversion of SAT Math scores to ACT Math score range|
|ebrw_sat_convert|integer|External| Conversion of SAT EBRW scores to ACT English+Reading score range|
|eng_read_act|float|Self-added| Total of ACT English and ACT Reading|

<br>

## Data Visualisations

***
**Note:** Visualisations were done on SAT VS ACT Scores

1. State maps were plotted from plotly express library with the usage of external USA abbreviations dataset
   1. To show the top 5 and bottom 5 states for SAT and ACT scores
   2. Found that the top 5 states are mostly clustered together
2. A distribution was plotted, and found that the distribution is roughly the same, but SAT shows a higher score overall.
3. Scatter plots of SAT Total vs SAT subjects, and ACT composite vs ACT subjects, and found a linear relationship which could help to predict that if a student has a high in any subject, the student would most likely do well overall.

<br>

## Findings
***

1. Top SAT scores are at the North region of the map
2. Top ACT scores are at the North-East region of the map
3. States with higher ACT scores tend to have lower SAT scores and vice versa
4. Scoring a high score in ACT/SAT related subjects most likely gives a high overall score in that respective examination
5. Lower SAT participation rates tends to give higher SAT scores and vice versa for ACT
6. States with high ACT/SAT scores tend to be clustered together geographically.

## Conlusions
***
### Overall Conclusions

1. Low SAT/ACT scores → High SAT/ACT participation rates
2. High SAT participation rate → Low ACT participation rate
3. Low SAT participation rate → High ACT participation rate
4. High Income/Big Population → High SAT participation, low ACT participation
5. High Income/Big Population → Low SAT performance, high ACT performance


<br>

## Overall Recommendations

1. More data required from:
   1. Years (ex. 2011 to 2021) for a better certainty on trends and association
   2. Different government/state education policies
   3. High school funds and expenditure
   4. More research on factors affecting:
   5. States with high SAT/ACT scores tend to be next to each other
   6. Low ACT scores from low income households
   7. States with high SAT scores with low SAT participation rates
   8. Relationship between household income and SAT scores
   9. Enrollment rates may be affected universities that opt out of ACT/SAT exams
