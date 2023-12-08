# Data Science Job Market

Getting a job data science job has been my goal and that I've been trying to achive since early 2023, and it's time consuming to read through all the job description to know the skills you need for the job. Over the pass few months, I have been actively searching and applying for data science jobs, and I found out the the skills companies are hiring for data science roles are quite large, depending on the company tech stack and data pipeline. So the initial goal of the project is to extract the skills given job description and perform an analysis on the skill extracted.

## Table of content

## Installation
[How to install packages using pip]([https://www.google.com](https://packaging.python.org/en/latest/tutorials/installing-packages/)https://packaging.python.org/en/latest/tutorials/installing-packages/)  
```pip install requirements.txt```


## Data Source

Most of data science jobs in Canada are posted on two major job board: Glassdoor and Indeed. Given that, the data for this project is collected from Indeed and Glassdoor by performing web scraping. As there are a lot of Javascript object in the websites and Indeed blocked HLMT request from random user, I Python with Selenium to scrape over 200 job postings in November 2023.   

The web scraper is in file `indeed_scrape.ipynb` and `glassdoor_scrape.ipynb`
