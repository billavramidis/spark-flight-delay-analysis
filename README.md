# Flight Delay Analytics with Apache Spark

---

* **University of Macedonia** | **Department of Applied Informatics**
* **Course:** Big Data
* **Instructor:** Asimina Dimara
* **Student:** Vasileios Rafail Avramidis (ics23033)
---

## 📌 Project Overview
This project performs a comparative analysis of **Apache Spark’s RDDs** and the **DataFrame API**. Using a flight dataset, the project aims to identify patterns in flight delays while benchmarking the performance, usability, and optimization capabilities of both Spark abstractions.


> **Note:** This project was originally developed as part of an academic assignment at the **University of Macedonia**. The source analysis and accompanying report were conducted in **Greek**, while this documentation presents the technical findings in English.

---

## 📂 Dataset
The analysis is based on a dataset containing **2,000 flight records**. The following attributes were used for processing:

* `FL_DATE`: Date of flight
* `AIRLINE`: Airline code
* `ORIGIN_AIRPORT`: Departure airport code
* `DEST_AIRPORT`: Destination airport code
* `DEP_DELAY`: Departure delay (in minutes)
* `ARR_DELAY`: Arrival delay (in minutes)
* `CANCELLED`: Flight status (0 = Active, 1 = Cancelled)

## 🛠️ Technology Stack
* **Environment:** **Google Colab**
* **Language:** Python 3.12
* **Framework:** Apache Spark (PySpark 3.5.1)
* **Hardware:** AMD EPYC 7B12 (2 vCPUs, 12.7 GB RAM)
* **Visualization:** Matplotlib

---

## ⚔️ Comparative Analysis: RDD vs. DataFrame

This project implemented two distinct analytical queries to evaluate the trade-offs between low-level control (RDD) and high-level abstraction (DataFrame).

### 1. RDD Implementation
* **Task:** Calculate the average departure delay per **Airport** and identify the top 10 airports with the highest delays.
* **Methodology:**
    The RDD implementation required a manual, functional approach. The process involved filtering out cancelled flights, mapping raw lines to key-value pairs `(Origin_Airport, (Delay, 1))`, and using `reduceByKey` to aggregate the total delay and flight count per airport.
* **Observation:**
    This demonstrated the lower-level nature of RDDs, where the developer must explicitly define the "how" of the execution, resulting in more verbose code.

### 2. DataFrame Implementation
* **Task:** Calculate the average departure delay per **Flight Route** (Origin-Destination) and identify the top 10 routes with the highest delays.
* **Methodology:**
    The DataFrame implementation utilized a declarative syntax. The logic was expressed through high-level transformations such as `groupBy("ORIGIN_AIRPORT", "DEST_AIRPORT")` and `avg("DEP_DELAY")`.
* **Observation:**
    This approach was significantly more concise and readable. It allowed Spark to handle the underlying aggregation logic automatically using the Catalyst Optimizer.

---

## 📊 Performance Benchmarks
To strictly measure performance, the analytical queries were executed **5 times** on Google Colab. The highest and lowest execution times were discarded, and the average of the remaining three runs was recorded.

| Metric | RDD Average Time | DataFrame Average Time |
| :--- | :--- | :--- |
| **Total Runtime** | 1.341 s | 0.761 s |
| **First Action** | 1.318 s | 0.641 s |

**Performance Analysis:**
The benchmarks confirm that the DataFrame API is approximately **twice as fast** as the RDD implementation in this Python environment.
* **Catalyst Optimizer:** DataFrames benefit from the Catalyst Optimizer, which creates an efficient physical execution plan, reducing unnecessary calculations and data transfers.
* **Optimization Gap:** RDDs do not have this feature and execute instructions exactly as written, requiring the developer to manually optimize operations.

---

## 📊 Data Visualization

To make the statistical findings accessible, the project utilizes **Matplotlib** to generate visual reports. The notebook produces the following charts:

1.  **Top 10 Routes by Average Delay:** A bar chart ranking the specific flight paths with the most severe delays.
2.  **Average Departure Delay by Hour:** A distribution chart showing how delays fluctuate throughout the day, identifying peak congestion hours.
3.  **Departure Delays Per Airline:** A comparative bar chart displaying the total number of delays attributed to each carrier.
4.  **Departure Delays Per Month:** A temporal chart highlighting seasonal trends and month-over-month performance changes.

---

## 🚀 How to Run in Google Colab

The project is structured into **three separate notebooks**, each focusing on a specific aspect of the analysis:

1.  **RDD Analysis Notebook:** Contains the low-level RDD implementation for the Airport Delay task.
2.  **DataFrame Analysis Notebook:** Contains the DataFrame implementation for the Route Delay task and performance benchmarks.
3.  **Extra Queries & Visualization Notebook:** Contains the advanced analytics (Time of Day, Seasonal, Airline) and Matplotlib visualizations.

**Execution Instructions:**

1.  **Open:** Open the desired `.ipynb` file in Google Colab.
2.  **Run All:** Select **"Runtime" > "Run all"** from the menu.

> **✅ Automated Setup:**
> You do not need to manually upload files or install packages. The notebooks are configured to automatically:
> * **Install Dependencies:** Automatically installs PySpark and imports necessary libraries.
> * **Load Data:** Automatically fetches the dataset (`flights_2000.csv`) directly from the GitHub repository.
