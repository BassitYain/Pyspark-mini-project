# Pyspark-mini-project
# PySpark Project

This project is built to demonstrate how to set up and work with Apache Spark in a Python environment using PySpark. It includes configurations for interacting with Spark via Jupyter notebooks and accessing datasets from Kaggle.

## Table of Contents

- [Requirements](#requirements)
- [Project Setup](#project-setup)
- [Usage](#usage)
- [Kaggle Integration](#kaggle-integration)
- [Running the Spark UI](#running-the-spark-ui)

## Requirements

Before starting, ensure you have the following installed:

- **Java 8** (required by Apache Spark)
- **Apache Spark 3.4.1** 
- **Python 3.x**
- **PySpark**
- **ngrok** (for accessing Spark UI remotely)

## Project Setup

### Step 1: Install Java and Spark

The notebook contains the following commands to install Java 8 and Spark on your local machine or cloud environment:

```bash
!apt-get install openjdk-8-jdk-headless -qq > /dev/null
!wget -q https://dlcdn.apache.org/spark/spark-3.4.1/spark-3.4.1-bin-hadoop3.tgz
!tar xf spark-3.4.1-bin-hadoop3.tgz
!pip install -q findspark
```

### Step 2: Set Up the PySpark Environment
In the notebook, we initialize a PySpark environment with the following code:

```python

import findspark
findspark.init()

from pyspark.sql import SparkSession

spark = (
    SparkSession
      .builder
      .appName("YourAppName")
      .master("local[*]")
      .config('spark.ui.port', '4050')  # Specify UI port
      .enableHiveSupport()  # Enables Hive support
      .getOrCreate()
)

sc = spark.sparkContext
```
### Step 3: Configure ngrok for Spark UI
To access the Spark UI remotely, ngrok is configured to expose the local port where Spark is running:

```bash
!wget https://bin.equinox.io/c/4VmDzA7iaHb/ngrok-stable-linux-amd64.zip
!unzip ngrok-stable-linux-amd64.zip
!./ngrok authtoken YOUR_NGROK_AUTH_TOKEN
!./ngrok http 4050 &
```
### Step 4: Python Libraries
Ensure you have the following Python packages installed:

- **PySpark**
- findspark
- kaggle
These are used for data manipulation, Spark session management, and downloading datasets from Kaggle.

## Usage

Once the environment is set up, you can run the notebook to perform data analysis using PySpark. Modify the Spark session configurations based on your system requirements or the scale of the data you are working with.

## Running Spark Jobs
You can write PySpark code for processing datasets, such as loading CSV files, performing transformations, and more.

Example code for initializing and using PySpark SQL functions:

```python

from pyspark.sql.functions import *
from pyspark.sql.types import *

# Example DataFrame manipulation
df = spark.read.csv('your_dataset.csv', header=True, inferSchema=True)
df.show()
```
## Kaggle Integration

To access Kaggle datasets, you need to authenticate using your Kaggle API credentials. Upload your kaggle.json file, which contains your Kaggle API token, and install the kaggle library with the following command:

```bash

!pip install -q kaggle
```
Then, authenticate with your Kaggle account and download datasets using the kaggle CLI.

### Running the Spark UI

To monitor Spark jobs and stages, the Spark UI is accessible through ngrok:

After setting up ngrok, you will receive a public URL for the Spark UI. You can retrieve this using the following command:
```bash

!curl -s http://localhost:4040/api/tunnels | grep -Po 'public_url":"(?=https)\\K[^"]*'
```
Open the provided URL to monitor the progress of your Spark jobs.

## License

This project is licensed under the MIT License. See the LICENSE file for details.

This README provides instructions on how to install, configure, and run the project, highlighting the use of PySpark, ngrok, and Kaggle. Let me know if you'd like to adjust any sections!
