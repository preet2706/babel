# Use an official pytorch runtime as a parent image
FROM pytorch/pytorch:1.13.1-cuda11.6-cudnn8-runtime

# Set the working directory
WORKDIR /app

# Copy the environment.yml file to the container
COPY environment.yml .

# Install dependencies without reinstalling pytorch and cuda
RUN apt-get update && \
    apt-get install -y wget git tmux && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Create a new conda environment called babel using environment.yml
# but exclude problematic packages that are already installed in the base image
RUN conda install -y -c conda-forge conda-libmamba-solver && \
    conda config --set solver libmamba && \
    grep -v "pytorch" environment.yml | grep -v "torchvision" | grep -v "cuda" > /tmp/filtered_env.yml && \
    conda env create -f /tmp/filtered_env.yml && \
    conda clean -afy

# Set up shell to use the babel environment by default
RUN conda init bash && \
    echo "conda activate babel" >> ~/.bashrc

# Make the environment available to the PATH
ENV PATH /opt/conda/envs/babel/bin:$PATH

# Set the working directory
WORKDIR /workspace

# Default command
CMD ["bash", "--login"]