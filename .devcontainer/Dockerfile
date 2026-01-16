FROM ubuntu:22.04

# 避免安装过程中的交互询问
ENV DEBIAN_FRONTEND=noninteractive

# 1. 安装基础编译工具、OpenMPI 和 Python
RUN apt-get update && apt-get install -y \
    build-essential \
    git \
    cmake \
    wget \
    python3 \
    python3-pip \
    openmpi-bin \
    libopenmpi-dev \
    libtool \
    automake \
    autoconf \
    vim \
    && rm -rf /var/lib/apt/lists/*

# 2. 安装 Python 数据分析库 (用于后续画图)
RUN pip3 install pandas numpy matplotlib seaborn jupyter

# 3. 下载并从源码编译 SST-DUMPI
# 这一步会自动下载代码，不需要你手动下载
WORKDIR /tmp
RUN git clone https://github.com/sstsimulator/sst-dumpi.git
WORKDIR /tmp/sst-dumpi
# 配置并编译
RUN ./bootstrap.sh && \
    ./configure --prefix=/usr/local/sst-dumpi --enable-quiet && \
    make -j$(nproc) && \
    make install

# 4. 清理垃圾文件
WORKDIR /workspace
RUN rm -rf /tmp/sst-dumpi

# 5. 设置环境变量，让系统找得到 DUMPI
ENV PATH="/usr/local/sst-dumpi/bin:${PATH}"
ENV LD_LIBRARY_PATH="/usr/local/sst-dumpi/lib:${LD_LIBRARY_PATH}"

CMD ["/bin/bash"]