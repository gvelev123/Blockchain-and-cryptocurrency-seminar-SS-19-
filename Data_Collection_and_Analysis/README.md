```yaml
Name of QuantLet: Data Collection and Data Analysis

Description: 'Retrieval of cryptocurrncy data via API call.'

Keywords: cryptocurrency, data, retrieval, analysis, API call

Author: Georg Velev, Iliyana Pekova

Submitted: Thu, July 25 2019 by Georg Velev, Iliyana Pekova

```

### Python Code
```python
#Custom Imports
import requests, pandas as pd, time, datetime, seaborn as sns, matplotlib.pyplot as plt, numpy import array, keras, numpy as np
from keras import optimizers
from keras.wrappers.scikit_learn import KerasClassifier
from keras.models import Sequential, Model
from keras.layers import Dense, Dropout, Activation, Flatten, Input, Bidirectional, RepeatVector, TimeDistributed
from keras.utils import np_utils
from keras.callbacks import EarlyStopping
from sklearn.model_selection import train_test_split
from keras.layers import LSTM

#This function establishes a request to the online plattform BitFinex:
def get_data_spec(coin, date, time_period):
    """ Query the API for the historical price data starting from "date". """
    url = "https://min-api.cryptocompare.com/data/{}?fsym={}&e=BitFinex&tsym=USD&toTs={}".format(time_period, coin, date)
    r = requests.get(url)
    ipdata = r.json()
    return ipdata

#This function collects the cryptocurrency data from for the specified time period: 
def get_df_spec(time_period, coin, from_date, to_date):
    """ Get historical price data between two dates. If further apart than query limit then query multiple times. """
    date = to_date
    holder = []
    while date > from_date:
        # Now we use the new function to query specific coins
        data = get_data_spec(coin, date, time_period) 
        holder.append(pd.DataFrame(data["Data"]))
        
        date = data['TimeFrom'] 
    df = pd.concat(holder, axis = 0)
    df = df[df['time']>from_date]
    df['time'] = pd.to_datetime(df['time'], unit='s') 
    df.set_index('time', inplace=True)
    df.sort_index(ascending=False, inplace=True)
    return df

#This function generates the average hourly prices from the opening and the closing price and uses the new feture to 
#compute the hourly returns:
def compute_Hourly_Returns(data):
    data["Average_hourly_price"]=(data["close"]+data["open"])/2
    data["Hourly_returns"]=data["Average_hourly_price"].divide(data["Average_hourly_price"].shift())-1
    data= data.iloc[1:] 
    return data
```
![Picture1](Stationary.png)
