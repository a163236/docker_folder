FROM ubuntu:latest
RUN apt-get update && apt-get upgrade -y
RUN apt-get install -y tzdata

# riscv パス
ENV RISCV=/opt/riscv
ENV PATH=$RISCV/bin:$PATH
# timezone setting
ENV TZ=Asia/Tokyo

RUN apt-get install -y git autoconf automake autotools-dev curl python3 libmpc-dev libmpfr-dev libgmp-dev gawk build-essential bison flex texinfo gperf libtool patchutils bc zlib1g-dev libexpat-dev

# riscv-gnu-toolchainのrvv-intrinsicブランチ
RUN git clone https://github.com/riscv/riscv-gnu-toolchain && \
  cd riscv-gnu-toolchain && \
  git checkout rvv-intrinsic && \
  ./configure --prefix=/opt/riscv && \
  make newlib -j$(nproc)

# riscv Spike
RUN apt-get install -y device-tree-compiler
RUN git clone https://github.com/riscv/riscv-isa-sim.git && \
  cd riscv-isa-sim && \
  mkdir build && \
  cd build && \
  ../configure --prefix=$RISCV && \
  make -j$(nproc) && \
  make install

# riscv procy kernel
RUN git clone https://github.com/riscv/riscv-pk && \
  cd riscv-pk && \
  mkdir build && \
  cd build && \
  ../configure --prefix=$RISCV --host=riscv64-unknown-elf && \
  make -j$(nproc) && \
  make install

# ================================================================　マルチステージビルド

FROM ubuntu:latest

RUN apt-get update && apt-get upgrade -y
RUN apt-get install -y tzdata

# 実行に最低限必要そうなパッケージ
RUN apt-get install -y gcc device-tree-compiler

# timezone setting
ENV TZ=Asia/Tokyo

# set local to jp
RUN apt-get install -y language-pack-ja && \
    update-locale LANG=ja_JP.UTF-8
ENV LANG ja_JP.UTF-8
ENV LANGUAGE ja_JP:ja
ENV LC_ALL ja_JP.UTF-8

# riscv パス
ENV RISCV=/opt/riscv
ENV PATH=$RISCV/bin:$PATH

# homeディレクトリへ
WORKDIR /home
# デフォルトのdash から bashへ  sourceコマンドが使えないから
SHELL ["/bin/bash", "-c"]
CMD ["/bin/bash"] 
COPY --from=0 /opt/riscv/ /opt/riscv

