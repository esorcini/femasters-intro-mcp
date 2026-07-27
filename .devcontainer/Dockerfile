FROM node:22-bookworm-slim

ARG USERNAME=dev
ARG USER_UID=784
ARG USER_GID=784

ENV DEBIAN_FRONTEND=noninteractive

# --------------------------------------------------------------------
# Base development tools
# --------------------------------------------------------------------

RUN apt-get update && apt-get install -y --no-install-recommends \
    bash \
    build-essential \
    ca-certificates \
    curl \
    fd-find \
    git \
    iputils-ping \
    jq \
    less \
    openssh-client \
    procps \
    python3 \
    python3-pip \
    python3-venv \
    ripgrep \
    sudo \
    tar \
    tree \
    vim \
    postgresql-client \
 && rm -rf /var/lib/apt/lists/*

# Debian ships fd as fdfind
RUN ln -s /usr/bin/fdfind /usr/local/bin/fd

# --------------------------------------------------------------------
# Non-root user
# --------------------------------------------------------------------

RUN groupadd --gid ${USER_GID} ${USERNAME} \
 && useradd \
      --uid ${USER_UID} \
      --gid ${USER_GID} \
      --create-home \
      --shell /bin/bash \
      ${USERNAME}

RUN echo "${USERNAME} ALL=(ALL) NOPASSWD:ALL" \
    > /etc/sudoers.d/${USERNAME} \
 && chmod 0440 /etc/sudoers.d/${USERNAME}

USER ${USERNAME}

ENV PATH="/home/${USERNAME}/.local/bin:${PATH}"

# --------------------------------------------------------------------
# User tools
# --------------------------------------------------------------------

RUN python3 -m pip install --user --break-system-packages uv
RUN curl -fsSL https://opencode.ai/install | bash

# --------------------------------------------------------------------
# Environment
# --------------------------------------------------------------------
ENV OLLAMA_HOST=http://host.docker.internal:11434

ENV EDITOR=vim
ENV PAGER=less
ENV TERM=xterm-256color

CMD ["/bin/bash"]