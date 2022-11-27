

1. Dataset Introduction:
  1) Autoinsurance_train.csv: 

    Background: A dataset related to fraud claims of auto insurance. The main goal is to use the features of the policy holder and the car in the accident to predict if the claim is a fraud claim.
    Type: Trainset
    Features: "age_of_driver" "safty_rating"  "annual_income"  "past_num_of_claims" "liab_prct" "claim_est_payout" "age_of_vehicle" "vehicle_price"  "vehicle_weight" (9 in total; all of them are numeric)
    Response: "fraud" (a binary variable; 1 represents this is a fraud claim; 0 represents this is not a fraud claim)
    Number of samples: 14000

  2) Autoinsurance_test.csv:

    Type: Testset
    Features: "age_of_driver" "safty_rating"  "annual_income"  "past_num_of_claims" "liab_prct" "claim_est_payout" "age_of_vehicle" "vehicle_price"  "vehicle_weight" (9 in total; all of them are numeric)
    Response: "fraud" (a binary variable; 1 represents this is a fraud claim; 0 represents this is not a fraud claim)   
    Number of samples: 3466


2. General Idea of the Project for data pretraining:
This project includes 3 parts:
I. See Part I in the file 'Models Comparison.ipynb': 
    1) Part I acts as our baseline neural network model and no data preprocessing and pretraining are done. 
    2) We stack linear, relu, batchnorm, relu, batchnorm, dropout and linear in sequence to contruct our neural network. Then BCEWITHLOGITLOSS (the loss combines a Sigmoid layer and the BCELoss) is used to construct our loss function. After that, adam optimizer is used as our optimization method.
    3) The trainset is divided into mini-batches with size 64. In each epoch, we use forward propagation to construct our loss function and use backward propagation to get the gradient of each parameter in the network within each batch. Then adam optimizer is used to update the network parameters. The number of epoch is set as 20 and we get our optimal network after the training.

II. See Part II in the file 'Data_preprocessing_Normalization+BoxCox.Rmd':
    1) In this part, we preprocess the train data and test data through BoxCox transformation and z-score normalization
    2) For Box-Cox transformation, we first find the lambda that maximizes the log-likelihood function of the corresponding predictor. Then the lamba is used as the power to transform this predictor. Box-Cox transformation can ensure the normality of the predictor. Details can be seen in the .Rmd file.
    3) After Box-Cox transformation, we compute the sample mean and standard deviation of each column. Then they are used to normalize the corresponding predictor. The normalization ensure the mean of each predictor is close to 0 and the variance of the each predictor is close to 1.  
    4) After these two transformations, we output the new trainset and testset for the pretraining in part III.  

III. See Part III in the file 'Models Comparison.ipynb': 
    1) In this part, based on the preproessed trainset obtained in part II, we use an autoencoder to pretrain the features in the trainset and then fit the same model as Part I to compare the two.
    2) The autoencoder is contructed as the network stacked by Linear(input=9,output=5), Linear(input=5,output=5), Linear(input=5,output=5) and Linear(input=5,output=9) in sequence. MSE loss function and adam optimizer are used to optimize the parameters in the network. Then by inputing the features of the trainset into the optimal network, the network outputs our new features. (The autoencoding process is a process of compression and rescontruction, which finally output a good representation of the features)
 


3. Files in the Data Pretraining folder:
1) 'Comparison of Prediction errors for models with or without data pretraining via Autoencoder.ipynb' 2) 'Data_preprocessing_Normalization+BoxCox.Rmd' 3) 'Autoinsurance_train.csv' 4) 'Autoinsurance_test.csv' 5) 'Readme.txt'


4. Instruction to run the files:
    1) Run all the chunks of 'Part I' in 'Models Comparison.ipynb' by Colab. Caution: When the 3rd chunk is run, upload 'Autoinsurance_train.csv' according to the comment. When the 6th chunk is run, upload 'Autoinsurance_test.csv' according to the comment.
    2) Run all the chunks of 'Part II' in 'Data_preprocessing_Normalization+BoxCox.Rmd' by R Studio. Then the preprocessed datasets 'Autoinsurance_trainnew.csv' and 'Autoinsurance_testnew.csv' will be output to the folder. (If it doesn't, you can use the same two datasets in 'Backup datasets' instead)
    3) Run all the chunks of 'Part III' in 'Models Comparison.ipynb' by Colab.  Caution: When the 13rd chunk is run, upload 'Autoinsurance_trainnew.csv' according to the comment. When the 16th chunk is run, upload 'Autoinsurance_testnew.csv' according to the comment.