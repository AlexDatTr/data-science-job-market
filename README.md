# Data Science Job Market

Getting a job data science job has been my goal and that I've been trying to achive since early 2023, and it's time consuming to read through all the job description to know the skills you need for the job. Over the pass few months, I have been actively searching and applying for data science jobs, and I found out the the skills companies are hiring for data science roles are quite large, depending on the company tech stack and data pipeline. So the initial goal of the project is to extract the skills given job description and perform an analysis on the skill extracted.

## Table of content

## Instruction

## Installation
All the package 
[How to install packages using pip]([https://www.google.com](https://packaging.python.org/en/latest/tutorials/installing-packages/)https://packaging.python.org/en/latest/tutorials/installing-packages/)  
```console
pip install requirements.txt
```


### Data

Most of data science jobs in Canada are posted on two major job board: Glassdoor and Indeed, so the data for this project will be collected from those 2 website. For this project, I collected over 600 job posting over Canada in November and early December 2023.  

Since Indeed and Glassdoor don't have API for developer to request the job listing on the site, I decide to collect the the data using web scraping.

![Recording 2023-12-08 at 13 17 57](https://github.com/alextr1602/data-science-job-market/assets/134574511/936f23c7-e1a4-444b-bae1-c08f2081c159)
 
Having a look at the website interface, we can see that Indeed and Glassdoor are dynamic websites, we need to click to job posting for the data to be viewed. I also tried to request HLMT source code from the website but got blocked, so it doesn't look like Beautifulsoup is not gonna be effective for these sites. Therefore, I will use Selenium Web Driver to collect data from these webs. The scraper are in file `indeed_scrape.ipynb` and `glassdoor_scrape.ipynb`  

## Code Structure 

### Data Collection

The data is collect using Selenium to collect data from these webs. The scraper are in file `indeed_scrape.ipynb` and `glassdoor_scrape.ipynb`.  

When you run the `indeed_scrape.ipynb` file, it will open a web driver connect to the indeed webpage. It find all the job listing objects and iterate through them. Then it clicks each of those job listing, wait for the page to load and copy the Job Title, Company Name, Location, Benefits, Job Type, and Job Description to a dictionary, which is eventualy add to an output list. For this project, the scraper will only run for the first 25 page of Indeed.

Fairly silimar approach is used with Glassdoor, except instead of looping through the pages, the scraper will click on the **`Show More`** button for 15 times, and for each time it will load 30 job listings, so we expect around 450 job posting from Glassdoor. And we only collect Job Title, Company Name, Location, Job Description. The benefit doesn't have its own object in Glassdoor, so we might later extract the benefits from job description.

The data extracted from these scraper are save in /data folder named `indeed_job.csv` and `glassdoor_job.csv`  

Here is a sample of the scraped data from 2 job boards

![Glassdoor scrape sample](https://github.com/alextr1602/data-science-job-market/assets/134574511/f3100043-f24b-44c7-804d-095c625dc4ee)
<p align="center" ><em> Glassdoor Scrape Sample </p></em>

![Indeed scrape sample](https://github.com/alextr1602/data-science-job-market/assets/134574511/c03bd64c-ee70-4e6c-acb2-92a7f46217e6)  
<p align="center"><em> Indeed Scrape Sample </p></em>
  
### Data Cleaning

Even though data clean is performed at different stage of the project, I will still perform some cleaning here to prepare the data for later use. The approach here is to clean data from each job board, transform data to same format and join two table.  

For data from Indeed. Perform cleaning for by columns as follow:
- Date: Remove all non digit character, except the dot(e.g "Poted/nPost" will be removed). From the age of the post get the date of the post.
- Location: If the location marks remote and hybrid in it, mark the job as remote and hybrid. Extract the province, city, address if available, otherwise leave the cell NULL
- Pay: Remove all non digit character, except the dot. Get the starting pay and maximum pay, transform the pay to anual pay and join them to a string to store data

For the data from Glassdoor. Perform cleaning for by columns as follow:
- Date: Remove all non digit character, except the dot(e.g "h" will be removed). From the age of the post get the date of the post.
- Location: If the location marks remote and hybrid in it, mark the job as remote and hybrid. Extract the province, city, address if available, otherwise leave the cell NULL
- Pay: Remove all non digit character, except the dot. Transform the pay to anual pay to join them to a string to store data.  

Join data from 2 job boards. For the purpose of this analysis, I only select the following column for the join table: location, job title, company name, address, city, province, remote, hybrid, pay, job description, benefits, job type. Then duplicate row with same company name, job title, date, will be removed, only keep the first value. Drop row with empty job description.

Detail of the cleaning process if in file `clean_jd.ipynb`

The sample of clean table for further processing.

![Untitled](https://github.com/alextr1602/data-science-job-market/assets/134574511/e1e98049-0f50-4e82-b46f-aa6bc7184946)
<p align="center"><em> Job Postings Sample </p></em>

### Exploratory Data Analysis

Exploratory Data Analysis(EDA) is necessary to get a general idea of the data set. I perform a univariate analysis on the data and found some intersting information. 

I group the data by location and the result look as expected. The most popular city for data science job is Toronto, follows by Vancouver and Montreal, three big cities in three of the biggest provinces of Canada. However, there are 64 cities in the dataset, and approximately 170 jobs, around 27% of total number of jobs, is from Toronto, which is a suprisingly large number for just one city. 
<p align="center">
  <img src="https://github.com/alextr1602/data-science-job-market/assets/134574511/748d55fa-5bcb-42b0-9595-283dde1bd032"/>
</p>
<p align="center"><em> Most Popular Cities </p></em>


When it come to most popular job title, its is unsurprisingly that **Data Scientist** is the most popular job title. However, even statistics play a significant role in data science, it is still unexpected to see role like **Statistician** as one of the most hired data science roles regarding the size of the dataset is quite small. 
<p align="center">
  <img src="https://github.com/alextr1602/data-science-job-market/assets/134574511/d9bc957a-c4d4-4c82-8428-e52e9a3b0b9b"/>
</p>
<p align="center"><em> Most Popular Job Titles </p></em>

The job description contain a lot of data in there, so initialli, I cleaned noises from the description. Then perform word contraction, tokenize the description and add Part of Speech tag to each word of the job description. Then I perform EDA on the POS tag and the job description. Here are some of the chart from the analysis. Complete EDA process can be accessed at file `EDA.ipynb`

<p align="center">
  <img src="https://github.com/AlexDatTr/data-science-job-market/assets/134574511/82bd9917-c8c9-4b1e-928b-65d5244ca145"/>
</p>

### Build Pipeline to Extract Skills from Job Description 





