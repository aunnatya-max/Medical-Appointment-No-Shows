# Medical Appointment No Shows – Data Cleaning and Preprocessing

## Project Objective
The objective of this project is to clean and preprocess the **Medical Appointment No Shows dataset** to make it ready for analysis. This includes fixing data types, handling missing values, removing duplicates, and creating new features for better insights.

---

## Description of Raw Data
The dataset contains **110,527 records** and the following columns:

| Column          | Non-Null Count | Dtype    | Description |
|-----------------|----------------|---------|-------------|
| PatientId       | 110,527        | float64 | Unique identifier for each patient |
| AppointmentID   | 110,527        | int64   | Unique identifier for each appointment |
| Gender          | 110,527        | object  | Patient gender |
| ScheduledDay    | 110,527        | object  | Date and time when the appointment was scheduled |
| AppointmentDay  | 110,527        | object  | Date of the appointment |
| Age             | 110,527        | int64   | Patient age |
| Neighbourhood   | 110,527        | object  | Location of the health facility |
| Scholarship     | 110,527        | int64   | Indicates whether patient is enrolled in welfare program |
| Hipertension    | 110,527        | int64   | Indicates if patient has hypertension |
| Diabetes        | 110,527        | int64   | Indicates if patient has diabetes |
| Alcoholism      | 110,527        | int64   | Indicates if patient is an alcoholic |
| Handcap         | 110,527        | int64   | Indicates if patient has disability |
| SMS_received    | 110,527        | int64   | Indicates if SMS reminder was sent |
| No-show         | 110,527        | object  | Indicates if patient attended the appointment |

---

## Steps Performed for Cleaning

1. **Convert PatientId to integer**
```python
df['PatientId'] = df['PatientId'].astype('int64')
```

2. **Convert ScheduledDay and AppointmentDay to datetime and create separate date/time columns**
```python
# ScheduledDay
df['ScheduledDay'] = pd.to_datetime(df['ScheduledDay'], format='%Y-%m-%dT%H:%M:%SZ')
scheduled_date = df['ScheduledDay'].dt.date
scheduled_time = df['ScheduledDay'].dt.time
idx = df.columns.get_loc('ScheduledDay')
df.insert(idx + 1, 'ScheduledDate', scheduled_date)
df.insert(idx + 2, 'ScheduledTime', scheduled_time)

# AppointmentDay
df['AppointmentDay'] = pd.to_datetime(df['AppointmentDay'], format='%Y-%m-%dT%H:%M:%SZ')
```

3. **Keep only valid ages**
```python
df = df[(df['Age'] >= 0) & (df['Age'] <= 120)]
```

4. **Standardize Neighbourhood formatting**
```python
df['Neighbourhood'] = df['Neighbourhood'].str.title().str.strip()
```

5. **Check for missing values**
```python
df.isnull().sum()
```

6. **Remove duplicate records**
```python
df.drop_duplicates(inplace=True)
```

7. **Check the number of records**
```python
num_records = len(df)
print("Number of records:", num_records)
```

8. **Reformat ScheduledDate and ScheduledTime to datetime format**
```python
df['ScheduledDate'] = pd.to_datetime(df['ScheduledDate'], format='%d-%m-%Y', errors='coerce')
df['ScheduledTime'] = pd.to_datetime(df['ScheduledTime'], format='%d-%m-%Y', errors='coerce')
```

---

## Observations

1. `PatientID` was in scientific notation and converted to integer.  
2. `ScheduledDay` was separated into `ScheduledDate` and `ScheduledTime`.  
3. `Neighbourhood` names were standardized to remove extra spaces and capitalization issues.

