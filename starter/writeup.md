# STEP 1: TRAINING AND DEPLOYMENT ON SAGAMAKER 
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

<strong>d. MULTI-INSTANCE TRAINING</strong><br>

Link: 
* https://sagemaker.readthedocs.io/en/stable/api/training/smd_model_parallel_general.html
* https://docs.aws.amazon.com/sagemaker/latest/dg/distributed-training.html
* https://github.com/aws/amazon-sagemaker-examples/blob/main/training/distributed_training/pytorch/data_parallel/mnist/pytorch_smdataparallel_mnist_demo.ipynb
* https://sagemaker.readthedocs.io/en/stable/frameworks/pytorch/sagemaker.pytorch.html

I changed the estimator definition with the following 2 changes:
1. instance_count = 2 instead if instance_count = 1
2. added the parameter: distribution={"smdistributed": {"dataparallel": {"enabled": True}}}

# STEP 2: EC2 TRAINING

<strong>a. EC2 SETUP</strong><br>
I choose no AMI but used Amazon instance ml.t3.large.
Then I create a virtual environment with the following commands:
> * python3 -m venv env
> * source env/bin/source
> * pip install .....all dependencies (e.g. torch, numpy, tqdm, pandas)

Then, I trained the model and saved it into TrainedModels folder: 
![trained model onto EC2](screenshot/09_step_2_EC2_Training.png "trained model onto EC2")

I also analyzed the code in the file ec2train.py and following I wrote up the differences:
1. No arguments are passed through argparse python library; that's why I launched the script directly and not from PyTorch estimator.
2. The function create_data_loaders works with already downloaded image folders, while the hpo.py leverage the data from S3 bucket downloaded from container.
3. I can conclude that the changes are minimal to adapt hpo.py to EC2 training.


