# syntax=docker/dockerfile:1.4


# Base Image
# =============================================================================

FROM ubuntu:22.04

ENV DEBIAN_FRONTEND=noninteractive

RUN \
    apt-get update && \
    apt-get -y install \
        --no-install-recommends \
        locales \
        curl \
        make \
        git \
        gnupg2 \
        software-properties-common \
        python3-dev \
        python3-pip \
        python3-venv \
        python3-wheel

# Install an Algorand node
RUN \
    curl -o - https://releases.algorand.com/key.pub | tee /etc/apt/trusted.gpg.d/algorand.asc && \
    # curl -o - https://releases.algorand.com/key.pub | sudo tee /etc/apt/trusted.gpg.d/algorand.asc && \
    add-apt-repository "deb [arch=amd64] https://releases.algorand.com/deb/ stable main"  && \
    apt-get update  && \
    # To get both algorand and the devtools:
    apt-get install -y algorand-devtools
    # Or, to only install algorand:
    # apt-get install -y algorand

# # Install Algorand Node and tools
# RUN curl -O https://releases.algorand.com/install/install_master_linux-amd64.tar.gz && \
#     tar xf install_master_linux-amd64.tar.gz && \
#     ./install_master_linux-amd64_2.5.4/install -i -y -d ~/node/data

# # Installing Algorand dev tools
# RUN apt-get update && apt-get install -y curl git
# RUN curl https://raw.githubusercontent.com/algorand/go-algorand-doc/master/downloads/installers/update_dev.sh -O && \
#     chmod 544 update_dev.sh && \
#     ./update_dev.sh -n -c stable -p ~/node -d ~/node/data -r https://github.com/algorand/go-algorand

# Set up PATH for Algorand binaries
ENV PATH="${PATH}:/root/node/data"

# Cleanup
RUN \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* 

# Add non-root user
ARG USERNAME=nonroot
RUN groupadd --gid 1000 $USERNAME && \
    useradd --uid 1000 --gid 1000 -m $USERNAME

## Make sure to reflect new user in PATH
ENV PATH="/home/${USERNAME}/.local/bin:${PATH}"
USER $USERNAME


# Setting up Python environment
RUN pip install --upgrade pip
RUN pip install pyteal

# COPY --chown=nonroot:1000 requirements.txt /tmp/requirements.txt
# RUN pip install -r /tmp/requirements.txt && \
#     rm /tmp/requirements.txt


# WORKDIR /app
