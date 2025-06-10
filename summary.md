# 📘 Real-Time Uber Trips Project – Technical Summary

This project simulates real-time data ingestion, transformation, and monitoring using Microsoft Fabric's modern data stack including Eventstream, Lakehouse, KQL, and Power BI.

---

## 🔹 1. Data Ingestion (Bronze Layer)

- **Source Data**: 6 CSV files (3 for fare, 3 for trip data) were zipped and uploaded to Google Drive.
- **Extraction**: The ZIP was downloaded and extracted within a notebook.
- **Streaming**: Data was sent in batches from the notebook to **Microsoft Fabric Eventstream** to simulate real-time flow.
- **Batch Handling**: Due to large file sizes, the data was chunked and a sleep interval was added between batch sends to avoid overload.
- **Bronze Storage**: The raw data was written to the Lakehouse in Delta format as the Bronze layer.

📸 *Eventstream.*  

![eventstream](https://github.com/user-attachments/assets/717f6b08-65de-4804-9111-9e53e9d49c5f)

📸 *Lakehouse storage structure:*  

![lakehouseSchema](https://github.com/user-attachments/assets/b1db4ca8-e47c-43b5-8596-da1233bc708c)

---

## 🔹 2. Data Cleaning & Transformation (Silver Layer)

### ✅ Trip Data Transformations

- Converted `pickup_datetime` and `dropoff_datetime` to timestamp.
- Casted `store_and_fwd_flag` to Boolean (`Y` → `True`, others → `False`).
- Casted `medallion` and `hack_license` to String.
- Filtered out rows with non-positive values in `passenger_count`, `trip_time_in_secs`, or `trip_distance`.
- `rate_code`: Invalid values (not 1–6) were marked as `None`; then cast to `ByteType`.
- Casted `passenger_count` to `ShortType`.
- Checked duplicates on combination of `medallion`, `hack_license`, `pickup_datetime`.
- Validated coordinates and removed extreme/zero values.
- Applied **IQR outlier analysis** on numeric columns.
- Created a `trip_id` column via SHA2 hash of `medallion + hack_license + pickup_datetime`.
- Extracted `month` from `pickup_datetime` for partitioning.
- Saved to `silver.silver_uber_trip` with `partitionBy("month")` in Delta format.

---

### ✅ Fare Data Transformations

- Read from `bronze_uber_fare`.
- Dropped unneeded columns: `EventProcessedUtcTime`, `PartitionId`, `EventEnqueuedUtcTime`.
- Converted data types (`string`, `float`, `timestamp`).
- Converted `pickup_datetime` to timestamp.
- Translated `payment_type` to numeric codes.
- Flagged tip anomalies → `is_tip_anomaly` column.
- Recalculated `total_amount` and replaced the original value.
- Dropped `calculated_total`, `total_mismatch`.
- Removed `has_negative_values` if no `True` values existed.
- Created `trip_id` (same logic as trip data).
- Applied IQR analysis for outliers in `fare_amount`, `tip_amount`, `total_amount`.
- Added outlier flags.
- Extracted `month` for partitioning.
- Saved to `silver.silver_uber_fare` with `partitionBy("month")`.

---

## 🔹 3. Data Modeling (Gold Layer)

- Created a **Dataflow** to merge trip and fare Silver tables into a **fact table**.
- Built **dimension tables** (e.g., vendor, zone, datetime) from the merged dataset.
- Modeled a **star schema** structure.
- Published the final model and tables to the **Warehouse** destination.

📸 *Dataflow pipeline:* 

![dataflow](https://github.com/user-attachments/assets/3e0b9e51-4035-4ef7-9dcb-12b5fb840e6c)

📸 *Warehouse destination:* 

![warehouse](https://github.com/user-attachments/assets/f29fbef3-77bc-4f30-8c2e-738e6386c5df)

📸 *Star Schema design:*  

![starschematrip](https://github.com/user-attachments/assets/0f591595-0c91-4fad-bc09-2d2dc892232e)

---

## 🔹 4. Reporting & Real-Time Monitoring

- Used **Power BI (Fabric)** to build a live dashboard with metrics and visuals.
- Built a **semantic model** in Warehouse for Power BI.
- Configured **Eventhouse** and created a **KQL database** to query streaming data.
- Used KQL queries to drive the real-time dashboard.
- Set up **Activator** for rule-based alerts (e.g., "distance > 5 miles").
- Integrated alerts into the live dashboard for immediate visibility.

📸 *Power BI dashboard overview:*

![uberTripOverview](https://github.com/user-attachments/assets/8505105d-4293-4773-a724-a5e310e39364)
![report2](https://github.com/user-attachments/assets/ad56b26f-af7f-4214-b02e-aff29b5009ac)


📸 *Live distance monitoring example:* 

![distance live](https://github.com/user-attachments/assets/54f337c4-409f-455e-a38a-554a037f5003)


📸 *Activator rule setup:* 

![activator](https://github.com/user-attachments/assets/a288a4f8-e04d-4d6c-a33c-eeb803f55a22)


---

## ✅ Key Technologies

- **Microsoft Fabric**: Eventstream, Lakehouse, Eventhouse, Activator
- **Delta Lake** architecture: Bronze → Silver → Gold
- **PySpark** for data transformation and validation
- **Power BI** for interactive visualization
- **KQL (Kusto Query Language)** for real-time analytics

---

This summary showcases a full end-to-end pipeline from raw data ingestion to real-time alerting with cloud-native tools and best practices in data engineering.
