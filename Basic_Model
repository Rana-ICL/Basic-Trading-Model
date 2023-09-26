import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# important do not forget check for correlation between apple and microsoft stock 

# reading apple and microsoft data files

apple_df=pd.read_csv("Apple_Data.csv")
microsoft_df=pd.read_csv("Microsoft_Data.csv")




##check missing values, duplicates outliers etc

#missing values
print(apple_df.isnull().sum())
print(microsoft_df.isnull().sum())

# duplicates
print("no of duplicates in apple data file",apple_df.duplicated().sum())
print("no of duplicates in microsoft data file",microsoft_df.duplicated().sum())

# check for outliers
sns.boxplot(data=apple_df[["Open","Close","Low","High"]])
plt.show()
sns.boxplot(data=microsoft_df[["Open","Close","Low","High"]])
plt.show()

#check for consistant data type
print(apple_df.dtypes)
print(microsoft_df.dtypes)




###clean the data

apple_df.drop_duplicates(inplace=True)
microsoft_df.drop_duplicates(inplace=True)

# check if there are still any other duplicates
print(apple_df.duplicated().sum())
print(microsoft_df.duplicated().sum())

# two methods one with interpolating the values and the other with dropping the nan values
# fill the missing nan with interpolation and extrapolation

#method drop nan vlaues
apple_df_drop_nan=apple_df.dropna()
microsoft_df_drop_nan=microsoft_df.dropna()

# 1.method interpolating/extrapolating nan values 
apple_df_interpolate=apple_df.interpolate(method="linear",direction="both")
microsoft_df_interpolate=microsoft_df.interpolate(method="linear",direction="both")

#2. method "droping nan"
apple_df_mean_dropNan=apple_df_drop_nan[["Open","High","Low","Close","Adj Close","Volume"]].mean()
apple_df_std_dropNan=apple_df_drop_nan[["Open","High","Low","Close","Adj Close","Volume"]].std()
microsoft_df_mean_dropNan=microsoft_df_drop_nan[["Open","High","Low","Close","Adj Close","Volume"]].mean()
microsoft_df_std_dropNan=microsoft_df_drop_nan[["Open","High","Low","Close","Adj Close","Volume"]].std()


## let's start with 2.method

# set the limits
uplimit_apple=apple_df_mean_dropNan + 2*apple_df_std_dropNan
lowlimit_apple=apple_df_mean_dropNan - 2*apple_df_std_dropNan
uplimit_microsoft=microsoft_df_mean_dropNan + 2*microsoft_df_std_dropNan
lowlimit_microsoft=microsoft_df_mean_dropNan - 2*microsoft_df_std_dropNan
print(uplimit_apple)


# removing the outliers

for x in ["Open", "High", "Low", "Close", "Adj Close", "Volume"]:
    apple_df_drop_nan = apple_df_drop_nan[
        (apple_df_drop_nan[x] <= uplimit_apple[x]) & 
        (apple_df_drop_nan[x] >= lowlimit_apple[x])
    ]

for x in ["Open", "High", "Low", "Close", "Adj Close", "Volume"]:
    microsoft_df_drop_nan = microsoft_df_drop_nan[
        (microsoft_df_drop_nan[x] <= uplimit_microsoft[x]) & 
        (microsoft_df_drop_nan[x] >= lowlimit_microsoft[x])
    ]
    
    
## check the outliers with a boxplot
sns.boxplot(data=apple_df_drop_nan[["Open","Close","Low","High"]])
plt.show()
sns.boxplot(data=microsoft_df_drop_nan[["Open","Close","Low","High"]])
plt.show()




## Trading Model based on weekly, monthly moving averaging

#calculate the returns (default is daily returns)

def stockReturns(data,offset=1):
    returns_calc=[]

    for x in range(0,len(data)-offset,1):
        retVal=(data.iloc[x]-data.iloc[x-offset])/data.iloc[x-offset]
        returns_calc.append(retVal)
    return np.array(returns_calc)


# weekly_returns
weekly_returns_apple=stockReturns(apple_df_drop_nan["Close"],5)
print("average apple weekly returns",weekly_returns_apple.mean())
weekly_returns_microsoft=stockReturns(microsoft_df_drop_nan["Close"],5)
print("average microsoft weekly returns",weekly_returns_microsoft.mean())

# monthly_returns        
monthly_returns_apple=stockReturns(apple_df_drop_nan["Close"],22)
print("average apple monthly returns",monthly_returns_apple.mean())
monthly_returns_microsoft=stockReturns(microsoft_df_drop_nan["Close"],22)
print("average microsoft mothly returns",monthly_returns_microsoft.mean())



### Trading Signal under development
