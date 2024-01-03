# InstagramPublications
A dataset that includes more than 8.5 million records with meta-information of publications over 13 months (January 2019 to February 2020) was given for this project.
Each publication is described by the following meta-information:
- lon, lat – geoposition coordinates rounded up to a 250x250 meter polygon (geographical longitude and latitude, respectively);
- timestamp – timestamp of the publication accurate to one hour;
- likescount – number of "likes" in the publication;
- commentscount – number of comments of the publication
- symbols_cnt – number of all symbols in the publication
- words_cnt – number of words (meaningful, not counting special characters and other meta-information)
- hashtags_cnt – number of hashtags 
- mentions_cnt – the number of mentions of other users
- links_cnt – number of links
- emoji_cnt – number of emoji.
- point – service field for matching coordinates from training, validation and test datasets (if two elements have the same point, they have the same coordinates, comparison of lat and lon may give an error)

**The goal was** to predict the number of publications in each 250x250 meter polygon for each hour 4 weeks (28 days) ahead of the last publication in the training set.

![image](https://github.com/alexxxroz/InstagramPublications/assets/107231688/2248258e-133d-4dfb-9cf2-2d507b07a0b0)

## 1st stage
The training data were aggregated by coordinates over time, thus, it was possible to see the density of publication for different regions, resulting in a new dataset with 6804 rows and 3 features (latitude, longitude and number of publications). This dataset was visualized on a map, using folium libarary. It turned out the all the data given for the territory of Saint Petersubrg city.

![image](https://github.com/alexxxroz/InstagramPublications/assets/107231688/1b5dedf1-556a-43bf-acb2-a72ad074af38)


But to train a model, it was necessary to reaggregate the data by hour to know how many posts were published in each cube hourly. As these cubes belong to the city, it was interesting to look into the districts as they may be a good predictors of users' activity as well. Additionaly, several other features were added from splitting the date of each post. The final dataset had the following components:
**Features**:
- district;
- hour;
- day;
- month;
- year;
- lat;
- lon.
  
**Target**:
- pph (posts per hour).
  
![image](https://github.com/alexxxroz/InstagramPublications/assets/107231688/ed8b34f8-a4ba-4139-ac22-822e575574dc)

## 2d stage
To forecast the spatial time series it was decided to use XGBoost regression model, as it can account for both - spatial and temporal data components. Using grid search a bunch of the best hypoparameters was acquired and then analyzed on the validation dataset. As it can be seen from the plot below, the coefficient of determination indicates a good fit between real and forecasted data, as well as the MAPE value. Generally, model had a worse perfomance for the regions lacking of publications or having a few of them in training data.

![image](https://github.com/alexxxroz/InstagramPublications/assets/107231688/34a1274d-ae93-42c0-8e80-a4a060afaa2d)

As an example a forecast of publications for the whole city was pictured down below:

![image](https://github.com/alexxxroz/InstagramPublications/assets/107231688/4ba6647a-335d-4fae-a112-15b7613a82a9)

