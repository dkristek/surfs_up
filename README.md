# surfs_up
## Overview
The objective of this analysis is to calculate the summary statistics of observed temperatures in Oahu in the months of June and December. The data will be used to determine whether a store that specializes in ice cream and surfing gear is a sustainable business year-round. 

## Analysis
### June Temperatures
![June Summary Statistics](https://github.com/dkristek/surfs_up/blob/main/resources/june_temps.png)
### December Temperatures
![December Summary Statistics](https://github.com/dkristek/surfs_up/blob/main/resources/dec_temps.png)
#### Takeaways
* June and December temperatures are both relatively constant across the years, with a standard deviation of 3.26 degrees and 3.75 degrees, respectively
* December and June are fairly similar in mean temperatures at 71.0 degrees Fahrenheit and 74.9 degrees Fahrenheit respectively
* The minimum temperature in December is colder than in June at 56 degrees Fahrenheit, while June has a higher maximum temperature at 64 degrees Fahrenheit

### Summary
From these two statistical summaries we can make a preliminary conclusion that weather is rather consistent and stable between winter and summer months. However, to make a stonger conclusion we will need more queries to gather further evidence. The first additional query used a for loop to gather the summary statistics for temperatures in all twelve months. The code can be seen below.
<details>
  <summary>Query 1</summary>
  
    months = ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October',
                                   'November', 'December']
    #creating dataframe before loop
    results_df =pd.DataFrame()

    #loop through for all 12 months
    for num in range(0,12):
    
      #query for temps in each month
      month_temp = pd.DataFrame(list(session.query(Measurement.tobs).\
        filter(extract('month',Measurement.date)==num+1).all()))
    
      #adding data to df
      results_df[months[num]] = month_temp
    
     results_df.describe()
</details>

The second query tabulated the frequency at which each unique temperatured was observed. The code for the query along with a graph of the frequencies can be found below.
<details>
  <summary>Query 2</summary>
  
  ```
  
  #query to get temp frequency
  results_df = pd.DataFrame(session.query(Measurement.tobs, func.count(Measurement.tobs)).group_by(Measurement.tobs).\
  order_by(func.count(Measurement.tobs).desc()).all(), columns=['Temperature', 'Frequency'])

  #plotting
  plt.scatter(x=results_df['Temperature'], y=results_df['Frequency'])
  plt.xlabel('Temperatures (F)')
  plt.ylabel('Frequency')
  plt.title('Temperature Frequencies
  
  ```
  ![Frequency Graph](https://github.com/dkristek/surfs_up/blob/main/resources/freq_plot.png)
  
</details>

In conclusion these two queries will provide more information regarding the year-round climate of Oahu.


