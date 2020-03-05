---
layout: post
title: Choropleth Maps in Python
---

How to create visually stunning Choropleth Maps in jupyter Notebooks quickly using plotly.  
Importing the required packages. For plotly to work in offline Jupyter notebook we need to import iplot from plotly.offline and initiate notebook mode.  

```python
import pandas as pd
import numpy as np  

import plotly.graph_objs as go 
from plotly.offline import init_notebook_mode,iplot
init_notebook_mode(connected=True) 
```

Let's load some data we can work with. The data contains the Electoral Votes share of each State in the U.S.

```python
#Load the Data
electoral_votes = pd.read_csv('hw2data/electoral_votes.csv')
electoral_votes.head()
```


<table border="1px">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>State</th>
      <th>Votes</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>California</td>
      <td>55</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Texas</td>
      <td>38</td>
    </tr>
    <tr>
      <th>2</th>
      <td>New York</td>
      <td>29</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Florida</td>
      <td>29</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Illinois</td>
      <td>20</td>
    </tr>
  </tbody>
</table>


Plotly takes locations in the form of Two-Letter State Codes, so this will come in handy if the data doesn't come with the State codes.
```python
states_abbrev = {

'AK': 'Alaska', 'AL': 'Alabama', 'AR': 'Arkansas', 'AS': 'American Samoa', 'AZ': 'Arizona', 'CA': 'California', 'CO': 'Colorado', 
'CT':  'Connecticut', 'DC': 'District of Columbia', 'DE': 'Delaware', 'FL': 'Florida', 'GA': 'Georgia', 'GU': 'Guam', 'HI': 'Hawaii',    'IA': 'Iowa', 'ID': 'Idaho', 'IL': 'Illinois', 'IN': 'Indiana', 'KS': 'Kansas', 'KY': 'Kentucky', 'LA': 'Louisiana', 
'MA': 'Massachusetts', 'MD': 'Maryland', 'ME': 'Maine', 'MI': 'Michigan', 'MN': 'Minnesota', 'MO': 'Missouri',
'MP': 'Northern Mariana Islands', 'MS': 'Mississippi', 'MT': 'Montana', 'NA': 'National', 'NC': 'North Carolina', 'ND': 'North Dakota', 'NE': 'Nebraska', 'NH': 'New Hampshire', 'NJ': 'New Jersey', 'NM': 'New Mexico', 'NV': 'Nevada', 'NY': 'New York', 'OH': 'Ohio',
'OK': 'Oklahoma', 'OR': 'Oregon', 'PA': 'Pennsylvania', 'PR': 'Puerto Rico', 'RI': 'Rhode Island', 'SC': 'South Carolina', 
'SD': 'South Dakota', 'TN': 'Tennessee', 'TX': 'Texas', 'UT': 'Utah', 'VA': 'Virginia', 'VI': 'Virgin Islands', 'VT': 'Vermont', 
'WA': 'Washington', 'WI': 'Wisconsin', 'WV': 'West Virginia', 'WY': 'Wyoming'

}

states_abbrev_reverse = dict(zip(states_abbrev.values(),states_abbrev.keys()))
```
The dataframe does not have the state code, so let's fix that first.  

```python
electoral_votes["Code"] = electoral_votes["State"].apply(lambda x: states_abbrev_reverse[x])
electoral_votes.head()
```
<table border="1" class="table">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>State</th>
      <th>Votes</th>
      <th>Code</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>California</td>
      <td>55</td>
      <td>CA</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Texas</td>
      <td>38</td>
      <td>TX</td>
    </tr>
    <tr>
      <th>2</th>
      <td>New York</td>
      <td>29</td>
      <td>NY</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Florida</td>
      <td>29</td>
      <td>FL</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Illinois</td>
      <td>20</td>
      <td>IL</td>
    </tr>
  </tbody>
</table>
