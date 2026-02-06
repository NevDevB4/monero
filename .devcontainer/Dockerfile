# Monero Development Container for VS Code Insiders
FROM ubuntu:22.04

# Avoid warnings by switching to noninteractive
ENV DEBIAN_FRONTEND=noninteractive

# Configure apt and install packages
RUN apt-get update && apt-get install -y \
    # Essential build tools
    build-essential \
    cmake \
    pkg-config \
    libssl-dev \
    # Version control
    git \
    # Additional development tools
    gdb \
    curl \
    wget \
    ca-certificates \
    # Libraries and dependencies for Monero
    libboost-all-dev \
    libzmq3-dev \
    libunbound-dev \
    libsodium-dev \
    libunwind8-dev \
    liblzma-dev \
    libreadline-dev \
    libldns-dev \
    libexpat1-dev \
    libgtest-dev \
    libhidapi-dev \
    libusb-1.0-0-dev \
    libprotobuf-dev \
    protobuf-compiler \
    libudev-dev \
    # Additional utilities
    ccache \
    doxygen \
    graphviz \
    python3 \
    python3-pip \
    # Tools for debugging and profiling
    valgrind \
    strace \
    ltrace \
    # Clean up
    && apt-get autoremove -y \
    && apt-get clean -y \
    && rm -rf /var/lib/apt/lists/*

# Create a non-root user to use
ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=$USER_UID

RUN groupadd --gid $USER_GID $USERNAME \
    && useradd --uid $USER_UID --gid $USER_GID -m $USERNAME \
    && apt-get update \
    && apt-get install -y sudo \
    && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME

# Setup ccache
RUN mkdir -p /usr/lib/ccache && \
    ln -s /usr/bin/ccache /usr/lib/ccache/gcc && \
    ln -s /usr/bin/ccache /usr/lib/ccache/g++ && \
    ln -s /usr/bin/ccache /usr/lib/ccache/cc && \
    ln -s /usr/bin/ccache /usr/lib/ccache/c++

ENV PATH="/usr/lib/ccache:${PATH}"

# Create workspace directory
RUN mkdir -p /workspace && chown -R $USERNAME:$USERNAME /workspace

# Switch back to dialog for any ad-hoc use of apt-get
ENV DEBIAN_FRONTEND=dialog

# Set the default user
USER $USERNAME

# Set working directory
WORKDIR /workspace
