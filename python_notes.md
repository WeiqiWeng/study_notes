# Python Study Notes
This note works as tips on Python. I will keep updating and adding useful uage.

## Pandas

### Create a Pandas DataFrame

#### Create a DataFrame from 2D Array
```python
import pandas as pd
raw = [[23, 'engineer', 7810], [32, 'accountant', 6980], [29, 'software', 7420]]
df = pd.DataFrame(raw, columns = ['age', 'job', 'income'])
```

Loop over rows. Let's say we have a Pandas dataframe df.
```python
for index, row in df.interrows():
    
```
