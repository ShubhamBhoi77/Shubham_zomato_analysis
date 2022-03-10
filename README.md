# Shubham_zomato_analysis
I have worked on the Zomato dataset, for the practice purpose and also made visuals to understand the data better.

import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns


# To read the dataset.........
df=pd.read_csv("zomato.csv")
df

# to get the summary of the dataset.
df.info()

# check the null values in the columns
df.isna().sum()

df.drop_duplicates(inplace=True)
df.shape       ## After the removal of the duplicate values from the dataset we are left with the following number of rows and columns.

df['rate'].unique()

def ratings(value):
    if(value=='NEW' or value=='-'):
        return np.nan
    else:
        value=str(value).split('/')
        value=value[0]
        return float(value)
df['rate']=df['rate'].apply(ratings)
df['rate'].head()

df.rate.isnull().sum()  
#to find the total number of null values in the column

#to replace the null values with the mean value of the rate column.
df['rate'].fillna(df['rate'].mean(), inplace=True)
df.rate.isnull().sum()


df.phone.isnull().sum()

**Dropping the Null values**
df.dropna(inplace=True)
df.head()

**Analyzing the columns**

df.rename(columns={'approx_cost(for two people)':'2paltescost','reviews_list':'reviews',}, inplace=True)
df.head()
#Renaming the column names 

df['menu_item']= df['menu_item'].replace(['[]'], 'Nan')
df.head(10)
#replacing the values of a desired column.


df['location'].unique()

df['location'].value_counts()
#To obtain the maximum number of restaurants in a location.

df= df.drop(['listed_in(city)'], axis = 1 )

#to remove the comma from the column '2platescost' and showing only the float or int value.

def handlecomma(value):
    value=str(value)
    if ',' in value:
        value=value.replace(',','')
        return float(value)
    else:
        return float(value)
    df['2paltescost']= df['2paltescost'].apply(handlecomma)
    df['2platescost'].unique()
    
   **cleaning the 'rest type' column**
   df['rest_type'].value_counts()
   
   rest_types=df['rest_type'].value_counts(ascending=False)       #DOUBT
rest_types

rest_type_lessthan1000= rest_types[rest_types<1000]
rest_type_lessthan1000

**Making rest_type less than 1000 as others**

def rest(value):
    if(value in rest_type_lessthan1000):
        return 'others'
    else:
        return value
df['rest_type']=df['rest_type'].apply(rest)
df['rest_type'].value_counts()

df['listed_in(city)'].value_counts()

city=df['listed_in(city)'].value_counts(ascending=True)
city

cityU1000= city[city<1000]
cityU1000

def city(value):
    if(value in 'listed_in(city)'<1000):
        return 'others'
    else:
        return value
df['listed_in(city)']=df['listed_in(city)'].apply(city)
df['listed_in(city)'].value_counts() 

cuisines= df['cuisines'].value_counts(ascending= False)
cuisines_lessthan100= cuisines[cuisines<100]

def handle_cuisines(value):
    if (value in cuisines_lessthan100):
        return 'others'
    else:
        return value
df['cuisines']= df['cuisines'].apply(handle_cuisines)
df['cuisines'].value_counts()

**Data is clean, now let us visualize the data.
Plotting the graphs for better analysis.**![download](https://user-images.githubusercontent.com/98741836/157719128-621d80c2-e182-4edc-a6aa-fa9fcd83c094.png)


plt.figure(figsize = (16,10))
ax= sns.countplot(df['location'])
plt.xticks(rotation=90)
![download](https://user-images.githubusercontent.com/98741836/157719170-98695265-79dd-44cd-b2ba-ceeac899ee49.png)

**In the above graph, we can infer that the places where there is least competion is suitable for opening a retaurant business, if the competition is concerned.**

plt.figure(figsize=(10,10))
sns.countplot(df['online_order'], palette = 'inferno')
![download](https://user-images.githubusercontent.com/98741836/157719398-61645372-c519-4363-bd29-92301a7f9e5e.png)

**Out of the total data set, more than 30k offer online order facility and around 20k do not offer the the online order facilty.**

plt.figure(figsize=(10,10))
sns.co
![download](https://user-images.githubusercontent.com/98741836/157719532-f6275e78-b39e-4a2a-a0e2-cc79cafc6b2b.png)
untplot(df['book_table'],palette='rainbow')

**We as a restaurant business must consider the facilty of providing the booking of the table, to the customer.**
df1=df.groupby(['location','online_order'])['name'].count()
df1.to_csv('location_online.csv')
df1=pd.read_csv('location_online.csv')
df1= pd.pivot_table(df1,values=None, index=['location'],columns= ['online_order'], fill_value=0, aggfunc=np.sum)
df1

df1.plot(kind='bar', figsize=(15,10))



![download](https://user-images.githubusercontent.com/98741836/157719741-6da64ad5-f415-4d41-baa7-c4f3d293ec6b.png)




   
