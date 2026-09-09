FROM rust:latest

RUN git clone https://github.com/MrDogeBro/quicknav
WORKDIR "/quicknav"
RUN git pull --force
RUN mkdir .cargo
RUN touch .cargo/config.toml
RUN echo "[net]\ngit-fetch-with-cli = true" >> .cargo/config.toml
RUN wget https://github.com/ryankurte/cargo-binstall/releases/latest/download/cargo-binstall-x86_64-unknown-linux-musl.tgz
RUN tar -xvzf cargo-binstall-x86_64-unknown-linux-musl.tgz
RUN ./cargo-binstall --no-confirm cargo-deb


RUN apt update -y && apt upgrade -y
RUN apt install libc6-dev-i386 -y
RUN git clone --depth=1 https://github.com/raspberrypi/tools /raspberrypi-tools
ENV PATH=/raspberrypi-tools/arm-bcm2708/gcc-linaro-arm-linux-gnueabihf-raspbian-x64/bin/:$PATH
ENV CARGO_TARGET_ARM_UNKNOWN_LINUX_GNUEABIHF_LINKER=arm-linux-gnueabihf-gcc
RUN mkdir /root/.cargo
RUN echo "[target.arm-unknown-linux-gnueabihf]" >> /root/.cargo/config
RUN echo "strip = { path = \"arm-linux-gnueabihf-strip\" }" >> /root/.cargo/config
RUN echo "objcopy = { path = \"arm-linux-gnueabihf-objcopy\" }" >> /root/.cargo/config

COPY ./entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh
ENTRYPOINT ["/entrypoint.sh"]
