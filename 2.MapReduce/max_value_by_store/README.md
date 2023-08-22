---
title: 'MapReduce: Max Value by Store'
updated: 2023-08-22 17:06:43Z
created: 2023-08-22 16:30:23Z
latitude: 19.08433770
longitude: 72.83597240
altitude: 0.0000
---

## Step 1: Create a directory named "sum"

`hdfs dfs -mkdir /store`


## Step 2: Upload example.txt from LFS to HDFS

`hdfs dfs -put example.txt /store`

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

This script reads input data in the form of tab-separated values, where each line represents a record with six fields: date, time, location, item, cost, and payment. The script processes this data and outputs only the location and cost fields.

Let's break down the script step by step:

1. **Shebang Line**: `#!/usr/bin/python`
   - Specifies that the script should be executed using the Python interpreter located at `/usr/bin/python`.

2. **Import Statements**:
   ```python
   import string
   import fileinput
   ```
   - Imports the `string` module, which provides various string-related constants and functions. However, in this script, the `string` module is imported but not used.
   - Imports the `fileinput` module, which provides a convenient way to read input from files or standard input.

3. **For Loop to Process Input**:
   ```python
   for line in fileinput.input():
   ```
   - Initiates a `for` loop that iterates over each line of input from either the specified input files or standard input.

4. **Data Processing**:
   ```python
   data = line.strip().split("\t")
   ```
   - For each input line, the script performs the following:
     - `line.strip()` removes any leading or trailing whitespace from the input line.
     - `.split("\t")` splits the cleaned line into a list of values using the tab character (`"\t"`) as the delimiter.
     - This results in the `data` list containing the individual values from the input line.

5. **Conditional Check**:
   ```python
   if len(data) == 6:
   ```
   - This `if` statement checks if the length of the `data` list is exactly 6.
   - If the length is 6, it indicates that the input line has the expected number of fields (date, time, location, item, cost, payment). If not, the script moves on to the next line.

6. **Data Extraction**:
   ```python
   date, time, location, item, cost, payment = data
   ```
   - If the length of `data` is 6, the script unpacks the values from the `data` list into separate variables: `date`, `time`, `location`, `item`, `cost`, and `payment`.

7. **Printing Results**:
   ```python
   print "{0}\t{1}".format(location, cost)
   ```
   - This line uses the `print` statement (Python 2.x syntax) to format and output the `location` and `cost` variables.
   - The `.format()` method is used to insert values into placeholders `{0}` and `{1}` within the string.
   - The result is a tab-separated output of `location` and `cost`.

In summary, this script reads tab-separated input lines, expects each line to have exactly 6 values representing a record, extracts the `location` and `cost` fields, and prints them in a tab-separated format. Any lines not meeting the expected format are skipped.

## Step 4: Test mapper.py locally

`cat example.txt | python mapper.py`


## Step 5: Create reducer.py

```
#!/usr/bin/python

import fileinput

max_value = 0
old_key = None

for line in fileinput.input():

    data = line.strip().split("\t")    

    if len(data) != 2:
        # Something has gone wrong. Skip this line.
        continue

    current_key, current_value = data

    # Refresh for new keys (i.e. locations in the example context)
    if old_key and old_key != current_key:
        print old_key, "\t", max_value
        old_key = current_key
        max_value = 0

    old_key = current_key
    if float(current_value) > float(max_value):
        max_value = float(current_value)

if old_key != None:
    print old_key, "\t", max_value
```
 This script processes input data where each line consists of two tab-separated values: a key (e.g., location) and a value. The script calculates the maximum value for each key and outputs the result for each key.

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
   max_value = 0
   old_key = None
   ```
   - Initializes two variables:
     - `max_value` to keep track of the maximum value encountered for a given key.
     - `old_key` to store the previously processed key. This is used to detect when a new key is encountered.

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
     - This results in the `data` list containing the individual key and value from the input line.

6. **Conditional Check**:
   ```python
   if len(data) != 2:
       # Something has gone wrong. Skip this line.
       continue
   ```
   - This `if` statement checks if the length of the `data` list is not equal to 2.
   - If the length is not 2, it indicates that the input line does not conform to the expected format (key and value). The script skips this line using the `continue` statement.

7. **Data Extraction**:
   ```python
   current_key, current_value = data
   ```
   - If the length of `data` is 2 (indicating valid input), the script unpacks the values from the `data` list into `current_key` and `current_value` variables.

8. **Key Change Detection and Output**:
   ```python
   if old_key and old_key != current_key:
       print old_key, "\t", max_value
       old_key = current_key
       max_value = 0
   ```
   - This block of code checks if a new key has been encountered (`old_key != current_key`). If so, it means that the maximum value for the previous key has been found.
   - The script then prints the old key and its corresponding maximum value.
   - The `max_value` is reset to 0 for the new key.

9. **Updating Old Key and Max Value**:
   ```python
   old_key = current_key
   if float(current_value) > float(max_value):
       max_value = float(current_value)
   ```
   - These lines update the `old_key` to the current key for tracking.
   - The script checks if the current value is greater than the current `max_value`. If it is, the `max_value` is updated with the current value.

10. **Final Output Handling**:
    ```python
    if old_key != None:
        print old_key, "\t", max_value
    ```
    - After the loop ends, this code block is executed to handle the last key-value pair.
    - If `old_key` is not `None`, it means that there are remaining values for the last key that haven't been printed. This block prints the key and its maximum value.

In summary, this script processes input data with key-value pairs, calculates the maximum value for each key, and outputs the result for each key. The script handles cases where keys change and ensures that the maximum value is tracked and printed appropriately.

## Step 6: Test reducer.py locally
`cat example.txt | python mapper.py | python reducer.py`


## Step 7: Make the python scripts - mapper.py and reducer.py executable

`chmod +x mapper.py reducer.py`

## Step 8: Run MapReduce over Hadoop

```
hadoop jar /usr/lib/hadoop-0.20-mapreduce/contrib/streaming/hadoop-streaming-mr1.jar    -input /store/example.txt   -output /store/output   -mapper /home/cloudera/Desktop/sf/2.MapReduce/max_value_by_store/mapper.py  -reducer /home/cloudera/Desktop/sf/2.MapReduce/max_value_by_store/reducer.py
```

## Step 9: View the output
`hdfs dfs -cat /store/output/part-00000`

