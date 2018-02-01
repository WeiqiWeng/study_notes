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

#### Manipulation

```python
# get column names
list(df)

# merge two dataframes: add rows
df = pd.concat([df, df])

# merge two dataframes: add columns
df = pd.concat([df, df], axis = 1)

# sort values
df.sort_values('income', ascending=True, inplace = True)

# rename
df.rename(columns = {income: 'month_income'}, inplace = True)

# drop columns
df.drop(['age', 'job'], axis = 1, inplace = True)
```

#### Subsets

Mind that although it's easy to pick out subset, it's not necessarily easy to update subset.
```python
# pick out subset
df[df.income > 7000]
```
This method is better for subset selection. You can't do something like `df[df.income > 7000] = 1`.

```python
# another way to do subset selection
df.loc[df.month_income > 7000, ['age', 'job']]

# you can update value within this subset selection method
df.loc[df.month_income > 7000, 'month_income'] = 7000

# pick out samples with specific values
df.month_income.isin([7810, 6980])

# or you can update by index column
df.iloc[1:3] = [[41, 'instructor', 5810], [38, 'manager', 6593]]

# sample from dataframe
df.sample(n = 10)
df.sample(frac = 0.4)

# n-largest/smallest
df.nlargest(n = 2, 'month_income')
df.nsmallest(n = 2, 'month_income')
```
Note that when updating with `.iloc`, you need to bear in mind that the order of index is literally the order in index column. Specifically, if the dataframe is sorted and the index column becomes `[1, 0, 2]` then `.iloc[1:3]` points to `[0, 2]`.


#### Data Preprocessing

```python
# pick out null/non-null values
df.loc[1, 'month_income'] = None
df.loc[pd.isnull(df.month_income), ['age', 'job']]
df.loc[pd.notnull(df.month_income), ['age', 'job']]

# drop duplicates
df.drop_duplicates(['age', 'job'], keep = 'first', inplace = True)

# drop rows with any column having NA/null data
df.dropna(axis = 0, how = 'any', columns = ['age', 'job']) # axis = 0 for index, 1 for columns. how = any/all

# fill NA/null values
df.fillna(value = 7810, axis = 0, inplace = True) # axis = 0 to fill NaN value by index, axis = 1 to fill NaN value by column

# transformation
mean = df.month_income.mean()
std = df.month_income.std()
df.month_income = df.month_income.apply(lambda x: (x - mean) / std, axis = 0) # axis = 0 for applying by index, 1 for applying by columns
```

#### Analysis

```python
# abstraction and summary
df.describe()
df.info()

# number of unique values
df.month_income.value_counts()

# get unique values
df.month_income.nunique()
```

Other useful functions to get statistics include `sum(), median(), quantile([0.1, 0.2, 0.3]), mean(), var(), std()`.


