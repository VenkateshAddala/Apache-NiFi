## 📊 Apache NiFi Data Pipeline Project

### 🔗 Project Overview

This project demonstrates the use of **Apache NiFi** for building **two end-to-end data pipelines**:

1. **CSV File Ingestion to PostgreSQL & Processing Workflow**
2. **API Data Fetching & Monitoring Workflow**

The pipelines showcase real-world data ingestion, transformation, storage, and monitoring using NiFi's visual interface and processors, integrating with PostgreSQL and REST APIs.

---

## 🗂️ Project Structure

```
📦 datasets/                  # Input CSV files (Workflow 1)
📦 generated_output_files/    # Output CSV files (Workflow 1)
📦 activities/                # Output JSON files (Workflow 2)
📦 NiFi_Flow_Images/          # Screenshots of workflows & configs
```

---

## 🛠️ Tools & Technologies

* **Apache NiFi 2.4.0**
* **PostgreSQL**
* **JDBC Drivers**
* **REST APIs (Bored API)**
* **Jolt Transform (JSON Transformation)**
* **Email Alerts (PutEmail Processor)**

---

## ⚙️ Workflow 1: CSV File Ingestion & Processing
<img width="1465" alt="Screenshot 2025-05-10 at 5 10 06 PM" src="https://github.com/user-attachments/assets/1dc0dd29-b42c-4bac-ae3f-cdb3632fd1d9" />


### Steps:
---
1. **GetFile Processor**: Reads CSV files from the local `datasets` folder and deletes them after reading.
<img width="490" alt="Screenshot 2025-05-16 at 1 52 40 PM" src="https://github.com/user-attachments/assets/e7b36a78-9a3e-4390-8da2-21bb386694a0" />
<img width="499" alt="Screenshot 2025-05-16 at 1 52 51 PM" src="https://github.com/user-attachments/assets/628487db-e72b-40cf-b7a8-7680eef209ac" />
---
2. **ReplaceText Processor**: Cleans null/blank values and replaces them with `NA`.
<img width="1002" alt="Screenshot 2025-05-16 at 2 05 56 PM" src="https://github.com/user-attachments/assets/34e0d6ff-096e-4d44-b42d-c167e8cfb2a0" />
<img width="1381" alt="Screenshot 2025-05-16 at 2 11 15 PM" src="https://github.com/user-attachments/assets/6886d338-62b1-433e-9f53-c64aac227646" />
---
3. **PutDatabaseRecord Processor**: Loads transformed data into PostgreSQL using DBCPConnectionPool controller service.
<img width="1470" alt="Screenshot 2025-05-16 at 1 50 39 PM" src="https://github.com/user-attachments/assets/a21c8837-8d05-4831-a545-68935c90b962" />
---
4. **ExecuteSQL Processors**: Executes SQL queries for filtering and aggregating the loaded data.

5. **ConvertRecord Processor**: Converts AVRO data to CSV format.
<img width="1162" alt="Screenshot 2025-05-16 at 1 47 30 PM" src="https://github.com/user-attachments/assets/2e806728-ec87-4727-bc60-c4b4ab156b4b" />
---
6. **UpdateAttribute Processor**: Dynamically renames grouped files.
<img width="1046" alt="Screenshot 2025-05-16 at 2 13 07 PM" src="https://github.com/user-attachments/assets/b2ac3c28-8a42-4f36-ab90-33018d7352b2" />
---
7. **PutFile Processor**: Writes final CSV files to the `generated_output_files` folder.
<img width="477" alt="Screenshot 2025-05-16 at 1 53 02 PM" src="https://github.com/user-attachments/assets/ec9c87aa-0c2d-4882-9302-b3323a9ae31c" />
---
### Sample Flow: After grouping the processors
<img width="1453" alt="Screenshot 2025-05-10 at 5 17 26 PM" src="https://github.com/user-attachments/assets/f43996d5-ac4a-4fa7-83d1-f4eb36e88637" />
---


---

## ⚙️ Workflow 2: API Data Fetching & Monitoring
<img width="1236" alt="Screenshot 2025-05-16 at 1 48 01 PM" src="https://github.com/user-attachments/assets/e1eed5dd-eb23-438a-a9ed-36916c127221" />
---
### Steps:
---
1. **InvokeHTTP Processors**: Calls multiple APIs to fetch JSON data (e.g., Bored API).
<img width="776" alt="Screenshot 2025-05-16 at 1 53 51 PM" src="https://github.com/user-attachments/assets/8035ee3f-ae51-45bf-b8fd-e3e32bd399c5" />
<img width="858" alt="Screenshot 2025-05-16 at 1 53 38 PM" src="https://github.com/user-attachments/assets/6479146a-e539-4a5e-a334-b9b0cca4719d" />

---
2. **Funnel**: Merges API responses for downstream processing.

<img width="1236" alt="Screenshot 2025-05-16 at 2 18 59 PM" src="https://github.com/user-attachments/assets/d4b3fd0b-e2bd-4304-94d6-6a6deb2c169d" />

---
3. **JoltTransformJSON Processor**: Reshapes JSON using Jolt specifications.
<img width="1166" alt="Screenshot 2025-05-16 at 2 20 58 PM" src="https://github.com/user-attachments/assets/2f4cc0e2-ec55-4e18-aeef-e7fb9fba3088" />
---
4. **UpdateAttribute Processor**: Generates unique filenames like `activity0.json`, `activity1.json`.
<img width="1036" alt="Screenshot 2025-05-16 at 2 22 33 PM" src="https://github.com/user-attachments/assets/70a4792d-a49e-495a-8112-9f596482de3f" />
---
5. **PutFile Processor**: Saves JSON files into the `activities` folder.
<img width="572" alt="Screenshot 2025-05-16 at 2 24 18 PM" src="https://github.com/user-attachments/assets/3409ab9c-5578-4b92-9fff-316087329326" />
---
6. **MonitorActivity Processor**: Monitors workflow status (active/inactive/restored).
<img width="1014" alt="Screenshot 2025-05-16 at 2 02 52 PM" src="https://github.com/user-attachments/assets/2a2580ec-6238-43de-95d2-da5580b3bff6" />
---
7. **PutEmail Processor**: Sends email alerts with FlowFile metadata upon activity changes.
<img width="1042" alt="Screenshot 2025-05-16 at 2 28 57 PM" src="https://github.com/user-attachments/assets/05dd751a-76e6-4983-b950-8f553ce9e703" />
---

---

## 📧 Example Output (Monitoring Email)

> An email alert is triggered when workflow activity is restored after inactivity:

```
Subject: Monitoring Status!!
Body:
Activity restored at time: 2025/05/16 14:01:44 after being inactive for 3 minutes

Standard FlowFile Metadata:
id = 'e0fcd4df6-bb9f-4f94-8fba-81638999ae4c'
entryDate = 'Fri May 16 14:01:44 EDT 2025'
fileSize = '81'
...
```

### Example API JSON Output:

```json
{
  "activity": "Take a bubble bath",
  "type": "relaxation"
}
```

---

## ✅ Key Highlights

* Complete **ETL Pipeline Automation** using NiFi visual flow.
* Integration with **PostgreSQL Database**.
* **REST API Consumption** and real-time data transformation.
* **File-based Data Storage** for processed outputs.
* **Workflow Monitoring with Email Alerts** for proactive tracking.

---

---

## 📝 Author

[Venkatesh Addala](https://github.com/yourusername)

---


## ⬇️ How to Use This Project

Download the NiFi Workflow Templates (JSON files):

```json
NiFi_FLow - API's.json
NiFi_Flow - postgresql.json
```
Install Apache NiFi on your local machine.
```
Official Docs: https://nifi.apache.org/docs.html
```
Import the Templates into your NiFi UI:
```
Open NiFi → Operate Palette → Upload Template → Select JSON files.
```
Configure Necessary Services:
```
Update controller services (e.g., DBCPConnectionPool for DB).
```
Adjust local paths for input/output folders.

Update email configurations if needed.

Start the Flow & Execute:

Simply start the processors and watch your pipeline in action.
