# Use an official Python runtime as a parent image
# https://hub.docker.com/r/nvidia/cuda/
FROM nvidia/cuda:11.7.1-base-ubuntu18.04

# Set the working directory in the container to /app
WORKDIR /app

# Install Miniconda
RUN apt-get update && apt-get install -y wget
RUN wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
RUN bash Miniconda3-latest-Linux-x86_64.sh -b
RUN rm Miniconda3-latest-Linux-x86_64.sh
ENV PATH=/root/miniconda3/bin:$PATH
RUN conda update -y conda

# Copy current directory files to the container at /app
COPY . /app

# Create and activate a new conda environment
RUN conda create -y -n clustering python=3.8

ENV PATH /root/miniconda3/envs/clustering/bin:$PATH

SHELL ["conda", "run", "-n", "clustering", "/bin/bash", "-c"]

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

# Install r and rpy2 in conda environment
RUN conda install r-base
RUN pip install rpy2

# Set the 'R_HOME' and 'R_LIBS_USER' environment variables
ENV R_HOME=/root/miniconda3/envs/clustering/lib/R
ENV R_LIBS_USER=/root/miniconda3/envs/clustering/lib/R/library

COPY install_mclust.py ./  
RUN python install_mclust.py

# Install STAGATE_pyG package from source code
WORKDIR /app/STAGATE_pyG
RUN python setup.py install

# Install Pysodb package from source code
WORKDIR /app/pysodb
RUN python setup.py install

RUN pip install palettable
RUN pip install scikit-misc
RUN pip install leidenalg
RUN pip install jupyter

WORKDIR /app


# Make port 8888 available to the world outside this container
EXPOSE 8888

# Run Jupyter Notebook when the container launches
CMD ["/root/miniconda3/envs/clustering/bin/jupyter", "notebook", "--no-browser", "--ip=0.0.0.0", "--allow-root", "--NotebookApp.token=''", "--NotebookApp.password=''", "--notebook-dir=/app"]
