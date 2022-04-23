# surfs_up

## Overview

Data on the Hawaiian island of Oahu's weather during the hot and cold season is needed to determine the viability of an outdoor business. In order to determine the key differences in Oahu's weather during the months of June and December, SQLAlchemy is used to query a SQLite database. 

## Results

![June Temperatures](Images/June_Temps.png) <br />
![December Temperatures](Images/December_Temps.png)

- While December has a lower average temperature than June, the difference is not significant enough to hinder potential business ventures on the island (74.9 vs 71)
- Although Oahu's average temperature during June and December does not greatly differ, their ranges do. While June has a temperature range of only 21 degrees (max=81, min=64), December has a range of 27 degrees (max=83, min=56). Specifically, December's minimum temperature indicates that an outdoor business could _potentially_ run into issues.
- Both June and December have standard deviations of less than 4 degrees. Given their mean temperatures, it can be determined that exceedingly low temperatures are quite rare and are outliers within the data. Therefore, while it is possible for Oahu's temperature to dip into the 50's during December, it should not be considered a common occurrence and should not deter potential outdoor business owners from establishing themselves on the island. 

In order to get a better idea of how temperatures vary between the two months on a more precise level, one could also query temperature by station for both months, and then convert them into two separate dataframes:
```
jstation = session.query(Measurement.tobs, Measurement.station).filter(extract('month', Measurement.date) == 6).all()
jstation_df = pd.DataFrame(jstation, columns=['June Temps', 'Station'])
dstation = session.query(Measurement.tobs, Measurement.station).filter(extract('month', Measurement.date) == 12).all()
dstation_df = pd.DataFrame(dstation, columns=['December Temps', 'Station'])
```
Furthermore, the two dataframes can then be merged into a singular dataframe that shows how each station's temperature varies between June and December: 
```
temps_by_station = jstation_df.merge(dstation_df, on='Station', how='inner').set_index('Station')
```

## Conclusion 

The business owner was concerned that Oahu's weather patterns could hinder the viability of an outdoor business year-round. However, the temperature data indicates that even in the occurrence that it drops into the 50's, it would be a rare enough occurrence that it would not have a significant impact on the business' profits. Therefore, Oahu is a prime location for an outdoor business that requires consistent good weather to operate at full capacity.
