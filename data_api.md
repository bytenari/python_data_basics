# Web scraping and Extracting Data using APIs

**Commands in a terminal**
```
python3.11 -m pip install pandas
python3.11 -m pip install bs4
```

*File Name: webscraping_movies.py*

**Importing Libraries"**
```
import requests
import sqlite3
import pandas as pd
from bs4 import BeautifulSoup
```

**Entities**
```
url = 'https://web.archive.org/web/20230902185655/https://en.everybodywiki.com/100_Most_Highly-Ranked_Films'
db_name = 'Movies.db'
table_name = 'Top_50'
csv_path = '/home/project/top_50_films.csv'
df = pd.DataFrame(columns=["Average Rank","Film","Year"])
count = 0
```

**Loading the webpage for Webscraping**
```
html_page = requests.get(url).text
data = BeautifulSoup(html_page, 'html.parser')
```

**Checking the HTML code**
Right-click the table and click Inspect at the bottom of the menu.
Notice the such objects like <tbody>, <tr>, <td> 

**Scraping of required information**
```
tables = data.find_all('tbody')
rows = tables[0].find_all('tr')

for row in rows:
    if count<50:
        col = row.find_all('td')  # Extract all the td data objects in the row and save them to col.
        if len(col)!=0:
            data_dict = {"Average Rank": col[0].contents[0],  # Create a dictionary data_dict with the keys same as the columns of the dataframe created for recording the output earlier and corresponding values from the first three headers of data.
                         "Film": col[1].contents[0],
                         "Year": col[2].contents[0]}
            df1 = pd.DataFrame(data_dict, index=[0]) # Convert the dictionary to a dataframe 
            df = pd.concat([df,df1], ignore_index=True) # and concatenate it with the existing one to appending through the loop
            count+=1
    else:
        break
```

**Storing the data**
```
df.to_csv(csv_path)   # save to a CSV file
conn = sqlite3.connect(db_name)     # initialize a connection to the database
df.to_sql(table_name, conn, if_exists='replace', index=False)    # save the dataframe as a table
conn.close()   # close the connection
```

**Excuting the code""
```
python3.11 webscraping_movies.py
```
