# Python Study Notes
This note works as tips on Python. I will keep updating and adding useful uage.

## Pandas

### Create a Pandas DataFrame

#### Create a DataFrame from Raw Data
```python
import pandas as pd
raw = [[23, 'engineer', 7810], [32, 'accountant', 6980], [29, 'software', 7420]]
df = pd.DataFrame(data = raw, columns = ['age', 'job', 'income'])

raw = {'age': [23, 32, 29], 'job': ['engineer', 'accountant', 'software'], 'income': [7810, 6980, 7420]}
df = pd.DataFrame(data = raw)

df.to_csv('data.csv', sep = ',', na_rep = '', columns = ['age', 'job', 'income'], index = False, encoding = 'utf-8') # na_rep for missing data representation
```

#### Create a DataFrame from CSV file
```python
df = pd.read_csv('data.csv', sep = ',')
```

Loop over rows. Let's say we have a Pandas dataframe df.
```python
for index, row in df.interrows():
    print(row['age'])
    print(row['job'])
    print(row['income'])
```

#### 
