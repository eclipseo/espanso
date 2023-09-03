FROM ubuntu:23.04

RUN apt-get update \
    && apt-get install -y libssl-dev \
    libxdo-dev libxtst-dev libx11-dev \
    libxkbcommon-dev libwxgtk3.2-dev libdbus-1-dev \
    wget git file build-essential pkg-config

ENV RUSTUP_HOME=/usr/local/rustup \
    CARGO_HOME=/usr/local/cargo \
    PATH=/usr/local/cargo/bin:$PATH \
    RUST_VERSION=1.69.0

RUN set -eux; \
    dpkgArch="$(dpkg --print-architecture)"; \
    case "${dpkgArch##*-}" in \
        amd64) rustArch='x86_64-unknown-linux-gnu'; rustupSha256=`wget -O - https://static.rust-lang.org/rustup/dist/${rustArch}/rustup-init.sha256 | awk '{print $1}'` ;; \
        armhf) rustArch='armv7-unknown-linux-gnueabihf'; rustupSha256=`wget -O - https://static.rust-lang.org/rustup/dist/${rustArch}/rustup-init.sha256 | awk '{print $1}'` ;; \
        arm64) rustArch='aarch64-unknown-linux-gnu'; rustupSha256=`wget -O - https://static.rust-lang.org/rustup/dist/${rustArch}/rustup-init.sha256 | awk '{print $1}'` ;; \
        i386) rustArch='i686-unknown-linux-gnu'; rustupSha256=`wget -O - https://static.rust-lang.org/rustup/dist/${rustArch}/rustup-init.sha256 | awk '{print $1}'` ;; \
        *) echo >&2 "unsupported architecture: ${dpkgArch}"; exit 1 ;; \
    esac; \
    url="https://static.rust-lang.org/rustup/dist/${rustArch}/rustup-init"; \
    wget "$url"; \
    echo "${rustupSha256} *rustup-init" | sha256sum -c -; \
    chmod +x rustup-init; \
    ./rustup-init -y --no-modify-path --profile minimal --default-toolchain $RUST_VERSION; \
    rm rustup-init; \
    chmod -R a+w $RUSTUP_HOME $CARGO_HOME; \
    rustup --version; \
    cargo --version; \
    rustc --version;

RUN mkdir espanso && cargo install rust-script --version "0.31.0" && cargo install --force cargo-make --version "0.36.13"

COPY . espanso

RUN cd espanso
