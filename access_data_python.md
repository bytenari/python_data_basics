# Accessing Databases using Python Script

*File Name : db_code.py*

**Load 'INSTRUCTOR.csv' as material**
```
wget https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMSkillsNetwork-PY0221EN-Coursera/labs/v2/INSTRUCTOR.csv
```

**Import Libraries and Connect to DB**
```
import sqlite3
import pandas as pd

conn = sqlite3.connect('STAFF.db')
```

**Assign for the attributes of the required table**
```
table_name = 'INSTRUCTOR'
attribute_list = ['ID', 'FNAME', 'LNAME', 'CITY', 'CCODE']
```

**Reading the CSV file**
```
file_path = '/home/project/INSTRUCTOR.csv'
df = pd.read_csv(file_path, names = attribute_list)
```

**Loading the data to a table**
```
df.to_sql(table_name, conn, if_exists = 'replace', index =False)
print('Table is ready')
```
if_exists = 'fail'	Default. The command doesn't work if a table with the same name exists in the database.
if_exists = 'replace'	The command replaces the existing table in the database with the same name.
if_exists = 'append'	The command appends the new data to the existing table with the same name.

**Running basic queries on data**
```
query_statement = f"SELECT * FROM {table_name}"
query_output = pd.read_sql(query_statement, conn)
print(query_statement)
print(query_output)

query_statement = f"SELECT FNAME FROM {table_name}"
query_output = pd.read_sql(query_statement, conn)
print(query_statement)
print(query_output)

query_statement = f"SELECT COUNT(*) FROM {table_name}"
query_output = pd.read_sql(query_statement, conn)
print(query_statement)
print(query_output)
```
```
# the output is as below

Table is ready
SELECT * FROM INSTRUCTOR
    ID    FNAME      LNAME      CITY CCODE
0    1      Rav      Ahuja   TORONTO    CA
1    2     Raul      Chong   Markham    CA
2    3     Hima  Vasudevan   Chicago    US
3    4     John     Thomas  Illinois    US
4    5    Alice      James  Illinois    US
5    6    Steve      Wells  Illinois    US
6    7  Santosh      Kumar  Illinois    US
7    8    Ahmed    Hussain  Illinois    US
8    9    Nancy      Allen  Illinois    US
9   10     Mary     Thomas  Illinois    US
10  11  Bharath      Gupta  Illinois    US
11  12   Andrea      Jones  Illinois    US
12  13      Ann      Jacob  Illinois    US
13  14     Amit      Kumar  NewDelhi    IN
SELECT FNAME FROM INSTRUCTOR
      FNAME
0       Rav
1      Raul
2      Hima
3      John
4     Alice
5     Steve
6   Santosh
7     Ahmed
8     Nancy
9      Mary
10  Bharath
11   Andrea
12      Ann
13     Amit
SELECT COUNT(*) FROM INSTRUCTOR
   COUNT(*)
0        14
```

**Append Some Data**
```
data_dict = {'ID' : [100],
            'FNAME' : ['John'],
            'LNAME' : ['Doe'],
            'CITY' : ['Paris'],
            'CCODE' : ['FR']}
data_append = pd.DataFrame(data_dict)

data_append.to_sql(table_name, conn, if_exists = 'append', index = False)
print('Data appended successfully')
```

**Must Close the Connection**
```
conn.close()
```
