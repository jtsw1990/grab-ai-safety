# grab-ai-safety
Modelling exercise for Grab AI Challenge: Safety dataset

### Project Plan
- Understand the data and telematics sufficiently enough to proceed with the exercise
- Decide on a most reasonable format to transform the data into before modelling
- Choose a suitable algorithm that gives reasonable test results
- Grid search to optimize
- Decide on a technique for deployment: Interactive web-app, Jupyter results, ect.

### Understanding the data
- bookingID: Numeric key to differentiate bookings. One to many relationship in terms of features to labels.
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
- On a high-level view, it seems to be a classification problem with binary labels safe and unsafe.
- Looks like most of the problem is going to be focused on feature engineering as the data does not have a 1 to 1 relationship between the features and response
- Also with so many data points, deep learning may be required to cut through all that noise (maybe) but always try a simple logistic regression first.
- Need to understand data better, but it as per instructions, a single booking may have many rows of data. This means that not every row will be meaningful? Again, looks like a feature engineering exercise or some PCA maybe to see what's important? Either way, some judgement for data manipulation is required.

### Exploratory Data Analysis
- A high-level view shows that not all the features are useful logically in prediction the safety category.
- Distribution plots show some differences between the categories for:
    - Speed
    - Acceleration
    - Gyro