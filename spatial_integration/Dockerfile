# Use an official Python runtime as a parent image
FROM continuumio/miniconda3:4.9.2

# Set the working directory in the container to /app
WORKDIR /app

# Copy current directory files to the container at /app
COPY . /app

# Create and activate a new conda environment
RUN conda create -y -n spatial_integration python=3.8

ENV PATH /opt/conda/envs/spatial_integration/bin:$PATH

SHELL ["conda", "run", "-n", "spatial_integration", "/bin/bash", "-c"]

RUN conda update -n base -c defaults conda
#RUN pip install --upgrade pip

# Install torch with CUDA
# Replace <torch_version> and <cuda_version> with your specific versions
RUN pip install torch==1.13+cu117 -f https://download.pytorch.org/whl/cu117/torch_stable.html

# Install PyTorch Geometric
# Replace the URL with the suitable version for your device system
RUN pip install pyg_lib torch_scatter torch_sparse torch_cluster torch_spline_conv torch_geometric -f https://data.pyg.org/whl/torch-1.13.0+cu117.html

# Install other python packages
RUN pip install -r STAGATE_pyG/requirement.txt

# Install STAGATE_pyG package from source code
WORKDIR /app/STAGATE_pyG
RUN python setup.py install

# Install Pysodb package from source code
WORKDIR /app/pysodb
RUN python setup.py install

RUN pip install palettable
RUN pip install harmonypy
RUN pip install igraph
RUN pip install louvain
RUN pip install leidenalg
RUN pip install scikit-misc
RUN pip install jupyter

WORKDIR /app

# Make port 8888 available to the world outside this container
EXPOSE 8888

# Run Jupyter Notebook when the container launches
CMD ["/opt/conda/envs/spatial_integration/bin/jupyter", "notebook", "--no-browser", "--ip=0.0.0.0", "--allow-root", "--NotebookApp.token=''", "--NotebookApp.password=''", "--notebook-dir=/app"]
