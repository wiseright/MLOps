# STEP 1: 
<strong>a. INITIAL SETUP</strong><br>
In this step I created the notebook instance. I chose ml.t3.medium because I thought it is a good tradeoff between speed and cost.<br>
Then I uploaded training_and_deploy-solution.ipynb and hpo.py files into notebook instance
> Sagemaker Dashboard screenshot:<br>
![Notebook instance](screenshot/01_step_1_notebook_instance.png "Notebook instance")

<strong>b. DOWNLOAD DATA TO AN S3 BUCKET</strong><br>
The first step of training_and_deploy-solution.ipynb is related to the loading of dataset to S3 bucket.
> S3 dashboard with training, validation and testing data uploaded:<br>
![S3 bucket setup](screenshot/02_step_1_S3_with_train_files.png "S3 bucket setup")

<strong>c. TRAINING AND DEPLOYMENT</strong><br>
I started hyperparameter training using tuner object by Sagemaker.
Then, I get the best HP of estimator and started the training.<br>
The trained model name is: 
>s3://sagemaker-us-east-1-827713284860/dog-pytorch-2023-02-11-12-00-42-741/output/model.tar.gz

> HPO tuner after complete:<br>
![HPO tuning](screenshot/04_step_1_HyperParameter_Optimization_Completed.png "HPO tuning")

> Training job after complete:<br>
![train job](screenshot/06_step_1_Training_Job_Completed.png "train job")

Then, I started the deployment of the above-mentioned model.<br>
The name of deployed model is: pytorch-inference-2023-02-11-13-10-12-631

> Model Deploy (In Progress):<br>
![Model Deploy In Progress](screenshot/07_step_1_Deploy_InProgress.png "Model Deploy In Progress")

> Model Deploy (Completed):<br>
![Model Deploy Completed](screenshot/08_step_1_Deploy_Completed.png "Model Deploy Completed")