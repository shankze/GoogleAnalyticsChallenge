

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
from pandas.io.json import json_normalize
import json
%matplotlib inline
```


```python
#%reset -f
#del train
```


```python
json_cols = ['device', 'geoNetwork', 'totals', 'trafficSource']
json_conv = {col: json.loads for col in (json_cols)}
train = pd.read_csv("train.csv",
                    #nrows = 10000,
                    dtype={'fullVisitorId': str},
                    converters={'device': json.loads,
                               'geoNetwork': json.loads,
                               'totals': json.loads,
                               'trafficSource': json.loads,
                              })
```


```python
train.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>channelGrouping</th>
      <th>date</th>
      <th>device</th>
      <th>fullVisitorId</th>
      <th>geoNetwork</th>
      <th>sessionId</th>
      <th>socialEngagementType</th>
      <th>totals</th>
      <th>trafficSource</th>
      <th>visitId</th>
      <th>visitNumber</th>
      <th>visitStartTime</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Organic Search</td>
      <td>20160902</td>
      <td>{'browser': 'Chrome', 'browserVersion': 'not a...</td>
      <td>1131660440785968503</td>
      <td>{'continent': 'Asia', 'subContinent': 'Western...</td>
      <td>1131660440785968503_1472830385</td>
      <td>Not Socially Engaged</td>
      <td>{'visits': '1', 'hits': '1', 'pageviews': '1',...</td>
      <td>{'campaign': '(not set)', 'source': 'google', ...</td>
      <td>1472830385</td>
      <td>1</td>
      <td>1472830385</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Organic Search</td>
      <td>20160902</td>
      <td>{'browser': 'Firefox', 'browserVersion': 'not ...</td>
      <td>377306020877927890</td>
      <td>{'continent': 'Oceania', 'subContinent': 'Aust...</td>
      <td>377306020877927890_1472880147</td>
      <td>Not Socially Engaged</td>
      <td>{'visits': '1', 'hits': '1', 'pageviews': '1',...</td>
      <td>{'campaign': '(not set)', 'source': 'google', ...</td>
      <td>1472880147</td>
      <td>1</td>
      <td>1472880147</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Organic Search</td>
      <td>20160902</td>
      <td>{'browser': 'Chrome', 'browserVersion': 'not a...</td>
      <td>3895546263509774583</td>
      <td>{'continent': 'Europe', 'subContinent': 'South...</td>
      <td>3895546263509774583_1472865386</td>
      <td>Not Socially Engaged</td>
      <td>{'visits': '1', 'hits': '1', 'pageviews': '1',...</td>
      <td>{'campaign': '(not set)', 'source': 'google', ...</td>
      <td>1472865386</td>
      <td>1</td>
      <td>1472865386</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Organic Search</td>
      <td>20160902</td>
      <td>{'browser': 'UC Browser', 'browserVersion': 'n...</td>
      <td>4763447161404445595</td>
      <td>{'continent': 'Asia', 'subContinent': 'Southea...</td>
      <td>4763447161404445595_1472881213</td>
      <td>Not Socially Engaged</td>
      <td>{'visits': '1', 'hits': '1', 'pageviews': '1',...</td>
      <td>{'campaign': '(not set)', 'source': 'google', ...</td>
      <td>1472881213</td>
      <td>1</td>
      <td>1472881213</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Organic Search</td>
      <td>20160902</td>
      <td>{'browser': 'Chrome', 'browserVersion': 'not a...</td>
      <td>27294437909732085</td>
      <td>{'continent': 'Europe', 'subContinent': 'North...</td>
      <td>27294437909732085_1472822600</td>
      <td>Not Socially Engaged</td>
      <td>{'visits': '1', 'hits': '1', 'pageviews': '1',...</td>
      <td>{'campaign': '(not set)', 'source': 'google', ...</td>
      <td>1472822600</td>
      <td>2</td>
      <td>1472822600</td>
    </tr>
  </tbody>
</table>
</div>




```python
train.describe()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>date</th>
      <th>visitId</th>
      <th>visitNumber</th>
      <th>visitStartTime</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>9.036530e+05</td>
      <td>9.036530e+05</td>
      <td>903653.000000</td>
      <td>9.036530e+05</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>2.016589e+07</td>
      <td>1.485007e+09</td>
      <td>2.264897</td>
      <td>1.485007e+09</td>
    </tr>
    <tr>
      <th>std</th>
      <td>4.697698e+03</td>
      <td>9.022124e+06</td>
      <td>9.283735</td>
      <td>9.022124e+06</td>
    </tr>
    <tr>
      <th>min</th>
      <td>2.016080e+07</td>
      <td>1.470035e+09</td>
      <td>1.000000</td>
      <td>1.470035e+09</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>2.016103e+07</td>
      <td>1.477561e+09</td>
      <td>1.000000</td>
      <td>1.477561e+09</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>2.017011e+07</td>
      <td>1.483949e+09</td>
      <td>1.000000</td>
      <td>1.483949e+09</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>2.017042e+07</td>
      <td>1.492759e+09</td>
      <td>1.000000</td>
      <td>1.492759e+09</td>
    </tr>
    <tr>
      <th>max</th>
      <td>2.017080e+07</td>
      <td>1.501657e+09</td>
      <td>395.000000</td>
      <td>1.501657e+09</td>
    </tr>
  </tbody>
</table>
</div>




```python
train.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 903653 entries, 0 to 903652
    Data columns (total 12 columns):
    channelGrouping         903653 non-null object
    date                    903653 non-null int64
    device                  903653 non-null object
    fullVisitorId           903653 non-null object
    geoNetwork              903653 non-null object
    sessionId               903653 non-null object
    socialEngagementType    903653 non-null object
    totals                  903653 non-null object
    trafficSource           903653 non-null object
    visitId                 903653 non-null int64
    visitNumber             903653 non-null int64
    visitStartTime          903653 non-null int64
    dtypes: int64(4), object(8)
    memory usage: 82.7+ MB
    


```python
def extractJsonColumns(df):
    for col in json_cols:
        print('Working on :' + col)
        jsonCol = json_normalize(df[col].tolist())
        jsonCol.columns = [col+'_'+jcol for jcol in jsonCol.columns]
        df = df.merge(jsonCol,left_index=True,right_index=True)
        df.drop(col,axis=1,inplace=True)
    return(df)
```


```python
train = extractJsonColumns(train)
train.columns
```

    Working on :device
    Working on :geoNetwork
    Working on :totals
    Working on :trafficSource
    




    Index(['channelGrouping', 'date', 'fullVisitorId', 'sessionId',
           'socialEngagementType', 'visitId', 'visitNumber', 'visitStartTime',
           'device_browser', 'device_browserSize', 'device_browserVersion',
           'device_deviceCategory', 'device_flashVersion', 'device_isMobile',
           'device_language', 'device_mobileDeviceBranding',
           'device_mobileDeviceInfo', 'device_mobileDeviceMarketingName',
           'device_mobileDeviceModel', 'device_mobileInputSelector',
           'device_operatingSystem', 'device_operatingSystemVersion',
           'device_screenColors', 'device_screenResolution', 'geoNetwork_city',
           'geoNetwork_cityId', 'geoNetwork_continent', 'geoNetwork_country',
           'geoNetwork_latitude', 'geoNetwork_longitude', 'geoNetwork_metro',
           'geoNetwork_networkDomain', 'geoNetwork_networkLocation',
           'geoNetwork_region', 'geoNetwork_subContinent', 'totals_bounces',
           'totals_hits', 'totals_newVisits', 'totals_pageviews',
           'totals_transactionRevenue', 'totals_visits', 'trafficSource_adContent',
           'trafficSource_adwordsClickInfo.adNetworkType',
           'trafficSource_adwordsClickInfo.criteriaParameters',
           'trafficSource_adwordsClickInfo.gclId',
           'trafficSource_adwordsClickInfo.isVideoAd',
           'trafficSource_adwordsClickInfo.page',
           'trafficSource_adwordsClickInfo.slot', 'trafficSource_campaign',
           'trafficSource_campaignCode', 'trafficSource_isTrueDirect',
           'trafficSource_keyword', 'trafficSource_medium',
           'trafficSource_referralPath', 'trafficSource_source'],
          dtype='object')




```python
len(train)
```




    903653




```python
def generateColumnInfo(df):
    cls = []
    nullCount = []
    nonNullCount = []
    nullsPct = []
    uniqCount = []
    dataType = []
    for i,col in enumerate(df.columns):
        cls.append(col)
        nullCount.append(df[col].isnull().sum())
        nonNullCount.append(len(df)-df[col].isnull().sum())
        nullsPct.append((df[col].isnull().sum())*(100)/len(df))
        uniqCount.append(df[col].nunique())
        dataType.append(df[col].dtype)
        
    column_info = pd.DataFrame(
        {'ColumnName': cls,
         'NullCount': nullCount,
         'NonNullCount': nonNullCount,
         'NullPercent': nullsPct,
         'UniqueValueCount': uniqCount,
         'DataType':dataType
        })
    return(column_info)
```


```python
generateColumnInfo(train)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>ColumnName</th>
      <th>NullCount</th>
      <th>NonNullCount</th>
      <th>NullPercent</th>
      <th>UniqueValueCount</th>
      <th>DataType</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>channelGrouping</td>
      <td>0</td>
      <td>903653</td>
      <td>0.000000</td>
      <td>8</td>
      <td>object</td>
    </tr>
    <tr>
      <th>1</th>
      <td>date</td>
      <td>0</td>
      <td>903653</td>
      <td>0.000000</td>
      <td>366</td>
      <td>int64</td>
    </tr>
    <tr>
      <th>2</th>
      <td>fullVisitorId</td>
      <td>0</td>
      <td>903653</td>
      <td>0.000000</td>
      <td>714167</td>
      <td>object</td>
    </tr>
    <tr>
      <th>3</th>
      <td>sessionId</td>
      <td>0</td>
      <td>903653</td>
      <td>0.000000</td>
      <td>902755</td>
      <td>object</td>
    </tr>
    <tr>
      <th>4</th>
      <td>socialEngagementType</td>
      <td>0</td>
      <td>903653</td>
      <td>0.000000</td>
      <td>1</td>
      <td>object</td>
    </tr>
    <tr>
      <th>5</th>
      <td>visitId</td>
      <td>0</td>
      <td>903653</td>
      <td>0.000000</td>
      <td>886303</td>
      <td>int64</td>
    </tr>
    <tr>
      <th>6</th>
      <td>visitNumber</td>
      <td>0</td>
      <td>903653</td>
      <td>0.000000</td>
      <td>384</td>
      <td>int64</td>
    </tr>
    <tr>
      <th>7</th>
      <td>visitStartTime</td>
      <td>0</td>
      <td>903653</td>
      <td>0.000000</td>
      <td>887159</td>
      <td>int64</td>
    </tr>
    <tr>
      <th>8</th>
      <td>device_browser</td>
      <td>0</td>
      <td>903653</td>
      <td>0.000000</td>
      <td>54</td>
      <td>object</td>
    </tr>
    <tr>
      <th>9</th>
      <td>device_browserSize</td>
      <td>0</td>
      <td>903653</td>
      <td>0.000000</td>
      <td>1</td>
      <td>object</td>
    </tr>
    <tr>
      <th>10</th>
      <td>device_browserVersion</td>
      <td>0</td>
      <td>903653</td>
      <td>0.000000</td>
      <td>1</td>
      <td>object</td>
    </tr>
    <tr>
      <th>11</th>
      <td>device_deviceCategory</td>
      <td>0</td>
      <td>903653</td>
      <td>0.000000</td>
      <td>3</td>
      <td>object</td>
    </tr>
    <tr>
      <th>12</th>
      <td>device_flashVersion</td>
      <td>0</td>
      <td>903653</td>
      <td>0.000000</td>
      <td>1</td>
      <td>object</td>
    </tr>
    <tr>
      <th>13</th>
      <td>device_isMobile</td>
      <td>0</td>
      <td>903653</td>
      <td>0.000000</td>
      <td>2</td>
      <td>bool</td>
    </tr>
    <tr>
      <th>14</th>
      <td>device_language</td>
      <td>0</td>
      <td>903653</td>
      <td>0.000000</td>
      <td>1</td>
      <td>object</td>
    </tr>
    <tr>
      <th>15</th>
      <td>device_mobileDeviceBranding</td>
      <td>0</td>
      <td>903653</td>
      <td>0.000000</td>
      <td>1</td>
      <td>object</td>
    </tr>
    <tr>
      <th>16</th>
      <td>device_mobileDeviceInfo</td>
      <td>0</td>
      <td>903653</td>
      <td>0.000000</td>
      <td>1</td>
      <td>object</td>
    </tr>
    <tr>
      <th>17</th>
      <td>device_mobileDeviceMarketingName</td>
      <td>0</td>
      <td>903653</td>
      <td>0.000000</td>
      <td>1</td>
      <td>object</td>
    </tr>
    <tr>
      <th>18</th>
      <td>device_mobileDeviceModel</td>
      <td>0</td>
      <td>903653</td>
      <td>0.000000</td>
      <td>1</td>
      <td>object</td>
    </tr>
    <tr>
      <th>19</th>
      <td>device_mobileInputSelector</td>
      <td>0</td>
      <td>903653</td>
      <td>0.000000</td>
      <td>1</td>
      <td>object</td>
    </tr>
    <tr>
      <th>20</th>
      <td>device_operatingSystem</td>
      <td>0</td>
      <td>903653</td>
      <td>0.000000</td>
      <td>20</td>
      <td>object</td>
    </tr>
    <tr>
      <th>21</th>
      <td>device_operatingSystemVersion</td>
      <td>0</td>
      <td>903653</td>
      <td>0.000000</td>
      <td>1</td>
      <td>object</td>
    </tr>
    <tr>
      <th>22</th>
      <td>device_screenColors</td>
      <td>0</td>
      <td>903653</td>
      <td>0.000000</td>
      <td>1</td>
      <td>object</td>
    </tr>
    <tr>
      <th>23</th>
      <td>device_screenResolution</td>
      <td>0</td>
      <td>903653</td>
      <td>0.000000</td>
      <td>1</td>
      <td>object</td>
    </tr>
    <tr>
      <th>24</th>
      <td>geoNetwork_city</td>
      <td>0</td>
      <td>903653</td>
      <td>0.000000</td>
      <td>649</td>
      <td>object</td>
    </tr>
    <tr>
      <th>25</th>
      <td>geoNetwork_cityId</td>
      <td>0</td>
      <td>903653</td>
      <td>0.000000</td>
      <td>1</td>
      <td>object</td>
    </tr>
    <tr>
      <th>26</th>
      <td>geoNetwork_continent</td>
      <td>0</td>
      <td>903653</td>
      <td>0.000000</td>
      <td>6</td>
      <td>object</td>
    </tr>
    <tr>
      <th>27</th>
      <td>geoNetwork_country</td>
      <td>0</td>
      <td>903653</td>
      <td>0.000000</td>
      <td>222</td>
      <td>object</td>
    </tr>
    <tr>
      <th>28</th>
      <td>geoNetwork_latitude</td>
      <td>0</td>
      <td>903653</td>
      <td>0.000000</td>
      <td>1</td>
      <td>object</td>
    </tr>
    <tr>
      <th>29</th>
      <td>geoNetwork_longitude</td>
      <td>0</td>
      <td>903653</td>
      <td>0.000000</td>
      <td>1</td>
      <td>object</td>
    </tr>
    <tr>
      <th>30</th>
      <td>geoNetwork_metro</td>
      <td>0</td>
      <td>903653</td>
      <td>0.000000</td>
      <td>94</td>
      <td>object</td>
    </tr>
    <tr>
      <th>31</th>
      <td>geoNetwork_networkDomain</td>
      <td>0</td>
      <td>903653</td>
      <td>0.000000</td>
      <td>28064</td>
      <td>object</td>
    </tr>
    <tr>
      <th>32</th>
      <td>geoNetwork_networkLocation</td>
      <td>0</td>
      <td>903653</td>
      <td>0.000000</td>
      <td>1</td>
      <td>object</td>
    </tr>
    <tr>
      <th>33</th>
      <td>geoNetwork_region</td>
      <td>0</td>
      <td>903653</td>
      <td>0.000000</td>
      <td>376</td>
      <td>object</td>
    </tr>
    <tr>
      <th>34</th>
      <td>geoNetwork_subContinent</td>
      <td>0</td>
      <td>903653</td>
      <td>0.000000</td>
      <td>23</td>
      <td>object</td>
    </tr>
    <tr>
      <th>35</th>
      <td>totals_bounces</td>
      <td>453023</td>
      <td>450630</td>
      <td>50.132407</td>
      <td>1</td>
      <td>object</td>
    </tr>
    <tr>
      <th>36</th>
      <td>totals_hits</td>
      <td>0</td>
      <td>903653</td>
      <td>0.000000</td>
      <td>274</td>
      <td>object</td>
    </tr>
    <tr>
      <th>37</th>
      <td>totals_newVisits</td>
      <td>200593</td>
      <td>703060</td>
      <td>22.198012</td>
      <td>1</td>
      <td>object</td>
    </tr>
    <tr>
      <th>38</th>
      <td>totals_pageviews</td>
      <td>100</td>
      <td>903553</td>
      <td>0.011066</td>
      <td>213</td>
      <td>object</td>
    </tr>
    <tr>
      <th>39</th>
      <td>totals_transactionRevenue</td>
      <td>892138</td>
      <td>11515</td>
      <td>98.725728</td>
      <td>5332</td>
      <td>object</td>
    </tr>
    <tr>
      <th>40</th>
      <td>totals_visits</td>
      <td>0</td>
      <td>903653</td>
      <td>0.000000</td>
      <td>1</td>
      <td>object</td>
    </tr>
    <tr>
      <th>41</th>
      <td>trafficSource_adContent</td>
      <td>892707</td>
      <td>10946</td>
      <td>98.788694</td>
      <td>44</td>
      <td>object</td>
    </tr>
    <tr>
      <th>42</th>
      <td>trafficSource_adwordsClickInfo.adNetworkType</td>
      <td>882193</td>
      <td>21460</td>
      <td>97.625195</td>
      <td>2</td>
      <td>object</td>
    </tr>
    <tr>
      <th>43</th>
      <td>trafficSource_adwordsClickInfo.criteriaParameters</td>
      <td>0</td>
      <td>903653</td>
      <td>0.000000</td>
      <td>1</td>
      <td>object</td>
    </tr>
    <tr>
      <th>44</th>
      <td>trafficSource_adwordsClickInfo.gclId</td>
      <td>882092</td>
      <td>21561</td>
      <td>97.614018</td>
      <td>17774</td>
      <td>object</td>
    </tr>
    <tr>
      <th>45</th>
      <td>trafficSource_adwordsClickInfo.isVideoAd</td>
      <td>882193</td>
      <td>21460</td>
      <td>97.625195</td>
      <td>1</td>
      <td>object</td>
    </tr>
    <tr>
      <th>46</th>
      <td>trafficSource_adwordsClickInfo.page</td>
      <td>882193</td>
      <td>21460</td>
      <td>97.625195</td>
      <td>8</td>
      <td>object</td>
    </tr>
    <tr>
      <th>47</th>
      <td>trafficSource_adwordsClickInfo.slot</td>
      <td>882193</td>
      <td>21460</td>
      <td>97.625195</td>
      <td>2</td>
      <td>object</td>
    </tr>
    <tr>
      <th>48</th>
      <td>trafficSource_campaign</td>
      <td>0</td>
      <td>903653</td>
      <td>0.000000</td>
      <td>10</td>
      <td>object</td>
    </tr>
    <tr>
      <th>49</th>
      <td>trafficSource_campaignCode</td>
      <td>903652</td>
      <td>1</td>
      <td>99.999889</td>
      <td>1</td>
      <td>object</td>
    </tr>
    <tr>
      <th>50</th>
      <td>trafficSource_isTrueDirect</td>
      <td>629648</td>
      <td>274005</td>
      <td>69.678073</td>
      <td>1</td>
      <td>object</td>
    </tr>
    <tr>
      <th>51</th>
      <td>trafficSource_keyword</td>
      <td>502929</td>
      <td>400724</td>
      <td>55.655102</td>
      <td>3659</td>
      <td>object</td>
    </tr>
    <tr>
      <th>52</th>
      <td>trafficSource_medium</td>
      <td>0</td>
      <td>903653</td>
      <td>0.000000</td>
      <td>7</td>
      <td>object</td>
    </tr>
    <tr>
      <th>53</th>
      <td>trafficSource_referralPath</td>
      <td>572712</td>
      <td>330941</td>
      <td>63.377425</td>
      <td>1475</td>
      <td>object</td>
    </tr>
    <tr>
      <th>54</th>
      <td>trafficSource_source</td>
      <td>0</td>
      <td>903653</td>
      <td>0.000000</td>
      <td>380</td>
      <td>object</td>
    </tr>
  </tbody>
</table>
</div>



### Test


```python
json_cols = ['device', 'geoNetwork', 'totals', 'trafficSource']
json_conv = {col: json.loads for col in (json_cols)}
test = pd.read_csv("test.csv",
                    #nrows = 10000,
                    dtype={'fullVisitorId': str},
                    converters={'device': json.loads,
                               'geoNetwork': json.loads,
                               'totals': json.loads,
                               'trafficSource': json.loads,
                              })
```


```python
len(test)
```




    804684




```python
test.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>channelGrouping</th>
      <th>date</th>
      <th>device</th>
      <th>fullVisitorId</th>
      <th>geoNetwork</th>
      <th>sessionId</th>
      <th>socialEngagementType</th>
      <th>totals</th>
      <th>trafficSource</th>
      <th>visitId</th>
      <th>visitNumber</th>
      <th>visitStartTime</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Organic Search</td>
      <td>20171016</td>
      <td>{'browser': 'Chrome', 'browserVersion': 'not a...</td>
      <td>6167871330617112363</td>
      <td>{'continent': 'Asia', 'subContinent': 'Southea...</td>
      <td>6167871330617112363_1508151024</td>
      <td>Not Socially Engaged</td>
      <td>{'visits': '1', 'hits': '4', 'pageviews': '4'}</td>
      <td>{'campaign': '(not set)', 'source': 'google', ...</td>
      <td>1508151024</td>
      <td>2</td>
      <td>1508151024</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Organic Search</td>
      <td>20171016</td>
      <td>{'browser': 'Chrome', 'browserVersion': 'not a...</td>
      <td>0643697640977915618</td>
      <td>{'continent': 'Europe', 'subContinent': 'South...</td>
      <td>0643697640977915618_1508175522</td>
      <td>Not Socially Engaged</td>
      <td>{'visits': '1', 'hits': '5', 'pageviews': '5',...</td>
      <td>{'campaign': '(not set)', 'source': 'google', ...</td>
      <td>1508175522</td>
      <td>1</td>
      <td>1508175522</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Organic Search</td>
      <td>20171016</td>
      <td>{'browser': 'Chrome', 'browserVersion': 'not a...</td>
      <td>6059383810968229466</td>
      <td>{'continent': 'Europe', 'subContinent': 'Weste...</td>
      <td>6059383810968229466_1508143220</td>
      <td>Not Socially Engaged</td>
      <td>{'visits': '1', 'hits': '7', 'pageviews': '7',...</td>
      <td>{'campaign': '(not set)', 'source': 'google', ...</td>
      <td>1508143220</td>
      <td>1</td>
      <td>1508143220</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Organic Search</td>
      <td>20171016</td>
      <td>{'browser': 'Safari', 'browserVersion': 'not a...</td>
      <td>2376720078563423631</td>
      <td>{'continent': 'Americas', 'subContinent': 'Nor...</td>
      <td>2376720078563423631_1508193530</td>
      <td>Not Socially Engaged</td>
      <td>{'visits': '1', 'hits': '8', 'pageviews': '4',...</td>
      <td>{'campaign': '(not set)', 'source': 'google', ...</td>
      <td>1508193530</td>
      <td>1</td>
      <td>1508193530</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Organic Search</td>
      <td>20171016</td>
      <td>{'browser': 'Safari', 'browserVersion': 'not a...</td>
      <td>2314544520795440038</td>
      <td>{'continent': 'Americas', 'subContinent': 'Nor...</td>
      <td>2314544520795440038_1508217442</td>
      <td>Not Socially Engaged</td>
      <td>{'visits': '1', 'hits': '9', 'pageviews': '4',...</td>
      <td>{'campaign': '(not set)', 'source': 'google', ...</td>
      <td>1508217442</td>
      <td>1</td>
      <td>1508217442</td>
    </tr>
  </tbody>
</table>
</div>




```python
test = extractJsonColumns(test)
test.columns
```

    Working on :device
    Working on :geoNetwork
    Working on :totals
    Working on :trafficSource
    




    Index(['channelGrouping', 'date', 'fullVisitorId', 'sessionId',
           'socialEngagementType', 'visitId', 'visitNumber', 'visitStartTime',
           'device_browser', 'device_browserSize', 'device_browserVersion',
           'device_deviceCategory', 'device_flashVersion', 'device_isMobile',
           'device_language', 'device_mobileDeviceBranding',
           'device_mobileDeviceInfo', 'device_mobileDeviceMarketingName',
           'device_mobileDeviceModel', 'device_mobileInputSelector',
           'device_operatingSystem', 'device_operatingSystemVersion',
           'device_screenColors', 'device_screenResolution', 'geoNetwork_city',
           'geoNetwork_cityId', 'geoNetwork_continent', 'geoNetwork_country',
           'geoNetwork_latitude', 'geoNetwork_longitude', 'geoNetwork_metro',
           'geoNetwork_networkDomain', 'geoNetwork_networkLocation',
           'geoNetwork_region', 'geoNetwork_subContinent', 'totals_bounces',
           'totals_hits', 'totals_newVisits', 'totals_pageviews', 'totals_visits',
           'trafficSource_adContent',
           'trafficSource_adwordsClickInfo.adNetworkType',
           'trafficSource_adwordsClickInfo.criteriaParameters',
           'trafficSource_adwordsClickInfo.gclId',
           'trafficSource_adwordsClickInfo.isVideoAd',
           'trafficSource_adwordsClickInfo.page',
           'trafficSource_adwordsClickInfo.slot', 'trafficSource_campaign',
           'trafficSource_isTrueDirect', 'trafficSource_keyword',
           'trafficSource_medium', 'trafficSource_referralPath',
           'trafficSource_source'],
          dtype='object')




```python
generateColumnInfo(test)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>ColumnName</th>
      <th>NullCount</th>
      <th>NonNullCount</th>
      <th>NullPercent</th>
      <th>UniqueValueCount</th>
      <th>DataType</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>channelGrouping</td>
      <td>0</td>
      <td>804684</td>
      <td>0.000000</td>
      <td>8</td>
      <td>object</td>
    </tr>
    <tr>
      <th>1</th>
      <td>date</td>
      <td>0</td>
      <td>804684</td>
      <td>0.000000</td>
      <td>272</td>
      <td>int64</td>
    </tr>
    <tr>
      <th>2</th>
      <td>fullVisitorId</td>
      <td>0</td>
      <td>804684</td>
      <td>0.000000</td>
      <td>617242</td>
      <td>object</td>
    </tr>
    <tr>
      <th>3</th>
      <td>sessionId</td>
      <td>0</td>
      <td>804684</td>
      <td>0.000000</td>
      <td>803863</td>
      <td>object</td>
    </tr>
    <tr>
      <th>4</th>
      <td>socialEngagementType</td>
      <td>0</td>
      <td>804684</td>
      <td>0.000000</td>
      <td>1</td>
      <td>object</td>
    </tr>
    <tr>
      <th>5</th>
      <td>visitId</td>
      <td>0</td>
      <td>804684</td>
      <td>0.000000</td>
      <td>779504</td>
      <td>int64</td>
    </tr>
    <tr>
      <th>6</th>
      <td>visitNumber</td>
      <td>0</td>
      <td>804684</td>
      <td>0.000000</td>
      <td>446</td>
      <td>int64</td>
    </tr>
    <tr>
      <th>7</th>
      <td>visitStartTime</td>
      <td>0</td>
      <td>804684</td>
      <td>0.000000</td>
      <td>780264</td>
      <td>int64</td>
    </tr>
    <tr>
      <th>8</th>
      <td>device_browser</td>
      <td>0</td>
      <td>804684</td>
      <td>0.000000</td>
      <td>109</td>
      <td>object</td>
    </tr>
    <tr>
      <th>9</th>
      <td>device_browserSize</td>
      <td>0</td>
      <td>804684</td>
      <td>0.000000</td>
      <td>1</td>
      <td>object</td>
    </tr>
    <tr>
      <th>10</th>
      <td>device_browserVersion</td>
      <td>0</td>
      <td>804684</td>
      <td>0.000000</td>
      <td>1</td>
      <td>object</td>
    </tr>
    <tr>
      <th>11</th>
      <td>device_deviceCategory</td>
      <td>0</td>
      <td>804684</td>
      <td>0.000000</td>
      <td>3</td>
      <td>object</td>
    </tr>
    <tr>
      <th>12</th>
      <td>device_flashVersion</td>
      <td>0</td>
      <td>804684</td>
      <td>0.000000</td>
      <td>1</td>
      <td>object</td>
    </tr>
    <tr>
      <th>13</th>
      <td>device_isMobile</td>
      <td>0</td>
      <td>804684</td>
      <td>0.000000</td>
      <td>2</td>
      <td>bool</td>
    </tr>
    <tr>
      <th>14</th>
      <td>device_language</td>
      <td>0</td>
      <td>804684</td>
      <td>0.000000</td>
      <td>1</td>
      <td>object</td>
    </tr>
    <tr>
      <th>15</th>
      <td>device_mobileDeviceBranding</td>
      <td>0</td>
      <td>804684</td>
      <td>0.000000</td>
      <td>1</td>
      <td>object</td>
    </tr>
    <tr>
      <th>16</th>
      <td>device_mobileDeviceInfo</td>
      <td>0</td>
      <td>804684</td>
      <td>0.000000</td>
      <td>1</td>
      <td>object</td>
    </tr>
    <tr>
      <th>17</th>
      <td>device_mobileDeviceMarketingName</td>
      <td>0</td>
      <td>804684</td>
      <td>0.000000</td>
      <td>1</td>
      <td>object</td>
    </tr>
    <tr>
      <th>18</th>
      <td>device_mobileDeviceModel</td>
      <td>0</td>
      <td>804684</td>
      <td>0.000000</td>
      <td>1</td>
      <td>object</td>
    </tr>
    <tr>
      <th>19</th>
      <td>device_mobileInputSelector</td>
      <td>0</td>
      <td>804684</td>
      <td>0.000000</td>
      <td>1</td>
      <td>object</td>
    </tr>
    <tr>
      <th>20</th>
      <td>device_operatingSystem</td>
      <td>0</td>
      <td>804684</td>
      <td>0.000000</td>
      <td>22</td>
      <td>object</td>
    </tr>
    <tr>
      <th>21</th>
      <td>device_operatingSystemVersion</td>
      <td>0</td>
      <td>804684</td>
      <td>0.000000</td>
      <td>1</td>
      <td>object</td>
    </tr>
    <tr>
      <th>22</th>
      <td>device_screenColors</td>
      <td>0</td>
      <td>804684</td>
      <td>0.000000</td>
      <td>1</td>
      <td>object</td>
    </tr>
    <tr>
      <th>23</th>
      <td>device_screenResolution</td>
      <td>0</td>
      <td>804684</td>
      <td>0.000000</td>
      <td>1</td>
      <td>object</td>
    </tr>
    <tr>
      <th>24</th>
      <td>geoNetwork_city</td>
      <td>0</td>
      <td>804684</td>
      <td>0.000000</td>
      <td>732</td>
      <td>object</td>
    </tr>
    <tr>
      <th>25</th>
      <td>geoNetwork_cityId</td>
      <td>0</td>
      <td>804684</td>
      <td>0.000000</td>
      <td>1</td>
      <td>object</td>
    </tr>
    <tr>
      <th>26</th>
      <td>geoNetwork_continent</td>
      <td>0</td>
      <td>804684</td>
      <td>0.000000</td>
      <td>6</td>
      <td>object</td>
    </tr>
    <tr>
      <th>27</th>
      <td>geoNetwork_country</td>
      <td>0</td>
      <td>804684</td>
      <td>0.000000</td>
      <td>219</td>
      <td>object</td>
    </tr>
    <tr>
      <th>28</th>
      <td>geoNetwork_latitude</td>
      <td>0</td>
      <td>804684</td>
      <td>0.000000</td>
      <td>1</td>
      <td>object</td>
    </tr>
    <tr>
      <th>29</th>
      <td>geoNetwork_longitude</td>
      <td>0</td>
      <td>804684</td>
      <td>0.000000</td>
      <td>1</td>
      <td>object</td>
    </tr>
    <tr>
      <th>30</th>
      <td>geoNetwork_metro</td>
      <td>0</td>
      <td>804684</td>
      <td>0.000000</td>
      <td>109</td>
      <td>object</td>
    </tr>
    <tr>
      <th>31</th>
      <td>geoNetwork_networkDomain</td>
      <td>0</td>
      <td>804684</td>
      <td>0.000000</td>
      <td>25750</td>
      <td>object</td>
    </tr>
    <tr>
      <th>32</th>
      <td>geoNetwork_networkLocation</td>
      <td>0</td>
      <td>804684</td>
      <td>0.000000</td>
      <td>1</td>
      <td>object</td>
    </tr>
    <tr>
      <th>33</th>
      <td>geoNetwork_region</td>
      <td>0</td>
      <td>804684</td>
      <td>0.000000</td>
      <td>376</td>
      <td>object</td>
    </tr>
    <tr>
      <th>34</th>
      <td>geoNetwork_subContinent</td>
      <td>0</td>
      <td>804684</td>
      <td>0.000000</td>
      <td>23</td>
      <td>object</td>
    </tr>
    <tr>
      <th>35</th>
      <td>totals_bounces</td>
      <td>383736</td>
      <td>420948</td>
      <td>47.687788</td>
      <td>1</td>
      <td>object</td>
    </tr>
    <tr>
      <th>36</th>
      <td>totals_hits</td>
      <td>0</td>
      <td>804684</td>
      <td>0.000000</td>
      <td>229</td>
      <td>object</td>
    </tr>
    <tr>
      <th>37</th>
      <td>totals_newVisits</td>
      <td>200314</td>
      <td>604370</td>
      <td>24.893499</td>
      <td>1</td>
      <td>object</td>
    </tr>
    <tr>
      <th>38</th>
      <td>totals_pageviews</td>
      <td>139</td>
      <td>804545</td>
      <td>0.017274</td>
      <td>160</td>
      <td>object</td>
    </tr>
    <tr>
      <th>39</th>
      <td>totals_visits</td>
      <td>0</td>
      <td>804684</td>
      <td>0.000000</td>
      <td>1</td>
      <td>object</td>
    </tr>
    <tr>
      <th>40</th>
      <td>trafficSource_adContent</td>
      <td>750893</td>
      <td>53791</td>
      <td>93.315264</td>
      <td>51</td>
      <td>object</td>
    </tr>
    <tr>
      <th>41</th>
      <td>trafficSource_adwordsClickInfo.adNetworkType</td>
      <td>750870</td>
      <td>53814</td>
      <td>93.312406</td>
      <td>3</td>
      <td>object</td>
    </tr>
    <tr>
      <th>42</th>
      <td>trafficSource_adwordsClickInfo.criteriaParameters</td>
      <td>0</td>
      <td>804684</td>
      <td>0.000000</td>
      <td>1</td>
      <td>object</td>
    </tr>
    <tr>
      <th>43</th>
      <td>trafficSource_adwordsClickInfo.gclId</td>
      <td>750822</td>
      <td>53862</td>
      <td>93.306441</td>
      <td>41317</td>
      <td>object</td>
    </tr>
    <tr>
      <th>44</th>
      <td>trafficSource_adwordsClickInfo.isVideoAd</td>
      <td>750870</td>
      <td>53814</td>
      <td>93.312406</td>
      <td>1</td>
      <td>object</td>
    </tr>
    <tr>
      <th>45</th>
      <td>trafficSource_adwordsClickInfo.page</td>
      <td>750870</td>
      <td>53814</td>
      <td>93.312406</td>
      <td>10</td>
      <td>object</td>
    </tr>
    <tr>
      <th>46</th>
      <td>trafficSource_adwordsClickInfo.slot</td>
      <td>750870</td>
      <td>53814</td>
      <td>93.312406</td>
      <td>3</td>
      <td>object</td>
    </tr>
    <tr>
      <th>47</th>
      <td>trafficSource_campaign</td>
      <td>0</td>
      <td>804684</td>
      <td>0.000000</td>
      <td>31</td>
      <td>object</td>
    </tr>
    <tr>
      <th>48</th>
      <td>trafficSource_isTrueDirect</td>
      <td>544171</td>
      <td>260513</td>
      <td>67.625428</td>
      <td>1</td>
      <td>object</td>
    </tr>
    <tr>
      <th>49</th>
      <td>trafficSource_keyword</td>
      <td>391032</td>
      <td>413652</td>
      <td>48.594479</td>
      <td>2415</td>
      <td>object</td>
    </tr>
    <tr>
      <th>50</th>
      <td>trafficSource_medium</td>
      <td>0</td>
      <td>804684</td>
      <td>0.000000</td>
      <td>7</td>
      <td>object</td>
    </tr>
    <tr>
      <th>51</th>
      <td>trafficSource_referralPath</td>
      <td>569361</td>
      <td>235323</td>
      <td>70.755850</td>
      <td>2197</td>
      <td>object</td>
    </tr>
    <tr>
      <th>52</th>
      <td>trafficSource_source</td>
      <td>0</td>
      <td>804684</td>
      <td>0.000000</td>
      <td>324</td>
      <td>object</td>
    </tr>
  </tbody>
</table>
</div>



## Drop Columns


```python
set(train.columns).difference(set(test.columns))
```




    {'totals_transactionRevenue', 'trafficSource_campaignCode'}




```python
train.drop('trafficSource_campaignCode',axis=1,inplace=True)
```


```python
trn_colInfo = generateColumnInfo(train)
trn_colInfo[(trn_colInfo['NullCount'] == 0) & (trn_colInfo['UniqueValueCount'] == 1)]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>ColumnName</th>
      <th>NullCount</th>
      <th>NonNullCount</th>
      <th>NullPercent</th>
      <th>UniqueValueCount</th>
      <th>DataType</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>4</th>
      <td>socialEngagementType</td>
      <td>0</td>
      <td>903653</td>
      <td>0.0</td>
      <td>1</td>
      <td>object</td>
    </tr>
    <tr>
      <th>9</th>
      <td>device_browserSize</td>
      <td>0</td>
      <td>903653</td>
      <td>0.0</td>
      <td>1</td>
      <td>object</td>
    </tr>
    <tr>
      <th>10</th>
      <td>device_browserVersion</td>
      <td>0</td>
      <td>903653</td>
      <td>0.0</td>
      <td>1</td>
      <td>object</td>
    </tr>
    <tr>
      <th>12</th>
      <td>device_flashVersion</td>
      <td>0</td>
      <td>903653</td>
      <td>0.0</td>
      <td>1</td>
      <td>object</td>
    </tr>
    <tr>
      <th>14</th>
      <td>device_language</td>
      <td>0</td>
      <td>903653</td>
      <td>0.0</td>
      <td>1</td>
      <td>object</td>
    </tr>
    <tr>
      <th>15</th>
      <td>device_mobileDeviceBranding</td>
      <td>0</td>
      <td>903653</td>
      <td>0.0</td>
      <td>1</td>
      <td>object</td>
    </tr>
    <tr>
      <th>16</th>
      <td>device_mobileDeviceInfo</td>
      <td>0</td>
      <td>903653</td>
      <td>0.0</td>
      <td>1</td>
      <td>object</td>
    </tr>
    <tr>
      <th>17</th>
      <td>device_mobileDeviceMarketingName</td>
      <td>0</td>
      <td>903653</td>
      <td>0.0</td>
      <td>1</td>
      <td>object</td>
    </tr>
    <tr>
      <th>18</th>
      <td>device_mobileDeviceModel</td>
      <td>0</td>
      <td>903653</td>
      <td>0.0</td>
      <td>1</td>
      <td>object</td>
    </tr>
    <tr>
      <th>19</th>
      <td>device_mobileInputSelector</td>
      <td>0</td>
      <td>903653</td>
      <td>0.0</td>
      <td>1</td>
      <td>object</td>
    </tr>
    <tr>
      <th>21</th>
      <td>device_operatingSystemVersion</td>
      <td>0</td>
      <td>903653</td>
      <td>0.0</td>
      <td>1</td>
      <td>object</td>
    </tr>
    <tr>
      <th>22</th>
      <td>device_screenColors</td>
      <td>0</td>
      <td>903653</td>
      <td>0.0</td>
      <td>1</td>
      <td>object</td>
    </tr>
    <tr>
      <th>23</th>
      <td>device_screenResolution</td>
      <td>0</td>
      <td>903653</td>
      <td>0.0</td>
      <td>1</td>
      <td>object</td>
    </tr>
    <tr>
      <th>25</th>
      <td>geoNetwork_cityId</td>
      <td>0</td>
      <td>903653</td>
      <td>0.0</td>
      <td>1</td>
      <td>object</td>
    </tr>
    <tr>
      <th>28</th>
      <td>geoNetwork_latitude</td>
      <td>0</td>
      <td>903653</td>
      <td>0.0</td>
      <td>1</td>
      <td>object</td>
    </tr>
    <tr>
      <th>29</th>
      <td>geoNetwork_longitude</td>
      <td>0</td>
      <td>903653</td>
      <td>0.0</td>
      <td>1</td>
      <td>object</td>
    </tr>
    <tr>
      <th>32</th>
      <td>geoNetwork_networkLocation</td>
      <td>0</td>
      <td>903653</td>
      <td>0.0</td>
      <td>1</td>
      <td>object</td>
    </tr>
    <tr>
      <th>40</th>
      <td>totals_visits</td>
      <td>0</td>
      <td>903653</td>
      <td>0.0</td>
      <td>1</td>
      <td>object</td>
    </tr>
    <tr>
      <th>43</th>
      <td>trafficSource_adwordsClickInfo.criteriaParameters</td>
      <td>0</td>
      <td>903653</td>
      <td>0.0</td>
      <td>1</td>
      <td>object</td>
    </tr>
  </tbody>
</table>
</div>



These columns have a single unique value. They can be dropped.


```python
train.drop(['socialEngagementType',
'device_browserSize',
'device_browserVersion',
'device_flashVersion',
'device_language',
'device_mobileDeviceBranding',
'device_mobileDeviceInfo',
'device_mobileDeviceMarketingName',
'device_mobileDeviceModel',
'device_mobileInputSelector',
'device_operatingSystemVersion',
'device_screenColors',
'device_screenResolution',
'geoNetwork_cityId',
'geoNetwork_latitude',
'geoNetwork_longitude',
'geoNetwork_networkLocation',
'totals_visits',
'trafficSource_adwordsClickInfo.criteriaParameters'],axis=1,inplace=True)
```

## Missing Values and EDA


```python
plt.figure(figsize=(15,8))
sns.heatmap(train.isnull(),yticklabels=False,cbar=False,cmap='viridis')
```




    <matplotlib.axes._subplots.AxesSubplot at 0x1d984634320>




![png](output_24_1.png)



```python
def plot_colCount(df,col,xtick=0,w=12,h=7):
    plt.figure(figsize=(w,h))
    p = sns.countplot(data =df,x=col)
    plt.xticks(rotation=xtick)
    plt.title('Count of ' + col)
    plt.show()
    
def plot_totalRevenue(df,col,xtick=0,w=12,h=7):
    groupedDf = df.groupby(col,as_index=False)['totals_transactionRevenue'].sum()
    groupedDf = groupedDf[groupedDf['totals_transactionRevenue']>0]
    plt.figure(figsize=(w,h))
    p = sns.barplot(data=groupedDf,x=col,y='totals_transactionRevenue')
    plt.xticks(rotation=xtick)
    plt.title('Total revenue by ' + col)
    plt.show()
    
def plot_revenuePerUnitCol(df,col,xtick=0,w=12,h=7):
    plt.figure(figsize=(w,h))
    plt.ylim()
    p = sns.barplot(data =df,x=col,y='totals_transactionRevenue',ci=False)
    plt.xticks(rotation=xtick)
    plt.title('Revenue per visit')
    plt.show()
```

### Target Column


```python
print(train['totals_transactionRevenue'].isnull().sum())
```

    0
    


```python
train['totals_transactionRevenue'].fillna(0,inplace=True)
train['totals_transactionRevenue'] = train['totals_transactionRevenue'].astype('int64')
```


```python
plt.figure(figsize=[10,6])
ax.set_ylabel('Distribuition')
sns.distplot(train['totals_transactionRevenue'])
```




    <matplotlib.axes._subplots.AxesSubplot at 0x1d877e149e8>




![png](output_29_1.png)



```python
plt.figure(figsize=[10,6])
ax = sns.distplot(np.log(train[train['totals_transactionRevenue'] > 0]["totals_transactionRevenue"] + 0.01), bins=40, kde=True)
ax.set_xlabel('totals_transactionRevenue_log') #seting the xlabel and size of font
ax.set_ylabel('Distribuition') #seting the ylabel and size of font
ax.set_title("Distribuition of Revenue Log") #seting the title and size of font
```




    Text(0.5,1,'Distribuition of Revenue Log')




![png](output_30_1.png)



```python
ax = sns.distplot(np.log(df_train[df_train['totals.transactionRevenue'] > 0]["totals.transactionRevenue"] + 0.01), bins=40, kde=True)
ax.set_xlabel('Transaction RevenueLog', fontsize=15) #seting the xlabel and size of font
ax.set_ylabel('Distribuition', fontsize=15) #seting the ylabel and size of font
ax.set_title("Distribuition of Revenue Log", fontsize=20) #seting the title and size of font
```

### Channel Grouping


```python
plot_colCount(train,'channelGrouping',30,10,6)
plot_totalRevenue(train,'channelGrouping',30,10,6)
plot_revenuePerUnitCol(train,'channelGrouping',30,10,6)
```


![png](output_33_0.png)



![png](output_33_1.png)



![png](output_33_2.png)


- Organic search generates the most number of visits
- Referral generates the 4th most number of visits but generates the most revenue
- Display generates the most revenue per visit

### Date


```python
train['date'] = pd.to_datetime(train['date'],format='%Y%m%d')
```


```python
import math
byDate = train.groupby('date',as_index=False).agg({'visitId':'count','totals_transactionRevenue':'sum'}).rename(columns={'visitId':'visits','totals_transactionRevenue':'totalRevenue'})
byDate['totalRevenue'] = byDate['totalRevenue']/1000000
byDateFlat = byDate.melt('date',var_name ='Numbers',value_name='values')
```


```python
plt.figure(figsize=(16,8))
new_labels = ['label 1', 'label 2']
sns.lineplot(data=byDateFlat,x='date',y='values',hue='Numbers')
plt.title('Visit Count and Total Revenue (in 1000000) by date')
plt.ylabel('')
plt.show()
del byDateFlat
del byDate
```


![png](output_38_0.png)


- There is an increase in visits during the holiday period
- There is an increase in the revenue during the same period


```python
train['date_year'],train['date_month'],train['date_weekday'] = train['date'].dt.year,train['date'].dt.month,train['date'].dt.weekday
train.drop('date',axis=1,inplace=True)
```


```python
plot_colCount(train,'date_weekday',0,10,6)
```


![png](output_41_0.png)



```python
plot_totalRevenue(train,'date_weekday',0,10,6)
#Monday=0, Sunday=6
```


![png](output_42_0.png)


- Tuesdays, Wednesdays and Thursdays generate the most visits and revenue


```python
plot_colCount(train,'date_month',0,10,6)
```


![png](output_44_0.png)



```python
plot_totalRevenue(train,'date_month',0,10,6)
```


![png](output_45_0.png)


- October and November have the highest traffic
- April, Agust and December generate the most revenue

### fullVisitorId


```python
train['fullVisitorId'].value_counts().head(10)
```




    1957458976293878100    278
    0824839726118485274    255
    3608475193341679870    201
    1856749147915772585    199
    3269834865385146569    155
    0720311197761340948    153
    7634897085866546110    148
    4038076683036146727    138
    0232377434237234751    135
    3694234028523165868    129
    Name: fullVisitorId, dtype: int64




```python
train.groupby('fullVisitorId').sum()['totals_transactionRevenue'].sort_values(ascending=False).head(10)
```




    fullVisitorId
    1957458976293878100    77113430000
    5632276788326171571    16023750000
    9417857471295131045    15170120000
    4471415710206918415    11211100000
    4984366501121503466     9513900000
    9089132392240687728     8951970000
    9029794295932939024     7846350000
    7463172420271311409     7225100000
    7311242886083854158     7143250000
    79204932396995037       7047150000
    Name: totals_transactionRevenue, dtype: int64



- Visitor ID 1957458976293878100 has 278 visits and generates the most revenue

### visitNumber


```python
train['visitNumber'].value_counts().head()
```




    1    703060
    2     92548
    3     35843
    4     19157
    5     11615
    Name: visitNumber, dtype: int64



### device_browser


```python
train['device_browser'].value_counts().head(10)
```




    Chrome               620364
    Safari               182245
    Firefox               37069
    Internet Explorer     19375
    Edge                  10205
    Android Webview        7865
    Safari (in-app)        6850
    Opera Mini             6139
    Opera                  5643
    UC Browser             2427
    Name: device_browser, dtype: int64




```python
plot_colCount(train,'device_browser',80)
plot_totalRevenue(train,'device_browser',30,10,6)
plot_revenuePerUnitCol(train,'device_browser',80)
```


![png](output_55_0.png)



![png](output_55_1.png)



![png](output_55_2.png)


- Chrome generates a significant majority of the visits and revenue 

### device_deviceCategory


```python
f = sns.FacetGrid(train,hue='device_deviceCategory',size=5,aspect=4)
plt.figure(figsize=(15,10))
f.map(sns.kdeplot,'totals_transactionRevenue',shade= True)
f.add_legend()
```

    C:\ProgramData\Anaconda3\lib\site-packages\seaborn\axisgrid.py:230: UserWarning: The `size` paramter has been renamed to `height`; please update your code.
      warnings.warn(msg, UserWarning)
    




    <seaborn.axisgrid.FacetGrid at 0x1d8311be278>




![png](output_58_2.png)



    <Figure size 1080x720 with 0 Axes>



```python
f = sns.FacetGrid(train,hue='device_deviceCategory',size=5,aspect=4)
plt.xlim(0, 500000000)
plt.figure(figsize=(15,10))
f.map(sns.kdeplot,'totals_transactionRevenue',shade= True)
f.add_legend()
```

    C:\ProgramData\Anaconda3\lib\site-packages\seaborn\axisgrid.py:230: UserWarning: The `size` paramter has been renamed to `height`; please update your code.
      warnings.warn(msg, UserWarning)
    




    <seaborn.axisgrid.FacetGrid at 0x1d9c21c44a8>




![png](output_59_2.png)



    <Figure size 1080x720 with 0 Axes>



```python
plot_colCount(train,'device_deviceCategory',0)
plot_totalRevenue(train,'device_deviceCategory',0,10,6)
plot_revenuePerUnitCol(train,'device_deviceCategory',0)
```


![png](output_60_0.png)



![png](output_60_1.png)



![png](output_60_2.png)


- Desktops generate the highest visits and revenue 
- Desktops generate the most high revenue transactions
- Desktops generate almost 8 times the revenue per visit compared to tablets and mobile
- Tablets generate the least total revenue 
- Tablets generate a high number of low revenue transactions

### device_isMobile


```python
plt.figure(figsize=(8,5))
sns.barplot(data =train,x='device_isMobile',y='totals_transactionRevenue')
```




    <matplotlib.axes._subplots.AxesSubplot at 0x1d831f9b5f8>




![png](output_63_1.png)


### device_operatingSystem


```python
plot_colCount(train,'device_operatingSystem',60)
plot_totalRevenue(train,'device_operatingSystem',30,10,6)
plot_revenuePerUnitCol(train,'device_operatingSystem',60)
```


![png](output_65_0.png)



![png](output_65_1.png)



![png](output_65_2.png)


Windows is the most popular operating system but Mac generates more revenue. Chrome generates more revenue per visit.

### geoNetwork_city


```python
topCities = train['geoNetwork_city'].value_counts().head(50).reset_index()
topCities.columns = ['city','count']
topCities = topCities[topCities.city !='not available in demo dataset']
topCities = topCities[topCities.city !='(not set)']
topCitiesTrain = train[train['geoNetwork_city'].isin(topCities['city'])]
```


```python
plot_colCount(topCitiesTrain,'geoNetwork_city',60)
plot_totalRevenue(topCitiesTrain,'geoNetwork_city',60)
plot_revenuePerUnitCol(topCitiesTrain,'geoNetwork_city',60)
del topCities
del topCitiesTrain
```


![png](output_69_0.png)



![png](output_69_1.png)



![png](output_69_2.png)


### geoNetwork_continent


```python
plot_colCount(train,'geoNetwork_continent',0,10,6)
plot_totalRevenue(train,'geoNetwork_continent',0,10,6)
plot_revenuePerUnitCol(train,'geoNetwork_continent',0,10,6)
```


![png](output_71_0.png)



![png](output_71_1.png)



![png](output_71_2.png)


### geoNetwork_country


```python
byCountry = train.groupby('geoNetwork_country',as_index=False).agg({'visitId':'count','totals_transactionRevenue':'sum'}).rename(columns={'visitId':'visits','totals_transactionRevenue':'totalRevenue'})
```


```python
import matplotlib.pyplot as plt
from plotly.offline import download_plotlyjs, init_notebook_mode,plot,iplot
import plotly.graph_objs as go
import cufflinks as cf
```


```python
init_notebook_mode(connected=True)
cf.go_offline()
```


<script>requirejs.config({paths: { 'plotly': ['https://cdn.plot.ly/plotly-latest.min']},});if(!window.Plotly) {{require(['plotly'],function(plotly) {window.Plotly=plotly;});}}</script>



<script type='text/javascript'>if(!window.Plotly){define('plotly', function(require, exports, module) {/**
* plotly.js v1.41.3
* Copyright 2012-2018, Plotly, Inc.
* All rights reserved.
* Licensed under the MIT license
*/



```python
data=dict(type='choropleth',
         locations = byCountry['geoNetwork_country'],
         locationmode = 'country names',
         colorscale = 'Blues',
         reversescale=True,
         text = ['text 1','text 2','text 3'],
         z=byCountry['visits'],
         colorbar={'title':'Total visits'})

layout = dict(title='Visit count by Country')

choromap = go.Figure(data=[data])
iplot(choromap)

```


<div id="702698a9-a767-4032-81bd-479e6bd4cfec" style="height: 525px; width: 100%;" class="plotly-graph-div"></div><script type="text/javascript">require(["plotly"], function(Plotly) { window.PLOTLYENV=window.PLOTLYENV || {};window.PLOTLYENV.BASE_URL="https://plot.ly";Plotly.newPlot("702698a9-a767-4032-81bd-479e6bd4cfec", [{"colorbar": {"title": "Total visits"}, "colorscale": "Blues", "locationmode": "country names", "locations": ["(not set)", "Afghanistan", "Albania", "Algeria", "American Samoa", "Andorra", "Angola", "Anguilla", "Antigua & Barbuda", "Argentina", "Armenia", "Aruba", "Australia", "Austria", "Azerbaijan", "Bahamas", "Bahrain", "Bangladesh", "Barbados", "Belarus", "Belgium", "Belize", "Benin", "Bermuda", "Bhutan", "Bolivia", "Bosnia & Herzegovina", "Botswana", "Brazil", "British Virgin Islands", "Brunei", "Bulgaria", "Burkina Faso", "Burundi", "Cambodia", "Cameroon", "Canada", "Cape Verde", "Caribbean Netherlands", "Cayman Islands", "Central African Republic", "Chad", "Chile", "China", "Colombia", "Comoros", "Congo - Brazzaville", "Congo - Kinshasa", "Cook Islands", "Costa Rica", "Croatia", "Cura\u00e7ao", "Cyprus", "Czechia", "C\u00f4te d\u2019Ivoire", "Denmark", "Djibouti", "Dominica", "Dominican Republic", "Ecuador", "Egypt", "El Salvador", "Equatorial Guinea", "Eritrea", "Estonia", "Ethiopia", "Faroe Islands", "Fiji", "Finland", "France", "French Guiana", "French Polynesia", "Gabon", "Gambia", "Georgia", "Germany", "Ghana", "Gibraltar", "Greece", "Greenland", "Grenada", "Guadeloupe", "Guam", "Guatemala", "Guernsey", "Guinea", "Guinea-Bissau", "Guyana", "Haiti", "Honduras", "Hong Kong", "Hungary", "Iceland", "India", "Indonesia", "Iran", "Iraq", "Ireland", "Isle of Man", "Israel", "Italy", "Jamaica", "Japan", "Jersey", "Jordan", "Kazakhstan", "Kenya", "Kosovo", "Kuwait", "Kyrgyzstan", "Laos", "Latvia", "Lebanon", "Lesotho", "Liberia", "Libya", "Liechtenstein", "Lithuania", "Luxembourg", "Macau", "Macedonia (FYROM)", "Madagascar", "Malawi", "Malaysia", "Maldives", "Mali", "Malta", "Marshall Islands", "Martinique", "Mauritania", "Mauritius", "Mayotte", "Mexico", "Moldova", "Monaco", "Mongolia", "Montenegro", "Morocco", "Mozambique", "Myanmar (Burma)", "Namibia", "Nepal", "Netherlands", "New Caledonia", "New Zealand", "Nicaragua", "Niger", "Nigeria", "Norfolk Island", "Northern Mariana Islands", "Norway", "Oman", "Pakistan", "Palestine", "Panama", "Papua New Guinea", "Paraguay", "Peru", "Philippines", "Poland", "Portugal", "Puerto Rico", "Qatar", "Romania", "Russia", "Rwanda", "R\u00e9union", "Samoa", "San Marino", "Saudi Arabia", "Senegal", "Serbia", "Seychelles", "Sierra Leone", "Singapore", "Sint Maarten", "Slovakia", "Slovenia", "Somalia", "South Africa", "South Korea", "Spain", "Sri Lanka", "St. Barth\u00e9lemy", "St. Kitts & Nevis", "St. Lucia", "St. Martin", "St. Pierre & Miquelon", "St. Vincent & Grenadines", "Sudan", "Suriname", "Swaziland", "Sweden", "Switzerland", "Syria", "S\u00e3o Tom\u00e9 & Pr\u00edncipe", "Taiwan", "Tajikistan", "Tanzania", "Thailand", "Timor-Leste", "Togo", "Trinidad & Tobago", "Tunisia", "Turkey", "Turkmenistan", "Turks & Caicos Islands", "U.S. Virgin Islands", "Uganda", "Ukraine", "United Arab Emirates", "United Kingdom", "United States", "Uruguay", "Uzbekistan", "Vanuatu", "Venezuela", "Vietnam", "Yemen", "Zambia", "Zimbabwe", "\u00c5land Islands"], "reversescale": true, "text": ["text 1", "text 2", "text 3"], "z": [1468, 57, 547, 2055, 1, 17, 58, 1, 5, 5037, 280, 29, 12698, 2796, 569, 40, 256, 2297, 55, 976, 4442, 27, 42, 38, 8, 318, 903, 14, 19783, 5, 65, 2046, 37, 12, 522, 175, 25869, 18, 4, 14, 4, 20, 1950, 3881, 4880, 2, 3, 103, 3, 554, 1396, 30, 463, 4247, 320, 3319, 10, 3, 934, 1251, 2343, 393, 4, 1, 656, 139, 18, 44, 959, 15832, 18, 20, 25, 10, 888, 19980, 381, 10, 3370, 5, 22, 29, 41, 581, 25, 31, 4, 27, 37, 188, 4718, 2513, 142, 51140, 9273, 41, 711, 6493, 3, 5563, 11332, 153, 19731, 41, 859, 968, 771, 435, 521, 134, 161, 705, 398, 6, 8, 92, 7, 848, 156, 115, 846, 47, 12, 6439, 89, 35, 175, 2, 61, 40, 87, 14, 13225, 319, 21, 241, 110, 1907, 74, 196, 36, 211, 11453, 23, 2194, 139, 10, 1446, 1, 12, 2250, 305, 4010, 191, 476, 11, 132, 5546, 9244, 9693, 2376, 732, 535, 6428, 11662, 46, 145, 1, 4, 3132, 151, 1872, 3, 8, 7172, 8, 1788, 729, 39, 2099, 5237, 11658, 1468, 1, 8, 24, 1, 1, 10, 92, 30, 14, 5315, 4427, 10, 1, 12996, 12, 260, 20123, 7, 32, 164, 1194, 20522, 5, 14, 22, 166, 5577, 3144, 37393, 364744, 627, 96, 3, 2132, 24598, 96, 40, 59, 1], "type": "choropleth", "uid": "260b37d4-4072-11e9-9c80-24fd523881a5"}], {}, {"showLink": true, "linkText": "Export to plot.ly"})});</script>



```python
data=dict(type='choropleth',
         locations = byCountry['geoNetwork_country'],
         locationmode = 'country names',
         colorscale = 'Blues',
         reversescale=True,
         text = ['text 1','text 2','text 3'],
         z=byCountry['totalRevenue'],
         colorbar={'title':'Total revenue'})

layout = dict(title='Total revenue by Country')

choromap = go.Figure(data=[data])
iplot(choromap)
```


<div id="ac7f9516-ab90-4758-8d43-dd43ee2d73f8" style="height: 525px; width: 100%;" class="plotly-graph-div"></div><script type="text/javascript">require(["plotly"], function(Plotly) { window.PLOTLYENV=window.PLOTLYENV || {};window.PLOTLYENV.BASE_URL="https://plot.ly";Plotly.newPlot("ac7f9516-ab90-4758-8d43-dd43ee2d73f8", [{"colorbar": {"title": "Total revenue"}, "colorscale": "Blues", "locationmode": "country names", "locations": ["(not set)", "Afghanistan", "Albania", "Algeria", "American Samoa", "Andorra", "Angola", "Anguilla", "Antigua & Barbuda", "Argentina", "Armenia", "Aruba", "Australia", "Austria", "Azerbaijan", "Bahamas", "Bahrain", "Bangladesh", "Barbados", "Belarus", "Belgium", "Belize", "Benin", "Bermuda", "Bhutan", "Bolivia", "Bosnia & Herzegovina", "Botswana", "Brazil", "British Virgin Islands", "Brunei", "Bulgaria", "Burkina Faso", "Burundi", "Cambodia", "Cameroon", "Canada", "Cape Verde", "Caribbean Netherlands", "Cayman Islands", "Central African Republic", "Chad", "Chile", "China", "Colombia", "Comoros", "Congo - Brazzaville", "Congo - Kinshasa", "Cook Islands", "Costa Rica", "Croatia", "Cura\u00e7ao", "Cyprus", "Czechia", "C\u00f4te d\u2019Ivoire", "Denmark", "Djibouti", "Dominica", "Dominican Republic", "Ecuador", "Egypt", "El Salvador", "Equatorial Guinea", "Eritrea", "Estonia", "Ethiopia", "Faroe Islands", "Fiji", "Finland", "France", "French Guiana", "French Polynesia", "Gabon", "Gambia", "Georgia", "Germany", "Ghana", "Gibraltar", "Greece", "Greenland", "Grenada", "Guadeloupe", "Guam", "Guatemala", "Guernsey", "Guinea", "Guinea-Bissau", "Guyana", "Haiti", "Honduras", "Hong Kong", "Hungary", "Iceland", "India", "Indonesia", "Iran", "Iraq", "Ireland", "Isle of Man", "Israel", "Italy", "Jamaica", "Japan", "Jersey", "Jordan", "Kazakhstan", "Kenya", "Kosovo", "Kuwait", "Kyrgyzstan", "Laos", "Latvia", "Lebanon", "Lesotho", "Liberia", "Libya", "Liechtenstein", "Lithuania", "Luxembourg", "Macau", "Macedonia (FYROM)", "Madagascar", "Malawi", "Malaysia", "Maldives", "Mali", "Malta", "Marshall Islands", "Martinique", "Mauritania", "Mauritius", "Mayotte", "Mexico", "Moldova", "Monaco", "Mongolia", "Montenegro", "Morocco", "Mozambique", "Myanmar (Burma)", "Namibia", "Nepal", "Netherlands", "New Caledonia", "New Zealand", "Nicaragua", "Niger", "Nigeria", "Norfolk Island", "Northern Mariana Islands", "Norway", "Oman", "Pakistan", "Palestine", "Panama", "Papua New Guinea", "Paraguay", "Peru", "Philippines", "Poland", "Portugal", "Puerto Rico", "Qatar", "Romania", "Russia", "Rwanda", "R\u00e9union", "Samoa", "San Marino", "Saudi Arabia", "Senegal", "Serbia", "Seychelles", "Sierra Leone", "Singapore", "Sint Maarten", "Slovakia", "Slovenia", "Somalia", "South Africa", "South Korea", "Spain", "Sri Lanka", "St. Barth\u00e9lemy", "St. Kitts & Nevis", "St. Lucia", "St. Martin", "St. Pierre & Miquelon", "St. Vincent & Grenadines", "Sudan", "Suriname", "Swaziland", "Sweden", "Switzerland", "Syria", "S\u00e3o Tom\u00e9 & Pr\u00edncipe", "Taiwan", "Tajikistan", "Tanzania", "Thailand", "Timor-Leste", "Togo", "Trinidad & Tobago", "Tunisia", "Turkey", "Turkmenistan", "Turks & Caicos Islands", "U.S. Virgin Islands", "Uganda", "Ukraine", "United Arab Emirates", "United Kingdom", "United States", "Uruguay", "Uzbekistan", "Vanuatu", "Venezuela", "Vietnam", "Yemen", "Zambia", "Zimbabwe", "\u00c5land Islands"], "reversescale": true, "text": ["text 1", "text 2", "text 3"], "z": [769780000, 0, 0, 0, 0, 0, 0, 10990000, 0, 262440000, 30480000, 0, 1745260000, 0, 0, 0, 0, 0, 0, 0, 992050000, 0, 0, 0, 0, 0, 0, 0, 641830000, 0, 0, 0, 0, 0, 0, 0, 32824540000, 0, 0, 0, 0, 0, 247260000, 211670000, 604870000, 0, 0, 0, 0, 0, 0, 206330000, 75440000, 44790000, 0, 25000000, 0, 0, 0, 674930000, 50960000, 76890000, 0, 0, 0, 0, 0, 0, 58980000, 446080000, 0, 0, 0, 0, 38980000, 549340000, 0, 0, 159330000, 0, 0, 27180000, 0, 60150000, 0, 0, 0, 0, 0, 0, 1467960000, 33590000, 0, 696660000, 1840380000, 0, 0, 144860000, 0, 619600000, 204090000, 0, 6728990000, 0, 0, 34980000, 5268700000, 0, 238220000, 0, 0, 0, 40770000, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 471850000, 0, 0, 0, 0, 0, 0, 0, 0, 1592790000, 0, 0, 0, 0, 0, 0, 0, 0, 0, 259050000, 0, 47970000, 63990000, 0, 3302400000, 0, 0, 0, 0, 57950000, 0, 111730000, 0, 0, 127580000, 138030000, 230000000, 215800000, 1202840000, 0, 75960000, 94940000, 0, 0, 0, 0, 265080000, 0, 0, 0, 0, 843020000, 0, 0, 0, 0, 65700000, 745710000, 384790000, 0, 0, 0, 116210000, 0, 0, 0, 0, 0, 0, 75590000, 595820000, 0, 0, 1920890000, 0, 0, 462710000, 0, 0, 0, 0, 102410000, 0, 0, 0, 0, 467520000, 370060000, 1689450000, 1452440650000, 3500000, 0, 0, 13374900000, 0, 0, 0, 0, 0], "type": "choropleth", "uid": "2a6dfc3e-4072-11e9-a207-24fd523881a5"}], {}, {"showLink": true, "linkText": "Export to plot.ly"})});</script>



```python
topCountries = train['geoNetwork_country'].value_counts().head(80).reset_index()
topCountries.columns = ['country','count']
topCountriesTrain = train[train['geoNetwork_country'].isin(topCountries['country'])]
```


```python
plot_colCount(topCountriesTrain,'geoNetwork_country',80,16)
plot_totalRevenue(topCountriesTrain,'geoNetwork_country',80,16)

```


![png](output_79_0.png)



![png](output_79_1.png)



```python
plot_revenuePerUnitCol(topCountriesTrain,'geoNetwork_country',80,16)
del topCountries
del topCountriesTrain
```


![png](output_80_0.png)


- United States generates the highest number of visits and revenue
- Surprisingly, Venezuela and Kenya generate the highest revenue per visit


```python
topCountries = train['geoNetwork_country'].value_counts().head(8).index
```


```python
def plotByCountry(plotCol,n_labels = 0, xtick=0,plotType = 'line',order=0):
    groupByCountry = train.groupby(['geoNetwork_country',plotCol],as_index=False).count()[['geoNetwork_country',plotCol,'visitId']]
    groupByCountry = groupByCountry[groupByCountry['geoNetwork_country'].isin(topCountries)]
    if n_labels != 0:
        topLabels = train[plotCol].value_counts().head(n_labels).index
        groupByCountry = groupByCountry[groupByCountry[plotCol].isin(topLabels)]
    groupByCountry.columns = ['geoNetwork_country', plotCol, 'visits']
    plt.figure(figsize=[14,10])
    plt.xticks(rotation=xtick)
    if plotType == 'line':
        sns.lineplot(data=groupByCountry,x=plotCol,y='visits',hue='geoNetwork_country')
    if plotType == 'bar':
        if order == 0:
            sns.barplot(data=groupByCountry,x=plotCol,y='visits',hue='geoNetwork_country')
        if order == 1:
            sns.barplot(data=groupByCountry,x='geoNetwork_country',y='visits',hue=plotCol)
```


```python
plotByCountry('date_month')
```


![png](output_84_0.png)


- There is a significant spike in traffic from Vietnam between October and December
- Thailand also shows a similar pattern
- Most countries show a spike in summer and holiday season
- Germany and Canada show very little seasonal fluctuation
- United Kingdom is the only country where the most visits occur outside the November to January period 


```python
plotByCountry('date_weekday')
```


![png](output_86_0.png)


- There is a significant difference in traffic from United States between weedkays and weekends. Other countries do not show this pattern


```python
plotByCountry('device_deviceCategory',plotType='bar',order=1)
```


![png](output_88_0.png)


- India and Thailand have very low desktop to tablet ratio. Tablets don't seem to be popular in these countries
- Vietnam has the least desktop to mobile ratio


```python
plotByCountry('channelGrouping',plotType='bar')
```


![png](output_90_0.png)



```python
plotByCountry('device_operatingSystem',n_labels=5,plotType='bar',order=1)
```


![png](output_91_0.png)


- Mac is the leader in United States.It has a significant presence in Thailand and Vietnam but lags in other countries
- Windows is the leader in all countries except United States
- Countries with more iOS traffic than Android: United States, Canada, United Kingdom


```python
plotByCountry('device_browser',plotType='bar',n_labels=5,order=1)
```


![png](output_93_0.png)


- As discussed above, Vietnam and Thailand have high Mac adoption and this could be the reason why they have almost equal traffic from Chrome and Safari

### geoNetwork_metro


```python
topMetrosTrain = train[~train['geoNetwork_metro'].isin(['not available in demo dataset','(not set)'])]
```


```python
plot_colCount(topMetrosTrain,'geoNetwork_metro',90,16)
plot_totalRevenue(topMetrosTrain,'geoNetwork_metro',90,16)
```


![png](output_97_0.png)



![png](output_97_1.png)



```python
plot_revenuePerUnitCol(topMetrosTrain,'geoNetwork_metro',90,16)
```


![png](output_98_0.png)


### geoNetwork_region


```python
topRegions = train['geoNetwork_region'].value_counts().head(80).reset_index()
topRegions.columns = ['region','count']
topRegions = topRegions[(topRegions.region !='not available in demo dataset') &(topRegions.region !='(not set)')]
topRegionsTrain = train[train['geoNetwork_region'].isin(topRegions['region'])]
```


```python
plot_colCount(topRegionsTrain,'geoNetwork_region',80,16)
plot_totalRevenue(topRegionsTrain,'geoNetwork_region',80,16)
```


![png](output_101_0.png)



![png](output_101_1.png)



```python
plot_revenuePerUnitCol(topRegionsTrain,'geoNetwork_region',80,16)
del topRegions
del topRegionsTrain
```


![png](output_102_0.png)


### geoNetwork_subContinent


```python
plot_colCount(train,'geoNetwork_subContinent',30,15,6)
plot_totalRevenue(train,'geoNetwork_subContinent',30,15,6)
plot_revenuePerUnitCol(train,'geoNetwork_subContinent',30,15,6)
```


![png](output_104_0.png)



![png](output_104_1.png)



![png](output_104_2.png)


### totals_bounces


```python
train['totals_bounces'].fillna(0,inplace=True)
train['totals_bounces'] = train['totals_bounces'].astype('int64')
```

### totals_newVisits


```python
train['totals_newVisits'].fillna(0,inplace=True)
train['totals_newVisits'] = train['totals_newVisits'].astype('int64')
```

### totals_hits


```python
train['totals_hits'] = train['totals_hits'].astype('int64')
```

### totals_pageviews


```python
#totals_pageviews
train['totals_pageviews'] = train['totals_pageviews'].astype(float)
print(train['totals_pageviews'].min())
print(train['totals_pageviews'].max())
train['totals_pageviews'].fillna(0,inplace=True)
```

    1.0
    469.0
    


```python
sns.distplot(train['totals_pageviews'])
```




    <matplotlib.axes._subplots.AxesSubplot at 0x1d875edfc18>




![png](output_113_1.png)



```python
sns.lmplot(data=train,x='totals_pageviews',y='totals_transactionRevenue',
           hue='geoNetwork_continent',col='geoNetwork_continent',col_wrap=2,fit_reg=False)
```




    <seaborn.axisgrid.FacetGrid at 0x1d876173dd8>




![png](output_114_1.png)


### Traffic Source Columns

There are 3 Traffic Source related columns that do not have any null values.
trafficSource_campaign
trafficSource_medium
trafficSource_source


```python
train['trafficSource_campaign'].value_counts()
```




    (not set)                                          865347
    Data Share Promo                                    16403
    AW - Dynamic Search Ads Whole Site                  14244
    AW - Accessories                                     7070
    test-liyuhz                                           392
    AW - Electronics                                       96
    Retail (DO NOT EDIT owners nophakun and tianyu)        50
    AW - Apparel                                           46
    All Products                                            4
    Data Share                                              1
    Name: trafficSource_campaign, dtype: int64



Though trafficsource_campaign does not contain any null values, there are many unknowns.


```python
train['trafficSource_medium'].value_counts()
```




    organic      381561
    referral     330955
    (none)       143026
    cpc           25326
    affiliate     16403
    cpm            6262
    (not set)       120
    Name: trafficSource_medium, dtype: int64




```python
train['trafficSource_medium'].replace('(not set)','none',inplace=True)
train['trafficSource_medium'].replace('(none)','none',inplace=True)
```


```python
plot_colCount(train,'trafficSource_medium',30,10,6)
plot_totalRevenue(train,'trafficSource_medium',30,10,6)
plot_revenuePerUnitCol(train,'trafficSource_medium',30,10,6)
```


![png](output_121_0.png)



![png](output_121_1.png)



![png](output_121_2.png)



```python
train['trafficSource_source'].value_counts().head()
```




    google                 400788
    youtube.com            212602
    (direct)               143028
    mall.googleplex.com     66416
    Partners                16411
    Name: trafficSource_source, dtype: int64




```python
#trafficSource_adwordsClickInfo.isVideoAd
train['trafficSource_adwordsClickInfo.isVideoAd'].unique()
```




    array([nan, False], dtype=object)



Not enough information. All non-values are 'False'. Dropping column.


```python
train.drop(['trafficSource_adwordsClickInfo.isVideoAd'],axis=1,inplace=True)
```


```python
#trafficSource_isTrueDirect
train['trafficSource_isTrueDirect'].fillna(0,inplace=True)
train['trafficSource_isTrueDirect'].replace(True,1,inplace=True)
train['trafficSource_isTrueDirect']=train['trafficSource_isTrueDirect'].astype(bool)
```


```python
#trafficSource_adContent
train['trafficSource_adContent'].fillna('Unknown',inplace=True)
```


```python
#trafficSource_adwordsClickInfo.adNetworkType
train['trafficSource_adwordsClickInfo.adNetworkType'].value_counts()
train['trafficSource_adwordsClickInfo.adNetworkType'].fillna('Unknown',inplace=True)
```


```python
#trafficSource_adwordsClickInfo.gclId
train['trafficSource_adwordsClickInfo.gclId'].fillna('Unknown',inplace=True)
```


```python
#trafficSource_adwordsClickInfo.page
train['trafficSource_adwordsClickInfo.page'].fillna(0,inplace=True)
train['trafficSource_adwordsClickInfo.page'] = train['trafficSource_adwordsClickInfo.page'].astype('int64')
```


```python
#trafficSource_referralPath
train['trafficSource_referralPath'].fillna(0,inplace=True)
```


```python
#trafficSource_adwordsClickInfo.slot
train['trafficSource_adwordsClickInfo.slot'].value_counts()
```




    Top    20956
    RHS      504
    Name: trafficSource_adwordsClickInfo.slot, dtype: int64




```python
train.drop(['trafficSource_adwordsClickInfo.slot'],axis=1,inplace=True)
```


```python
#trafficSource_keyword
train['trafficSource_keyword'].fillna(0,inplace=True)
```


```python
train.drop(['sessionId',
            'visitId','visitStartTime',
            'geoNetwork_region'],axis=1,inplace=True)
```

### Test Set


```python
test.drop(['socialEngagementType',
'device_browserSize',
'device_browserVersion',
'device_flashVersion',
'device_language',
'device_mobileDeviceBranding',
'device_mobileDeviceInfo',
'device_mobileDeviceMarketingName',
'device_mobileDeviceModel',
'device_mobileInputSelector',
'device_operatingSystemVersion',
'device_screenColors',
'device_screenResolution',
'geoNetwork_cityId',
'geoNetwork_latitude',
'geoNetwork_longitude',
'geoNetwork_networkLocation',
'totals_visits',
'trafficSource_adwordsClickInfo.criteriaParameters'],axis=1,inplace=True)

plt.figure(figsize=(15,8))
sns.heatmap(test.isnull(),yticklabels=False,cbar=False,cmap='viridis')
```




    <matplotlib.axes._subplots.AxesSubplot at 0x1d98462a7b8>




![png](output_137_1.png)



```python
test['date'] = pd.to_datetime(test['date'],format='%Y%m%d')
test['date_year'],test['date_month'],test['date_weekday'] = test['date'].dt.year,test['date'].dt.month,test['date'].dt.weekday
test.drop('date',axis=1,inplace=True)
```


```python
test['totals_bounces'].fillna(0,inplace=True)
test['totals_bounces'] = test['totals_bounces'].astype('int64')
test['totals_newVisits'].fillna(0,inplace=True)
test['totals_newVisits'] = test['totals_newVisits'].astype('int64')
test['totals_hits'] = test['totals_hits'].astype('int64')
test['totals_pageviews'] = test['totals_pageviews'].astype(float)
test['totals_pageviews'].fillna(0,inplace=True)
test['trafficSource_medium'].replace('(not set)','none',inplace=True)
test['trafficSource_medium'].replace('(none)','none',inplace=True)
test.drop(['trafficSource_adwordsClickInfo.isVideoAd'],axis=1,inplace=True)
test['trafficSource_isTrueDirect'].fillna(0,inplace=True)
test['trafficSource_isTrueDirect'].replace(True,1,inplace=True)
test['trafficSource_isTrueDirect']=test['trafficSource_isTrueDirect'].astype(bool)
test['trafficSource_adContent'].fillna('Unknown',inplace=True)
test['trafficSource_adwordsClickInfo.adNetworkType'].fillna('Unknown',inplace=True)
test['trafficSource_adwordsClickInfo.gclId'].fillna('Unknown',inplace=True)
test['trafficSource_adwordsClickInfo.page'].fillna(0,inplace=True)
test['trafficSource_adwordsClickInfo.page'] = test['trafficSource_adwordsClickInfo.page'].astype('int64')
test['trafficSource_referralPath'].fillna(0,inplace=True)
test.drop(['trafficSource_adwordsClickInfo.slot'],axis=1,inplace=True)
test['trafficSource_keyword'].fillna(0,inplace=True)
test.drop(['sessionId',
            'visitId','visitStartTime',
            'geoNetwork_region'],axis=1,inplace=True)
```


```python
test.columns
```




    Index(['channelGrouping', 'fullVisitorId', 'visitNumber', 'device_browser',
           'device_deviceCategory', 'device_isMobile', 'device_operatingSystem',
           'geoNetwork_city', 'geoNetwork_continent', 'geoNetwork_country',
           'geoNetwork_metro', 'geoNetwork_networkDomain',
           'geoNetwork_subContinent', 'totals_bounces', 'totals_hits',
           'totals_newVisits', 'totals_pageviews', 'trafficSource_adContent',
           'trafficSource_adwordsClickInfo.adNetworkType',
           'trafficSource_adwordsClickInfo.gclId',
           'trafficSource_adwordsClickInfo.page', 'trafficSource_campaign',
           'trafficSource_isTrueDirect', 'trafficSource_keyword',
           'trafficSource_medium', 'trafficSource_referralPath',
           'trafficSource_source', 'date_year', 'date_month', 'date_weekday'],
          dtype='object')



## Categorical Variables


```python
from sklearn import preprocessing
encoder = preprocessing.OneHotEncoder()
```


```python
train.columns
```




    Index(['channelGrouping', 'fullVisitorId', 'visitNumber', 'device_browser',
           'device_deviceCategory', 'device_isMobile', 'device_operatingSystem',
           'geoNetwork_city', 'geoNetwork_continent', 'geoNetwork_country',
           'geoNetwork_metro', 'geoNetwork_networkDomain',
           'geoNetwork_subContinent', 'totals_bounces', 'totals_hits',
           'totals_newVisits', 'totals_pageviews', 'totals_transactionRevenue',
           'trafficSource_adContent',
           'trafficSource_adwordsClickInfo.adNetworkType',
           'trafficSource_adwordsClickInfo.gclId',
           'trafficSource_adwordsClickInfo.page', 'trafficSource_campaign',
           'trafficSource_isTrueDirect', 'trafficSource_keyword',
           'trafficSource_medium', 'trafficSource_referralPath',
           'trafficSource_source', 'date_year', 'date_month', 'date_weekday'],
          dtype='object')




```python
test.columns
```




    Index(['channelGrouping', 'fullVisitorId', 'visitNumber', 'device_browser',
           'device_deviceCategory', 'device_isMobile', 'device_operatingSystem',
           'geoNetwork_city', 'geoNetwork_continent', 'geoNetwork_country',
           'geoNetwork_metro', 'geoNetwork_networkDomain',
           'geoNetwork_subContinent', 'totals_bounces', 'totals_hits',
           'totals_newVisits', 'totals_pageviews', 'trafficSource_adContent',
           'trafficSource_adwordsClickInfo.adNetworkType',
           'trafficSource_adwordsClickInfo.gclId',
           'trafficSource_adwordsClickInfo.page', 'trafficSource_campaign',
           'trafficSource_isTrueDirect', 'trafficSource_keyword',
           'trafficSource_medium', 'trafficSource_referralPath',
           'trafficSource_source', 'date_year', 'date_month', 'date_weekday'],
          dtype='object')



### One Hot Encoding Variables


```python
#combined = pd.get_dummies(pd.concat([train,test],keys=['tr','ts']),columns=['device_deviceCategory','geoNetwork_continent',
#                                      'trafficSource_adwordsClickInfo.adNetworkType',
#                                      'channelGrouping','date_month','date_weekday'])
```


```python
#combined.drop(['geoNetwork_continent_(not set)','trafficSource_adwordsClickInfo.adNetworkType_Unknown'],axis=1,inplace=True)
```

### Label Encoding


```python
combined = pd.concat([train,test],keys=['tr','ts'])
```

    C:\ProgramData\Anaconda3\lib\site-packages\ipykernel_launcher.py:1: FutureWarning:
    
    Sorting because non-concatenation axis is not aligned. A future version
    of pandas will change to not sort by default.
    
    To accept the future behavior, pass 'sort=True'.
    
    To retain the current behavior and silence the warning, pass sort=False
    
    
    


```python
leColumns = ['device_deviceCategory','geoNetwork_continent','trafficSource_adwordsClickInfo.adNetworkType',
                'channelGrouping','date_month','date_weekday','geoNetwork_subContinent','trafficSource_medium',
                'geoNetwork_city','geoNetwork_networkDomain','trafficSource_adContent','trafficSource_campaign',
                'trafficSource_keyword','trafficSource_source','device_operatingSystem','device_browser', 
             'geoNetwork_metro','geoNetwork_country','trafficSource_referralPath' ,
             'trafficSource_adwordsClickInfo.gclId']


for col in leColumns:
    print('Processing column ' + col)
    le = preprocessing.LabelEncoder()
    le.fit(combined[col].astype(str))
    combined[col] = le.transform(combined[col].astype(str)) 
```

    Processing column device_deviceCategory
    Processing column geoNetwork_continent
    Processing column trafficSource_adwordsClickInfo.adNetworkType
    Processing column channelGrouping
    Processing column date_month
    Processing column date_weekday
    Processing column geoNetwork_subContinent
    Processing column trafficSource_medium
    Processing column geoNetwork_city
    Processing column geoNetwork_networkDomain
    Processing column trafficSource_adContent
    Processing column trafficSource_campaign
    Processing column trafficSource_keyword
    Processing column trafficSource_source
    Processing column device_operatingSystem
    Processing column device_browser
    Processing column geoNetwork_metro
    Processing column geoNetwork_country
    Processing column trafficSource_referralPath
    Processing column trafficSource_adwordsClickInfo.gclId
    


```python
train,test = combined.xs('tr'),combined.xs('ts')
del combined
```


```python
train.columns
```




    Index(['channelGrouping', 'date_month', 'date_weekday', 'date_year',
           'device_browser', 'device_deviceCategory', 'device_isMobile',
           'device_operatingSystem', 'fullVisitorId', 'geoNetwork_city',
           'geoNetwork_continent', 'geoNetwork_country', 'geoNetwork_metro',
           'geoNetwork_networkDomain', 'geoNetwork_subContinent', 'totals_bounces',
           'totals_hits', 'totals_newVisits', 'totals_pageviews',
           'totals_transactionRevenue', 'trafficSource_adContent',
           'trafficSource_adwordsClickInfo.adNetworkType',
           'trafficSource_adwordsClickInfo.gclId',
           'trafficSource_adwordsClickInfo.page', 'trafficSource_campaign',
           'trafficSource_isTrueDirect', 'trafficSource_keyword',
           'trafficSource_medium', 'trafficSource_referralPath',
           'trafficSource_source', 'visitNumber'],
          dtype='object')




```python
test.columns
```




    Index(['channelGrouping', 'date_month', 'date_weekday', 'date_year',
           'device_browser', 'device_deviceCategory', 'device_isMobile',
           'device_operatingSystem', 'fullVisitorId', 'geoNetwork_city',
           'geoNetwork_continent', 'geoNetwork_country', 'geoNetwork_metro',
           'geoNetwork_networkDomain', 'geoNetwork_subContinent', 'totals_bounces',
           'totals_hits', 'totals_newVisits', 'totals_pageviews',
           'totals_transactionRevenue', 'trafficSource_adContent',
           'trafficSource_adwordsClickInfo.adNetworkType',
           'trafficSource_adwordsClickInfo.gclId',
           'trafficSource_adwordsClickInfo.page', 'trafficSource_campaign',
           'trafficSource_isTrueDirect', 'trafficSource_keyword',
           'trafficSource_medium', 'trafficSource_referralPath',
           'trafficSource_source', 'visitNumber'],
          dtype='object')



## Correlation


```python
plt.figure(figsize=(26,18))
sns.heatmap(train.corr(),annot=True)
```




    <matplotlib.axes._subplots.AxesSubplot at 0x1d86cb354e0>




![png](output_155_1.png)



```python
pd.DataFrame(train.corr()['totals_transactionRevenue']).abs().sort_values('totals_transactionRevenue',ascending=False).head(30)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>totals_transactionRevenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>totals_transactionRevenue</th>
      <td>1.000000</td>
    </tr>
    <tr>
      <th>totals_pageviews</th>
      <td>0.155589</td>
    </tr>
    <tr>
      <th>totals_hits</th>
      <td>0.154333</td>
    </tr>
    <tr>
      <th>visitNumber</th>
      <td>0.051366</td>
    </tr>
    <tr>
      <th>totals_newVisits</th>
      <td>0.041164</td>
    </tr>
    <tr>
      <th>totals_bounces</th>
      <td>0.032206</td>
    </tr>
    <tr>
      <th>trafficSource_isTrueDirect</th>
      <td>0.030819</td>
    </tr>
    <tr>
      <th>trafficSource_referralPath</th>
      <td>0.026763</td>
    </tr>
    <tr>
      <th>geoNetwork_continent</th>
      <td>0.025523</td>
    </tr>
    <tr>
      <th>geoNetwork_country</th>
      <td>0.022578</td>
    </tr>
    <tr>
      <th>geoNetwork_networkDomain</th>
      <td>0.020199</td>
    </tr>
    <tr>
      <th>device_isMobile</th>
      <td>0.016555</td>
    </tr>
    <tr>
      <th>device_deviceCategory</th>
      <td>0.015580</td>
    </tr>
    <tr>
      <th>device_browser</th>
      <td>0.015088</td>
    </tr>
    <tr>
      <th>device_operatingSystem</th>
      <td>0.011925</td>
    </tr>
    <tr>
      <th>geoNetwork_subContinent</th>
      <td>0.009144</td>
    </tr>
    <tr>
      <th>trafficSource_source</th>
      <td>0.008508</td>
    </tr>
    <tr>
      <th>date_weekday</th>
      <td>0.007812</td>
    </tr>
    <tr>
      <th>channelGrouping</th>
      <td>0.006644</td>
    </tr>
    <tr>
      <th>date_month</th>
      <td>0.004527</td>
    </tr>
    <tr>
      <th>geoNetwork_metro</th>
      <td>0.003989</td>
    </tr>
    <tr>
      <th>trafficSource_campaign</th>
      <td>0.003375</td>
    </tr>
    <tr>
      <th>geoNetwork_city</th>
      <td>0.003328</td>
    </tr>
    <tr>
      <th>date_year</th>
      <td>0.003194</td>
    </tr>
    <tr>
      <th>trafficSource_medium</th>
      <td>0.002520</td>
    </tr>
    <tr>
      <th>trafficSource_keyword</th>
      <td>0.000875</td>
    </tr>
    <tr>
      <th>trafficSource_adwordsClickInfo.adNetworkType</th>
      <td>0.000835</td>
    </tr>
    <tr>
      <th>trafficSource_adwordsClickInfo.gclId</th>
      <td>0.000797</td>
    </tr>
    <tr>
      <th>trafficSource_adwordsClickInfo.page</th>
      <td>0.000775</td>
    </tr>
    <tr>
      <th>trafficSource_adContent</th>
      <td>0.000648</td>
    </tr>
  </tbody>
</table>
</div>




```python
#train.drop(['trafficSource_source','geoNetwork_city','totals_hits'],axis=1,inplace=True)
#test.drop(['trafficSource_source','geoNetwork_city','totals_hits'],axis=1,inplace=True)
```


```python
train.columns
```




    Index(['channelGrouping', 'date_month', 'date_weekday', 'date_year',
           'device_browser', 'device_deviceCategory', 'device_isMobile',
           'device_operatingSystem', 'fullVisitorId', 'geoNetwork_city',
           'geoNetwork_continent', 'geoNetwork_country', 'geoNetwork_metro',
           'geoNetwork_networkDomain', 'geoNetwork_subContinent', 'totals_bounces',
           'totals_hits', 'totals_newVisits', 'totals_pageviews',
           'totals_transactionRevenue', 'trafficSource_adContent',
           'trafficSource_adwordsClickInfo.adNetworkType',
           'trafficSource_adwordsClickInfo.gclId',
           'trafficSource_adwordsClickInfo.page', 'trafficSource_campaign',
           'trafficSource_isTrueDirect', 'trafficSource_keyword',
           'trafficSource_medium', 'trafficSource_referralPath',
           'trafficSource_source', 'visitNumber'],
          dtype='object')



## Extract X and y


```python
import math
from sklearn.model_selection import train_test_split
```


```python
X = train.drop(['totals_transactionRevenue','fullVisitorId'],axis=1)
y = train['totals_transactionRevenue'].apply(lambda x:0 if x==0 else math.log(x))    
```


```python
X_train, X_val, y_train, y_val = train_test_split(X, y, test_size=0.33, random_state=42)
#X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.00001,random_state=10)
```


```python
print(len(X_train))
print(len(X_val))
```

    605447
    298206
    

### LGBM


```python
params = {'objective' : 'regression',
         'metric' :'rmse',
         'bagging_fraction' :0.5, 
          'bagging_frequency':8 ,
          'feature_fraction':0.7, 
          'learning_rate':0.01,
           'max_bin' :100, 
           'max_depth' :7, 
            'num_leaves':30}
```


```python
import lightgbm as lgb
from math import sqrt
from sklearn.metrics import mean_squared_error

lgbmReg = lgb.LGBMRegressor(**params,n_estimators=1000) 
lgbmReg.fit(X_train,y_train,eval_set=[(X_val,y_val)],early_stopping_rounds=100,verbose=30,eval_metric='rmse')
```

    Training until validation scores don't improve for 100 rounds.
    [30]	valid_0's rmse: 1.88514
    [60]	valid_0's rmse: 1.80972
    [90]	valid_0's rmse: 1.75542
    [120]	valid_0's rmse: 1.72287
    [150]	valid_0's rmse: 1.69964
    [180]	valid_0's rmse: 1.68307
    [210]	valid_0's rmse: 1.67205
    [240]	valid_0's rmse: 1.66493
    [270]	valid_0's rmse: 1.65861
    [300]	valid_0's rmse: 1.65364
    [330]	valid_0's rmse: 1.64989
    [360]	valid_0's rmse: 1.64696
    [390]	valid_0's rmse: 1.64442
    [420]	valid_0's rmse: 1.64253
    [450]	valid_0's rmse: 1.64093
    [480]	valid_0's rmse: 1.63962
    [510]	valid_0's rmse: 1.63852
    [540]	valid_0's rmse: 1.63734
    [570]	valid_0's rmse: 1.63623
    [600]	valid_0's rmse: 1.63549
    [630]	valid_0's rmse: 1.63462
    [660]	valid_0's rmse: 1.63369
    [690]	valid_0's rmse: 1.63297
    [720]	valid_0's rmse: 1.63234
    [750]	valid_0's rmse: 1.63175
    [780]	valid_0's rmse: 1.6311
    [810]	valid_0's rmse: 1.63074
    [840]	valid_0's rmse: 1.63028
    [870]	valid_0's rmse: 1.62986
    [900]	valid_0's rmse: 1.62952
    [930]	valid_0's rmse: 1.62909
    [960]	valid_0's rmse: 1.62841
    [990]	valid_0's rmse: 1.62787
    Did not meet early stopping. Best iteration is:
    [1000]	valid_0's rmse: 1.62768
    




    LGBMRegressor(bagging_fraction=0.5, bagging_frequency=8, boosting_type='gbdt',
           class_weight=None, colsample_bytree=1.0, feature_fraction=0.7,
           importance_type='split', learning_rate=0.01, max_bin=100,
           max_depth=7, metric='rmse', min_child_samples=20,
           min_child_weight=0.001, min_split_gain=0.0, n_estimators=1000,
           n_jobs=-1, num_leaves=30, objective='regression', random_state=None,
           reg_alpha=0.0, reg_lambda=0.0, silent=True, subsample=1.0,
           subsample_for_bin=200000, subsample_freq=0)




```python
imp = pd.DataFrame({'Feature':X_val.columns,'Importance':lgbmReg.booster_.feature_importance()})
imp.sort_values(by='Importance',ascending=False)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Feature</th>
      <th>Importance</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>15</th>
      <td>totals_hits</td>
      <td>4715</td>
    </tr>
    <tr>
      <th>17</th>
      <td>totals_pageviews</td>
      <td>4545</td>
    </tr>
    <tr>
      <th>1</th>
      <td>date_month</td>
      <td>2308</td>
    </tr>
    <tr>
      <th>28</th>
      <td>visitNumber</td>
      <td>2114</td>
    </tr>
    <tr>
      <th>11</th>
      <td>geoNetwork_metro</td>
      <td>1866</td>
    </tr>
    <tr>
      <th>8</th>
      <td>geoNetwork_city</td>
      <td>1756</td>
    </tr>
    <tr>
      <th>12</th>
      <td>geoNetwork_networkDomain</td>
      <td>1481</td>
    </tr>
    <tr>
      <th>10</th>
      <td>geoNetwork_country</td>
      <td>1441</td>
    </tr>
    <tr>
      <th>2</th>
      <td>date_weekday</td>
      <td>1145</td>
    </tr>
    <tr>
      <th>7</th>
      <td>device_operatingSystem</td>
      <td>1041</td>
    </tr>
    <tr>
      <th>26</th>
      <td>trafficSource_referralPath</td>
      <td>915</td>
    </tr>
    <tr>
      <th>27</th>
      <td>trafficSource_source</td>
      <td>650</td>
    </tr>
    <tr>
      <th>16</th>
      <td>totals_newVisits</td>
      <td>620</td>
    </tr>
    <tr>
      <th>3</th>
      <td>date_year</td>
      <td>604</td>
    </tr>
    <tr>
      <th>23</th>
      <td>trafficSource_isTrueDirect</td>
      <td>506</td>
    </tr>
    <tr>
      <th>6</th>
      <td>device_isMobile</td>
      <td>435</td>
    </tr>
    <tr>
      <th>9</th>
      <td>geoNetwork_continent</td>
      <td>433</td>
    </tr>
    <tr>
      <th>0</th>
      <td>channelGrouping</td>
      <td>428</td>
    </tr>
    <tr>
      <th>5</th>
      <td>device_deviceCategory</td>
      <td>344</td>
    </tr>
    <tr>
      <th>4</th>
      <td>device_browser</td>
      <td>299</td>
    </tr>
    <tr>
      <th>24</th>
      <td>trafficSource_keyword</td>
      <td>289</td>
    </tr>
    <tr>
      <th>25</th>
      <td>trafficSource_medium</td>
      <td>278</td>
    </tr>
    <tr>
      <th>14</th>
      <td>totals_bounces</td>
      <td>212</td>
    </tr>
    <tr>
      <th>20</th>
      <td>trafficSource_adwordsClickInfo.gclId</td>
      <td>197</td>
    </tr>
    <tr>
      <th>13</th>
      <td>geoNetwork_subContinent</td>
      <td>164</td>
    </tr>
    <tr>
      <th>22</th>
      <td>trafficSource_campaign</td>
      <td>39</td>
    </tr>
    <tr>
      <th>18</th>
      <td>trafficSource_adContent</td>
      <td>34</td>
    </tr>
    <tr>
      <th>19</th>
      <td>trafficSource_adwordsClickInfo.adNetworkType</td>
      <td>13</td>
    </tr>
    <tr>
      <th>21</th>
      <td>trafficSource_adwordsClickInfo.page</td>
      <td>2</td>
    </tr>
  </tbody>
</table>
</div>




```python
plt.figure(figsize=(14,20))
sns.barplot(data=imp.sort_values(by='Importance',ascending=False),x='Importance',y='Feature')
```




    <matplotlib.axes._subplots.AxesSubplot at 0x1d86c7a4a58>




![png](output_168_1.png)



```python
XSub = test.drop(['totals_transactionRevenue','fullVisitorId'],axis=1)
ySubmission_gb_pred = lgbmReg.predict(XSub,num_iteration=lgbmReg.best_iteration_)
submission = pd.DataFrame({"fullVisitorId":test['fullVisitorId'],
                          "PredictedLogRevenue":ySubmission_gb_pred})
submission['PredictedLogRevenue'] = submission['PredictedLogRevenue'].apply(lambda x:0.0 if x<0 else x)
submissionByVisitor = submission.groupby('fullVisitorId').sum()
submissionByVisitor.to_csv("submission.csv")
```