## Predicting elevator outages at subway stations in Frankfurt, Germany

<!-- <img src="../images/postgresnav_db.jpg?raw=true"/> -->

**Project description:** As parents in the early years of our child's life, it has become increasingly apparent to my wife and me how important accessibility in public transport can be. An elevator outage at a subway station can quickly derail the trip to child daycare as a fully loaded buggy needs to be carried down some stairs. To at least come prepared, I started webscraping the accessibility information page of Frankfurt public transport provider VGF ([Link](https://www.vgf-ffm.de/de/fahrgastinfo/barrierefreies-reisen/status-aufzuege/)) in the attempt of training a machine learning model to predict an elevator's outage status at a given point in time - for example our drop-off time at daycare. The website is scraped using Beautiful Soup and Selenium in Python. On a 30-minute interval during operating hours of the subway, a Github Action triggers the scraping and subsequent insertion into a cloud-hosted PostgreSQL database. After a sufficient amount of data has been collected, I intend to train a machine learning model with it, initially starting off with a Random Forest classifier. The model will be made accessible via a Flask Web App with user authentication to make it available to us ahead of the commute to the daycare facility - indicating whether driving by car would be a better idea that morning. The following sections highlight some noteworthy aspects of the project.

### Building a useful dataset
The scraped dataset described above comes with an obvious caveat: scraping the stations currently undergoing an elevator outage leaves only those stations in the dataset that actually experience outages. 

### Feature Engineering and Data Drift
To properly train a classifier on the data, datapoints representing both functional and non-functional elevators require the same set of features. This precludes additional information scraped from the public transport provider from being used, as datapoints of functional elevators would, for example, not have an outage description attached to them. 

### Related repositories
For more details on the project, see [Elevator Accessibility Project]().
