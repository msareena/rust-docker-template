FROM ubuntu:22.04

ARG USER_NAME=docker-user
ARG USER_HOME=/home/${USER_NAME}
ARG USER_UID=1000
ARG USER_GID=1000
ARG USER_SHELL=/bin/bash
ARG WORKSPACE=/home/docker-user/workspace

RUN apt-get update && \
    DEBIAN_FRONTEND="noninteractive" apt-get install -y \
    curl \
    ssh \
    sudo \
    vim \
    build-essential \
    gdb \
    cmake \
    pkg-config \
    libssl-dev \
    git && \
    apt-get clean

RUN \
    # Create a non-root user
    groupadd --gid ${USER_GID} ${USER_NAME} && \
    useradd -m -u ${USER_UID} -g ${USER_GID} -G sudo \
            -s ${USER_SHELL} ${USER_NAME} && \
    mkdir ${WORKSPACE} && \
    chown -R ${USER_NAME}:${USER_NAME} ${WORKSPACE} && \
    echo ${USER_NAME} ALL=\(ALL\) NOPASSWD:ALL \
         > /etc/sudoers.d/${USER_NAME} && \
    chmod 0440 /etc/sudoers.d/${USER_NAME}

RUN \
    echo 'root:docker' | chpasswd && \
    echo 'docker-user:docker' | chpasswd

WORKDIR ${WORKSPACE}

USER ${USER_NAME}

RUN curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y

ENV PATH="${USER_HOME}/.cargo/bin:${PATH}"

RUN cargo install cargo-edit
