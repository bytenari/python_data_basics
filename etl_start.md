**This content is all recorded based on the practice exercises from the Coursera course 'IBM Data Engineering Professional Certificate.**

# ETL(Extraction, Transformation, Loading and Logging)

## Extraction

**Source Data Download**
```
wget https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-PY0221EN-SkillsNetwork/labs/module%206/Lab%20-%20Extract%20Transform%20Load/data/source.zip

unzip source.zip
```

**Install Panda Librariy and Import**
```
python3.11 -m pip install pandas
```

*file name: etl_code.py*
```
import glob 
import pandas as pd 
import xml.etree.ElementTree as ET 
from datetime import datetime 
```

**To store the final output data and store all the logs**
```
log_file = "log_file.txt" 
target_file = "transformed_data.csv"
```

**Define functions to extract from csv, json, xml files**
```
def extract_from_csv(file_to_process): 
    dataframe = pd.read_csv(file_to_process) 
    return dataframe 

def extract_from_json(file_to_process): 
    dataframe = pd.read_json(file_to_process, lines=True) # to enable the function to read the file as a JSON object on line to line basis.
    return dataframe 

def extract_from_xml(file_to_process): 
    dataframe = pd.DataFrame(columns=["name", "height", "weight"]) 
    tree = ET.parse(file_to_process) # to parse the data from the file using the ElementTree function, then extract relevant information from this data and append it to a pandas dataframe
    root = tree.getroot() 
    for person in root: 
        name = person.find("name").text 
        height = float(person.find("height").text) 
        weight = float(person.find("weight").text) 
        dataframe = pd.concat([dataframe, pd.DataFrame([{"name":name, "height":height, "weight":weight}])], ignore_index=True) 
    return dataframe 
```

**Process all csv, json, xml files**
```
def extract(): 
    extracted_data = pd.DataFrame(columns=['name','height','weight']) # to create an empty data frame to hold extracted data 
     
    for csvfile in glob.glob("*.csv"): 
        extracted_data = pd.concat([extracted_data, pd.DataFrame(extract_from_csv(csvfile))], ignore_index=True) 
         
    for jsonfile in glob.glob("*.json"): 
        extracted_data = pd.concat([extracted_data, pd.DataFrame(extract_from_json(jsonfile))], ignore_index=True) 
     
    for xmlfile in glob.glob("*.xml"): 
        extracted_data = pd.concat([extracted_data, pd.DataFrame(extract_from_xml(xmlfile))], ignore_index=True) 
         
    return extracted_data 
```



## Transformation
The height in the extracted data is in inches, and the weight is in pounds. However, for your application, the height is required to be in meters, and the weight is required to be in kilograms.

```def transform(data):
    data['height'] = round(data.height * 0.0254,2)
    data['weight'] = round(data.weight * 0.45359237,2)

    return data
```



## Loading and Logging
**To load the transformed data to a CSV file**
```

def log_progress(message): 
    timestamp_format = '%Y-%h-%d-%H:%M:%S' # Year-Monthname-Day-Hour-Minute-Second 
    now = datetime.now() # get current timestamp 
    timestamp = now.strftime(timestamp_format) # to convert the timestamp to a string format
    with open(log_file,"a") as f: 
        f.write(timestamp + ',' + message + '\n') 
```

**To record the log message**
```
# Log the initialization of the ETL process 
log_progress("ETL Job Started") 
 
# Log the beginning of the Extraction process 
log_progress("Extract phase Started") 
extracted_data = extract() 
 
# Log the completion of the Extraction process 
log_progress("Extract phase Ended") 
 
# Log the beginning of the Transformation process 
log_progress("Transform phase Started") 
transformed_data = transform(extracted_data) 
print("Transformed Data") 
print(transformed_data) 
 
# Log the completion of the Transformation process 
log_progress("Transform phase Ended") 
 
# Log the beginning of the Loading process 
log_progress("Load phase Started") 
load_data(target_file,transformed_data) 
 
# Log the completion of the Loading process 
log_progress("Load phase Ended") 
 
# Log the completion of the ETL process 
log_progress("ETL Job Ended") 
```

### Excute the code
```
python3.11 etl_code.py 
```





