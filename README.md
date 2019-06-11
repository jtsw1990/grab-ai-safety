# grab-ai-safety
Modelling exercise for Grab AI Challenge: Safety dataset

### Instructions
- TBA

### Project Plan
- Understand the data and telematics sufficiently enough to proceed with the exercise
- Decide on a most reasonable format to transform and extract features from the data before modelling
- Choose a suitable algorithm (or ensemble) that gives reasonable test results
- Grid search to optimize
- Decide on a technique for deployment: Interactive web-app, Jupyter results, ect.

### Understanding the data
- bookingID: Numeric key to differentiate bookings. One to many relationship in terms of features to labels
- Accuracy: How accurate the gps location is with reference to what?
- Bearing: Your GPS bearing is the compass direction from your current position to your intended destination. In other words, it describes the direction of a destination or object. If you're facing due north, and you want to move toward a building directly to your right, the bearing would be east or 90 degrees
- acceleration_x: How fast speed changes in the x direction
- acceleration_y: How fast speed changes in the y direction
- acceleration_z: How fast speed changes in the z direction
- gyro_x: How fast angular position changes in x
- gyro_y: How fast angular position changes in y
- gyro_z: How fast angular position changes in z
- second: Each booking starts at 0 and ends whenever the last record is taken.
- Speed: Speed of the vehicle at the time of measure


### Initial observations
- On a high-level view, it seems to be a classification problem with binary labels safe and unsafe
- Looks like most of the problem is going to be focused on feature engineering/ extraction as the data does not have a 1 to 1 relationship between the features and response
- Also with so many data points, deep learning may be required to cut through all that noise (maybe)


### Exploratory Data Analysis
- A high-level view shows that not all the features are useful in prediction the safety category
- Also, there are 18 duplicated labels. WHether or not they are supposed to be dangerous or safe is unknown and will be taken out of the dataset for modelling purposes
- Some bookings show extremely long trips which may not make sense?
- Distribution plots show some differences between the categories for:
    - Speed
    - Acceleration
    - Gyro
- Used some feature engineering ideas from an SJSU thesis on driver telematics analysis [here](https://scholarworks.sjsu.edu/cgi/viewcontent.cgi?referer=https://www.google.com/&httpsredir=1&article=1394&context=etd_projects)

### Chosen features
- Distance travelled: This gives the total length of a trip in meters. This is calculated as:
    - Duration of current record * speed recorded

- Duration of trip: Measured in Seconds and taken from the last recorded entry per booking
- Acceleration, Gyro: X, Y and Z figures are combined into a single datapoint using Euclidean distance:
    - sqrt( x^2 + y^2 + z^2 )
    
- Gyro, Acceleration, Speed, Change in bearing
    - Maximum per trip
    - Minimum per trip
    - Mean per trip
    - Standard deviation per trip

### Notes
- Models tried: GBM, Random Forests, Neural Networks, Logistic Regression
- First few attempts showed consistent training and testing accuracy but both at low values of ~77%. This shows signs of underfitting
- Iteration involved feature engineering before cycling through models
- Tried polynomial features to increase the accuracy but not effective
- Noticed a degree of class imbalance between 0 and 1 from the confusion matrix results and overall base accuracy. Try up-sampling the minority class
    - Resampling data dropped accuracy from 78% to 68% as expected
    - However, AUC score improved which suggests better results over different decision thresholds
    - Trying to avoid upsampling as it may cause some bias in results. Use sample weights instead