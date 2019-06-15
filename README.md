# grab-ai-safety
Modelling exercise for Grab AI Challenge: Safety dataset

### Instructions
- Project written in `python 3.6`
- Repository contains:
    - 5 Jupyter notebooks showing the whole workflow from data download to submission predictions
    - Requirements.txt specifying all the dependancies required for model evaluation
    - Store folder containing older iterations and backups
    - Outputs folder containing pickled:
        - Feature transformation functions
        - Saved model weights
- Notebooks labelled 1-4 are not required for submission but showcases:
    - Thought process
    - Code quality
    
1. Download dependancies required with `pip install requirements.txt`    
2. For evaluation of model, please open `5. Grab Safety - Submission Notebook` and edit the `SOURCE_LIST` list to include all the paths of the feature data.
3. Once `SOURCE_LIST` is updated, just run the all cells in the kernel and predictions will be stored in the global variable `preds`.
4. Depending on requirements, a to_csv command has been commented out.


### Project Plan
- Understand the data and telematics sufficiently enough to proceed with the exercise
- Decide on a most reasonable format to transform and extract features from the data before modelling
- Choose a suitable algorithm (or ensemble) that gives reasonable test results
- Grid search to optimize
- Decide on a technique for deployment: Interactive web-app, Jupyter results, ect.
    - Chosen method of deployment: Jupyter notebooks

### Understanding the data
- `bookingID`: Numeric key to differentiate bookings. One to many relationship in terms of features to labels
- `Accuracy`: How accurate the gps location is with reference to what?
- `Bearing`: Your GPS bearing is the compass direction from your current position to your intended destination. In other words, it describes the direction of a destination or object. If you're facing due north, and you want to move toward a building directly to your right, the bearing would be east or 90 degrees
- `acceleration_x`: How fast speed changes in the x direction
- `acceleration_y`: How fast speed changes in the y direction
- `acceleration_z`: How fast speed changes in the z direction
- `gyro_x`: How fast angular position changes in x
- `gyro_y`: How fast angular position changes in y
- `gyro_z`: How fast angular position changes in z
- `second`: Each booking starts at 0 and ends whenever the last record is taken.
- `Speed`: Speed of the vehicle at the time of measure


### Exploratory Data Analysis
- A high-level view shows that not all the features are useful in prediction the safety category
- Also, there are 18 duplicated labels. Whether or not they are supposed to be dangerous or safe is unknown and will be taken out of the dataset for modelling purposes
- Some bookings show extremely long trips which may not make sense as it converts into approximately 47 years. These entries are removed.
- Some entries also show ridiculous speed readings converting to about 500km/h. These entries are also removed.
- Distribution plots show some differences between the categories for:
    - Duration of trip 
    - Speed

### Chosen features
- `Distance travelled`: This gives the total length of a trip in meters. This is calculated as:
    - Duration of current record * speed recorded

- `Duration of trip`: Measured in Seconds and taken from the last recorded entry per booking
- `Acceleration`, `Gyro`: X, Y and Z figures are combined into a single datapoint using Euclidean distance:
    - `sqrt( x^2 + y^2 + z^2 )`
    
- `Gyro`, `Acceleration`, `Speed`, `Change in bearing`
    - Maximum per trip
    - Minimum per trip
    - Mean per trip
    - Standard deviation per trip
    - Various percentiles

### Modelling
- Models tried: GBM, Random Forests, Neural Networks, Logistic Regression
- First few attempts showed consistent training and testing accuracy but both at low values of ~77%. This shows signs of underfitting
- Iteration involved feature engineering before cycling through models
- Tried polynomial features to increase the accuracy but not effective
- Noticed a degree of class imbalance between 0 and 1 from the confusion matrix results and overall base accuracy. Try up-sampling the minority class
    - Resampling data dropped accuracy from 78% to 68% as expected
    - However, AUC score improved which suggests better results over different decision thresholds
    - Trying to avoid upsampling as it may cause some bias in results. Use sample weights instead
 - Realised that there are data leakage issues in the current model with outlier conditions and aggregations. To restructure process to minimize this.
     - Split data into training and testing before performing any feature extractions
     - Choose a more robust condition for second outliers instead of using the full dataset. (That was dumb) 
- Model performance dropped from 96% back down to 78% accuracy after fixing for data leakage issue.
- Focusing on the models:
    - Gradient Boosted Machine
    - Logistic Regression
    - Random Forests
- All 3 models show signs of underfitting with lower than desired training and testing accuracy.

## References

- [Driver Telematics Analysis thesis from SJSU](https://scholarworks.sjsu.edu/cgi/viewcontent.cgi?referer=https://www.google.com/&httpsredir=1&article=1394&context=etd_projects)
