---
layout: post

title: Estimating whether a credit card client will default or not.
---

#### Introduction   

Prior to become a Data Scientist, I was working as Software Engineer in a Fortune 500 company. Most of my experience there was working for clients in banking and financial services. What I did mostly for them is to build and maintain mainframe applications. Though they had different rules, different policies but I always noticed all of them had one common issue . That is, whether  their credit card client will pay the bill on time or not. This question intrigued me lot, so I tried to find if it is possible to estimate whether a credit card client will default or not. 

#### Data

For this problem, I used UCI default of credit card clients Data Set [Data](https://archive.ics.uci.edu/ml/datasets/default+of+credit+card+clients). In this data set, there are 30,000 examples and 24 features, and the goal is to estimate whether a person will default (fail to pay) their credit card bills; this column is labeled "default payment next month" in the data.

![data](/images/CC/img1.PNG)

#### Exploring the data

To see if the predictors has strong influence on the response variable: 

![data](/images/CC/img2.PNG)

From the above correlation we can see that history of payments that is X6 to X11 has strong linear correalation with our target Y (default payment).

Plot for one of the above correlated variable 'X6'.

```
# Plot to show the trend of X6 (old payment) with target Y 
fig = plt.figure(figsize=(17, 8))

import seaborn as sns
sns.violinplot(data["X6"],data["Y"])
plt.ylabel("Payment default")
plt.xlabel("X6 - Payment missed in past")
```

![data](/images/CC/img3.PNG)  

From the plot we see the values of X6 = 2 has higher density for Y = 1 , and for X6 = 8 gives lower density for Y= 1 . So it gives us the trend of defaulting months in X6 with restpect to target Y (1 is default payment ) . Also the average values for -1 (paid on time ), 0(paid) , 1(defaulted by 1 month ) are in Y = 0 , whereas the default on 3 months and onwards has average on Y= 1 (defaulter). We can expect similar trends for columns X7 -X11 as all shows high positive correlation with Y .

Plot to visualize the 'Age' range with past payment 'X6'.

```
# Plot shows age range with default and not default with resp. to past default payment.
mydata1=data[data.Y==0]
mydata2=data[data.Y==1]
plot1=mydata1.plot.scatter(x="X5", y="X6",color="orange",label="No default")
plot2=mydata2.plot.scatter(x="X5", y="X6", ax=plot1, label="Default")
plt.xlabel("Age")
plt.ylabel("Past payment")
plt.title("Age range of past payment defaulter")
```

![data](/images/CC/img4.PNG)  

This shows that all age groups has typically defaulted for maximum in the range of 4 months with respect to past history of payment.

#### Data splitting

Randomly splited the data into train, validation, test sets. The validation set will be used for experiments.

```
# Splitting the data into train(60%) , validation(20%) , test(20%) .
X = data.iloc[:, 0:23]
y = data.Y

# Splitting the data into train and test : 
Xtrain, Xtest , ytrain , ytest = train_test_split(X ,y , test_size=0.20)

# Splittingg the data into train and validate :
X_train, Xvalid , y_train , yvalid = train_test_split(Xtrain ,ytrain , test_size=0.20)
```

#### Baseline model

Creating a baseline model and cheking the accuracy with all features.

![data](/images/CC/img5.PNG) 

The train and validation score does not look very good with the DummyClassifier. 

#### Logistic Regression  

Logistic regression is used to describe data and to explain the relationship between dependent binary variable "Default payment" and independent variables. I used L1 regularisation to reduce the generalization error.

![data](/images/CC/img6.PNG)   

Train/validation error/accuracy vs. regularization strength : 

```
C_range = np.arange(-4.0, 8.0)
data = np.zeros((C_range.shape[0],2))
index = ['C: '+ i for i in map(str,10**C_range)]

for idx, val in enumerate(C_range):

    lr2 = LogisticRegression(C=10**val,penalty = 'l1' )
    lr2.fit(X_train,y_train)
    
    data[idx,0] = lr2.score(X_train,y_train)
    data[idx,1] = lr2.score(Xvalid,yvalid)
    
df = pd.DataFrame(data, index = index, 
                  columns=['Training score', 'Valid score'])

#Printing the dataframe: 
df
```

![data](/images/CC/img7.PNG) 

Plot for training/validation score against different C values: 

![data](/images/CC/img8.PNG)

The graph shows that with increase in C values makes the validation error constant.

#### Features

* The features X6 to X11 has multicollinearity , so I will only take X6 .
* I will add lag variables.
* I keep the combination of feature which increase at least some accuracy in validation error.
* Using forward selection to select features.

```
# Adding lag variables .
data['lag1']=data['X6'].shift(1)
data['lag2']=data['X6'].shift(2)

data['lag3']=data['X8'].shift(1)
data['lag4']=data['X8'].shift(2)

data=data.dropna()
```
![data](/images/CC/img9.PNG)

After the features selection getting my splits again after reloading the data and adding updated features.

```
# Splitting the data into train(60%) , validation(20%) , test(20%) .
feat_set = ['X6','lag3','lag4','lag1','lag2']

X = data[feat_set]
y = data.Y

# Splitting the data into train and test : 
Xtrain, Xtest , ytrain , ytest = train_test_split(X ,y , test_size=0.20)

# Splittingg the data into train and validate :
X_train, Xvalid , y_train , yvalid = train_test_split(Xtrain ,ytrain , test_size=0.20)
```

#### Trying models with selected features.

* Random forest
* SVM
* Sklearn NN
* Logistic Regression


```
classifiers = {
    'random forest' : RandomForestClassifier(n_estimators=50),
    'SVM'           : SVC(C=100),
    'sklearn NN'    : MLPClassifier(),
    'Logistic Reg'  : LogisticRegression(C=100, penalty = 'l1')
}

train_scores = dict()
valid_scores = dict()
test_scores = dict()

for classifier_name, classifier_obj in classifiers.items():
    print("Fitting", classifier_name)
    classifier_obj.fit(X_train, y_train)
    
    train_scores[classifier_name] = classifier_obj.score(X_train, y_train)
    valid_scores[classifier_name] = classifier_obj.score(Xvalid, yvalid)
    
pd.options.display.float_format = '{:,.2f}'.format # make things look prettier when printing
data = {"train acc": train_scores, "valid acc" : valid_scores}
df = pd.DataFrame(data, columns=data.keys())
df.index = list(classifiers.keys())
df
```

The result : 

![data](/images/CC/img10.PNG)

#### Hyperparameter Optimisation.

To get better result and reduce overfitting, hyperparameter optimisation has been performed with RandomizedSearchCV.

![data](/images/CC/img11.PNG)

![data](/images/CC/img12.PNG)

![data](/images/CC/img13.PNG)

The score improves with MLPClassifier and RandomForestClassifier with chosen hyperparameters. However , RandomForestClassifier gives the most consistent score. Hence, picking that as final model .

Running the validation and test data on RandomForestClassifier:

![data](/images/CC/img14.PNG)

Yes, the test accuracy agrees fairly well with the validation accuracy. My validation accuracy was 83.14 % with random forrests.    
Final test accuracy is 82.61%.

#### Summary of the result.

![data](/images/CC/img15.PNG)

* Conclusion: 

The variables of the data set gives model accuracy almost 80% for Logistic regression. After feature engineering the and setting the hyperparameters I increased the complexity of the models, hence giving better validation accuracy. RandomForrest classifier was most consistent with the score , so I selected Randomforrests classifier as my best model.
