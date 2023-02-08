## Cyclistic Case Study

**Project description:** This case study was completed for the Google Professional Data Analytics Certificate profram. It is focused around a fictional bike sharing company called Cyclistic. The company runs a bike-share program that features over 5,800 bicycles and 600 docking stations. They have both casual riders as well as membership plans and offer various types of bicycles. The goal of the case study is to use Cyclistic’s rider data to analyze how casual riders differ from member riders in order to think of marketing strategies to increase membership sales.

**Technologies Used:** Excel, Power BI

### 1. Questions and Business Task

Business Task: Analyze trip data from the past year to identify differences between casual riders and members in order to market towards causal riders to increase annual memberships.

-	How do annual members and causal riders use Cyclistic Bikes Differently?
-	Why would causal riders buy Cyclistic annual memberships?
-	How can Cyclistic use digital media to influence riders to become members?

### 2. Data Sources

The data sources used for this case study consisted of twelve csv files, all containing rider data for each month for the past year. Each data set represents one month and all contain the same attributes. These consist of:
-	Ride_id: alphanumeric code representing a single ride
-	Rideable_type: type of bike used for the ride
-	Started_at, ended_at:  date and time which the ride began and ended
-	start_station_name, start_station_id: name and alphanumeric id for starting station
-	end_station_name, end_station_id: name and alphanumeric id for ending station
-	(start/end)_lat, (start/end)_lng: starting and ending latitude and longitudes
-	Member_casual: type of rider

Each of these datasets consisted of thousands of rides, but differed in size depending on month as some months contained significantly more rides than others. The data also contained various types of errors and null values throughout which must be accounted for.

### 3. Data Cleaning

  Before starting the process of data cleaning, I looked at the datasets to see how complete they were in order to decide which attributes would be most useful in my analysis. Looking at the data, each ride was mostly complete except for the start/end station name and id. These fields were often null and provided little context for the analysis I would be performing so I decided not to include them in the data I would be working with. Along with these, I also decided to exclude the starting and ending longitude and latitude. These would prove much more useful if I were to be doing a geographic analysis but since I was not, I removed these columns as well. The final column I removed was the ride_id. This was a unique alphanumeric code which would not be useful in the final dataset as the id would be different for each ride.
  All of the data cleaning for this case study was done using excels built in functions. The first step in the data cleaning process was to identify and delete null values. The first column I did this for was the member_casual column. I saw this column as the most important as it identified what to compare and was the basis for this analysis. Because of this, I decided to remove any rows which contained a null value for this column. This was done by filtering and deleting all of the null values. Next, I needed to ensure that the values, “member” and “causal” were the only two for this column so used excels built in filter function to check. Thankfully the data was already complete in this regard. These two steps could be replicated for the rideable type column as it was a column with only three possible values. The next step of the data cleaning phase was to look at the ride lengths. In order to clean these columns, I first did the same steps as before to delete null and error values from each column. After this, I decided to add another column which consisted of the actual length of the ride. Once I created this column, I then looked for value errors within it. There were many occurrences of the starting time being after the ending time which were then deleted as this would not be possible. Along with deleting these columns, I sorted this column by length to see how long the rides could potentially last. When doing this, I found rides that were logged at being ridiculous lengths like 36 hours long. I assumed that this error was likely either a system error where the end was logged incorrectly, or a rider that forgot to end the ride and decided that they were not representative of the population. I decided that any ride over 8 hours long would be considered an error. The steps I took were for every csv file used. 

### 4. Data Preparation

After cleaning the datasets, I loaded them all into power bi to prepare for my analysis. I created a single table that combine the data from the twelve datasets. This would allow me to perform operations on all of the data at once rather than each of the twelve on their own. I then had to create three new columns; one column extracting the day of the week from the starting date and saving it as a number 1-7, one column  extracting just the date from the starting date, and one column converting the ride time from being a decimal value representing the proportion of the day, to the actual total minutes of each ride. The commands used were:

```DAX
Date = DateValue([started_at])
Ride Length minutes = [ride_length] * 60 * 24
```
Next I needed to create the secondary tables which summarized the data to be visualized. The first table I made was the daily ride lengths table, which consisted of the original data table grouped by each day, in order to give daily statistics. This would include the date, member type, average ride length, member count, and counts for each type of bike. The command used was:

```DAX
Daily Ride Lengths = SUMMARIZECOLUMNS('Cyclistic-trip-data-year'[Date], 'Cyclistic-trip-data-year'[member_casual], "Average Ride Length", AVERAGE('Cyclistic-trip-data-year'[Ride Length minutes]), "rider count", COUNT('Cyclistic-trip-data-year'[member_casual]), "Electric Count", COUNTX(FILTER('Cyclistic-trip-data-year', 'Cyclistic-trip-data-year'[rideable_type] = "electric_bike"), 'Cyclistic-trip-data-year'[rideable_type]), "Classic Count",  COUNTX(FILTER('Cyclistic-trip-data-year', 'Cyclistic-trip-data-year'[rideable_type] = "classic_bike"), 'Cyclistic-trip-data-year'[rideable_type]), "Docked Count", COUNTX(FILTER('Cyclistic-trip-data-year','Cyclistic-trip-data-year'[rideable_type] = "docked_bike"), 'Cyclistic-trip-data-year'[rideable_type]))
```
The next table I created was the riding statistics for each day of the week. The command used was:

```DAX
Weekly Distributions = SUMMARIZECOLUMNS('Cyclistic-trip-data-year'[day_of_week] ,'Cyclistic-trip-data-year'[member_casual], "Number of Riders", COUNT('Cyclistic-trip-data-year'[member_casual])/52, "ride length", AVERAGE('Cyclistic-trip-data-year'[ride_length]))
```

### 5. Visualizations and Analysis

After creating these tables, I was ready to visualize the data and perform my analysis. The first thing I wanted to visualize was the overall distribution of rider types throughout the year. I chose to look at this because it gives insight into the overall proportion of members vs casual riders, as well as how the amount changes throughout the year. I created 4 visualizations using the rider counts data. These included a pie chart showing the overall proportion of each type, a scatter plot showing the amount of each type daily throughout the year, a bar chart showing the monthly rider counts, and a stacked bar chart showing the weekly averages.





For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).
