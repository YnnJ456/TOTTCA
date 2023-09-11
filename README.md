# TOP-TTCA
TOP-TTCA: Prediction of Tumor T Cell Antigens Using Two-stage Optimization of Machine Learning Models 

# Description
This is the source code of TOP-TTCA, a machine learning predictor for tumor T cell antigen based on two-stage optimization (manuscript under review). The first stage is the optimization of feature vector, and the second stage is the optimization of machine learning models. The trained models are included in this package, facilitating prediction on a given data set.

# Installation
Requiremenets:
* Python = 3.8, pycaret[full] = 2.3.10

Packages
* Install required packages using `pip install -r requirements.txt`

# Usage
Modify main_predict.py for your data set in fasta format
* Input file
  * Fasta file
  
* output file
  * binary_vector.csv -- The prediction output in binary format (1 for positive and 0 for negative)
    
    ![image](https://github.com/YnnJ456/TOP-TTCA/assets/95170485/89e9b8ac-c49a-465d-8119-069b7852807a)

  * probability.csv -- The prediction probability estimate
    
    ![image](https://github.com/YnnJ456/TOP-TTCA/assets/95170485/c03deada-58cc-4c1f-814f-301f9362fa21)

When dataset = 'DS1', the program will use models trained on DS1, corresponding features and their normalization scaler to process data and perform prediction.

When dataset = 'DS2' , the program will use models trained on DS2, corresponding features and their normalization scaler to process data and perform prediction.
```py
# If you want to use different model, you can change dataset
dataset = 'DS1'
if dataset == 'DS1':
    model_use = '1'
elif dataset == 'DS2':
    model_use = '2'
```

```py
# Path setting
pathDict = {'paramPath': f'../data/param/{dataset}/',  # The path by default consists of featureTypeDict.pkl and robust.pkl
            'saveCsvPath': '../data/mlData/new_data/',  # Your encoded data will be automatically saved in this path
            'modelPath': f'../data/finalModel/{dataset}/',  # This path by default consists of ML models of catboost, et, and gbc
            'outputPath': '../data/output/'}  # Prediction result files will be saved in the path
```

The following dictionary specificies the fasta file for prediction. They keys of 0, 1, and -1 stand for negative, positive, and unknown label of the sequences in a fasta file.

0 = negative sequences; 
1 = positive sequences; 
-1 = sequences of unknown label

'None' can be used if the corresponding file is not provided.

```py
# Input your FASTA file, the example file can be found in data/mlData/DS1/test_neg.FASTA
inputDataDict = {0: '../data/mlData/DS1/test_neg.FASTA',
                 1: '../data/mlData/DS1/test_pos.FASTA',
                 -1: None}
```



Here is the code in main_predict.py of which parameters are set and the program is ready to be excecuted.

If your data have label, please set True in havelabel, else set False

```py
# If your data have label, please set True in havelabel, else set False
topObj = TOP_TTCA_Predict(model_use=model_use, pathDict=pathDict, haveLabel=True)
topObj.loadData(inputDataDict=inputDataDict)
topObj.featureEncode()
topObj.doPredict()
```
