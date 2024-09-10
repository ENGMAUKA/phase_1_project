# phase_1_project

# PROJECT OVERVIEW

The complex ecosystem of airports, supporting organisations, and aircraft used for general aviation, commercial aviation, and military activities is referred to as the aviation industry. General aviation incorporates private air travel wheteher for business or pleasure. Commercial aviation focusses on public transportation of passengers and cargo. On the other hand, millitary aviation encompasses all flights operated by the armed forces. 

4.6% of Kenya's yearly GDP is contributed by the aviation sector (KNBS, 2022). In 2017, Kenya handled over 4.6 million passenger travels, resulting in a $3.2 billion gross value addition to the country's GDP from the tourism and aviation sectors. This also translated into 410,000 jobs.

Internationally, the commercial aviation sector experienced 30 accidents in 2023 as compared to 42 accidents in 2022.Therefore, between 2022 and 2023, the overall accident rate dropped from 1.30 per million sectors to 0.80. Overall, one accident occurred per every 880,293 flights on average (IATA Annual Safety Report, 2023). According to statistics, human error is to blame for up to 80% of all aviation accidents. Takeoff and landing, as well as the moments just before and after, are the riskiest times.

According to a 2022 security audit conducted by the International Civil Aviation Organisation (ICAO), Kenya has the second-best aviation safety standards in Africa, scoring 91.77%.

# BUSINESS UNDERSTANDING

Maureen Inc. is diversifying its holdings and considers entry into the aviation industry. Its specific interest is in the genearal and commericial services. The company plans to procure aircrafts for the new venture. To help in decision making, the company would like insihgts on the aircrafts that would have the lowest risk for the aviation division. 

#DATA UNDERSTANDING

## Problem Statement
Identify the least risky planes (aircrafts) for Maureen Inc. to launch its new venture

## Metrics of Success
The project will be a success if i am able to identify low risk aircrafts and the factors affecting aircraft safety.

## Methodology
The analysis adopts the first three phases of CRISP - DM that is Domain knowledge (Business Understanding), Data Understanding and Data Preparation.

## Data Description

Importing Libraries


```python
#importing libraries 
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
```

Reading the dataset from csv files


```python
#Reading data from the CSV file

df = pd.read_csv('Data/AviationData.csv')
df1 = pd.read_csv('Data/USState_Codes.csv')
```

    /opt/anaconda3/envs/learn-env/lib/python3.8/site-packages/IPython/core/interactiveshell.py:3145: DtypeWarning: Columns (6,7,28) have mixed types.Specify dtype option on import or set low_memory=False.
      has_raised = await self.run_ast_nodes(code_ast.body, cell_name,


Previewing the Aviation Data Set


```python
#first five rows
df.head()
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
      <th>Event.Id</th>
      <th>Investigation.Type</th>
      <th>Accident.Number</th>
      <th>Event.Date</th>
      <th>Location</th>
      <th>Country</th>
      <th>Latitude</th>
      <th>Longitude</th>
      <th>Airport.Code</th>
      <th>Airport.Name</th>
      <th>...</th>
      <th>Purpose.of.flight</th>
      <th>Air.carrier</th>
      <th>Total.Fatal.Injuries</th>
      <th>Total.Serious.Injuries</th>
      <th>Total.Minor.Injuries</th>
      <th>Total.Uninjured</th>
      <th>Weather.Condition</th>
      <th>Broad.phase.of.flight</th>
      <th>Report.Status</th>
      <th>Publication.Date</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>20001218X45444</td>
      <td>Accident</td>
      <td>SEA87LA080</td>
      <td>1948-10-24</td>
      <td>MOOSE CREEK, ID</td>
      <td>United States</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>Personal</td>
      <td>NaN</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>UNK</td>
      <td>Cruise</td>
      <td>Probable Cause</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>20001218X45447</td>
      <td>Accident</td>
      <td>LAX94LA336</td>
      <td>1962-07-19</td>
      <td>BRIDGEPORT, CA</td>
      <td>United States</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>Personal</td>
      <td>NaN</td>
      <td>4.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>UNK</td>
      <td>Unknown</td>
      <td>Probable Cause</td>
      <td>19-09-1996</td>
    </tr>
    <tr>
      <th>2</th>
      <td>20061025X01555</td>
      <td>Accident</td>
      <td>NYC07LA005</td>
      <td>1974-08-30</td>
      <td>Saltville, VA</td>
      <td>United States</td>
      <td>36.922223</td>
      <td>-81.878056</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>Personal</td>
      <td>NaN</td>
      <td>3.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>IMC</td>
      <td>Cruise</td>
      <td>Probable Cause</td>
      <td>26-02-2007</td>
    </tr>
    <tr>
      <th>3</th>
      <td>20001218X45448</td>
      <td>Accident</td>
      <td>LAX96LA321</td>
      <td>1977-06-19</td>
      <td>EUREKA, CA</td>
      <td>United States</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>Personal</td>
      <td>NaN</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>IMC</td>
      <td>Cruise</td>
      <td>Probable Cause</td>
      <td>12-09-2000</td>
    </tr>
    <tr>
      <th>4</th>
      <td>20041105X01764</td>
      <td>Accident</td>
      <td>CHI79FA064</td>
      <td>1979-08-02</td>
      <td>Canton, OH</td>
      <td>United States</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>Personal</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>2.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>VMC</td>
      <td>Approach</td>
      <td>Probable Cause</td>
      <td>16-04-1980</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 31 columns</p>
</div>




```python
#Last five rows
df.tail()
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
      <th>Event.Id</th>
      <th>Investigation.Type</th>
      <th>Accident.Number</th>
      <th>Event.Date</th>
      <th>Location</th>
      <th>Country</th>
      <th>Latitude</th>
      <th>Longitude</th>
      <th>Airport.Code</th>
      <th>Airport.Name</th>
      <th>...</th>
      <th>Purpose.of.flight</th>
      <th>Air.carrier</th>
      <th>Total.Fatal.Injuries</th>
      <th>Total.Serious.Injuries</th>
      <th>Total.Minor.Injuries</th>
      <th>Total.Uninjured</th>
      <th>Weather.Condition</th>
      <th>Broad.phase.of.flight</th>
      <th>Report.Status</th>
      <th>Publication.Date</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>88884</th>
      <td>20221227106491</td>
      <td>Accident</td>
      <td>ERA23LA093</td>
      <td>2022-12-26</td>
      <td>Annapolis, MD</td>
      <td>United States</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>Personal</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>29-12-2022</td>
    </tr>
    <tr>
      <th>88885</th>
      <td>20221227106494</td>
      <td>Accident</td>
      <td>ERA23LA095</td>
      <td>2022-12-26</td>
      <td>Hampton, NH</td>
      <td>United States</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>88886</th>
      <td>20221227106497</td>
      <td>Accident</td>
      <td>WPR23LA075</td>
      <td>2022-12-26</td>
      <td>Payson, AZ</td>
      <td>United States</td>
      <td>341525N</td>
      <td>1112021W</td>
      <td>PAN</td>
      <td>PAYSON</td>
      <td>...</td>
      <td>Personal</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>VMC</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>27-12-2022</td>
    </tr>
    <tr>
      <th>88887</th>
      <td>20221227106498</td>
      <td>Accident</td>
      <td>WPR23LA076</td>
      <td>2022-12-26</td>
      <td>Morgan, UT</td>
      <td>United States</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>Personal</td>
      <td>MC CESSNA 210N LLC</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>88888</th>
      <td>20221230106513</td>
      <td>Accident</td>
      <td>ERA23LA097</td>
      <td>2022-12-29</td>
      <td>Athens, GA</td>
      <td>United States</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>Personal</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>30-12-2022</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 31 columns</p>
</div>



Accessing information about the data set


```python
#Information about the dataset
df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 88889 entries, 0 to 88888
    Data columns (total 31 columns):
     #   Column                  Non-Null Count  Dtype  
    ---  ------                  --------------  -----  
     0   Event.Id                88889 non-null  object 
     1   Investigation.Type      88889 non-null  object 
     2   Accident.Number         88889 non-null  object 
     3   Event.Date              88889 non-null  object 
     4   Location                88837 non-null  object 
     5   Country                 88663 non-null  object 
     6   Latitude                34382 non-null  object 
     7   Longitude               34373 non-null  object 
     8   Airport.Code            50249 non-null  object 
     9   Airport.Name            52790 non-null  object 
     10  Injury.Severity         87889 non-null  object 
     11  Aircraft.damage         85695 non-null  object 
     12  Aircraft.Category       32287 non-null  object 
     13  Registration.Number     87572 non-null  object 
     14  Make                    88826 non-null  object 
     15  Model                   88797 non-null  object 
     16  Amateur.Built           88787 non-null  object 
     17  Number.of.Engines       82805 non-null  float64
     18  Engine.Type             81812 non-null  object 
     19  FAR.Description         32023 non-null  object 
     20  Schedule                12582 non-null  object 
     21  Purpose.of.flight       82697 non-null  object 
     22  Air.carrier             16648 non-null  object 
     23  Total.Fatal.Injuries    77488 non-null  float64
     24  Total.Serious.Injuries  76379 non-null  float64
     25  Total.Minor.Injuries    76956 non-null  float64
     26  Total.Uninjured         82977 non-null  float64
     27  Weather.Condition       84397 non-null  object 
     28  Broad.phase.of.flight   61724 non-null  object 
     29  Report.Status           82508 non-null  object 
     30  Publication.Date        75118 non-null  object 
    dtypes: float64(5), object(26)
    memory usage: 21.0+ MB


The dataset has 88889 entries with 31 columns. A number of the columns had missing values e.g. Airport Code, Aircraft Category, Engine Type, Aircraft Dmage etc

The dataset is made up of two main data types, 5 columns of integer (float) type and 25 columns of type string (object)


```python
df.shape
```




    (88889, 31)



The data set ia made up of 88889 rows and 31 columns


```python
#names of the data columns
df.columns
```




    Index(['Event.Id', 'Investigation.Type', 'Accident.Number', 'Event.Date',
           'Location', 'Country', 'Latitude', 'Longitude', 'Airport.Code',
           'Airport.Name', 'Injury.Severity', 'Aircraft.damage',
           'Aircraft.Category', 'Registration.Number', 'Make', 'Model',
           'Amateur.Built', 'Number.of.Engines', 'Engine.Type', 'FAR.Description',
           'Schedule', 'Purpose.of.flight', 'Air.carrier', 'Total.Fatal.Injuries',
           'Total.Serious.Injuries', 'Total.Minor.Injuries', 'Total.Uninjured',
           'Weather.Condition', 'Broad.phase.of.flight', 'Report.Status',
           'Publication.Date'],
          dtype='object')



Summary Statistics


```python
df.describe()
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
      <th>Number.of.Engines</th>
      <th>Total.Fatal.Injuries</th>
      <th>Total.Serious.Injuries</th>
      <th>Total.Minor.Injuries</th>
      <th>Total.Uninjured</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>82805.000000</td>
      <td>77488.000000</td>
      <td>76379.000000</td>
      <td>76956.000000</td>
      <td>82977.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>1.146585</td>
      <td>0.647855</td>
      <td>0.279881</td>
      <td>0.357061</td>
      <td>5.325440</td>
    </tr>
    <tr>
      <th>std</th>
      <td>0.446510</td>
      <td>5.485960</td>
      <td>1.544084</td>
      <td>2.235625</td>
      <td>27.913634</td>
    </tr>
    <tr>
      <th>min</th>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>1.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>1.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>1.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>2.000000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>8.000000</td>
      <td>349.000000</td>
      <td>161.000000</td>
      <td>380.000000</td>
      <td>699.000000</td>
    </tr>
  </tbody>
</table>
</div>



A total of 77,488 fatalities were reported from aircraft accident between 1962 and 2023. Serious injuries reported from the accidents during the same period were 76,379 compared to 76,956 minor injuries. 82,977 were uninjured from the incidents. The aircrafts under analysis had an average of one engine 

Creating a deep copy of the dataframe df.


```python
#Deep copy copies the original data frame and assigns it to a new dataframe with new names
#The deep copy is a true replica of the original dataset and independent of the original. The deep copy has its own memory loaction and data.
# Any changes make on the original dataset will not affect the copy and vice versa.

df2 = df.copy(deep=True) 
```

# DATA PREPARATION

## Data Cleaning

### Validity Test/Challenges

a) Dropping irrelevant observations


```python
df2.drop(['Publication.Date', 'Schedule', 'Latitude', 'Longitude', 'FAR.Description', 'Airport.Name', 'Airport.Code', 'Aircraft.Category', 'Air.carrier' ], axis=1, inplace=True)
df2.head()
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
      <th>Event.Id</th>
      <th>Investigation.Type</th>
      <th>Accident.Number</th>
      <th>Event.Date</th>
      <th>Location</th>
      <th>Country</th>
      <th>Injury.Severity</th>
      <th>Aircraft.damage</th>
      <th>Registration.Number</th>
      <th>Make</th>
      <th>...</th>
      <th>Number.of.Engines</th>
      <th>Engine.Type</th>
      <th>Purpose.of.flight</th>
      <th>Total.Fatal.Injuries</th>
      <th>Total.Serious.Injuries</th>
      <th>Total.Minor.Injuries</th>
      <th>Total.Uninjured</th>
      <th>Weather.Condition</th>
      <th>Broad.phase.of.flight</th>
      <th>Report.Status</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>20001218X45444</td>
      <td>Accident</td>
      <td>SEA87LA080</td>
      <td>1948-10-24</td>
      <td>MOOSE CREEK, ID</td>
      <td>United States</td>
      <td>Fatal(2)</td>
      <td>Destroyed</td>
      <td>NC6404</td>
      <td>Stinson</td>
      <td>...</td>
      <td>1.0</td>
      <td>Reciprocating</td>
      <td>Personal</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>UNK</td>
      <td>Cruise</td>
      <td>Probable Cause</td>
    </tr>
    <tr>
      <th>1</th>
      <td>20001218X45447</td>
      <td>Accident</td>
      <td>LAX94LA336</td>
      <td>1962-07-19</td>
      <td>BRIDGEPORT, CA</td>
      <td>United States</td>
      <td>Fatal(4)</td>
      <td>Destroyed</td>
      <td>N5069P</td>
      <td>Piper</td>
      <td>...</td>
      <td>1.0</td>
      <td>Reciprocating</td>
      <td>Personal</td>
      <td>4.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>UNK</td>
      <td>Unknown</td>
      <td>Probable Cause</td>
    </tr>
    <tr>
      <th>2</th>
      <td>20061025X01555</td>
      <td>Accident</td>
      <td>NYC07LA005</td>
      <td>1974-08-30</td>
      <td>Saltville, VA</td>
      <td>United States</td>
      <td>Fatal(3)</td>
      <td>Destroyed</td>
      <td>N5142R</td>
      <td>Cessna</td>
      <td>...</td>
      <td>1.0</td>
      <td>Reciprocating</td>
      <td>Personal</td>
      <td>3.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>IMC</td>
      <td>Cruise</td>
      <td>Probable Cause</td>
    </tr>
    <tr>
      <th>3</th>
      <td>20001218X45448</td>
      <td>Accident</td>
      <td>LAX96LA321</td>
      <td>1977-06-19</td>
      <td>EUREKA, CA</td>
      <td>United States</td>
      <td>Fatal(2)</td>
      <td>Destroyed</td>
      <td>N1168J</td>
      <td>Rockwell</td>
      <td>...</td>
      <td>1.0</td>
      <td>Reciprocating</td>
      <td>Personal</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>IMC</td>
      <td>Cruise</td>
      <td>Probable Cause</td>
    </tr>
    <tr>
      <th>4</th>
      <td>20041105X01764</td>
      <td>Accident</td>
      <td>CHI79FA064</td>
      <td>1979-08-02</td>
      <td>Canton, OH</td>
      <td>United States</td>
      <td>Fatal(1)</td>
      <td>Destroyed</td>
      <td>N15NY</td>
      <td>Cessna</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Personal</td>
      <td>1.0</td>
      <td>2.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>VMC</td>
      <td>Approach</td>
      <td>Probable Cause</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 22 columns</p>
</div>




```python
df2.columns
```




    Index(['Event.Id', 'Investigation.Type', 'Accident.Number', 'Event.Date',
           'Location', 'Country', 'Injury.Severity', 'Aircraft.damage',
           'Registration.Number', 'Make', 'Model', 'Amateur.Built',
           'Number.of.Engines', 'Engine.Type', 'Purpose.of.flight',
           'Total.Fatal.Injuries', 'Total.Serious.Injuries',
           'Total.Minor.Injuries', 'Total.Uninjured', 'Weather.Condition',
           'Broad.phase.of.flight', 'Report.Status'],
          dtype='object')



Syntax Errors:
b) Removing leading and trailing spaces in columns


```python
df2.columns = df2.columns.str.strip()
df2.head()
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
      <th>Event.Id</th>
      <th>Investigation.Type</th>
      <th>Accident.Number</th>
      <th>Event.Date</th>
      <th>Location</th>
      <th>Country</th>
      <th>Injury.Severity</th>
      <th>Aircraft.damage</th>
      <th>Registration.Number</th>
      <th>Make</th>
      <th>...</th>
      <th>Number.of.Engines</th>
      <th>Engine.Type</th>
      <th>Purpose.of.flight</th>
      <th>Total.Fatal.Injuries</th>
      <th>Total.Serious.Injuries</th>
      <th>Total.Minor.Injuries</th>
      <th>Total.Uninjured</th>
      <th>Weather.Condition</th>
      <th>Broad.phase.of.flight</th>
      <th>Report.Status</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>20001218X45444</td>
      <td>Accident</td>
      <td>SEA87LA080</td>
      <td>1948-10-24</td>
      <td>MOOSE CREEK, ID</td>
      <td>United States</td>
      <td>Fatal(2)</td>
      <td>Destroyed</td>
      <td>NC6404</td>
      <td>Stinson</td>
      <td>...</td>
      <td>1.0</td>
      <td>Reciprocating</td>
      <td>Personal</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>UNK</td>
      <td>Cruise</td>
      <td>Probable Cause</td>
    </tr>
    <tr>
      <th>1</th>
      <td>20001218X45447</td>
      <td>Accident</td>
      <td>LAX94LA336</td>
      <td>1962-07-19</td>
      <td>BRIDGEPORT, CA</td>
      <td>United States</td>
      <td>Fatal(4)</td>
      <td>Destroyed</td>
      <td>N5069P</td>
      <td>Piper</td>
      <td>...</td>
      <td>1.0</td>
      <td>Reciprocating</td>
      <td>Personal</td>
      <td>4.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>UNK</td>
      <td>Unknown</td>
      <td>Probable Cause</td>
    </tr>
    <tr>
      <th>2</th>
      <td>20061025X01555</td>
      <td>Accident</td>
      <td>NYC07LA005</td>
      <td>1974-08-30</td>
      <td>Saltville, VA</td>
      <td>United States</td>
      <td>Fatal(3)</td>
      <td>Destroyed</td>
      <td>N5142R</td>
      <td>Cessna</td>
      <td>...</td>
      <td>1.0</td>
      <td>Reciprocating</td>
      <td>Personal</td>
      <td>3.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>IMC</td>
      <td>Cruise</td>
      <td>Probable Cause</td>
    </tr>
    <tr>
      <th>3</th>
      <td>20001218X45448</td>
      <td>Accident</td>
      <td>LAX96LA321</td>
      <td>1977-06-19</td>
      <td>EUREKA, CA</td>
      <td>United States</td>
      <td>Fatal(2)</td>
      <td>Destroyed</td>
      <td>N1168J</td>
      <td>Rockwell</td>
      <td>...</td>
      <td>1.0</td>
      <td>Reciprocating</td>
      <td>Personal</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>IMC</td>
      <td>Cruise</td>
      <td>Probable Cause</td>
    </tr>
    <tr>
      <th>4</th>
      <td>20041105X01764</td>
      <td>Accident</td>
      <td>CHI79FA064</td>
      <td>1979-08-02</td>
      <td>Canton, OH</td>
      <td>United States</td>
      <td>Fatal(1)</td>
      <td>Destroyed</td>
      <td>N15NY</td>
      <td>Cessna</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Personal</td>
      <td>1.0</td>
      <td>2.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>VMC</td>
      <td>Approach</td>
      <td>Probable Cause</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 22 columns</p>
</div>



c) Removing . between column names and replacing with space


```python
df2.columns = df2.columns.str.replace('.', ' ')
df2.columns
```

    <ipython-input-64-3410bcadee4b>:1: FutureWarning: The default value of regex will change from True to False in a future version. In addition, single character regular expressions will*not* be treated as literal strings when regex=True.
      df2.columns = df2.columns.str.replace('.', ' ')





    Index(['Event Id', 'Investigation Type', 'Accident Number', 'Event Date',
           'Location', 'Country', 'Injury Severity', 'Aircraft damage',
           'Registration Number', 'Make', 'Model', 'Amateur Built',
           'Number of Engines', 'Engine Type', 'Purpose of flight',
           'Total Fatal Injuries', 'Total Serious Injuries',
           'Total Minor Injuries', 'Total Uninjured', 'Weather Condition',
           'Broad phase of flight', 'Report Status'],
          dtype='object')




```python
df2.dtypes
```




    Event Id                   object
    Investigation Type         object
    Accident Number            object
    Event Date                 object
    Location                   object
    Country                    object
    Injury Severity            object
    Aircraft damage            object
    Registration Number        object
    Make                       object
    Model                      object
    Amateur Built              object
    Number of Engines         float64
    Engine Type                object
    Purpose of flight          object
    Total Fatal Injuries      float64
    Total Serious Injuries    float64
    Total Minor Injuries      float64
    Total Uninjured           float64
    Weather Condition          object
    Broad phase of flight      object
    Report Status              object
    dtype: object



d) Convert Event date from string to date data type


```python
df2['Event Date'] = pd.to_datetime(df2['Event Date'])
df2.dtypes
```




    Event Id                          object
    Investigation Type                object
    Accident Number                   object
    Event Date                datetime64[ns]
    Location                          object
    Country                           object
    Injury Severity                   object
    Aircraft damage                   object
    Registration Number               object
    Make                              object
    Model                             object
    Amateur Built                     object
    Number of Engines                float64
    Engine Type                       object
    Purpose of flight                 object
    Total Fatal Injuries             float64
    Total Serious Injuries           float64
    Total Minor Injuries             float64
    Total Uninjured                  float64
    Weather Condition                 object
    Broad phase of flight             object
    Report Status                     object
    dtype: object




```python
df2.head()
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
      <th>Event Id</th>
      <th>Investigation Type</th>
      <th>Accident Number</th>
      <th>Event Date</th>
      <th>Location</th>
      <th>Country</th>
      <th>Injury Severity</th>
      <th>Aircraft damage</th>
      <th>Registration Number</th>
      <th>Make</th>
      <th>...</th>
      <th>Number of Engines</th>
      <th>Engine Type</th>
      <th>Purpose of flight</th>
      <th>Total Fatal Injuries</th>
      <th>Total Serious Injuries</th>
      <th>Total Minor Injuries</th>
      <th>Total Uninjured</th>
      <th>Weather Condition</th>
      <th>Broad phase of flight</th>
      <th>Report Status</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>20001218X45444</td>
      <td>Accident</td>
      <td>SEA87LA080</td>
      <td>1948-10-24</td>
      <td>MOOSE CREEK, ID</td>
      <td>United States</td>
      <td>Fatal(2)</td>
      <td>Destroyed</td>
      <td>NC6404</td>
      <td>Stinson</td>
      <td>...</td>
      <td>1.0</td>
      <td>Reciprocating</td>
      <td>Personal</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>UNK</td>
      <td>Cruise</td>
      <td>Probable Cause</td>
    </tr>
    <tr>
      <th>1</th>
      <td>20001218X45447</td>
      <td>Accident</td>
      <td>LAX94LA336</td>
      <td>1962-07-19</td>
      <td>BRIDGEPORT, CA</td>
      <td>United States</td>
      <td>Fatal(4)</td>
      <td>Destroyed</td>
      <td>N5069P</td>
      <td>Piper</td>
      <td>...</td>
      <td>1.0</td>
      <td>Reciprocating</td>
      <td>Personal</td>
      <td>4.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>UNK</td>
      <td>Unknown</td>
      <td>Probable Cause</td>
    </tr>
    <tr>
      <th>2</th>
      <td>20061025X01555</td>
      <td>Accident</td>
      <td>NYC07LA005</td>
      <td>1974-08-30</td>
      <td>Saltville, VA</td>
      <td>United States</td>
      <td>Fatal(3)</td>
      <td>Destroyed</td>
      <td>N5142R</td>
      <td>Cessna</td>
      <td>...</td>
      <td>1.0</td>
      <td>Reciprocating</td>
      <td>Personal</td>
      <td>3.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>IMC</td>
      <td>Cruise</td>
      <td>Probable Cause</td>
    </tr>
    <tr>
      <th>3</th>
      <td>20001218X45448</td>
      <td>Accident</td>
      <td>LAX96LA321</td>
      <td>1977-06-19</td>
      <td>EUREKA, CA</td>
      <td>United States</td>
      <td>Fatal(2)</td>
      <td>Destroyed</td>
      <td>N1168J</td>
      <td>Rockwell</td>
      <td>...</td>
      <td>1.0</td>
      <td>Reciprocating</td>
      <td>Personal</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>IMC</td>
      <td>Cruise</td>
      <td>Probable Cause</td>
    </tr>
    <tr>
      <th>4</th>
      <td>20041105X01764</td>
      <td>Accident</td>
      <td>CHI79FA064</td>
      <td>1979-08-02</td>
      <td>Canton, OH</td>
      <td>United States</td>
      <td>Fatal(1)</td>
      <td>Destroyed</td>
      <td>N15NY</td>
      <td>Cessna</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Personal</td>
      <td>1.0</td>
      <td>2.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>VMC</td>
      <td>Approach</td>
      <td>Probable Cause</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 22 columns</p>
</div>




```python
df2['Number of Engines'].unique()
```




    array([ 1., nan,  2.,  0.,  3.,  4.,  8.,  6.])




```python
#Adding a new column for total Injuiries
df2['Total Injuries'] = df2['Total Fatal Injuries'] + df2['Total Serious Injuries'] + df2['Total Minor Injuries']
df2['Total Injuries']
```




    0        2.0
    1        4.0
    2        NaN
    3        2.0
    4        NaN
            ... 
    88884    1.0
    88885    0.0
    88886    0.0
    88887    0.0
    88888    1.0
    Name: Total Injuries, Length: 88889, dtype: float64




```python
df2.head()
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
      <th>Event Id</th>
      <th>Investigation Type</th>
      <th>Accident Number</th>
      <th>Event Date</th>
      <th>Location</th>
      <th>Country</th>
      <th>Injury Severity</th>
      <th>Aircraft damage</th>
      <th>Registration Number</th>
      <th>Make</th>
      <th>...</th>
      <th>Engine Type</th>
      <th>Purpose of flight</th>
      <th>Total Fatal Injuries</th>
      <th>Total Serious Injuries</th>
      <th>Total Minor Injuries</th>
      <th>Total Uninjured</th>
      <th>Weather Condition</th>
      <th>Broad phase of flight</th>
      <th>Report Status</th>
      <th>Total Injuries</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>20001218X45444</td>
      <td>Accident</td>
      <td>SEA87LA080</td>
      <td>1948-10-24</td>
      <td>MOOSE CREEK, ID</td>
      <td>United States</td>
      <td>Fatal(2)</td>
      <td>Destroyed</td>
      <td>NC6404</td>
      <td>Stinson</td>
      <td>...</td>
      <td>Reciprocating</td>
      <td>Personal</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>UNK</td>
      <td>Cruise</td>
      <td>Probable Cause</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>20001218X45447</td>
      <td>Accident</td>
      <td>LAX94LA336</td>
      <td>1962-07-19</td>
      <td>BRIDGEPORT, CA</td>
      <td>United States</td>
      <td>Fatal(4)</td>
      <td>Destroyed</td>
      <td>N5069P</td>
      <td>Piper</td>
      <td>...</td>
      <td>Reciprocating</td>
      <td>Personal</td>
      <td>4.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>UNK</td>
      <td>Unknown</td>
      <td>Probable Cause</td>
      <td>4.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>20061025X01555</td>
      <td>Accident</td>
      <td>NYC07LA005</td>
      <td>1974-08-30</td>
      <td>Saltville, VA</td>
      <td>United States</td>
      <td>Fatal(3)</td>
      <td>Destroyed</td>
      <td>N5142R</td>
      <td>Cessna</td>
      <td>...</td>
      <td>Reciprocating</td>
      <td>Personal</td>
      <td>3.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>IMC</td>
      <td>Cruise</td>
      <td>Probable Cause</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>20001218X45448</td>
      <td>Accident</td>
      <td>LAX96LA321</td>
      <td>1977-06-19</td>
      <td>EUREKA, CA</td>
      <td>United States</td>
      <td>Fatal(2)</td>
      <td>Destroyed</td>
      <td>N1168J</td>
      <td>Rockwell</td>
      <td>...</td>
      <td>Reciprocating</td>
      <td>Personal</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>IMC</td>
      <td>Cruise</td>
      <td>Probable Cause</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>20041105X01764</td>
      <td>Accident</td>
      <td>CHI79FA064</td>
      <td>1979-08-02</td>
      <td>Canton, OH</td>
      <td>United States</td>
      <td>Fatal(1)</td>
      <td>Destroyed</td>
      <td>N15NY</td>
      <td>Cessna</td>
      <td>...</td>
      <td>NaN</td>
      <td>Personal</td>
      <td>1.0</td>
      <td>2.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>VMC</td>
      <td>Approach</td>
      <td>Probable Cause</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 23 columns</p>
</div>



### Accuracy Test/Challenges

Identifying and clearing outliers


```python
df2.boxplot(column=['Total Serious Injuries'], grid=False)
```




    <AxesSubplot:>




    
![png](output_36_1.png)
    



```python
df2.boxplot(column=['Total Fatal Injuries'], grid=False)
```




    <AxesSubplot:>




    
![png](output_37_1.png)
    



```python
df2.boxplot(column=['Total Minor Injuries'], grid=False)
```




    <AxesSubplot:>




    
![png](output_38_1.png)
    



```python
df2.boxplot(column=['Total Uninjured'], grid=False)
```




    <AxesSubplot:>




    
![png](output_39_1.png)
    



```python
# definition a function for handling outliers
def handle_outliers(df2, column):
    Q1 = df2[column].quantile(0.25)
    Q3 = df2[column].quantile(0.75)
    IQR = Q3 - Q1
    lower_bound = Q1 - 1.5 * IQR
    upper_bound = Q3 - 1.5 * IQR
    df2[column] = df2[column].clip(lower=lower_bound, upper=upper_bound)
    return df2

```


```python
df2 = handle_outliers(df2, 'Total Minor Injuries')
df2
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
      <th>Event Id</th>
      <th>Investigation Type</th>
      <th>Accident Number</th>
      <th>Event Date</th>
      <th>Location</th>
      <th>Country</th>
      <th>Injury Severity</th>
      <th>Aircraft damage</th>
      <th>Registration Number</th>
      <th>Make</th>
      <th>...</th>
      <th>Engine Type</th>
      <th>Purpose of flight</th>
      <th>Total Fatal Injuries</th>
      <th>Total Serious Injuries</th>
      <th>Total Minor Injuries</th>
      <th>Total Uninjured</th>
      <th>Weather Condition</th>
      <th>Broad phase of flight</th>
      <th>Report Status</th>
      <th>Total Injuries</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>20001218X45444</td>
      <td>Accident</td>
      <td>SEA87LA080</td>
      <td>1948-10-24</td>
      <td>MOOSE CREEK, ID</td>
      <td>United States</td>
      <td>Fatal(2)</td>
      <td>Destroyed</td>
      <td>NC6404</td>
      <td>Stinson</td>
      <td>...</td>
      <td>Reciprocating</td>
      <td>Personal</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>UNK</td>
      <td>Cruise</td>
      <td>Probable Cause</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>20001218X45447</td>
      <td>Accident</td>
      <td>LAX94LA336</td>
      <td>1962-07-19</td>
      <td>BRIDGEPORT, CA</td>
      <td>United States</td>
      <td>Fatal(4)</td>
      <td>Destroyed</td>
      <td>N5069P</td>
      <td>Piper</td>
      <td>...</td>
      <td>Reciprocating</td>
      <td>Personal</td>
      <td>4.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>UNK</td>
      <td>Unknown</td>
      <td>Probable Cause</td>
      <td>4.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>20061025X01555</td>
      <td>Accident</td>
      <td>NYC07LA005</td>
      <td>1974-08-30</td>
      <td>Saltville, VA</td>
      <td>United States</td>
      <td>Fatal(3)</td>
      <td>Destroyed</td>
      <td>N5142R</td>
      <td>Cessna</td>
      <td>...</td>
      <td>Reciprocating</td>
      <td>Personal</td>
      <td>3.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>IMC</td>
      <td>Cruise</td>
      <td>Probable Cause</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>20001218X45448</td>
      <td>Accident</td>
      <td>LAX96LA321</td>
      <td>1977-06-19</td>
      <td>EUREKA, CA</td>
      <td>United States</td>
      <td>Fatal(2)</td>
      <td>Destroyed</td>
      <td>N1168J</td>
      <td>Rockwell</td>
      <td>...</td>
      <td>Reciprocating</td>
      <td>Personal</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>IMC</td>
      <td>Cruise</td>
      <td>Probable Cause</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>20041105X01764</td>
      <td>Accident</td>
      <td>CHI79FA064</td>
      <td>1979-08-02</td>
      <td>Canton, OH</td>
      <td>United States</td>
      <td>Fatal(1)</td>
      <td>Destroyed</td>
      <td>N15NY</td>
      <td>Cessna</td>
      <td>...</td>
      <td>NaN</td>
      <td>Personal</td>
      <td>1.0</td>
      <td>2.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>VMC</td>
      <td>Approach</td>
      <td>Probable Cause</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>88884</th>
      <td>20221227106491</td>
      <td>Accident</td>
      <td>ERA23LA093</td>
      <td>2022-12-26</td>
      <td>Annapolis, MD</td>
      <td>United States</td>
      <td>Minor</td>
      <td>NaN</td>
      <td>N1867H</td>
      <td>PIPER</td>
      <td>...</td>
      <td>NaN</td>
      <td>Personal</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>88885</th>
      <td>20221227106494</td>
      <td>Accident</td>
      <td>ERA23LA095</td>
      <td>2022-12-26</td>
      <td>Hampton, NH</td>
      <td>United States</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>N2895Z</td>
      <td>BELLANCA</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>88886</th>
      <td>20221227106497</td>
      <td>Accident</td>
      <td>WPR23LA075</td>
      <td>2022-12-26</td>
      <td>Payson, AZ</td>
      <td>United States</td>
      <td>Non-Fatal</td>
      <td>Substantial</td>
      <td>N749PJ</td>
      <td>AMERICAN CHAMPION AIRCRAFT</td>
      <td>...</td>
      <td>NaN</td>
      <td>Personal</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>VMC</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>88887</th>
      <td>20221227106498</td>
      <td>Accident</td>
      <td>WPR23LA076</td>
      <td>2022-12-26</td>
      <td>Morgan, UT</td>
      <td>United States</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>N210CU</td>
      <td>CESSNA</td>
      <td>...</td>
      <td>NaN</td>
      <td>Personal</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>88888</th>
      <td>20221230106513</td>
      <td>Accident</td>
      <td>ERA23LA097</td>
      <td>2022-12-29</td>
      <td>Athens, GA</td>
      <td>United States</td>
      <td>Minor</td>
      <td>NaN</td>
      <td>N9026P</td>
      <td>PIPER</td>
      <td>...</td>
      <td>NaN</td>
      <td>Personal</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.0</td>
    </tr>
  </tbody>
</table>
<p>88889 rows × 23 columns</p>
</div>




```python
df2.boxplot(column=['Total Serious Injuries'], grid=False)
```




    <AxesSubplot:>




    
![png](output_42_1.png)
    


### Completeness Test/Challenges

Dropping all rows with missing values and creating a new cleaned dataframe to provide for credibility of the dataset


```python
clean_df2 = df2.dropna()
clean_df2
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
      <th>Event Id</th>
      <th>Investigation Type</th>
      <th>Accident Number</th>
      <th>Event Date</th>
      <th>Location</th>
      <th>Country</th>
      <th>Injury Severity</th>
      <th>Aircraft damage</th>
      <th>Registration Number</th>
      <th>Make</th>
      <th>...</th>
      <th>Engine Type</th>
      <th>Purpose of flight</th>
      <th>Total Fatal Injuries</th>
      <th>Total Serious Injuries</th>
      <th>Total Minor Injuries</th>
      <th>Total Uninjured</th>
      <th>Weather Condition</th>
      <th>Broad phase of flight</th>
      <th>Report Status</th>
      <th>Total Injuries</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>20001218X45444</td>
      <td>Accident</td>
      <td>SEA87LA080</td>
      <td>1948-10-24</td>
      <td>MOOSE CREEK, ID</td>
      <td>United States</td>
      <td>Fatal(2)</td>
      <td>Destroyed</td>
      <td>NC6404</td>
      <td>Stinson</td>
      <td>...</td>
      <td>Reciprocating</td>
      <td>Personal</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>UNK</td>
      <td>Cruise</td>
      <td>Probable Cause</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>20001218X45447</td>
      <td>Accident</td>
      <td>LAX94LA336</td>
      <td>1962-07-19</td>
      <td>BRIDGEPORT, CA</td>
      <td>United States</td>
      <td>Fatal(4)</td>
      <td>Destroyed</td>
      <td>N5069P</td>
      <td>Piper</td>
      <td>...</td>
      <td>Reciprocating</td>
      <td>Personal</td>
      <td>4.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>UNK</td>
      <td>Unknown</td>
      <td>Probable Cause</td>
      <td>4.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>20001218X45448</td>
      <td>Accident</td>
      <td>LAX96LA321</td>
      <td>1977-06-19</td>
      <td>EUREKA, CA</td>
      <td>United States</td>
      <td>Fatal(2)</td>
      <td>Destroyed</td>
      <td>N1168J</td>
      <td>Rockwell</td>
      <td>...</td>
      <td>Reciprocating</td>
      <td>Personal</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>IMC</td>
      <td>Cruise</td>
      <td>Probable Cause</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>6</th>
      <td>20001218X45446</td>
      <td>Accident</td>
      <td>CHI81LA106</td>
      <td>1981-08-01</td>
      <td>COTTON, MN</td>
      <td>United States</td>
      <td>Fatal(4)</td>
      <td>Destroyed</td>
      <td>N4988E</td>
      <td>Cessna</td>
      <td>...</td>
      <td>Reciprocating</td>
      <td>Personal</td>
      <td>4.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>IMC</td>
      <td>Unknown</td>
      <td>Probable Cause</td>
      <td>4.0</td>
    </tr>
    <tr>
      <th>7</th>
      <td>20020909X01562</td>
      <td>Accident</td>
      <td>SEA82DA022</td>
      <td>1982-01-01</td>
      <td>PULLMAN, WA</td>
      <td>United States</td>
      <td>Non-Fatal</td>
      <td>Substantial</td>
      <td>N2482N</td>
      <td>Cessna</td>
      <td>...</td>
      <td>Reciprocating</td>
      <td>Personal</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>2.0</td>
      <td>VMC</td>
      <td>Takeoff</td>
      <td>Probable Cause</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>63893</th>
      <td>20080104X00022</td>
      <td>Accident</td>
      <td>MIA08LA032</td>
      <td>2007-12-26</td>
      <td>SARASOTA, FL</td>
      <td>United States</td>
      <td>Non-Fatal</td>
      <td>Substantial</td>
      <td>N5875Q</td>
      <td>Mooney</td>
      <td>...</td>
      <td>Reciprocating</td>
      <td>Personal</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>VMC</td>
      <td>Takeoff</td>
      <td>Probable Cause</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>63896</th>
      <td>20071231X02008</td>
      <td>Incident</td>
      <td>DEN08IA044</td>
      <td>2007-12-26</td>
      <td>Aspen, CO</td>
      <td>United States</td>
      <td>Incident</td>
      <td>Minor</td>
      <td>N47BC</td>
      <td>Piper</td>
      <td>...</td>
      <td>Reciprocating</td>
      <td>Personal</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>4.0</td>
      <td>VMC</td>
      <td>Climb</td>
      <td>Probable Cause</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>63900</th>
      <td>20080102X00006</td>
      <td>Accident</td>
      <td>SEA08LA054</td>
      <td>2007-12-28</td>
      <td>MURRIETA, CA</td>
      <td>United States</td>
      <td>Non-Fatal</td>
      <td>Substantial</td>
      <td>N365SX</td>
      <td>Hein</td>
      <td>...</td>
      <td>Reciprocating</td>
      <td>Personal</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>VMC</td>
      <td>Cruise</td>
      <td>Probable Cause</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>63906</th>
      <td>20080103X00010</td>
      <td>Accident</td>
      <td>DFW08LA052</td>
      <td>2007-12-29</td>
      <td>Crowley, TX</td>
      <td>United States</td>
      <td>Fatal(2)</td>
      <td>Destroyed</td>
      <td>N136DG</td>
      <td>Althouse</td>
      <td>...</td>
      <td>Reciprocating</td>
      <td>Personal</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>VMC</td>
      <td>Maneuvering</td>
      <td>Probable Cause</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>63908</th>
      <td>20080109X00032</td>
      <td>Accident</td>
      <td>NYC08FA071</td>
      <td>2007-12-30</td>
      <td>CHEROKEE, AL</td>
      <td>United States</td>
      <td>Fatal(3)</td>
      <td>Substantial</td>
      <td>N109AE</td>
      <td>Bell</td>
      <td>...</td>
      <td>Turbo Shaft</td>
      <td>Other Work Use</td>
      <td>3.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>VMC</td>
      <td>Maneuvering</td>
      <td>Probable Cause</td>
      <td>3.0</td>
    </tr>
  </tbody>
</table>
<p>46498 rows × 23 columns</p>
</div>




```python
#checking missing values
clean_df2.isnull().sum()
```




    Event Id                  0
    Investigation Type        0
    Accident Number           0
    Event Date                0
    Location                  0
    Country                   0
    Injury Severity           0
    Aircraft damage           0
    Registration Number       0
    Make                      0
    Model                     0
    Amateur Built             0
    Number of Engines         0
    Engine Type               0
    Purpose of flight         0
    Total Fatal Injuries      0
    Total Serious Injuries    0
    Total Minor Injuries      0
    Total Uninjured           0
    Weather Condition         0
    Broad phase of flight     0
    Report Status             0
    Total Injuries            0
    dtype: int64



### Checking for duplicates in the dataset


```python
clean_df2.duplicated().any()

```




    False



No duplicates observed


```python
clean_df2.dtypes
```




    Event Id                          object
    Investigation Type                object
    Accident Number                   object
    Event Date                datetime64[ns]
    Location                          object
    Country                           object
    Injury Severity                   object
    Aircraft damage                   object
    Registration Number               object
    Make                              object
    Model                             object
    Amateur Built                     object
    Number of Engines                float64
    Engine Type                       object
    Purpose of flight                 object
    Total Fatal Injuries             float64
    Total Serious Injuries           float64
    Total Minor Injuries             float64
    Total Uninjured                  float64
    Weather Condition                 object
    Broad phase of flight             object
    Report Status                     object
    Total Injuries                   float64
    dtype: object




```python
# Fixing colum names to be in upper case
clean_df2.columns = map(lambda x: str(x).upper(), clean_df2)
clean_df2.head()
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
      <th>EVENT ID</th>
      <th>INVESTIGATION TYPE</th>
      <th>ACCIDENT NUMBER</th>
      <th>EVENT DATE</th>
      <th>LOCATION</th>
      <th>COUNTRY</th>
      <th>INJURY SEVERITY</th>
      <th>AIRCRAFT DAMAGE</th>
      <th>REGISTRATION NUMBER</th>
      <th>MAKE</th>
      <th>...</th>
      <th>ENGINE TYPE</th>
      <th>PURPOSE OF FLIGHT</th>
      <th>TOTAL FATAL INJURIES</th>
      <th>TOTAL SERIOUS INJURIES</th>
      <th>TOTAL MINOR INJURIES</th>
      <th>TOTAL UNINJURED</th>
      <th>WEATHER CONDITION</th>
      <th>BROAD PHASE OF FLIGHT</th>
      <th>REPORT STATUS</th>
      <th>TOTAL INJURIES</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>20001218X45444</td>
      <td>Accident</td>
      <td>SEA87LA080</td>
      <td>1948-10-24</td>
      <td>MOOSE CREEK, ID</td>
      <td>United States</td>
      <td>Fatal(2)</td>
      <td>Destroyed</td>
      <td>NC6404</td>
      <td>Stinson</td>
      <td>...</td>
      <td>Reciprocating</td>
      <td>Personal</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>UNK</td>
      <td>Cruise</td>
      <td>Probable Cause</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>20001218X45447</td>
      <td>Accident</td>
      <td>LAX94LA336</td>
      <td>1962-07-19</td>
      <td>BRIDGEPORT, CA</td>
      <td>United States</td>
      <td>Fatal(4)</td>
      <td>Destroyed</td>
      <td>N5069P</td>
      <td>Piper</td>
      <td>...</td>
      <td>Reciprocating</td>
      <td>Personal</td>
      <td>4.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>UNK</td>
      <td>Unknown</td>
      <td>Probable Cause</td>
      <td>4.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>20001218X45448</td>
      <td>Accident</td>
      <td>LAX96LA321</td>
      <td>1977-06-19</td>
      <td>EUREKA, CA</td>
      <td>United States</td>
      <td>Fatal(2)</td>
      <td>Destroyed</td>
      <td>N1168J</td>
      <td>Rockwell</td>
      <td>...</td>
      <td>Reciprocating</td>
      <td>Personal</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>IMC</td>
      <td>Cruise</td>
      <td>Probable Cause</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>6</th>
      <td>20001218X45446</td>
      <td>Accident</td>
      <td>CHI81LA106</td>
      <td>1981-08-01</td>
      <td>COTTON, MN</td>
      <td>United States</td>
      <td>Fatal(4)</td>
      <td>Destroyed</td>
      <td>N4988E</td>
      <td>Cessna</td>
      <td>...</td>
      <td>Reciprocating</td>
      <td>Personal</td>
      <td>4.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>IMC</td>
      <td>Unknown</td>
      <td>Probable Cause</td>
      <td>4.0</td>
    </tr>
    <tr>
      <th>7</th>
      <td>20020909X01562</td>
      <td>Accident</td>
      <td>SEA82DA022</td>
      <td>1982-01-01</td>
      <td>PULLMAN, WA</td>
      <td>United States</td>
      <td>Non-Fatal</td>
      <td>Substantial</td>
      <td>N2482N</td>
      <td>Cessna</td>
      <td>...</td>
      <td>Reciprocating</td>
      <td>Personal</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>2.0</td>
      <td>VMC</td>
      <td>Takeoff</td>
      <td>Probable Cause</td>
      <td>0.0</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 23 columns</p>
</div>



### Exporting the Cleaned Dataset 


```python
clean_df2.to_csv('cleaned_aviation_data.csv')
```

## Data Analysis


```python
df3 = pd.read_csv('cleaned_aviation_data.csv')
df3.head()
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
      <th>Unnamed: 0</th>
      <th>EVENT ID</th>
      <th>INVESTIGATION TYPE</th>
      <th>ACCIDENT NUMBER</th>
      <th>EVENT DATE</th>
      <th>LOCATION</th>
      <th>COUNTRY</th>
      <th>INJURY SEVERITY</th>
      <th>AIRCRAFT DAMAGE</th>
      <th>REGISTRATION NUMBER</th>
      <th>...</th>
      <th>ENGINE TYPE</th>
      <th>PURPOSE OF FLIGHT</th>
      <th>TOTAL FATAL INJURIES</th>
      <th>TOTAL SERIOUS INJURIES</th>
      <th>TOTAL MINOR INJURIES</th>
      <th>TOTAL UNINJURED</th>
      <th>WEATHER CONDITION</th>
      <th>BROAD PHASE OF FLIGHT</th>
      <th>REPORT STATUS</th>
      <th>TOTAL INJURIES</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>20001218X45444</td>
      <td>Accident</td>
      <td>SEA87LA080</td>
      <td>1948-10-24</td>
      <td>MOOSE CREEK, ID</td>
      <td>United States</td>
      <td>Fatal(2)</td>
      <td>Destroyed</td>
      <td>NC6404</td>
      <td>...</td>
      <td>Reciprocating</td>
      <td>Personal</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>UNK</td>
      <td>Cruise</td>
      <td>Probable Cause</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>20001218X45447</td>
      <td>Accident</td>
      <td>LAX94LA336</td>
      <td>1962-07-19</td>
      <td>BRIDGEPORT, CA</td>
      <td>United States</td>
      <td>Fatal(4)</td>
      <td>Destroyed</td>
      <td>N5069P</td>
      <td>...</td>
      <td>Reciprocating</td>
      <td>Personal</td>
      <td>4.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>UNK</td>
      <td>Unknown</td>
      <td>Probable Cause</td>
      <td>4.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>20001218X45448</td>
      <td>Accident</td>
      <td>LAX96LA321</td>
      <td>1977-06-19</td>
      <td>EUREKA, CA</td>
      <td>United States</td>
      <td>Fatal(2)</td>
      <td>Destroyed</td>
      <td>N1168J</td>
      <td>...</td>
      <td>Reciprocating</td>
      <td>Personal</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>IMC</td>
      <td>Cruise</td>
      <td>Probable Cause</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>6</td>
      <td>20001218X45446</td>
      <td>Accident</td>
      <td>CHI81LA106</td>
      <td>1981-08-01</td>
      <td>COTTON, MN</td>
      <td>United States</td>
      <td>Fatal(4)</td>
      <td>Destroyed</td>
      <td>N4988E</td>
      <td>...</td>
      <td>Reciprocating</td>
      <td>Personal</td>
      <td>4.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>IMC</td>
      <td>Unknown</td>
      <td>Probable Cause</td>
      <td>4.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>7</td>
      <td>20020909X01562</td>
      <td>Accident</td>
      <td>SEA82DA022</td>
      <td>1982-01-01</td>
      <td>PULLMAN, WA</td>
      <td>United States</td>
      <td>Non-Fatal</td>
      <td>Substantial</td>
      <td>N2482N</td>
      <td>...</td>
      <td>Reciprocating</td>
      <td>Personal</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>2.0</td>
      <td>VMC</td>
      <td>Takeoff</td>
      <td>Probable Cause</td>
      <td>0.0</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 24 columns</p>
</div>




```python
#delete the unnamed column
df3.drop(['Unnamed: 0'], axis=1, inplace=True)
df3.head()
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
      <th>EVENT ID</th>
      <th>INVESTIGATION TYPE</th>
      <th>ACCIDENT NUMBER</th>
      <th>EVENT DATE</th>
      <th>LOCATION</th>
      <th>COUNTRY</th>
      <th>INJURY SEVERITY</th>
      <th>AIRCRAFT DAMAGE</th>
      <th>REGISTRATION NUMBER</th>
      <th>MAKE</th>
      <th>...</th>
      <th>ENGINE TYPE</th>
      <th>PURPOSE OF FLIGHT</th>
      <th>TOTAL FATAL INJURIES</th>
      <th>TOTAL SERIOUS INJURIES</th>
      <th>TOTAL MINOR INJURIES</th>
      <th>TOTAL UNINJURED</th>
      <th>WEATHER CONDITION</th>
      <th>BROAD PHASE OF FLIGHT</th>
      <th>REPORT STATUS</th>
      <th>TOTAL INJURIES</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>20001218X45444</td>
      <td>Accident</td>
      <td>SEA87LA080</td>
      <td>1948-10-24</td>
      <td>MOOSE CREEK, ID</td>
      <td>United States</td>
      <td>Fatal(2)</td>
      <td>Destroyed</td>
      <td>NC6404</td>
      <td>Stinson</td>
      <td>...</td>
      <td>Reciprocating</td>
      <td>Personal</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>UNK</td>
      <td>Cruise</td>
      <td>Probable Cause</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>20001218X45447</td>
      <td>Accident</td>
      <td>LAX94LA336</td>
      <td>1962-07-19</td>
      <td>BRIDGEPORT, CA</td>
      <td>United States</td>
      <td>Fatal(4)</td>
      <td>Destroyed</td>
      <td>N5069P</td>
      <td>Piper</td>
      <td>...</td>
      <td>Reciprocating</td>
      <td>Personal</td>
      <td>4.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>UNK</td>
      <td>Unknown</td>
      <td>Probable Cause</td>
      <td>4.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>20001218X45448</td>
      <td>Accident</td>
      <td>LAX96LA321</td>
      <td>1977-06-19</td>
      <td>EUREKA, CA</td>
      <td>United States</td>
      <td>Fatal(2)</td>
      <td>Destroyed</td>
      <td>N1168J</td>
      <td>Rockwell</td>
      <td>...</td>
      <td>Reciprocating</td>
      <td>Personal</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>IMC</td>
      <td>Cruise</td>
      <td>Probable Cause</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>20001218X45446</td>
      <td>Accident</td>
      <td>CHI81LA106</td>
      <td>1981-08-01</td>
      <td>COTTON, MN</td>
      <td>United States</td>
      <td>Fatal(4)</td>
      <td>Destroyed</td>
      <td>N4988E</td>
      <td>Cessna</td>
      <td>...</td>
      <td>Reciprocating</td>
      <td>Personal</td>
      <td>4.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>IMC</td>
      <td>Unknown</td>
      <td>Probable Cause</td>
      <td>4.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>20020909X01562</td>
      <td>Accident</td>
      <td>SEA82DA022</td>
      <td>1982-01-01</td>
      <td>PULLMAN, WA</td>
      <td>United States</td>
      <td>Non-Fatal</td>
      <td>Substantial</td>
      <td>N2482N</td>
      <td>Cessna</td>
      <td>...</td>
      <td>Reciprocating</td>
      <td>Personal</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>2.0</td>
      <td>VMC</td>
      <td>Takeoff</td>
      <td>Probable Cause</td>
      <td>0.0</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 23 columns</p>
</div>




```python
df3.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 46498 entries, 0 to 46497
    Data columns (total 23 columns):
     #   Column                  Non-Null Count  Dtype  
    ---  ------                  --------------  -----  
     0   EVENT ID                46498 non-null  object 
     1   INVESTIGATION TYPE      46498 non-null  object 
     2   ACCIDENT NUMBER         46498 non-null  object 
     3   EVENT DATE              46498 non-null  object 
     4   LOCATION                46498 non-null  object 
     5   COUNTRY                 46498 non-null  object 
     6   INJURY SEVERITY         46498 non-null  object 
     7   AIRCRAFT DAMAGE         46498 non-null  object 
     8   REGISTRATION NUMBER     46498 non-null  object 
     9   MAKE                    46498 non-null  object 
     10  MODEL                   46498 non-null  object 
     11  AMATEUR BUILT           46498 non-null  object 
     12  NUMBER OF ENGINES       46498 non-null  float64
     13  ENGINE TYPE             46498 non-null  object 
     14  PURPOSE OF FLIGHT       46498 non-null  object 
     15  TOTAL FATAL INJURIES    46498 non-null  float64
     16  TOTAL SERIOUS INJURIES  46498 non-null  float64
     17  TOTAL MINOR INJURIES    46498 non-null  float64
     18  TOTAL UNINJURED         46498 non-null  float64
     19  WEATHER CONDITION       46498 non-null  object 
     20  BROAD PHASE OF FLIGHT   46498 non-null  object 
     21  REPORT STATUS           46498 non-null  object 
     22  TOTAL INJURIES          46498 non-null  float64
    dtypes: float64(6), object(17)
    memory usage: 8.2+ MB



```python
df3.info(verbose = False)
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 46498 entries, 0 to 46497
    Columns: 23 entries, EVENT ID to TOTAL INJURIES
    dtypes: float64(6), object(17)
    memory usage: 8.2+ MB


Concise Summary Statistics


```python
df3.describe()
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
      <th>NUMBER OF ENGINES</th>
      <th>TOTAL FATAL INJURIES</th>
      <th>TOTAL SERIOUS INJURIES</th>
      <th>TOTAL MINOR INJURIES</th>
      <th>TOTAL UNINJURED</th>
      <th>TOTAL INJURIES</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>46498.000000</td>
      <td>46498.000000</td>
      <td>46498.000000</td>
      <td>46498.0</td>
      <td>46498.000000</td>
      <td>46498.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>1.133834</td>
      <td>0.399910</td>
      <td>0.192718</td>
      <td>0.0</td>
      <td>2.704934</td>
      <td>0.918986</td>
    </tr>
    <tr>
      <th>std</th>
      <td>0.409843</td>
      <td>2.338675</td>
      <td>0.824727</td>
      <td>0.0</td>
      <td>16.927730</td>
      <td>3.295152</td>
    </tr>
    <tr>
      <th>min</th>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>1.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>1.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.0</td>
      <td>1.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>1.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.0</td>
      <td>2.000000</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>4.000000</td>
      <td>230.000000</td>
      <td>81.000000</td>
      <td>0.0</td>
      <td>507.000000</td>
      <td>283.000000</td>
    </tr>
  </tbody>
</table>
</div>



Describing Categorical data


```python
df3.describe(include='object')
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
      <th>EVENT ID</th>
      <th>INVESTIGATION TYPE</th>
      <th>ACCIDENT NUMBER</th>
      <th>EVENT DATE</th>
      <th>LOCATION</th>
      <th>COUNTRY</th>
      <th>INJURY SEVERITY</th>
      <th>AIRCRAFT DAMAGE</th>
      <th>REGISTRATION NUMBER</th>
      <th>MAKE</th>
      <th>MODEL</th>
      <th>AMATEUR BUILT</th>
      <th>ENGINE TYPE</th>
      <th>PURPOSE OF FLIGHT</th>
      <th>WEATHER CONDITION</th>
      <th>BROAD PHASE OF FLIGHT</th>
      <th>REPORT STATUS</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>46498</td>
      <td>46498</td>
      <td>46498</td>
      <td>46498</td>
      <td>46498</td>
      <td>46498</td>
      <td>46498</td>
      <td>46498</td>
      <td>46498</td>
      <td>46498</td>
      <td>46498</td>
      <td>46498</td>
      <td>46498</td>
      <td>46498</td>
      <td>46498</td>
      <td>46498</td>
      <td>46498</td>
    </tr>
    <tr>
      <th>unique</th>
      <td>45897</td>
      <td>2</td>
      <td>46498</td>
      <td>6957</td>
      <td>13280</td>
      <td>36</td>
      <td>43</td>
      <td>3</td>
      <td>43292</td>
      <td>3100</td>
      <td>6199</td>
      <td>2</td>
      <td>6</td>
      <td>16</td>
      <td>3</td>
      <td>12</td>
      <td>2</td>
    </tr>
    <tr>
      <th>top</th>
      <td>20001214X45071</td>
      <td>Accident</td>
      <td>NYC95LA005</td>
      <td>1982-05-16</td>
      <td>ANCHORAGE, AK</td>
      <td>United States</td>
      <td>Non-Fatal</td>
      <td>Substantial</td>
      <td>NONE</td>
      <td>Cessna</td>
      <td>152</td>
      <td>No</td>
      <td>Reciprocating</td>
      <td>Personal</td>
      <td>VMC</td>
      <td>Landing</td>
      <td>Probable Cause</td>
    </tr>
    <tr>
      <th>freq</th>
      <td>3</td>
      <td>45530</td>
      <td>1</td>
      <td>25</td>
      <td>346</td>
      <td>46231</td>
      <td>36677</td>
      <td>32801</td>
      <td>292</td>
      <td>16722</td>
      <td>1869</td>
      <td>42607</td>
      <td>41976</td>
      <td>26989</td>
      <td>42290</td>
      <td>11366</td>
      <td>46497</td>
    </tr>
  </tbody>
</table>
</div>



Out of 46,498 incidents and accidents, there was a reported 36,677 non-fatal injuries and 32,801 substantial damages to the aircraft. 11,366 of the incidences occurred during landing with 42,290 occuring during VMC weather conditions. 41,976 of the incidences involved aircrafts with reciprocating engine.

### Univariate analysis


```python
mode_values = df3[['INVESTIGATION TYPE', 'NUMBER OF ENGINES' ,'INJURY SEVERITY', 'AIRCRAFT DAMAGE', 'MAKE','MODEL', 'ENGINE TYPE', 'PURPOSE OF FLIGHT', 'WEATHER CONDITION', 'BROAD PHASE OF FLIGHT']].mode()
mode_values
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
      <th>INVESTIGATION TYPE</th>
      <th>NUMBER OF ENGINES</th>
      <th>INJURY SEVERITY</th>
      <th>AIRCRAFT DAMAGE</th>
      <th>MAKE</th>
      <th>MODEL</th>
      <th>ENGINE TYPE</th>
      <th>PURPOSE OF FLIGHT</th>
      <th>WEATHER CONDITION</th>
      <th>BROAD PHASE OF FLIGHT</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Accident</td>
      <td>1.0</td>
      <td>Non-Fatal</td>
      <td>Substantial</td>
      <td>Cessna</td>
      <td>152</td>
      <td>Reciprocating</td>
      <td>Personal</td>
      <td>VMC</td>
      <td>Landing</td>
    </tr>
  </tbody>
</table>
</div>



Most incidences resulting from aircraft accidents were non fatal. There was however substantial damage to the aircrafts resulting from the incidences. 

Cessna model 152 aircraft with one reciprocating engine experienced the most accidents and incidents. 

Most of the safety incidences and accidents were reported on general aviation as compared to commercial services and military planes.


```python
fig, axs = plt.subplots(nrows=3, ncols=2, figsize=(18,18))

sns.countplot(x=df3['INVESTIGATION TYPE'], ax=axs[0,0], color='#83FFE1')
axs[0,0].set_title('Investigation Type Count Plot')
axs[0,0].set_ylabel('Count')
axs[0,0].set_xlabel('Investigation Type')


sns.countplot(x=df3['NUMBER OF ENGINES'], ax=axs[0,1], color='#83FFE1')
axs[0,1].set_title('Number of Engines Count Plot')
axs[0,1].set_ylabel('Count')
axs[0,1].set_xlabel('Number of Engines')

#flight_count = df3['PURPOSE OF FLIGHT'].value_counts()
sns.histplot(x=df3['ENGINE TYPE'], bins=5, ax=axs[1,1], color='#83FFE1')
axs[1,1].set_title('Engine Type Count Plot')
axs[1,1].set_ylabel('Count')
axs[1,1].set_xlabel('Engine Type')

sns.histplot(x=df3['AIRCRAFT DAMAGE'], bins=5, ax=axs[1,0], color='#83FFE1')
axs[1,0].set_title('Aircraft Damage Count Plot')
axs[1,0].set_ylabel('Count')
axs[1,0].set_xlabel('Aircraft Damage')

sns.countplot(x=df3['WEATHER CONDITION'], ax=axs[2,0], color='#83FFE1')
axs[2,0].set_title('Weather Condition Count Plot')
axs[2,0].set_ylabel('Count')
axs[2,0].set_xlabel('Weather Condition')


sns.countplot(x=df3['AMATEUR BUILT'], ax=axs[2,1], color='#83FFE1')
axs[2,1].set_title('Amateur Built Count Plot')
axs[2,1].set_ylabel('Count')
axs[2,1].set_xlabel('Amateur Built')


plt.tight_layout()
plt.show()



```


    
![png](output_67_0.png)
    


A majority of the aircrafts involved in accidents were not amateur built. Amateur-built aircraft is defined as an aircraft "the major portion of which has been fabricated and assembled by person(s) who undertook the construction project solely for their own education or recreation."

Instrument Meteorological Conditions (IMC) are weather conditions that require pilots to fly primarily by reference to flight instruments, and therefore under instrument flight rules (IFR). On the other hand, Visual Meteorological Conditions (VMC) is an aviation flight category that allows visual flight rules (VFR) in public and private flights. This basically means that the pilot of an aircraft can fly according to their visual ability versus relying on their instrumentation. These criteria vary depending on the airspace class and include: Visibility: The minimum visibility requirements under VMC range from 1 mile (1.6 km) to 5 miles (8 km), depending on the airspace class and whether the flight is conducted during the day or night.UNK impies Unknown Weather Conditions.

Most accidents were reported during landing with VMC weather conditions. 

As the number of engines increases, the number of accidents reduces. This can be attritubed to the availablility of a redundancy/emergency engine in case of one engine failure


```python
plt.figure(figsize=(18,8))
sns.histplot(x=df3['PURPOSE OF FLIGHT'], bins=15, color='#83FFE1')
plt.title('Purpose of Flight Count Plot')
plt.xticks(rotation=45)
plt.show()


```


    
![png](output_69_0.png)
    


Flights for personal use experienced the highest accidents followed by instructional flights. The higher rates of incidents from instructional flights could be attributed to the trainee pilots guiding the aircraft.


```python
plt.figure(figsize=(18,8))
sns.histplot(x=df3['BROAD PHASE OF FLIGHT'], bins=15, color='#83FFE1')
plt.title('Broad Phase of Flight Count Plot')
plt.xticks(rotation=45)
plt.show()
```


    
![png](output_71_0.png)
    


Most accidents occurred during landing followed by takeoff, cruise, maneuvering and approach


```python
#selecting only numerical columns
df3_num = df3.select_dtypes(include=['int', 'float'])
df3_num.head()
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
      <th>NUMBER OF ENGINES</th>
      <th>TOTAL FATAL INJURIES</th>
      <th>TOTAL SERIOUS INJURIES</th>
      <th>TOTAL MINOR INJURIES</th>
      <th>TOTAL UNINJURED</th>
      <th>TOTAL INJURIES</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1.0</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1.0</td>
      <td>4.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>4.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1.0</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1.0</td>
      <td>4.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>4.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>2.0</td>
      <td>0.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
df3_num.agg(['mean', 'median'])
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
      <th>NUMBER OF ENGINES</th>
      <th>TOTAL FATAL INJURIES</th>
      <th>TOTAL SERIOUS INJURIES</th>
      <th>TOTAL MINOR INJURIES</th>
      <th>TOTAL UNINJURED</th>
      <th>TOTAL INJURIES</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>mean</th>
      <td>1.133834</td>
      <td>0.39991</td>
      <td>0.192718</td>
      <td>0.0</td>
      <td>2.704934</td>
      <td>0.918986</td>
    </tr>
    <tr>
      <th>median</th>
      <td>1.000000</td>
      <td>0.00000</td>
      <td>0.000000</td>
      <td>0.0</td>
      <td>1.000000</td>
      <td>0.000000</td>
    </tr>
  </tbody>
</table>
</div>



The average fatality rate per incident is 0.4 with a serious injury rate of 0.19.  

### Bivariate Analysis


```python

model_class = pd.crosstab(df3['ENGINE TYPE'], df3['AIRCRAFT DAMAGE'])
model_class.plot(kind='bar', figsize=(12,6))
plt.title('Comparison of degree of damage and engine type')
plt.show()
```


    
![png](output_77_0.png)
    


Reciprocating engines experience more failures. The best performing engines are Turbo jet


```python
plt.figure(figsize=(12,6))
sns.scatterplot(x=df3['NUMBER OF ENGINES'], y=df3['TOTAL FATAL INJURIES'])
plt.title('Number of Engines against Fatal Injuries')
plt.show()
```


    
![png](output_79_0.png)
    


Accidents involving aircrafts with two engines resulted to the most fatal injuries.  


```python
plt.figure(figsize=(12,6))
sns.scatterplot(x=df3['BROAD PHASE OF FLIGHT'], y=df3['TOTAL INJURIES'])
plt.title('Total Injuries versus Phase of Flight')
plt.xticks(rotation=45)
plt.show()
```


    
![png](output_81_0.png)
    


The flight phases that require keen attention are takeoff, cruise, approach, landing and climb


```python
data_int = df3[['NUMBER OF ENGINES', 'TOTAL FATAL INJURIES', 'TOTAL SERIOUS INJURIES', 'TOTAL UNINJURED', 'TOTAL INJURIES']]
corr = data_int.corr()
corr
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
      <th>NUMBER OF ENGINES</th>
      <th>TOTAL FATAL INJURIES</th>
      <th>TOTAL SERIOUS INJURIES</th>
      <th>TOTAL UNINJURED</th>
      <th>TOTAL INJURIES</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>NUMBER OF ENGINES</th>
      <td>1.000000</td>
      <td>0.104144</td>
      <td>0.042613</td>
      <td>0.375839</td>
      <td>0.124680</td>
    </tr>
    <tr>
      <th>TOTAL FATAL INJURIES</th>
      <td>0.104144</td>
      <td>1.000000</td>
      <td>0.208296</td>
      <td>-0.019982</td>
      <td>0.813973</td>
    </tr>
    <tr>
      <th>TOTAL SERIOUS INJURIES</th>
      <td>0.042613</td>
      <td>0.208296</td>
      <td>1.000000</td>
      <td>0.009554</td>
      <td>0.577254</td>
    </tr>
    <tr>
      <th>TOTAL UNINJURED</th>
      <td>0.375839</td>
      <td>-0.019982</td>
      <td>0.009554</td>
      <td>1.000000</td>
      <td>0.059870</td>
    </tr>
    <tr>
      <th>TOTAL INJURIES</th>
      <td>0.124680</td>
      <td>0.813973</td>
      <td>0.577254</td>
      <td>0.059870</td>
      <td>1.000000</td>
    </tr>
  </tbody>
</table>
</div>




```python
print(plt.colormaps(), end= ' ')
```

    ['Accent', 'Accent_r', 'Blues', 'Blues_r', 'BrBG', 'BrBG_r', 'BuGn', 'BuGn_r', 'BuPu', 'BuPu_r', 'CMRmap', 'CMRmap_r', 'Dark2', 'Dark2_r', 'GnBu', 'GnBu_r', 'Greens', 'Greens_r', 'Greys', 'Greys_r', 'OrRd', 'OrRd_r', 'Oranges', 'Oranges_r', 'PRGn', 'PRGn_r', 'Paired', 'Paired_r', 'Pastel1', 'Pastel1_r', 'Pastel2', 'Pastel2_r', 'PiYG', 'PiYG_r', 'PuBu', 'PuBuGn', 'PuBuGn_r', 'PuBu_r', 'PuOr', 'PuOr_r', 'PuRd', 'PuRd_r', 'Purples', 'Purples_r', 'RdBu', 'RdBu_r', 'RdGy', 'RdGy_r', 'RdPu', 'RdPu_r', 'RdYlBu', 'RdYlBu_r', 'RdYlGn', 'RdYlGn_r', 'Reds', 'Reds_r', 'Set1', 'Set1_r', 'Set2', 'Set2_r', 'Set3', 'Set3_r', 'Spectral', 'Spectral_r', 'Wistia', 'Wistia_r', 'YlGn', 'YlGnBu', 'YlGnBu_r', 'YlGn_r', 'YlOrBr', 'YlOrBr_r', 'YlOrRd', 'YlOrRd_r', 'afmhot', 'afmhot_r', 'autumn', 'autumn_r', 'binary', 'binary_r', 'bone', 'bone_r', 'brg', 'brg_r', 'bwr', 'bwr_r', 'cividis', 'cividis_r', 'cool', 'cool_r', 'coolwarm', 'coolwarm_r', 'copper', 'copper_r', 'crest', 'crest_r', 'cubehelix', 'cubehelix_r', 'flag', 'flag_r', 'flare', 'flare_r', 'gist_earth', 'gist_earth_r', 'gist_gray', 'gist_gray_r', 'gist_heat', 'gist_heat_r', 'gist_ncar', 'gist_ncar_r', 'gist_rainbow', 'gist_rainbow_r', 'gist_stern', 'gist_stern_r', 'gist_yarg', 'gist_yarg_r', 'gnuplot', 'gnuplot2', 'gnuplot2_r', 'gnuplot_r', 'gray', 'gray_r', 'hot', 'hot_r', 'hsv', 'hsv_r', 'icefire', 'icefire_r', 'inferno', 'inferno_r', 'jet', 'jet_r', 'magma', 'magma_r', 'mako', 'mako_r', 'nipy_spectral', 'nipy_spectral_r', 'ocean', 'ocean_r', 'pink', 'pink_r', 'plasma', 'plasma_r', 'prism', 'prism_r', 'rainbow', 'rainbow_r', 'rocket', 'rocket_r', 'seismic', 'seismic_r', 'spring', 'spring_r', 'summer', 'summer_r', 'tab10', 'tab10_r', 'tab20', 'tab20_r', 'tab20b', 'tab20b_r', 'tab20c', 'tab20c_r', 'terrain', 'terrain_r', 'turbo', 'turbo_r', 'twilight', 'twilight_r', 'twilight_shifted', 'twilight_shifted_r', 'viridis', 'viridis_r', 'vlag', 'vlag_r', 'winter', 'winter_r'] 


```python
plt.figure(figsize=(14, 7))
sns.heatmap(corr, annot= True, fmt='.2f', cmap='BuGn')
plt.title('Heat Map')
plt.show()
```


    
![png](output_85_0.png)
    


There is a low positive correlation between the number of engines and total injuries experienced during an accident

### Multivariate Analysis


```python
plt.figure(figsize=(12,6))
sns.scatterplot(x=df3['BROAD PHASE OF FLIGHT'], y=df3['TOTAL INJURIES'], hue=df3['AIRCRAFT DAMAGE'])
plt.show()
```


    
![png](output_88_0.png)
    


Accidents occurrences during takeoff, cruise, landing ans approach resulted to either destruction of the aircraft or its substantial damage. Safety concerns on this areas should be stringent


```python
plt.figure(figsize=(12,8))
sns.scatterplot(y=df3['TOTAL INJURIES'], x=df3['PURPOSE OF FLIGHT'], hue=df3['WEATHER CONDITION'])
plt.xticks(rotation=45)
plt.show()
```


    
![png](output_90_0.png)
    



```python
plt.figure(figsize=(12,8))
sns.scatterplot(y=df3['TOTAL INJURIES'], hue=df3['NUMBER OF ENGINES'], x=df3['ENGINE TYPE'])
plt.xticks(rotation=45)
plt.show()
```


    
![png](output_91_0.png)
    


# RECOMMENDATIONS

The company should focus on provide safe aviation transport in the personal (general aviation) niches. This is because the data indicated flights for personal use experience most accidents and incidences. This is an area which can per explored for potential growth.

It is recommended that the aircrafts to be procured should have turbo jet or turbo fan engines with at least 3 engines. Aircrafts mounted with reciprocating engines are worst performing in terms of aviation safety.

The aircrafts to be procured should have advance and more sophisticate weather monitoring instrumentations to be used during IMC condiitons. In addition, a greater percentage of the recorded accidents occurred during VMC weather conditions. It is therefore recommended that Maureen Inc develops Standard Operating Procedures that are safety stringent during VMC conditions.

Considering the commercial nature of the company, it is recommended that the organizations procures professionally built aircrafts as opposed to amateur built despite the statistics indiation most accidents occured on professionally built aircrafts.



