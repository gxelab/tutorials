# Essential pandas
reference: Official Documentation & Cheetsheet

acrynyms:
- `df`: DataFrame
- `dfg`: DataFrame.GroupBy objects
- `s`: Series
- UDF: user-defined functions

## Series basics
```python
s.value_counts()  # table(R)
s.isna()          # NA?
s.isin()          # within
```

## DataFrame basics
select rows/columns/both: `df.loc[]` or `df.iloc[]`
select elements: `df.at` or `df.iat`
```python
df[['a', 'b', 'c']]  # DataFrame with columns a,b,c
df['a']              # Series
df.a                 # same as above, do not work with some column names
df[['a']]            # DataFrame with a single column
df[df['a'] > 10]     # rows where values in column a > 10

df.iloc[10:20]       # rows 10-20
df.iloc[:, [1, 3]]   # columns 1,3

df.loc[:, 'a':'c']   # all columns between a to c (both a & c inclusive)
df.loc[df['a'] > 10, ['a', 'c']]

df.iat[1, 2]
df.at[1, 'c']
```

## General parameters for DataFrame methods
```
axis: {0 or ‘index’, 1 or ‘columns’}
inplace: bool. Whether to modify the DataFrame rather than creating a new one
```

## Column operations
```python
# rename
mapper_dict = {'old_name1': 'new_name1', 'old_name2': 'new_name2'}
df.rename(columns=mapper_dict)
df.rename(mapper_dict, axis=1)

# remove
df.drop(columns=['a', 'b', 'c'])  # drop selected columns

# create
df.assign(c=lambda df: df.a * df.b)
df['c'] = dff['a'] * df['b']          # same as above
df['c'] = idmap_fca['a'].str.cat(idmap_fca['b'], sep='_')  # concatenate two string columns row-wise
df['c'] = df['a'].clip(lower=-3, upper=3)  # trim values into range
df['c'] = pd.qcut(df['a'], n, labels=False)  # bin columns into n buckets
```

## Row operations
```python
# subset /filter
df.sample(frac=0.5)
df.sample(n=10)
df.head(n)
df.tail(n)
df.nlargest(n, 'value')
df.nsmallest(n, 'value')

df[df.['a'].str.contains('xxx')]  # select rows matching a pattern in a column
df.drop_duplicates()  # Return DataFrame with duplicate rows removed.

# query
df.query('(a == "xxx") && (b >= 5)')  # dplyr::filter alike
df.query('(a == "xxx") && (b + @const >= 5)')  # `const` is in the environment
df.query('(`cell type` == "xxx") && (b + @const >= 5)')  # quote colnames that are not valid object name with backstick
df.query('a.str.startswith("abc")', engine='python')
# note: column names that are python keywords (like list, import, for, etc) cannot be used.

# sort
df.sort_values('a', assending=False)
df.sort_index()

# other
len(df)        # number of rows
df.shape       # Tuple (rows, columns)
df.describe()  # basic descriptive statistics
df.dropna()    # drow rows with NA in any columns
df.fillna()    # repalce NA/NULL data with value
```


# Index
```python
df.reset_index()  # convert index(s) to column(s)
df.reset_index(drop=True, inplace=True)  # remove index (i.e., set to default integer range)
df.rename(index=mapper_dict, inplace=True)  # update index values by dict {old_value: new_value}
```

## Groupby
https://pandas.pydata.org/docs/user_guide/groupby.html

```python
# groupby columns
df.groupby('a')   # = df.groupby(['a'])
df.groupby(['a', 'b'])
df.groupby('a', observed=True)  # drop unobserved levels if a is `Categorical`   

# groupby indexes (int, name or sequence of such)
df.groupby(level=0)
df.groupby(level=['a', 'b'])  # multiIndex

# mixed groupby (index + columns)
df.groupby([pd.Grouper(level=0), "a"])
```

- By default the group keys are sorted during the groupby operation. You may however pass `sort=False` for potential speedups.
- In the agg/transform/filter/apply result, the keys of the groups appear in the index by default. They can be instead included in the columns by passing `as_index=False`.

```python
dfg = df.groupby('a')
dfg.groups()     # dict {groupname: corresponding_index_values}
len(dfg.groups)  # number of groups
dfg['b']         # grouped series

dfg.get_group('group_1')           # select a single group
dfg.get_group(('key_1', 'key_2'))  # select a single group

# iterate groups
for name, group in dfg:
    print(name)   # group key value or tuple of group key values if grouped with multiple columns
    print(group)  # all columns of each group (including grouping columns)
```

Group-wise operations:

| method |input | output |
|---|---|---|
| `.agg` | `pd.Series` | scalar |
|`.transform` | `pd.Series` | `pd.Series` or scalar | 
| `.filter` | `pd.Series` or `pd.DataFrame` | scalar (boolean) |
| `.apply` | `pd.DataFrame` | scalar or `pd.Series` or `pd.DataFrame`

### Groupby -> Aggregation (summarize)
compute a summary statistic (or statistics) for each group.
```python
# compute sum of each non-grouping columns
df.groupby('a').sum()
df.groupby('a').agg('sum')  # .agg is preferred over aggregate, both are same
df.groupby('a').std(numeric_only=True)  # excludign non-numeric columns
# builtin agg funs: any, all, size, count (non-NA), nunique, first, last, sum, max, min, mean, median, std, var, ...

# UDF: takes a series, returns a series or scalar
df.group('a').agg(lambda x: x.astype(int).sum())  # supply a function that will applied to each column (x: series)

# multiple functions
df.groupby('a').agg(['sum', 'mean'])
df.groupby('a')['b'].agg(['sum', 'mean'])
df.groupby('a')['b'].agg([lambda x: x.max() - x.min(), lambda x: median])
df.groupby('a').agg({'b': 'sum', 'c': 'median'})

# named agg
df.groupby('a')['b'].agg(min_b='min', max_b='max')
df.groupby('a').agg(
    min_b = pd.NamedAgg(column='b', aggfun='min'),
    max_b = pd.NamedAgg('b', 'max'),
    median_c = pd.NamedAgg('c', 'median')
)
```

### Groupby -> Transformation (mutate)
perform some group-specific computations and return a like-indexed object. A common use of a transformation is to add the result back into the original DataFrame.
```python
df.groupby('a').cumsum()                  # cumsum within each group
df.groupby('a').transform('cumsum')       # same as above
df['cumsum'] = df.groupby('a').cumsum()   # add group-wise cumsum to the original df
# builtin trans: bfill, ffill, shift, diff, rank, cumcount, cumsum, cummax, cummin, cumprod, pct_change
```

UDFs for `.transform` should expect a series as input and [must](https://pandas.pydata.org/docs/user_guide/groupby.html#the-transform-method):
> - Return a result that is either the same size as the group chunk or broadcastable to the size of the group chunk;
> - Operate column-by-column on the group chunk. The transform is applied to the first group chunk using chunk.apply.
> - Not perform in-place operations on the group chunk. 

```python
df.groupby('a').transform(lambda x: (x - x.mean()) / x.std())  # UDF returns a result of the same size as chunk
df.groupby('a').transform(lambda x: x.max() - x.min())         # UDF returns a result broadcastable to the chunk
df.groupby('a').transform(lambda x: x.fillna(x.mean()))        # fill NA with group mean
```
`exapnding`, `resample`, `rolling` can be applied to grouped df, too.

### Groupby -> Filtraiton
discard some groups, according to a group-wise computation that evaluates to `True` or `False`. The result of the filter method is then the subset of groups for which the UDF (expects series or dataframe as input) returned True.
```python
# series
s.groupby(s).filter(lambda x: x.sum() > 2)

# For DataFrames with multiple columns, filters should explicitly specify a column as the filter criterion.
df.groupby('a').filter(lambda x: x['b'].sum() > 2)
# works when a single column remains after excluding the group columns
df.groupby('a').filter(lambda x: x.sum() > 2)  
```

### General group-wise operations: Groupby -> Apply
> The function passed to `apply` must take a series or dataframe as its first argument and return a `DataFrame`, `Series` or scalar. `apply` will then take care of combining the results back together into a single dataframe or series.
```python
df.groupby('a')["b"].apply(lambda x: x.describe()) 
df.groupby('a')["b"].apply(lambda x: pd.DataFrame({'ori': x, 'new': x - x.mean()}))  # returns DataFrame
```

### Other
```python
dfg.head(1)
dfg.tail(1)
```

## Merge/Join
```python
# merge using columns as keys
# how: inner, left, right, outer
pd.merge(df1, df2, how='inner', on='key1')
pd.merge(df1, df2, how='inner', on=['key1', 'key2'])
pd.merge(df1, df2, how='left', left_on='key1_df1', right_on='key1_df2')

# merge using index as keys
df1.join(df2)

# keep rows only appear in one df
pd.merge(ydf, zdf, how='outer', indicator=True).
    query('_merge == "left_only"').drop(columns=['_merge'])
```

## Reshaping
```python
pd.melt(df)  # wide to long
pd.pivot(df) # long to wide
pd.concat([df1, df2])  # vertical stack
pd.concat([df1, df2], axis=1) # horizontal stack (side by side)
```


## IO
```python
# read
pd.read_table('df.tsv')
pd.read_csv('df.tsv', sep='\t')
pd.read_excel('df.xlsx')
```

## plot
```python
df.plot.hist()  # hist for each column

df.plot.scatter(x='a', y='b')
```