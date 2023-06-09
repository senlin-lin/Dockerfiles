# Use an official Python runtime as a parent image
FROM continuumio/miniconda3:4.9.2

# Set the working directory in the container to /app
WORKDIR /app

# Copy current directory files to the container at /app
COPY . /app

# Create and activate a new conda environment
RUN conda create -y -n svg_detection python=3.9

ENV PATH /opt/conda/envs/svg_detection/bin:$PATH

SHELL ["conda", "run", "-n", "svg_detection", "/bin/bash", "-c"]

RUN conda update -n base -c defaults conda
RUN pip install --upgrade pip

# Install gcc and build-essential
RUN echo 'debconf debconf/frontend select Noninteractive' | debconf-set-selections
RUN apt-get update --allow-releaseinfo-change && apt-get install -y gcc build-essential

# Fix numpy version to be compatible with numba
RUN pip install "numpy<1.25,>=1.21"

# Install sepal package from source code
WORKDIR /app/sepal
RUN python setup.py install
RUN pip install anndata lap tqdm kneed scikit-learn gprofiler-official

# Install Pysodb package from source code
WORKDIR /app/pysodb
RUN python setup.py install

RUN pip install jupyter

WORKDIR /app

# Make port 8888 available to the world outside this container
EXPOSE 8888

# Run Jupyter Notebook when the container launches
CMD ["/opt/conda/envs/svg_detection/bin/jupyter", "notebook", "--no-browser", "--ip=0.0.0.0", "--allow-root", "--NotebookApp.token=''", "--NotebookApp.password=''", "--notebook-dir=/app"]
