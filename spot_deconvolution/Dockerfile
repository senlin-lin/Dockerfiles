# Use an official Python runtime as a parent image
FROM continuumio/miniconda3:4.9.2

# Set the working directory in the container to /app
WORKDIR /app

# Copy current directory files to the container at /app
COPY . /app

# Create and activate a new conda environment
RUN conda create -y -n spot_deconvolution python=3.8

# Update pip
RUN pip install --upgrade pip

ENV PATH /opt/conda/envs/spot_deconvolution/bin:$PATH

SHELL ["conda", "run", "-n", "spot_deconvolution", "/bin/bash", "-c"]

RUN conda update -n base -c defaults conda
#RUN pip install --upgrade pip

# Install tangram-sc
RUN pip install tangram-sc

# Install python packages
RUN pip install -r requirement.txt

# Install Pysodb package from source code
WORKDIR /app/pysodb
RUN python setup.py install

RUN pip install palettable
RUN pip install squidpy
RUN pip install scikit-misc
RUN pip install jupyter

WORKDIR /app

# Make port 8888 available to the world outside this container
EXPOSE 8888

# Run Jupyter Notebook when the container launches
CMD ["/opt/conda/envs/spot_deconvolution/bin/jupyter", "notebook", "--no-browser", "--ip=0.0.0.0", "--allow-root", "--NotebookApp.token=''", "--NotebookApp.password=''", "--notebook-dir=/app"]
