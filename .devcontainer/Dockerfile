FROM ubuntu:latest

ENV PATH="/root/miniconda3/bin:${PATH}"
ARG PATH="/root/miniconda3/bin:${PATH}"

RUN set -x

# Basic Installation and fixing timezone
RUN apt-get update  \
    && apt-get -y install curl \
    && apt-get -y install net-tools \
    && apt-get -y install procps \
    && apt-get -y install build-essential \
    && apt-get -y install docker.io\
    && apt-get install -yq tzdata sudo jq \
    && ln -fs /usr/share/zoneinfo/Asia/Dubai /etc/localtime \
    && dpkg-reconfigure -f noninteractive tzdata \
    && apt-get install -y wget graphviz libgraphviz-dev unzip git sudo \
    && rm -rf /var/lib/apt/lists/*

# Github CLI Installation
RUN type -p curl >/dev/null || (sudo apt update && sudo apt install curl -y)

RUN curl -fsSL https://cli.github.com/packages/githubcli-archive-keyring.gpg | sudo dd of=/usr/share/keyrings/githubcli-archive-keyring.gpg \
    && sudo chmod go+r /usr/share/keyrings/githubcli-archive-keyring.gpg \
    && echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/githubcli-archive-keyring.gpg] https://cli.github.com/packages stable main" | sudo tee /etc/apt/sources.list.d/github-cli.list > /dev/null \
    && sudo apt update \
    && sudo apt install gh -y
    
# Miniconda Installation
RUN wget \
    https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh \
    && mkdir /root/.conda \
    && bash Miniconda3-latest-Linux-x86_64.sh -b \
    && rm -f Miniconda3-latest-Linux-x86_64.sh 

RUN conda update -n base -c defaults conda -y \
    && conda init bash \
    && conda install -y numpy pandas matplotlib scipy sympy autopep8 requests \
    && conda install -c conda-forge notebook \
    && conda clean -a     

# FROM base AS promptflow
COPY requirements.txt .
RUN pip install -r requirements.txt

# Install notebook depenency
RUN pip install ipython ipykernel
RUN ipython kernel install --user --name promptflow


RUN set +x

CMD bash
