FROM ubuntu:latest
RUN apt-get update && apt-get upgrade -y
RUN apt-get install -y tzdata

# riscv パス
ENV RISCV=/opt/riscv32
ENV PATH=$RISCV/bin:$PATH
# timezone setting
ENV TZ=Asia/Tokyo

RUN apt-get install -y git autoconf automake autotools-dev curl python3 libmpc-dev libmpfr-dev libgmp-dev gawk build-essential bison flex texinfo gperf libtool patchutils bc zlib1g-dev libexpat-dev

# riscv-gnu-toolchainのrvv-intrinsicブランチ
RUN git clone https://github.com/a163236/riscv-gnu-toolchain.git && \
  cd riscv-gnu-toolchain && \
  git submodule update --init --recursive && \
  ./configure --prefix=/opt/riscv32 --with-arch=rv32i --with-abi=ilp32 && \
  make newlib -j$(nproc)

# ================================================================　マルチステージビルド

FROM ubuntu:latest

RUN apt-get update && apt-get upgrade -y
RUN apt-get install -y tzdata
RUN apt-get install -y nano bsdmainutils git build-essential

# timezone setting
ENV TZ=Asia/Tokyo

# set local to jp
RUN apt-get install -y language-pack-ja && \
    update-locale LANG=ja_JP.UTF-8
ENV LANG ja_JP.UTF-8
ENV LANGUAGE ja_JP:ja
ENV LC_ALL ja_JP.UTF-8

# riscv パス
ENV RISCV=/opt/riscv32
ENV PATH=$RISCV/bin:$PATH

# homeディレクトリへ
WORKDIR /home
# デフォルトのdash から bashへ  sourceコマンドが使えないから
SHELL ["/bin/bash", "-c"]
CMD ["/bin/bash"] 
COPY --from=0 /opt/riscv32/ /opt/riscv32

