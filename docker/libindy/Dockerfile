FROM ubuntu:16.04

# Install environment
RUN apt-get update -y && \
apt-get install -y \
    git \
    curl \
    build-essential \
    pkg-config \
    cmake \
    libssl-dev \
    libsqlite3-dev \
    libsodium-dev \
    libzmq3-dev \
    apt-transport-https \
    ca-certificates

RUN apt-get install -y sqlite3 libsqlite3-dev

ENV HOME=/app
WORKDIR $HOME

# Install rust toolchain
RUN curl -o rustup https://sh.rustup.rs
RUN chmod +x rustup
RUN ./rustup -y

RUN git clone https://github.com/bcgov/indy-sdk.git
WORKDIR $HOME/indy-sdk/libindy
RUN git fetch
RUN $HOME/.cargo/bin/cargo build

# Move libindy to lib path
RUN mv target/debug/libindy.so /usr/lib

# Remove build tool chain
RUN rm -rf $HOME/.cargo \
  $HOME/indy-sdk \
  $HOME/.rustup \
  $HOME/.multirust \
  $HOME/rustup

RUN apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 68DB5E88
ARG indy_stream=master
RUN echo "deb https://repo.sovrin.org/deb xenial $indy_stream" >> /etc/apt/sources.list

WORKDIR $HOME
