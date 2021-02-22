# Sharks
Shark attacks Worldwide 2020
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt
import numpy as np
from IPython.display import display
%matplotlib inline
import plotly.offline as py
import plotly.graph_objs as go
import plotly.tools as tls
py.init_notebook_mode(connected=True)
import sys
import os
import warnings
warnings.filterwarnings('ignore')

dfsharks = pd.read_excel('2020-ISAF-Data-1.xlsx')
dfsharks.info()

dfsharks.head()
dfsharks.isna().sum()
dfsharks.fillna(0.0, inplace=True)
dfsharks

col_to_include = ['Classification','Locality','Bite Count',
                 'Total unprovoked bites',
                 'Fatal','Victim Activity at Time of Attack','%']
renaming = {'Locality':'Country',
           'Victim Activity at Time of Attack': 'Activity'}
df = dfsharks[col_to_include].rename(renaming, axis=1)
key_vars = ['Bite Count','Fatal','Total unprovoked bites','%']
df

# In this section I updated sum of the entries. With some research from https://www.floridamuseum.ufl.edu/shark-attacks/yearly-worldwide-summary/ I corrected some of the inputs that where incorrect e.g. Maine had fatal shark attack in 2020
df= pd.DataFrame(df, columns= ['Classification','Country','Bite Count','Total unprovoked bites',
                               'Fatal','Activity','%'])
df['Bite Count'] = df['Bite Count'].replace(['Unprovoked bites'],0.0)
df['Total unprovoked bites'] = df['Total unprovoked bites'].replace(['Unprovoked bites'],0.0)
df['Country'] = df['Country'].replace(['Miami-Dade'],'Maine-Harpswell')
df['Total unprovoked bites'] = df['Total unprovoked bites'].replace([16.0],0.0)
df['Country'] = df['Country'].replace(['U.S.'],'United States')
df['Classification'] = df['Classification'].replace(['U.S. Statistics'],'US Statistics')
df['Country'] = df['Country'].replace(['Total'],'South Carolina')
df['Country'] = df['Country'].replace(['Florida counties'],'Florida')
df['Country'] = df['Country'].replace(['Palm beach county'],'North Carolina')
df['Country'] = df['Country'].replace(['St. Johns County'],'Oregon')
df.iloc[[9],[1]] = 'Unknown'
df.iloc[[9],[0]] = 'Unknown'
df.iloc[[10],[0,1]] = 'Unknown'
df.iloc[[11],[1]] = 'Cities-Counties'
df.iloc[[12],[3]] = 8.0
df.iloc[[13],[3]] = 1
df.iloc[[17],[4]] = 1
df.iloc[[21],[1,2,3,4]] = ['US Totals',33.0,33.0,3.0]
df.iloc[[14],[1,4]] = ['California',1.0]
df.iloc[[16],[1,4]] = ['Hawaii',1.0]
df.iloc[[0,1],[2]] = [33.0,18.0]
df.iloc[[2,5,6,7],[2]] = [1.0]
df.iloc[[8],[0]] = 'Total Attacks'
df.iloc[[13,15,],[1]] = ['Alabama','Delaware']
df

df= pd.DataFrame(df, columns= ['Country','Bite Count','Total unprovoked bites',
                               'Fatal','Activity','%'])
df

SEED = 10
np.random.seed(SEED)

df.describe()

g = sns.pairplot(df)

def plotHist(df,nameOfFeature):
    cls_train = df[nameOfFeature]
    data_array = cls_train
    hist_data = np.histogram(data_array)
    binsize = 20

    trace1 = go.Histogram(
        x=data_array,
        histnorm='probability',
        name='Histogram Shark Attacks 2020',
        autobinx=False,
        xbins=dict(
            start=df[nameOfFeature].min()-1,
            end=df[nameOfFeature].max()+1,
            size=binsize
        )
    )
    
    trace_data = [trace1]
    layout = go.Layout(
        bargroupgap=0.3,
         title='The number of  ' + nameOfFeature,
        xaxis=dict(
            title=nameOfFeature,
            titlefont=dict(
                family='Courier New, monospace',
                size=18,
                color='#7f7f7f'
            )
        ),
        yaxis=dict(
            title='Number of labels',
            titlefont=dict(
                family='Courier New, monospace',
                size=18,
                color='#7f7f7f'
            )
        )
    )
    fig = go.Figure(data=trace_data, layout=layout)
    py.iplot(fig)
    

plotHist(df,'Bite Count')

from scipy.stats import skew
from scipy.stats import kurtosis

def plotBarCat(df,feature,target):
    x0 = df[df[target]==0][feature]
    x1 = df[df[target]==1][feature]

    trace1 = go.Histogram(
        x=x0,
        opacity=0.75
    )
    trace2 = go.Histogram(
        x=x1,
        opacity=0.75
    )

    data = [trace1, trace2]
    layout = go.Layout(barmode='overlay',
                      title=feature,
                       yaxis=dict(title='Count'
        ))
    fig = go.Figure(data=data, layout=layout)

    py.iplot(fig, filename='overlaid histogram')
    
    def DescribeFloatSkewKurt(df,target):
        print('-*-'*25)
        print("{0} mean : ".format(target), np.mean(df[target]))
        print("{0} var  : ".format(target), np.var(df[target]))
        print("{0} skew : ".format(target), skew(df[target]))
        print("{0} kurt : ".format(target), kurtosis(df[target]))
        print('-*-'*25)
    
    DescribeFloatSkewKurt(df,target)
    
plotBarCat(df,'Country','Bite Count')
plotBarCat(df,'Country','Fatal')

# Pie chart Countries
labels = ['United States','Australia','Fiji Islands','French Polynesia','New Caledonia',
          'New Zealand','St.Martin','Thailand']
sizes = [33,18,1,1,1,1,1,1]
explode = (0, 0.2,0,0,0,0,0,0)

fig1, ax1 = plt.subplots()
# Here I used startangle 45 degrees @90 degrees names of the countries start overlapping each other 
ax1.pie(sizes,explode=explode,labels=labels,autopct='%1.1f%%',
        shadow=True, startangle=45)
ax1.axis('equal')  # Equal aspect ratio ensures that pie is drawn as a circle.

plt.show()
    
# Pie chart Activities
labels = ['Surfing/board sports','Swimming/wading','Snorkeling/free-diving','Body surfing/horseplay']
sizes = [61.4035,26.3158,3.5088,5.2632]
explode = (0, 0.2,0,0)

fig1, ax1 = plt.subplots()
ax1.pie(sizes,explode=explode,labels=labels,autopct='%1.1f%%',
        shadow=True, startangle=90)
ax1.axis('equal')  # Equal aspect ratio ensures that pie is drawn as a circle.

plt.show()

from sklearn.preprocessing import StandardScaler
from sklearn.model_selection import train_test_split
from sklearn.model_selection import KFold
from sklearn.model_selection import StratifiedKFold
from sklearn.model_selection import cross_val_score
from sklearn.model_selection import GridSearchCV
from sklearn.metrics import classification_report
from sklearn.metrics import confusion_matrix
from sklearn.metrics import accuracy_score
from sklearn.pipeline import Pipeline
from sklearn.linear_model import LogisticRegression
from sklearn.tree import DecisionTreeClassifier
from sklearn.neighbors import KNeighborsClassifier
from sklearn.discriminant_analysis import LinearDiscriminantAnalysis
from sklearn.naive_bayes import GaussianNB
from sklearn.svm import SVC
from sklearn.ensemble import AdaBoostClassifier
from sklearn.ensemble import GradientBoostingClassifier
from sklearn.ensemble import RandomForestClassifier
from sklearn.ensemble import ExtraTreesClassifier

df.groupby('Country')
df.set_index('Country', inplace=True)
df

# Extract out the numerical data and scale it to zero mean and unit standard deviation
X = df[key_vars]
Y = df['Bite Count']
scaler = StandardScaler().fit(X)
X_scaled = pd.DataFrame(scaler.fit_transform(X), columns = X.columns, index = X.index)

X_train, X_test, y_train, y_test =train_test_split(X,Y,
                                                   test_size=0.20,
                                                   random_state=0)
                                                   
# Checking Algorithms
def GetBasedModel():
    basedModels = []
    basedModels.append(('LR'   , LogisticRegression()))
    basedModels.append(('LDA'  , LinearDiscriminantAnalysis()))
    basedModels.append(('KNN'  , KNeighborsClassifier()))
    basedModels.append(('CART' , DecisionTreeClassifier()))
    basedModels.append(('NB'   , GaussianNB()))
    basedModels.append(('SVM'  , SVC(probability=True)))
    basedModels.append(('AB'   , AdaBoostClassifier()))
    basedModels.append(('GBM'  , GradientBoostingClassifier()))
    basedModels.append(('RF'   , RandomForestClassifier()))
    basedModels.append(('ET'   , ExtraTreesClassifier()))

    
    return basedModels
    
def BasedLine2(X_train, y_train,models):
    # Test options and evaluation metric
    num_folds = 9
    scoring = 'accuracy'

    results = []
    names = []
    for name, model in models:
        kfold = StratifiedKFold(n_splits=num_folds, random_state=SEED)
        cv_results = cross_val_score(model, X_train, y_train, cv=kfold, scoring=scoring)
        results.append(cv_results)
        names.append(name)
        msg = "%s: %f (%f)" % (name, cv_results.mean(), cv_results.std())
        print(msg)
        
    return names, results
    
class PlotBoxR(object):
    
    
    def __Trace(self,nameOfFeature,value): 
    
        trace = go.Box(
            y=value,
            name = nameOfFeature,
            marker = dict(
                color = 'rgb(0, 128, 128)',
            )
        )
        return trace

    def PlotResult(self,names,results):
        
        data = []

        for i in range(len(names)):
            data.append(self.__Trace(names[i],results[i]))


        py.iplot(data)
        
  
models = GetBasedModel()
names,results = BasedLine2(X_train, y_train,models)
PlotBoxR().PlotResult(names,results)

def HeatMap(df,x=True):
        correlations = df.corr()
        ## Create color map ranging between two colors
        cmap = sns.diverging_palette(220, 10, as_cmap=True)
        fig, ax = plt.subplots(figsize=(10, 10))
        fig = sns.heatmap(correlations, cmap=cmap, vmax=1.0, center=0, fmt='.2f',
                          square=True, linewidths=.5, annot=x, cbar_kws={"shrink": .75})
        fig.set_xticklabels(fig.get_xticklabels(), rotation = 90, fontsize = 10)
        fig.set_yticklabels(fig.get_yticklabels(), rotation = 0, fontsize = 10)
        plt.tight_layout()
        plt.show()

HeatMap(df,x=True)

sns.clustermap(X_scaled,
               method ='average',
               metric = 'euclidean',
               cmap='mako_r',linewidths=3,
               figsize = (15,40),annot=True
              )
              
sns.heatmap(X_scaled,cmap='mako_r',
           linewidths=.5,annot=True)
plt.figure(figsize = (10,50))
plt.show()

k = df['Bite Count'].nunique()
print(k)

from sklearn.cluster import KMeans
kcluster = KMeans(n_clusters=9)
kcluster.fit(X_scaled)

kcluster.labels_

df_clustered = df.copy()
df_clustered['klabel'] = kcluster.labels_
X_scaled_clustered = X_scaled.copy()
X_scaled_clustered['klabel'] = kcluster.labels_
df_clustered

clf = ExtraTreesClassifier(n_estimators=250,
                              random_state=SEED)

clf.fit(X_train, y_train)

# Plot feature importance
feature_importance = clf.feature_importances_

# Make importances relative to max importance
feature_importance = 100.0 * (feature_importance / feature_importance.max())
sorted_idx = np.argsort(feature_importance)
pos = np.arange(sorted_idx.shape[0]) + .5
plt.subplot(1, 2, 2)
plt.barh(pos, feature_importance[sorted_idx], align='center')
plt.yticks(pos, df.columns[sorted_idx])#boston.feature_names[sorted_idx])
plt.xlabel('Relative Importance')
plt.title('Variable Importance')
plt.show()


