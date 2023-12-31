FROM nvidia/cuda:11.8.0-cudnn8-devel-ubuntu22.04

ARG USERNAME=dockeruser
ARG USER_UID=1000
ARG USER_GID=$USER_UID
ARG DEBIAN_FRONTEND=noninteractive

RUN apt update && \
    apt install -y software-properties-common && \
    add-apt-repository ppa:git-core/ppa && \
    apt-add-repository ppa:fish-shell/release-3 && \
    apt-add-repository ppa:neovim-ppa/unstable && \
    add-apt-repository ppa:deadsnakes/ppa && \
    apt install -y \
        build-essential \
        cmake \
        curl \
        fish \
        git \
        neovim \
        python3.11 \
        python3.11-dev \
        python3-pip \
        ripgrep \
        sudo \
        tar \
        tmux \
        wget \
        && \
    update-alternatives --install /usr/bin/python python /usr/bin/python3.11 1

# Install python dependencies
COPY pyproject.toml poetry.lock ./
RUN python -m pip install poetry==1.7.0 && \
    python -m poetry config virtualenvs.create false && \
    python -m poetry install --no-interaction --no-ansi --no-root

# Create the user
RUN usermod --shell /usr/bin/fish root

# Setup dotfiles
ADD https://api.github.com/repos/andylolu2/dotfiles/git/refs/heads/main /tmp/version.json
RUN git clone https://github.com/andylolu2/dotfiles $HOME/.dotfiles && \
    $HOME/.dotfiles/main.fish