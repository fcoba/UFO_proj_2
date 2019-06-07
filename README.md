# UFO REAL
## Travel Agency

## Data science Module 3: Project

Ever wonder where to go for UFO sigthsing? Look no more! in this document we'll analyze the data from +100 thousand of reports collected over the past 50 years and tell you where you should go, and what you can expect to see.

### Data
For this project we collected the data from the [National UFO Reporting Center](http://www.nuforc.org/index.html)

For each report the data contains the folling information

| Column        | Descritption |
| ------------- |:------------:| 
| Date/ Time    | Date and time of the events |
| City          | City where of the sighting |
| State         | State or province where of the sighting |
| Shape         | Shape of the object observed |
| Duration      | Duration of the event |
| Summary       | Descrption of the event |
| Posted        | Date when the event was posted to the database |

In order to compare the nuber of UFO sightings between the different states, we also collected the population from the [United States Census Bureau](https://www.census.gov/). 

The census data we used contains the total population for each of the 50 US states per year, from 2010 to 2018. 
To use the census data we groupd out UFO data by state and by year and counted how many sightings happened between 2010 and 2018. 

#### Collecting an cleaning the data.
For the ufo data, we scrapped the data from the website. For this analysis we scrapped the pages summarizing the sightings per month [like this one](http://www.nuforc.org/webreports/ndxe201905.html) instead of scrapping each individual report, as shown [here](http://www.nuforc.org/webreports/146/S146041.html). The downside of scrapping the summary table,  is that we do not have the full description of the event, so will not use this informatin for this analysis.

The data needs some cleaning before we can start analyzing it. In the input forms, the field `Duration` is collected as a string, resulting in a variety of formats. The most common are the duration time as seconds, minutes or hours (for example `5 minutes` or `2:30` or `1 hour & 30 minutes`), a time interval (like `10-15 minutes`) or give a limit (`>1 minute`). In addition to dealing with the different formats, we had to take into account the most common typos, such as writing `2O` instead of `20`.

After converting the duration time into seconds and removing the rows with a null value, or where we could not find a number, 
50% of our time in this project was dedicated on cleaning, we eneded with ~87,700 reports out of the original ~110,000.

The census data we selected showed the total population by state and by year from 2010 to 2018. There was no need to clean this data.

### Preliminary EDA

After looking at the data, ur first question was, where should we go to see some ufos? To answer this, we counted the total number of sightings per state, finding that California has the highest number
![UFO_sightsBYState](Figures/ufos_by_state.png)

This way of looking at the data doesn't give us a lot of information when compared to other states because the population of California is much larger than smaller states, such as Rhode Island. To do a true comparison we loaded up the census data to get the number of sightings per capita. This analysis is reported in the next section. 

From the `Shapes` column we found there are +40 shapes reported, the top three being 

| Top Shapes (out of 43)       |  % |
| ------------- |:------------:| 
| Light   | 21.2 |
| Circle          | 10.6 |
| Triangle      | 9.9 |



### Data Analysis
To answer, which state has the higher activity per capita, we imported the data from both databases into a single SQL database, using `sqlite3`. We used the different SQL commands to group and sum the information we needed, 
for example to count all the sighting by state and by year we did

```sql
SELECT state , 
 sum(CAST ("y2010" AS int)) as N2010, 
 sum(CAST ("y2011" AS int)) as N2011, 
 FROM 
 	(SELECT state , 
 	 COUNT( CASE WHEN strftime('%Y', event_date) = '2010' THEN shape END) AS 'y2010', 
 	 COUNT( CASE WHEN strftime('%Y', event_date) = '2011' THEN shape END) AS 'y2011', 
 	 strftime('%Y', event_date) as year FROM ufos 
 	 GROUP BY strftime('%Y',event_date), state)
 GROUP BY state
 ```
 The result was then loaded into a pandas DataFrame, and then joined to a second dataframe containing tthe population number.  
 To calculate the number of sights per capita, we simplly divide the number of sights for a particular state and year by the population for that same state and year. 
 
 To compare if any state had a an unusual number of sights, we compare the sights per capita to the average number of sights for the whole country, and then perform a $\chi^2$ test defined as 
