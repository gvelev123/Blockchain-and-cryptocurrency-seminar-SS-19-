```yaml
Name of QuantLet: Data Collection and Data Analysis

Published in: Statistical Tools for Finance and Insurance ??????

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

#Retrieve Data from BitFinex
def get_data_spec(coin, date, time_period):
    """ Query the API for the historical price data starting from "date". """
    url = "https://min-api.cryptocompare.com/data/{}?fsym={}&e=BitFinex&tsym=USD&toTs={}".format(time_period, coin, date)
    r = requests.get(url)
    ipdata = r.json()
    return ipdata
```
![Picture1](Stationary.png)
