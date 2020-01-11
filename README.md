# Pandas-reminders

Some things i've found useful while working in Pandas.

## Combining multiple csv files

One option, especially if there are many files with like columns:

`import glob, os  
df = pd.concat(map(pd.read_csv, glob.glob(os.path.join('', "my_files*.csv"))))`

Though it also works to import the files individually, then: 

`hf = pd.concat(map(pd.read_csv, ['file_1.csv', 'file_2.csv']))`

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


