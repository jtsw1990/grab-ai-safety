# grab-ai-safety
Modelling exercise for Grab AI Challenge: Safety dataset

### Project Plan
- Understand the data and telematics sufficiently enough to proceed with the exercise
- Decide on a most reasonable format to transform the data into before modelling
- Choose a suitable algorithm that gives reasonable test results
- Grid search to optimize
- Decide on a technique for deployment: Interactive web-app, Jupyter results, ect.

### Initial observations
- On a high-level view, it seems to be a classification problem with binary labels safe and unsafe.
- Looks like most of the problem is going to be focused on feature engineering as the data does not have a 1 to 1 relationship between the features and response
- Also with so many data points, deep learning may be required to cut through all that noise (maybe) but always try a simple logistic regression first.
- Need to understand data better, but it as per instructions, a single booking may have many rows of data. This means that not every row will be meaningful? Again, looks like a feature engineering exercise or some PCA maybe to see what's important? Either way, some judgement for data manipulation is required.