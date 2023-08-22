---
title: 'MapReduce: Count and Sum'
updated: 2023-08-22 16:26:25Z
created: 2023-08-22 12:16:26Z
author: Gaurav Ojha
---

## Step 1: Create a directory named "sum"

`hdfs dfs -mkdir /sum`
![8f6271817253d824b0449e880c7ce75b.png](/2.MapReduce/count_and_sum/_resources/8f6271817253d824b0449e880c7ce75b.png)

## Step 2: Upload example.txt from LFS to HDFS

`hdfs dfs -put example.txt /sum`
![b841835e30e9f6313f03e5ab5a803bb5.png](/2.MapReduce/count_and_sum/_resources/b841835e30e9f6313f03e5ab5a803bb5.png)

## Step 3: Create mapper.py
```
#!/usr/bin/python
import string
import fileinput

for line in fileinput.input():
    data = line.strip().split("\t")
    if len(data) == 6:
        date, time, location, item, cost, payment = data
        print "{0}\t{1}".format(location, cost)
```

Let's break down the code step by step:

1. **Shebang Line**: `#!/usr/bin/python`
   - This is called a "shebang" line and is used in Unix-like operating systems to specify the interpreter that should be used to execute the script. In this case, it indicates that the Python interpreter located at `/usr/bin/python` should be used to run the script.

2. **Import Statements**:
   ```python
   import string
   import fileinput
   ```
   - The `import` statements bring in functionality from two Python modules: `string` and `fileinput`.
   - The `string` module provides a collection of constants and functions related to string manipulation, but in this script, it doesn't appear to be used.
   - The `fileinput` module provides a way to iterate over lines from multiple input sources, including files and standard input.

3. **For Loop to Process Input**:
   ```python
   for line in fileinput.input():
   ```
   - This `for` loop iterates over each line of input from either the specified input files or standard input (if no files are specified).
   - The `fileinput.input()` function returns an iterator that provides each line as a string.

4. **Data Processing**:
   ```python
   data = line.strip().split("\t")
   ```
   - For each line of input, the line is first stripped of leading and trailing whitespace using the `.strip()` method.
   - Then, the line is split into a list of values using the `split("\t")` method, where `"\t"` is the tab character. This assumes that the input data is tab-separated.

5. **Conditional Check**:
   ```python
   if len(data) == 6:
   ```
   - This `if` statement checks if the length of the `data` list is equal to 6. This implies that the script expects each line to have exactly 6 tab-separated values.

6. **Unpacking Data**:
   ```python
   date, time, location, item, cost, payment = data
   ```
   - If the length of `data` is indeed 6, this line unpacks the values from the `data` list into individual variables: `date`, `time`, `location`, `item`, `cost`, and `payment`.

7. **Printing Results**:
   ```python
   print "{0}\t{1}".format(location, cost)
   ```
   - This line uses the `print` statement (in Python 2.x syntax) to print a formatted string.
   - The `.format()` method is used to insert values into placeholders `{0}` and `{1}` within the string.
   - The values inserted are `location` and `cost`, which were extracted from the `data` list.
   - The result is a tab-separated output of `location` and `cost`.

In summary, this script reads tab-separated input lines, expects each line to have 6 values, extracts the `location` and `cost` from each line, and prints them in a tab-separated format.

## Step 4: Test mapper.py locally

`cat example.txt | python mapper.py`
![0d8460c74074f1eb90da6615a19e4a99.png](/2.MapReduce/count_and_sum/_resources/0d8460c74074f1eb90da6615a19e4a99.png)

## Step 5: Create reducer.py

```
#!/usr/bin/python

import fileinput

transactions_count = 0
sales_total = 0

for line in fileinput.input():

    data = line.strip().split("\t")    

    if len(data) != 2:
        # Something has gone wrong. Skip this line.
        continue

    current_key, current_value = data

    transactions_count += 1
    sales_total += float(current_value)

print transactions_count, "\t", sales_total
```

The script calculates the total number of transactions and the total sales amount and prints these values.

Let's break down the script step by step:

1. **Shebang Line**: `#!/usr/bin/python`
   - Specifies that the script should be executed using the Python interpreter located at `/usr/bin/python`.

2. **Import Statement**:
   ```python
   import fileinput
   ```
   - Imports the `fileinput` module, which provides a convenient way to read input from files or standard input.

3. **Initialization of Variables**:
   ```python
   transactions_count = 0
   sales_total = 0
   ```
   - Initializes two variables, `transactions_count` and `sales_total`, both set to zero. These variables will be used to accumulate transaction data.

4. **For Loop to Process Input**:
   ```python
   for line in fileinput.input():
   ```
   - Initiates a `for` loop that iterates over each line of input from either the specified input files or standard input.

5. **Data Processing**:
   ```python
   data = line.strip().split("\t")
   ```
   - For each input line, the script performs the following:
     - `line.strip()` removes any leading or trailing whitespace from the input line.
     - `.split("\t")` splits the cleaned line into a list of values using the tab character (`"\t"`) as the delimiter.
     - This results in the `data` list containing the individual values from the input line.

6. **Conditional Check**:
   ```python
   if len(data) != 2:
       # Something has gone wrong. Skip this line.
       continue
   ```
   - This `if` statement checks if the length of the `data` list is not equal to 2.
   - If the length is not 2, it indicates that the input line does not conform to the expected format (location and cost). The script skips this line using the `continue` statement.

7. **Data Extraction**:
   ```python
   current_key, current_value = data
   ```
   - If the length of `data` is 2 (indicating valid input), the script unpacks the values from the `data` list into `current_key` and `current_value` variables.

8. **Updating Count and Total**:
   ```python
   transactions_count += 1
   sales_total += float(current_value)
   ```
   - These lines increment the `transactions_count` by 1 for each valid input line, thus counting the number of transactions processed.
   - The `sales_total` is updated by adding the floating-point value of `current_value`, representing the cost of the transaction.

9. **Printing Results**:
   ```python
   print transactions_count, "\t", sales_total
   ```
   - Finally, the script prints the accumulated `transactions_count` and `sales_total` separated by tabs.
   - The output will be displayed in the same format, where the first column is the transaction count and the second column is the total sales amount.

In summary, this script processes input data representing transactions with location and cost values. It calculates and prints the total number of transactions and the total sales amount.

## Step 6: Test reducer.py locally
`cat example.txt | python mapper.py | python reducer.py`
![6ea949cfce6e8e1500c7183b3c868497.png](/2.MapReduce/count_and_sum/_resources/6ea949cfce6e8e1500c7183b3c868497.png)

## Step 7: Make the python scripts - mapper.py and reducer.py executable

`chmod +x mapper.py reducer.py`

## Step 8: Run MapReduce over Hadoop

```
hadoop jar /usr/lib/hadoop-0.20-mapreduce/contrib/streaming/hadoop-streaming-mr1.jar    -input /sum/example.txt   -output /sum/output   -mapper /home/cloudera/Desktop/sf/2.MapReduce/count_and_sum/mapper.py  -reducer /home/cloudera/Desktop/sf/2.MapReduce/count_and_sum/reducer.py
```

## Step 9: View the output
![0f14ecc0d933f3e359b2ded5c533a65b.png](/2.MapReduce/count_and_sum/_resources/0f14ecc0d933f3e359b2ded5c533a65b.png)

