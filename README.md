# city_salaries_project

Link to ShinyApp: https://kpclifton.shinyapps.io/Baltimore_City_Salaries/

## DESCRIPTION

This app predicts salaries for employees of Baltimore City based on records from 2012 to 2019 of salaries for select job titles and agencies. 

The current iteration of this app includes employees of the Police Department with the following job titles: 
- ACCOUNTANT II
- ANALYST/PROGRAMMER II
- COMMUNITY SERVICE OFFICER
- COMPUTER OPERATOR IV
- CRIME RECORD TECHNICIAN
- OFFICE SUPERVISOR
- POLICE INFORMATION TECHNICIAN
- POLICE LIEUTENANT
- POLICE OFFICER
- POLICE REPORT REVIEWER
- POLICE SERGEANT
- SECRETARY III

To make a prediction, a model is fit to the data using Holt-Winters exponential smoothing. For each job, salaries  are forecasted for four fiscal years 2020 - 2023. One the provided graph, the black line plots the previous trends for salaries based on the selected agency and job title. The forecast trend is displayed a blue line with the 80% and 95% prediction intervals as the dark and light purple shaded regions, respectively.

## BACKGROUND
The source data includes many other agencies such as the Fire Department, Library, Health Department, and Public Works. Furthermore, these datasets available on data.gov have information on over 15,000 employees for each fiscal year from 2012 to 2019. The columns of the original data include employee name, job title, agency ID and description, hiring date, annual salary, and gross pay. 

Initially, I was interested in predicting what percentage of their salaries would employees earn as gross pay based on knowledge of job title, agency, time since hiring and yearly salary trends. With this aim in mind, I added calculated employed time in days and years using end of the fiscal year (June 30) minus hiring date. Also, I added a column for percent paid = gross pay/annual salary x 100. The modified csv files are in this public github repo **kpclifton/city_salaries_project** and are called in the code by the public url.

Next I began to clean up the data with the intention of removing entries that had formatting that was different from the majority. I filterd out jobs that do not have annual salaries which included election judges (indicated by agencyIDs that begin with D,R, or U). Also, I removed the youth workers (indicated by agencyIDs that begin with D,R, or U) whom were not reported in all fiscal years. Futhermore, I filtered out rows which were missing hiring dates or did not have any gross pay for the fiscal year. Finally, to simplify I selected only five of the two dozen or more agencies. The agencies I chose were Fire Department, Police Department, Library, Health Department, and Law Department, which are idenified by the first two digits of the agencyID code (64,99,75,65,30).

After organizing the data (which were separated by fiscal year) and joining all data sheets into one dataframe, I wanted to plot the time series data and realized that the data needed to be in the long format, where the years are entries instead of column names. Once I had tidy data and began to test out plots, I realized that in order to have the output be responsive to user input I would need to simplify the problem much more. At that point, I decided to focus on just the police department and summarized the data to have mean annual salary per job title for 2012-2019.

For my first attempt at modeling time series data, I tried using splines but I could not figure out how to extrapolate for future time points. Instead, I found a function that did exponential smoothing and forecasting for time series data. The HoltWinters() function is compatible with ggplot but not plotly. Unfortunately, the graph does not have interactivity with the cursor, but it does respond to user input in choosing which job to forecast.

The drop-down select menus have default options so that when the app is opened the user has an example of how the app works.

## Necessary R Packages
    library(tidyr)
    library(dplyr)
    library(RColorBrewer)
    library(munsell)
    library(ggplot2)
    library(magrittr)
    library(plotly)
    library(scales)
    library(forecast)


 ## Sources:
- https://catalog.data.gov/dataset/baltimore-city-employee-salaries-fy2012-36e83
- https://catalog.data.gov/dataset/baltimore-city-employee-salaries-fy2013-0706d
- https://catalog.data.gov/dataset/baltimore-city-employee-salaries-fy2014-5924b
- https://catalog.data.gov/dataset/baltimore-city-employee-salaries-fy2015
- https://catalog.data.gov/dataset/baltimore-city-employee-salaries-fy2016
- https://catalog.data.gov/dataset/baltimore-city-employee-salaries-fy2017
- https://catalog.data.gov/dataset/baltimore-city-employee-salaries-fy2018
- https://catalog.data.gov/dataset/baltimore-city-employee-salaries-fy2019

