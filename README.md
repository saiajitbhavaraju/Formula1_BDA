# F1-BDA: A Big Data Analytics Platform

This project is an end-to-end data platform for ingesting, processing, and analyzing Formula 1 data. It uses a modern data stack (Docker, Spark, MinIO) to build a scalable system capable of running complex analyses, training ML models, and simulating real-time predictions.

## 🚀 Platform Architecture

This project runs a complete, containerized data platform on your local machine:
* **Ingestion (JupyterLab + FastF1):** A Python environment for fetching raw data from the FastF1 API.
* **Data Lake (MinIO):** An S3-compatible object storage for our "single source of truth." All raw data lands here, and all processed data is stored here.
* **Processing (Spark Cluster):** A distributed cluster (1 Master, 1 Worker) for running heavy, scalable data transformations and ML models.
* **Workbench (JupyterLab):** The central 'control center' for running ingestion scripts, analysis jobs, and visualizations.

[FastF1 API] --> [JupyterLab (Ingestion)] --> [MinIO Data Lake (raw-data)] | v [Spark Cluster] <--------------------------------- | v [MinIO Data Lake (processed-data & models)] --> [JupyterLab (Analysis)]

## 🛠️ Technology Stack

* **Orchestration:** Docker Compose
* **Data Lake:** MinIO (S3-Compatible Object Storage)
* **Processing Engine:** Apache Spark
* **Workbench & Ingestion:** JupyterLab (with `fastf1`, `pyspark`, `boto3`)

## 🚦 How to Use This Project

Follow these instructions in order. The notebooks are numbered and **must be run sequentially** to build the data, models, and services.

### Step 1: Launch the Platform

From the project's root directory, run the `docker-compose` command to build and start all the services in the background.

```
docker-compose up -d --build
```
### Step 2: Access Your Services
Once launched, you can access the UIs for each service in your browser:

JupyterLab (Your Workbench): http://localhost:8888
Password: f1project (as set in your docker-compose.yml)
MinIO (Your Data Lake): http://localhost:9001
Login: minioadmin
Password: minioadmin
Spark Master (Your Cluster): http://localhost:8080

### Step 3: Run the Data Pipeline
Open JupyterLab (http://localhost:8888) and run the notebooks in the following order.

Phase 1: Basic Pipelines (Laps & Telemetry)
01_Ingestion.ipynb: Fetches data for one race and saves it to MinIO.
02_Spark_Analysis.ipynb: Reads from MinIO, processes with Spark, and saves results.
03_Visualization.ipynb: Visualizes the results from Phase 1.
04_Telemetry_Ingestion.ipynb: Ingests large telemetry data.
05_Spark_Analysis.ipynb: Processes the telemetry data.
06_Telemetry_Viz.ipynb: Visualizes the telemetry comparison.

Phase 2: Build the "Product" (ML Model)
07_Bulk_Ingestion.ipynb: (This will take a few minutes) Ingests the entire 2024 season into MinIO.
08_ML_Model_Training.ipynb: (This will take a few minutes) Reads all 24 races from MinIO and trains a predictive ML model, saving it back to MinIO.

Phase 3: Run the Final Analyses
11_Pit_Stop_Power_Analysis.ipynb: (This will take a few minutes) Runs a complex Window Function on the entire 24-race dataset to find the "undercut" power.

Phase 4: Run the "Real-Time" Demo
You need two notebooks open for this:
First, run: 09_Streaming_Prediction_Service.ipynb. Run all cells. The last cell will "hang" and start listening for new files. This is correct.
Second, run: 10_Streaming_Trigger.ipynb. Run the cells to drop a new file into the streaming folder.
Watch Notebook 09: You will see it automatically detect the new file and print live predictions.

Step 4: Shut Down the Platform
When you are finished, stop and remove all the containers with this command:

```
docker-compose down
```
