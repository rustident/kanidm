FROM rust:latest

ARG USERNAME=kanidmdev
# We need to keep the user as 1001 to match the GitHub runner's UID.
# See https://github.com/actions/checkout/issues/956.
ARG USER_UID=1001
ARG USER_GID=$USER_UID

ENV DEBIAN_FRONTEND=noninteractive


# Create the user
RUN groupadd --gid $USER_GID $USERNAME \
    && useradd --uid $USER_UID --gid $USER_GID -m $USERNAME \
    && apt-get update \
    && apt-get install -y sudo \
    && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME

WORKDIR /home/kanidmdev/app

COPY . /home/kanidmdev/app/


RUN ./scripts/install_ubuntu_dependencies.sh

RUN chown -R kanidmdev:kanidmdev /home/kanidmdev/app

RUN RUSTFLAGS=-Ctarget-feature=-crt-static cargo install wasm-pack \
    && rustup target add wasm32-unknown-unknown

USER $USERNAME
ENV CARGO_HOME=/home/$USERNAME/.cargo
ENV SHELL=/bin/bash
