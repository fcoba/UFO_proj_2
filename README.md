# UFO REAL
## Travel Agency

## Data science Module 3: Project

Ever wonder where to go for UFO sigthsing? Look no more!, in this document we'll analyze the data from +100 thousand of reports collected over the past 50 years and tell you where you should go, and what you can expect to see.

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
For the ufo data, we scrapped the data from the website and used the


