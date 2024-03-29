# labAI900

In Azure Machine Learning studio, view the Automated ML page (under Authoring).

Create a new Automated ML job with the following settings, using Next as required to progress through the user interface:

Basic settings:

Job name: mslearn-bike-automl
New experiment name: mslearn-bike-rental
Description: Automated machine learning for bike rental prediction
Tags: none
Task type & data:

Select task type: Regression
Select dataset: Create a new dataset with the following settings:
Data type:
Name: bike-rentals
Description: Historic bike rental data
Type: Tabular
Data source:
Select From web files
Web URL:
Web URL: https://aka.ms/bike-rentals
Skip data validation: do not select
Settings:
File format: Delimited
Delimiter: Comma
Encoding: UTF-8
Column headers: Only first file has headers
Skip rows: None
Dataset contains multi-line data: do not select
Schema:
Include all columns other than Path
Review the automatically detected types
Select Create. After the dataset is created, select the bike-rentals dataset to continue to submit the Automated ML job.

Task settings:

Task type: Regression
Dataset: bike-rentals
Target column: Rentals (integer)
Additional configuration settings:
Primary metric: Normalized root mean squared error
Explain best model: Unselected
Use all supported models: Unselected. You’ll restrict the job to try only a few specific algorithms.
Allowed models: Select only RandomForest and LightGBM — normally you’d want to try as many as possible, but each model added increases the time it takes to run the job.
Limits: Expand this section
Max trials: 3
Max concurrent trials: 3
Max nodes: 3
Metric score threshold: 0.085 (so that if a model achieves a normalized root mean squared error metric score of 0.085 or less, the job ends.)
Timeout: 15
Iteration timeout: 15
Enable early termination: Selected
Validation and test:
Validation type: Train-validation split
Percentage of validation data: 10
Test dataset: None
Compute:

Select compute type: Serverless
Virtual machine type: CPU
Virtual machine tier: Dedicated
Virtual machine size: Standard_DS3_V2*
Number of instances: 1
* If your subscription restricts the VM sizes available to you, choose any available size.

Submit the training job. It starts automatically.

Wait for the job to finish. It might take a while — now might be a good time for a coffee break!

Review the best model
When the automated machine learning job has completed, you can review the best model it trained.

On the Overview tab of the automated machine learning job, note the best model summary. Screenshot of the best model summary of the automated machine learning job with a box around the algorithm name.

Note You may see a message under the status “Warning: User specified exit score reached…”. This is an expected message. Please continue to the next step.

Select the text under Algorithm name for the best model to view its details.

Select the Metrics tab and select the residuals and predicted_true charts if they are not already selected.

Review the charts which show the performance of the model. The residuals chart shows the residuals (the differences between predicted and actual values) as a histogram. The predicted_true chart compares the predicted values against the true values.

Deploy and test the model
On the Model tab for the best model trained by your automated machine learning job, select Deploy and use the Web service option to deploy the model with the following settings:
Name: predict-rentals
Description: Predict cycle rentals
Compute type: Azure Container Instance
Enable authentication: Selected
Wait for the deployment to start - this may take a few seconds. The Deploy status for the predict-rentals endpoint will be indicated in the main part of the page as Running.
Wait for the Deploy status to change to Succeeded. This may take 5-10 minutes.
Test the deployed service
Now you can test your deployed service.

In Azure Machine Learning studio, on the left hand menu, select Endpoints and open the predict-rentals real-time endpoint.

On the predict-rentals real-time endpoint page view the Test tab.

In the Input data to test endpoint pane, replace the template JSON with the following input data:

Code
 {
   "Inputs": { 
     "data": [
       {
         "day": 1,
         "mnth": 1,   
         "year": 2022,
         "season": 2,
         "holiday": 0,
         "weekday": 1,
         "workingday": 1,
         "weathersit": 2, 
         "temp": 0.3, 
         "atemp": 0.3,
         "hum": 0.3,
         "windspeed": 0.3 
       }
     ]    
   },   
   "GlobalParameters": 1.0
 }
Click the Test button.

Review the test results, which include a predicted number of rentals based on the input features - similar to this:

Code
 {
   "Results": [
     444.27799000000000
   ]
 }
The test pane took the input data and used the model you trained to return the predicted number of rentals.

Let’s review what you have done. You used a dataset of historical bicycle rental data to train a model. The model predicts the number of bicycle rentals expected on a given day, based on seasonal and meteorological features.
