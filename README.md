# DDT1-01
Developing a regression-based model for screening compounds with 3CLpro inhibitory activity. This project is about the task DDT1-01 as provided in the Drug Discovery Hackathon 2020. (https://www.mygov.in/task/drug-discovery-hackathon-2020/)
This project develops a regression model for COVID-19 drug discovery.

## Input:

### smiles.pdf
Contains the SMILES of various molecules along with their pIC50 values.
## Output:
CSV files containing the pIC50 values of blinded molecules and the top 100 CAS antiviral molecules are available in pIC50_values_csv/ folder

## Follow this order of execution of jupyter notebooks to make sense of this project:

  ### 1. training_and_test_compounds_to_sdf.ipynb
  This notebook converts SMILES in smiles.pdf to sdf. This sdf is input to a third party tool (Padel Descriptor). Padel converts sdf to topological molecular descriptors. Descriptor file is available in molecular_descriptors_csv/molecular_descriptors_with_pIC50_values.csv
  
  ### 2. preprocessing_training.ipynb
  This notebook removes 0 variance descriptors and Performs min-max scaling (0,1) of descriptors and pIC50 values. Also preprocesses test descriptors.
  
  ### 3. create_dataset.ipynb
  Splits data into train and test.Also creates leave-one-out-datasets. More info available in notebook. 
  
  ### 4. CNN_regression.ipynb
  This is where the magic happens. Instead of going for linear regression I tried to train a Convolutional Neural Net to regress on pIC50 values. SInce the number of descriptors for a particular molecule is quite large (about 1120), I reshaped them into a 2D array. So each molecule can be represented as grayscale images of shape 35 by 32. These images are put through a CNN and trained to output corresponding pIC50 values.69 models are trained using the leave one out strategy. Out of 69, 35 best models were chosen according to R-squared test values. R-squared (test) value peaked at 0.9048 (see best_models/performance_parameters_of_best_models/performance_parameters.xlsx for best model external validation parameters). The external validation parameters were generated using a third party tool called XternalValidationPlus (https://sites.google.com/site/dtclabxvplus/). This notebook also writes predicted and observed pIC50 values of the test set for all models trained using LOO technique. This is written in csv files and can be found in data_to_evaluate_xternal_validation_parameters/ on running this notebook.
  
  ### 5. blinded_compounds_to_sdf.ipynb
  This notebook converts the blinded molecules from smiles to sdf. This sdf file is again passed through Padel descriptor (third party software) to generate molecular descriptors.
  
  ### 6. preprocessing_blinded.ipynb
  Applies min-max scaling to blinded compounds molecular descriptors. The same scaler is used which was used to scale training data.The scaler is available in scaler_data/ folder. This note book also filters out descriptors which were used during training process. Other descriptors are rejected.
  
  ### 7. applicability_domain_and_blinded_molecules_pIC50_computation_from_best_models.ipynb
  As evident from the name this notebook defines the applicability domain and computes pIC50 values for blinded molecules. The values for blinded molecules are saved in pIC50_values_csv/blinded_pIC50_values.csv. More details regarding the applicability domain are available in the notebook.
  
  ### 8. preprocessing_cas_antiviral.ipynb
  The CAS antiviral database sdf file is available in sdf/cas_antiviral.sdf. This sdf file is supplied to Padel descriptor to generate molecular descriptors. The molecular descriptors for CAS antiviral compounds are saved in molecular_descriptors_csv/cas_antiviral_molecular_descriptors.csv.
  This notebook takes the raw molecular descriptors and applies min-max scaling. The same scaler (available in scaler_data/) used to scale training set compounds is used. Molecules having NaN values as any of the descriptors are dropped. Only those descriptors are retained which are used during training. The resultant processed descriptor file is saved in molecular_descriptors_csv/min_max_scaled_cas_antiviral_molecular_descriptors.csv
  
 ### 9. predict_cas_antiviral.ipynb
 This notebook predicts pIC50 values of CAS antiviral molecules by using 35 best models used during training. Molecules are ranked in descending order of pIC50 values which are within the applicbility domain of the models. The top 100 ranked compounds are available in pIC50_values_csv/cas_antiviral_top_100_pIC50_vals.csv
 
 ## About the folders in this project:
 
 ### 1. best_models/
 Saves the best model architectures and weights. Saves data used to train and test the models. Also saves the performance parameters of best models.
 ### 2. data/
 Saves data when create_dataset.ipynb is run. Contains training, leave one out and test datasets.
 
 ### 3. models_info/
 Saves model architectures,weights and model parameters when CNN_regression.ipynb is run. Also saves data to compute external validation parameters.
 ### 4. molecular_descriptors_csv/
 Saves raw and scaled molecular descriptors for training/test, blinded and CAS antiviral compounds. This folder serves as the master dataset for the project.
 
 ### 5. pIC50_values_csv/
 Saves pIC50 values for blinded and top 100 CAS antiviral compounds. This folder serves as the output of this project.
 
 ### 6. scaler_data/
 Saves the min-max scaler attributes used to transform training set molecular descriptors.
 
 ### 7. sdf/
 saves sdf files for training,test,blinded and CAS antiviral molecules..
 
