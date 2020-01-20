# Pandas-reminders

Some things i've found useful while working in Pandas, with some resources along the way. Most helpful of all is Ben Welsh's [First Python Notebook](https://first-python-notebook.readthedocs.io/index.html).

If you're coming from Excel, [this is also](https://github.com/ank0409/Ditching-Excel-for-Python/blob/master/Ditching%20Excel%20for%20Python!.ipynb) a great resource.

## Combining multiple csv files

One option, especially if there are many files with like columns:

`import glob, os  
df = pd.concat(map(pd.read_csv, glob.glob(os.path.join('', "my_files*.csv"))))`

Though it also works to import the files individually, then: 

`pdList = [df19, df20]  
df = pd.concat(pdList)
df.info()`

Or, simply:

`df = pd.concat(map(pd.read_csv, ['file_1.csv', 'file_2.csv']))`

Here's a [good resource](https://towardsdatascience.com/combining-pandas-dataframes-the-easy-way-41eb0f2c1ebf)

## Where queries

For a single numeric value:

`df[df['column_name'] == 2019]`

For multiple values, via [Pandas Cheat Sheets](http://sy-edm.com/stories/pandas_cheat_sheets.html):

`valuelist = ['value1', 'value2', 'value3']
df = df[df.column.isin(value_list)]`

Where the column doesn't have those values:

`df = df[~df.column.isin(value_list)]`

## Groupbys & counts

To count items in a column:

`df.groupby(['column']).size().reset_index(name='counts')`

Groupby with sums:

`df.groupby('column_to_group_by').column_to_sum.sum().reset_index()`

Groupby with multiple fields:

`df.groupby[('column_to_group_by', 'column_to_group_by_2)].column_to_sum.sum().reset_index()`

Group and sort by descending:

`df.groupby('column_to_group_by').column_to_sum.sum().reset_index().sort_values("column_to_sum", ascending=False)`

## Slice columns

Sometimes you want to extract soemthing a column to create a new field. Precinct numbers in Colorado, for instance, begin with the congressional district, then the state Senate district, then the House district, etc. Keep in mind that the first digit in Python is 0 here, so this example extracts the second and third digits of the field (and goes to 3 because that's where it's stopping).

`df['new_column_name']= df['column'].astype(str).str[1:3]`

[Resource](https://stackoverflow.com/questions/20025882/add-a-string-prefix-to-each-value-in-a-string-column-using-pandas)


## Standardizing field names

This is a two-step process, based on Ben Welsh's [Hello Cleaning](https://first-python-notebook.readthedocs.io/cleaning/index.html) section. I use this for files that are updated periodically (specifically Colorado lobbying files) to standardize names. Often, this is how we manipulate data. To correct people's spelling. There may be more efficient ways to do this, but this works for me. With tons of names, it takes a while to process tho.

Step one are the changes you want to make:

`def combine_names(row):
    if row.row_name_here.startswith('Aetna'):
        return 'Aetna Inc.'
     elif row.row_name_here.startswith('Millercoors'):
        return 'MillerCoors'
 return row.row_name_here`
 
 Step two, after running the cell/code above, applies the changes:
 
` df['new_column_name'] = df.apply(combine_emp_names, axis=1)`
