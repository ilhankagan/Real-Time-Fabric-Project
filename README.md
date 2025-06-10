# 🚕 Real-Time NYC Taxi Trips Monitoring with Microsoft Fabric

## 📌 Project Overview
This project simulates real-time taxi trip data using Microsoft Fabric and organizes the data using a Medallion Architecture (Bronze → Silver → Gold). We build streaming analytics, clean the data with PySpark, and visualize key metrics on Power BI dashboards.

## 🗂️ Project Structure
- **Bronze Layer**: Raw data ingestion via Eventstream
- **Silver Layer**: Data cleaning, deduplication, and partitioning with PySpark
- **Gold Layer**: Fact and dimension tables + Star Schema for analytics
- **Dashboard**: Real-time metrics, speed, duration, and route analysis
- **Eventstream / Eventhouse / KQL / Activator**: Streaming architecture

## 📊 Technologies Used
- Microsoft Fabric: Eventstream, Lakehouse, Eventhouse, KQL, Activator
- PySpark & Delta Tables
- Power BI (embedded in Fabric)
- GitHub for version control & documentation

## 📸 Screenshots
| Description | Image |
|------------|-------|
| Dashboard | ![uberTripOverview](https://github.com/user-attachments/assets/6ca8a2a1-d19b-44eb-8904-78dfc7c546f4) |
| Eventstream Panel | ![eventstream](https://github.com/user-attachments/assets/678ec6c5-d0e5-418a-862e-d102d44cd340) |
| Activator Alerts | ![activator](https://github.com/user-attachments/assets/3f9e09ff-7798-4ca4-b218-9953b2dd66f9) |
| Star Schema | ![starschematrip](https://github.com/user-attachments/assets/8d99b0e8-a437-4002-88fd-efa88de35346) |
| Real-Time Dashboard |![distance live](https://github.com/user-attachments/assets/93202c41-5a45-4505-a846-0a80571dddff) |

## 📁 How to Navigate This Repo
- `notebooks/`: PySpark notebooks for each layer
- `eventstream/`, `eventhouse/`, `activator/`: Configs and screenshots
- `summary.md`: Step-by-step explanation of what was done

## 🚀 Getting Started
You can simulate the project by downloading the datasets (not included due to size), or follow the instructions in the notebooks to run your own data.

## 🧠 Lessons Learned
- Medallion architecture simplifies complex ETL flows.
- Streaming analytics in Fabric is powerful and scalable.
- Data cleaning is critical before dashboarding.



