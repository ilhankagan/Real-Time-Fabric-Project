## 🔹 Bronze Layer
- Downloaded CSV data from Google Drive.
- Sent data to → Eventstream.
- Wrote to Lakehouse in Delta format.

## 🔹 Silver Layer
- Cleaned data using PySpark: removed duplicates, fixed timestamps, validated coordinates.
- Generated `trip_id` using sha2 hash.
- Wrote to partitioned Delta tables.

## 🔹 Gold Layer
- Built dimensional model: fact_trips, dim_vendor, dim_zone, etc.
- Created semantic model in Power BI.
