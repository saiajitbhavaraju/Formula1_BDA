# jupyter/Dockerfile

# Start from a base image that already has Jupyter and Spark
FROM jupyter/pyspark-notebook:spark-3.5.0

# Switch to user to install pip packages
USER $NB_UID

# Install FastF1, libraries for S3 (MinIO), and plotting
RUN pip install --no-cache-dir \
    fastf1 \
    boto3 \
    s3fs \
    matplotlib \
    seaborn

# Switch back to root for any system-level changes if needed (optional)
# USER root