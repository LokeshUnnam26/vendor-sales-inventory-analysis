# **Sales & Inventory Project**

- This project is an end-to-end Sales & Inventory Data Pipeline and Analysis System.
- It covers data ingestion → database storage → aggregation → logging → exploratory analysis → visualization.

The final insights are also consumed in Power BI dashboards.
 
## **Features**

- Centralized logging system **(logger_setup.py)**
- SQL Server database setup with SQL Authentication
- Data ingestion from CSV to SQL Server **(ingestion.py)**
- Aggregated table creation with performance optimizations **(aggTableCreation.py)** for reference look at **(aggTable.ipynb)**
- Exploratory Data Analysis with hypothesis testing **(analysis.ipynb)**
- Power BI dashboard for interactive insights

## **Installation**
1. Clone the repository
git clone https://github.com/<your-username>/sales-inventory.git
cd sales-inventory

2. Create a virtual environment
python -m venv venv
source venv/bin/activate    # Mac/Linux
venv\Scripts\activate       # Windows

3. Install dependencies
pip install -r requirements.txt

## **SQL Server Setup** (Very Important Step)
1. Install & Configure SQL Server + SSMS
      - Install SQL Server Express
      - Install SQL Server Management Studio (SSMS)

2. Enable SQL Server Authentication
      - By default, SQL Server runs in Windows Authentication Mode.
      - To enable SQL Authentication: Open SQL Server Configuration Manager
      - Navigate to: SQL Server Network Configuration → Protocols for SQLEXPRESS **Enable TCP/IP**
      - Restart SQL Server service
   
3. Set up Authentication
      - Open SSMS → Right click server → Properties → Security
      - Select SQL Server and Windows Authentication mode
      - Restart the SQL Server instance

4. Create a SQL Login
     - CREATE LOGIN root WITH PASSWORD = 'your_password';
     - CREATE USER root FOR LOGIN root;
     - ALTER SERVER ROLE sysadmin ADD MEMBER root;

5. Create Database
    - CREATE DATABASE sales_inventory_db;


## **Project Structure:**
## Project Files Overview

| File/Folder           | Description                                      |
|-----------------------|--------------------------------------------------|
| `ingestion.py`        | Loads CSV files into SQL Server database         |
| `aggTable.py`         | Creates aggregated tables for analysis           |
| `logger_setup.py`     | Centralized logging setup for all scripts        |
| `analysis.ipynb`      | Exploratory analysis & hypothesis testing        |
| `exploringData.ipynb` | Initial data exploration and EDA                 |
| `requirements.txt`    | Python dependencies for the project              |
| `.env`                | Environment variables (DB credentials, paths)    |
| `logs/`               | Folder storing centralized log files             |

## **Environment Variables (.env)**
      USERNAME=root
      PASSWORD=your_password
      SERVER=YOUR_SERVER_NAME\SQLEXPRESS01
      DATABASE=sales_inventory_db
      DRIVER=ODBC Driver 17 for SQL Server
      CSV_FOLDER=C:/Projects/EcommerceProject/Data

## **Common Errors & Fixes:**
| ❌ Error | ✅ Fix |
|----------|--------|
| **Cannot connect using SQL Authentication** | - Enable **TCP/IP** in SQL Server Configuration Manager <br> - Ensure **SQL Authentication mode** is enabled <br> - Use correct connection string:<br>`mssql+pyodbc://USERNAME:PASSWORD@SERVER/DATABASE?driver=ODBC+Driver+17+for+SQL+Server` |
| **Cannot drop database – “database in use”** | ```sql<br>ALTER DATABASE sales_inventory_db SET SINGLE_USER WITH ROLLBACK IMMEDIATE;<br>DROP DATABASE sales_inventory_db;<br>``` |
| **pyodbc.InterfaceError (Error 28000)** | Incorrect authentication mode. Enable **SQL Server Authentication** as explained above. |
| **TCP/IP not enabled** | Enable **TCP/IP** in SQL Server Configuration Manager → Restart SQL Server instance. |
| **Dynamic Ports (port = 0)** | - Disable dynamic ports, set static port **1433** <br> - Add port in connection string if needed:<br>`SERVER=SERVER_NAME,1433` |



## **Analysis & Hypothesis Testing:**

Example question:
“Is there a significant difference in profit margins between top-performing and low-performing vendors?”
Steps in analysis.ipynb:
   - Split vendors by quartiles of sales
   - Run Welch’s t-test
   - Result: Reject H₀ → significant difference exists
   - P-value < 0.05


## **Logging:**
   - All scripts use a centralized logger
   - Logs are saved inside /logs folder with timestamps
