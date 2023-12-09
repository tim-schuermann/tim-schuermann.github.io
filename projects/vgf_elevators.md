## Predicting elevator outages at subway stations in Frankfurt, Germany

<!-- <img src="../images/postgresnav_db.jpg?raw=true"/> -->

**Project description:** As parents in the early years of our child's life, it has become increasingly apparent to my wife and me how important accessibility in public transport can be. An elevator outage at a subway station can quickly derail the trip to child daycare as a fully loaded buggy needs to be carried down some stairs. To at least come prepared, I started webscraping the accessibility information page of Frankfurt public transport provider VGF ([Link](https://www.vgf-ffm.de/de/fahrgastinfo/barrierefreies-reisen/status-aufzuege/)) in the attempt of training a machine learning model to predict an elevator's outage status at a given point in time - for example our drop-off time at daycare. The website is scraped using Beautiful Soup and Selenium in Python. On a 30-minute interval during operating hours of the subway, a Github Action triggers the scraping and subsequent insertion into a cloud-hosted PostgreSQL database. After a sufficient amount of data was collected, I trained a Random Forest classifier. The model is made accessible via a Flask Web App with user authentication to make it available to us ahead of the commute to the daycare facility - indicating whether driving by car would be a better idea that morning. The following sections highlight some noteworthy aspects of the project.

### Building a useful dataset
The scraped dataset described above comes with an obvious caveat: scraping the stations currently undergoing an elevator outage leaves only those stations in the dataset that actually experience outages. To compensate for this, I used the OpenStreetMap API to retrieve all names of subway stations in Frankfurt. For any given timestamp, I can now add the remaining stations as having no elevator outage. Of course, this creates a fairly skewed dataset, with approximately 80% of entries at a given timestamp without an outage. Splitting the data into train- and test-splits needs to account for this class imbalance. Additionally, the train-test-split needs to respect the temporal order of the data - after all, we are interested in predicting future datapoints, not arbitrary ones from within the timeframe of the data collection process.

### Feature Engineering and Data Drift
To properly train a classifier on the data, datapoints representing both functional and non-functional elevators require the same set of features. This precludes additional information scraped from the public transport provider from being used, as datapoints of functional elevators would, for example, not have an outage description attached to them. I therefore rely on the associated timestamp of each datapoint to generate the time since the last elevator outage at a given station. Taken together with the station ID, the time since the last outage forms the basis for the outage prediction. Moving forward, the project requires monitoring for data drift. After all, it may be the case that for a given subway station, wednesdays at 9AM constitute regularly scheduled elevator cleanings which cause the outages during the initial training period. In later months, this schedule may change and elevators are functioning on wednesdays at 9AM, causing misclassifications. Monitoring the distributions of features as well as the target variable will identify the need for retraining on more recent data.

### Initial performance
The Random Forest classifier trained on the station name and the time since last outage performs well on both the train and test set. It achieves a test set accuracy of 0.99 based on the following confusion matrix:
|        | Predicted working | Predicted not working |
| ---    | ---   | --- |
| Working | 16178 | 0 |
| Not working | 6 | 2389 |

The data used for training and testing currently encompasses the third quarter of 2023. Regularly scheduled model updates on a quarterly basis are planned, with the option for intermediate retraining based on data drift indicators.

### Related repositories
For more details on the project, see [Elevator Accessibility Project]().
