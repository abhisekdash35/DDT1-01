# DDT1-01
Developing a regression-based model for screening compounds with 3CLpro inhibitory activity. This project is about the task DDT1-01 as provided in the Drug Discovery Hackathon 2020. (https://www.mygov.in/task/drug-discovery-hackathon-2020/)
This project develops a regression model for COVID-19 drug discovery.

## Input:

### smiles.pdf:
Contains the SMILES of various molecules along with their pIC50 values.

## Follow this order of execution to make sense of this project:

  ### training_and_test_compounds_to_sdf.ipynb: 
  Converts SMILES in smiles.pdf to sdf. This sdf is input to a third party tool (Padel Descriptor). Padel converts sdf to topological molecular descriptors. Descriptor file is available in molecular_descriptors_csv/molecular_descriptors_with_pIC50_values.csv
  
  ### preprocessing_training.ipynb:
  Removes 0 variance descriptors. Performs min-max scaling (0,1) of descriptors and pIC50 values. Also preprocesses test descriptors.
  
  ### create_dataset.ipynb
  Splits data into train and test.Also creates leave-one-out-datasets. More info available in notebook. 
  
  ### CNN_regression.ipynb
  This is where the magic happens. Instead of going for linear regression I tried to train a Convolutional Neural Net to regress on pIC50 values. SInce the number of descriptors for a particular molecule is quite large (about 1120), I reshaped them into a 2D array. So each molecule can be represented as grayscale images of shape 35 by 32. These images are put through a CNN and trained to output corresponding pIC50 values.69 models are trained using the leave one out strategy. Out of 69, 35 best models were chosen according to R-squared test values. R-squared (test) value peaked at 0.9048 (see best_models/performance_parameters_of_best_models/performance_parameters.xlsx for best model external validation parameters). The external validation parameters were generated using a third party tool called XternalValidationPlus (https://sites.google.com/site/dtclabxvplus/).
