Here’s a refined version of your README content:

# Data Science Job Market

Landing a data science job has been my goal since early 2023, and I've been diligently working towards it. Reading through numerous job descriptions to identify the necessary skills can be time-consuming. Over the past few months, I've been actively searching for and applying to data science positions. I’ve observed that the required skills for these roles vary significantly depending on the company's tech stack and data pipeline. The primary objective of this project is to extract skills from job descriptions and analyze the extracted data.

## Table of Contents

## Instructions

## Installation
To install the required packages, use pip:

```console
pip install -r requirements.txt
```

For more details on installing packages using pip, refer to [this tutorial](https://packaging.python.org/en/latest/tutorials/installing-packages/).

### Data

Most data science jobs in Canada are posted on two major job boards: Glassdoor and Indeed. For this project, I collected data from these two websites, gathering over 600 job postings across Canada in November and early December 2023.

Since Indeed and Glassdoor don't provide APIs for developers to request job listings, I decided to use web scraping to collect the data.

![Recording 2023-12-08 at 13 17 57](https://github.com/alextr1602/data-science-job-market/assets/134574511/936f23c7-e1a4-444b-bae1-c08f2081c159)

Given the dynamic nature of the Indeed and Glassdoor websites, where job postings require interaction to reveal data, I found that traditional methods like BeautifulSoup were insufficient. Therefore, I used Selenium WebDriver to scrape data from these sites. The scrapers are located in the files `indeed_scrape.ipynb` and `glassdoor_scrape.ipynb`.

## Code Structure 

### Data Collection

Data is collected using Selenium to navigate and scrape information from the job boards. The scrapers are available in `indeed_scrape.ipynb` and `glassdoor_scrape.ipynb`.

Running the `indeed_scrape.ipynb` file opens a web driver that connects to the Indeed webpage. The script locates all job listing objects and iterates through them. It clicks each job listing, waits for the page to load, and then extracts details such as Job Title, Company Name, Location, Benefits, Job Type, and Job Description. This data is stored in a dictionary, which is eventually added to an output list. For this project, the scraper processes the first 25 pages of Indeed.

A similar approach is used with Glassdoor. However, instead of looping through pages, the scraper clicks the **`Show More`** button 15 times, each time loading 30 job listings, resulting in approximately 450 postings. The scraper collects Job Title, Company Name, Location, and Job Description. Since Glassdoor does not provide a separate object for benefits, they may later be extracted from the job descriptions.

The extracted data is saved in the `/data` folder as `indeed_job.csv` and `glassdoor_job.csv`.

Below are samples of the scraped data from the two job boards:

![Glassdoor scrape sample](https://github.com/alextr1602/data-science-job-market/assets/134574511/f3100043-f24b-44c7-804d-095c625dc4ee)
<p align="center"><em> Glassdoor Scrape Sample </em></p>

![Indeed scrape sample](https://github.com/alextr1602/data-science-job-market/assets/134574511/c03bd64c-ee70-4e6c-acb2-92a7f46217e6)
<p align="center"><em> Indeed Scrape Sample </em></p>

### Data Cleaning

Although data cleaning is performed at different stages of the project, additional cleaning is done here to prepare the data for further use. The approach involves cleaning data from each job board, transforming it into a consistent format, and then merging the two datasets.

For data from Indeed, the cleaning process includes:
- **Date**: Remove all non-digit characters, except for the dot (e.g., "Posted/nPost" is removed). Use the post's age to determine the posting date.
- **Location**: If the location is marked as remote or hybrid, categorize the job accordingly. Extract the province, city, and address if available; otherwise, leave the cell NULL.
- **Pay**: Remove all non-digit characters, except for the dot. Extract the starting and maximum pay, convert the pay to annual amounts, and store them as a string.

For data from Glassdoor, the cleaning process includes:
- **Date**: Remove all non-digit characters, except for the dot (e.g., "h" is removed). Use the post's age to determine the posting date.
- **Location**: Similar to Indeed, mark jobs as remote or hybrid based on the location data. Extract the province, city, and address if available; otherwise, leave the cell NULL.
- **Pay**: Remove all non-digit characters, except for the dot. Convert the pay to annual amounts and store them as a string.

After cleaning, the data from the two job boards is merged. For the purpose of this analysis, I selected the following columns for the combined table: location, job title, company name, address, city, province, remote, hybrid, pay, job description, benefits, and job type. Duplicate rows with the same company name, job title, and date were removed, keeping only the first entry. Rows with empty job descriptions were also dropped.

Details of the cleaning process can be found in the `clean_jd.ipynb` file.

Below is a sample of the cleaned data prepared for further analysis:

![Untitled](https://github.com/alextr1602/data-science-job-market/assets/134574511/e1e98049-0f50-4e82-b46f-aa6bc7184946)
<p align="center"><em> Job Postings Sample </em></p>

### Exploratory Data Analysis

Exploratory Data Analysis (EDA) is essential to gain a general understanding of the dataset. I performed univariate analysis on the data and uncovered some interesting insights.

Grouping the data by location, the results were as expected. Toronto is the most popular city for data science jobs, followed by Vancouver and Montreal—three major cities in three of Canada’s largest provinces. Interestingly, out of the 64 cities in the dataset, approximately 170 jobs (around 27% of the total) are from Toronto, which is a surprisingly large percentage for a single city.
<p align="center">
  <img src="https://github.com/alextr1602/data-science-job-market/assets/134574511/748d55fa-5bcb-42b0-9595-283dde1bd032"/>
</p>
<p align="center"><em> Most Popular Cities </em></p>

When it comes to the most popular job titles, it’s no surprise that **Data Scientist** is at the top. However, it’s unexpected to see a role like **Statistician** as one of the most frequently listed positions, given the relatively small size of the dataset.
<p align="center">
  <img src="https://github.com/alextr1602/data-science-job-market/assets/134574511/d9bc957a-c4d4-4c82-8428-e52e9a3b0b9b"/>
</p>
<p align="center"><em> Most Popular Job Titles </em></p>

The job descriptions contain a wealth of data, so the first step was to clean the descriptions to remove noise. I then performed word contraction, tokenized the descriptions, and added Part of Speech (POS) tags to each word. EDA was conducted on the POS tags and job descriptions. Below are some of the charts from the analysis. The complete EDA process is documented in the `EDA.ipynb` file.

<p align="center">
  <img src="https://github.com/AlexDatTr/data-science-job-market/assets/134574511/82bd9917-c8c9-4b1e-928b-65d5244ca145"/>
</p>

### Building a Pipeline to Extract Skills from Job Descriptions 

Building a Pipeline to Extract Skills from Job Descriptions Using LLaMA
The exploratory data analysis (EDA) revealed that job descriptions contain valuable data, particularly regarding the skills required for data science roles. To efficiently extract these skills, I leveraged the LLaMA model with a few-shot learning approach.

Using LLaMA, I created a pipeline where the model learns from a small set of annotated examples (few-shot examples) and applies this understanding to new job descriptions. This approach takes advantage of the model's ability to generalize from minimal input.

Here’s how the pipeline was constructed:

Prepare Few-Shot Examples: I manually annotated a few job descriptions by labeling the specific skills required. These examples were used to guide the model in recognizing similar patterns in other job descriptions.

Create an Input Prompt: I formatted the few-shot examples into a prompt that included both the annotated job descriptions and the job description that needed skill extraction. This prompt was structured to teach the LLaMA model the patterns to look for when identifying skills.

Tokenize the Input: The input prompt, including the few-shot examples and the target job description, was tokenized using the LLaMA tokenizer.

Generate Predictions: The tokenized input was fed into the LLaMA model to generate a continuation of the prompt, where the model predicted the relevant skills for the job description based on the patterns it learned from the examples.

Post-process the Output: The generated output was cleaned and validated to remove irrelevant or duplicate entries, ensuring that the list of extracted skills was accurate and usable.

This approach allowed for the flexible and accurate extraction of skills from job descriptions with minimal manual effort, utilizing the powerful few-shot learning capabilities of the LLaMA model. The result was a scalable method to identify required skills across various job postings, ready for further analysis or model training.
