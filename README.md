# Evaluating the Impact of BMI on Cholesterol

Project page: <https://rachlllg.github.io/project/2023-Evaluating_the_impact_of_BMI_on_Cholesterol/>

## Background & Requirements:

This was the final project for the Statistics for Data Science class in my Masters in Data Science program, a collaborative effort involving me and three other classmates. To retain confidentiality of the assignment, listed below are some **key** requirements which the project was built upon.

1.  The **research question** must be specific and should clearly state an X (a design feature of a product that could be modified in the production process) and a Y (a metric of success).

-   'Product' and 'success' are loosely defined and goes beyond the scope of physical goods and profits.
-   The goal of the project is to formulate an *explanatory* study, therefore, the design feature must be modifiable and the causal relationship from feature to success must be clear. An example of un-modifiable feature would be weather (not within human control), and an example of unclear causal relationship would be education and wealth (do higher education lead to higher wealth or do higher wealth provide for higher education?).

2.  The **dataset** must be publicly available and be relevant to the research question.

-   The dataset must be *large* enough to split the data into an exploration set and a confirmation set, with at least 200 observations in the exploration set (usually around 30% of the entire dataset).
-   The data must be *cross-sectional* (not have multiple measurements for a single unit of observation). An example of non-cross-sectional data would be time series.
-   The outcome variable (Y) must be *metric*. An example of non-metric outcome variable is the Likert Scale.
-   The data should attempt to include (at least a proxy) *omitted variables* that would hinder the model result.

## Dataset:

The dataset was sourced from National Health and Nutrition Examination Survey (NHANES), 2005-2006.

Source: <https://www.icpsr.umich.edu/web/ICPSR/studies/25504/datadocumentation>

The zip file in the Dataset folder contains the entire original dataset downloaded from the ICPSR Institute for Social Research, University of Michigan. Each participant is identifiable via the unique SEQN in each sub-dataset.

### Raw Data:

Below listed are the sub-datasets we extracted from the complete dataset for our analysis.

The raw sub-datasets can be found in the 'Dataset \> Raw' folder. And the user guides for each sub-dataset can be found in the 'Dataset \> User_guides' foler.

| Dataset                                      | Variable Name | Variable Description                                | User guide page number |
|----------------------------------------------|---------------|-----------------------------------------------------|------------------------|
| DS13 Examination: Body Measurements          | SEQN          | Respondent sequence number                          | 10/129                 |
|                                              | BMXBMI        | Body Mass Index (kg/m\*\*2)                         | 15/129                 |
| DS129 Laboratory: Total Cholesterol          | SEQN          | Respondent sequence number                          | 8/65                   |
|                                              | LBXTC         | Total cholesterol (mg/dL)                           | 8/65                   |
| DS111 Laboratory: HDL Cholesterol            | SEQN          | Respondent sequence number                          | 10/67                  |
|                                              | LBDHDD        | Direct HDL-Cholesterol (mg/dL)                      | 10/67                  |
| DS110 Laboratory: Glycohemoglobin            | SEQN          | Respondent sequence number                          | 9/66                   |
|                                              | LBXGH         | Glycohemoglobin (%)\*                               | 9/66                   |
| DS001 DS1 Demographics                       | SEQN          | Respondent sequence number                          | 8/25                   |
|                                              | RIAGENDR      | Gender of the sample person                         | 9/25                   |
|                                              | RIDAGEYR      | Age in years of the sample person                   | 10/25                  |
| DS242 Questionnaire: Smoking - Cigarette Use | SEQN          | Respondent sequence number                          | 10/68                  |
|                                              | SMQ020        | Smoked at least 100 cigarettes in life              | 10/68                  |
|                                              | SMD641        | \# of days smoke cigarette in past 30 days          | 15/68                  |
|                                              | SMD650        | \# of cigarettes smoked/day on days that you smoked | 16/68                  |

\* Glycohemoglobin is a lab test that measures the average level of blood sugar in an individual. It is the primary test used to diagnose diabetes.

### Clean Data:

A series of data cleaning were performed in data_cleaning.rmd file. Below are the cleaned dataset output from the data_cleaning.rmd file.

| File name | Variable Name   | Variable Description              | Variable Nature         |
|-----------|-----------------|-----------------------------------|-------------------------|
| main.csv  | TC/HDL-C        | Ratio of total to HDL-Cholesterol | Primary Y variable      |
|           | BMI             | Body Mass Index (kg/m\*\*2)       | Primary X variable      |
|           | Glycohemoglobin | Glycohemoglobin (%)               | Supplemental X variable |
|           | Age             | Age (years)                       | Supplemental X variable |
|           | Gender          | Gender (Female/Male)              | Supplemental X variable |

Of the 6360 respondents who had valid measurements for BMI, Cholesterol, and Glycohemoglobin, only around 20% (1248) answered they had smoked at least 100 cigarettes in life and provided responses for the frequency of cigarette smoking. We were unable to determine the smoking habit of the other 80%.

Consider the response on frequency of smoking could be inaccurate as respondents could feel a social pressure to under-report their frequency of smoking, we believed we should not narrow down our main dataset to only include the 1248 respondents who answered the smoking habit question. Therefore, we treated the sample of 1248 respondents as a separate dataset from the main dataset of the 6360 respondents and analyzed the effect of smoking on Cholesterol separately.

| File name   | Variable Name   | Variable Description              | Variable Nature         |
|-------------|-----------------|-----------------------------------|-------------------------|
| smoking.csv | TC/HDL-C        | Ratio of total to HDL-Cholesterol | Primary Y variable      |
|             | BMI             | Body Mass Index (kg/m\*\*2)       | Primary X variable      |
|             | Glycohemoglobin | Glycohemoglobin (%)               | Supplemental X variable |
|             | Age             | Age (years)                       | Supplemental X variable |
|             | Gender          | Gender (Female/Male)              | Supplemental X variable |
|             | num_cigarettes  | \# of cigarettes in a year        | Supplemental X variable |

Each output file was split to explore (30% of the output file) and confirm (70% of the output file). All data analysis were performed on explore dataset while the final results were generated from confirm dataset.
