# Use an official Python runtime as a parent image
FROM continuumio/miniconda3:4.9.2

# Set the working directory in the container to /app
WORKDIR /app

# Copy current directory files to the container at /app
COPY . /app

# Create and activate a new conda environment
RUN conda create -y -n alignment python=3.8

ENV PATH /opt/conda/envs/alignment/bin:$PATH

SHELL ["conda", "run", "-n", "alignment", "/bin/bash", "-c"]

RUN conda update -n base -c defaults conda
#RUN pip install --upgrade pip

# Install paste-bio
RUN pip install paste-bio

# Install other python packages
RUN pip install -r requirements.txt

# Install Pysodb package from source code
WORKDIR /app/pysodb
RUN python setup.py install

RUN pip install jupyter

WORKDIR /app


# Make port 8888 available to the world outside this container
EXPOSE 8888

# Run Jupyter Notebook when the container launches
CMD ["/opt/conda/envs/alignment/bin/jupyter", "notebook", "--no-browser", "--ip=0.0.0.0", "--allow-root", "--NotebookApp.token=''", "--NotebookApp.password=''", "--notebook-dir=/app"]
