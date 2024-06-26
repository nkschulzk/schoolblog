---
layout: post
title:  "Scraping Your Way To Strategic Skiing"
author: Nels Schulzke
description: "How can you use datascraping to make million dollar decisions and hedge against external factors that jeopardize revenue? Learn how below"
image: "/assets/images/ski.avif"
---
# Ethical Data Collection and Scraping: Ensuring Quality Weather Data for Skiing Conditions Analysis

Welcome, I am going to guide you through the process of creating a dataset for analyzing skiing conditions in resorts internationally, with a focus on climate stability and snow days annually. In this post, I'll detail how I approached data collection, ensuring ethical considerations and good scraping practices using Python and data from [Wunderground](https://www.wunderground.com) for weather data and [SkiResort.info](https://www.skiresort.info) for elevation data.

## Introduction

Imagine you're a strategic advisor to Veil Resorts, a global leader in skiing and resort management. Your task is to recommend global expansion strategies, taking into account climate stability and snow days annually to mitigate climate risks. This scenario inspired my journey to analyze weather data for resorts worldwide, crucial for strategic decision-making in the skiing industry. Before diving into exploratory data analysis (EDA), I needed a reliable dataset. Here's how I achieved it:

## Determining Ethical and Allowable Data Collection

Respecting the terms of service and ensuring ethical data collection were paramount, especially for a company like Veil Resorts, known for its commitment to sustainability and ethical practices. I opted to scrape weather data from [Wunderground](https://www.wunderground.com), a reputable source trusted by millions for accurate weather information. To ensure compliance, I meticulously reviewed Wunderground's terms of service and robots.txt file, ensuring that our data collection process aligned with their policies and guidelines. By adhering to these principles, I guaranteed that our data collection process was both ethical and permissible. There was an implemented delay, which I abided by in my code, but I additionally looped user agents to try to decrease the ware on the site from a given user agent.

## Overall Strategy
The overall strategy of getting the assembled data involves the following:
1. Scrape the data from Wunderground.com
2. Merge on the elevation data from Skiresort.info for the ski resort elevation
3. Create adjustments in snow days and temperatures based on change in elevation between the ski resort and the weather station

## Nitty-Gritty
To scrape through the weather data on Wunderground, the entirety of the scraping is based on this URL format: 

https://www.wunderground.com/dashboard/pws/{station_code}/table/{start-date}/{end-date}/monthly

The general strategy is to iterate through the station code closest to the location you care about, then iterate through the combinations of start date and end date.

I had ChatGPT draft a skeleton code from what I wrote, to help you get started. The full code is in my Github Repo. 

Here is the starter code below:

```python
import requests
from bs4 import BeautifulSoup
from datetime import datetime, timedelta
import pandas as pd
import random
import time

# Define the list of user agents if you need to pivot through them
USER_AGENTS = []

# Function to scrape weather data for a given date and station code
def scrape_weather_data_for_date(url):
    # Implement your scraping function here
    pass

# Function to generate date range by month in reverse order
def generate_monthly_date_range(start_date, end_date):
    # Implement your date range generator function here
    pass

# Define start and end dates
start_date = datetime(2016, 1, 1)
end_date = datetime(2023, 12, 1)

# Define base URL
base_url = 'https://www.wunderground.com/dashboard/pws/'

# List of station codes from the weather stations on Wunderground you are interested in
station_codes = []

# Initialize empty lists to store data
data = []

# Loop through each station code
for station_code in station_codes:
    # Loop through each month in the date range in reverse order
    for date in generate_monthly_date_range(start_date, end_date):
        # Construct URL for the first day of the current month and station code
        url = f'{base_url}{station_code}/table/{date.strftime("%Y-%m-%d")}/{date.strftime("%Y-%m-%d")}/monthly'
        
        # Scrape weather data for the current date and station code
        station_name, elevation, weather_data = scrape_weather_data_for_date(url)
        
        # Extract relevant data (these are just examples of important metrics below)
        for entry in weather_data:
            data.append({
                'Station': station_code,
                'Location': entry['Data'][0],
                'Elevation': entry['Data'][1],
                'Date': entry['Date'],
                'Temp Max': entry['Data'][3],
                'Temp Avg': entry['Data'][4],
                'Temp Min': entry['Data'][5],
                'Precip Total': entry['Data'][6]
            })

# Create a DataFrame from the data
df = pd.DataFrame(data)

# Write DataFrame to Excel file
output_file = 'weather_data.xlsx'
df.to_excel(output_file, index=False)
```
## Conclusion
In this blog post, I've outlined my approach to ethical data collection and scraping practices using Python and data from Wunderground. By respecting terms of service, implementing robust scraping code, and adhering to best practices, I successfully gathered weather data for analyzing skiing conditions in resorts worldwide, crucial for strategic decision-making at Veil Resorts.

Stay tuned for my next blog post, where I'll delve into the exciting world of exploratory data analysis (EDA) using the dataset we've created. In the meantime, please comment below. I would love to see how you could implement a Selenium approach to solving this scraping problem as well!

GitHub Repository: [here](https://github.com/nkschulzk/webscraping-project)

Thank you for joining me on this skiing adventure, and happy coding!