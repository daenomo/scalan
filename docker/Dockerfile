# 1. ベースイメージとして公式のUbuntu 20.04 (Focal) を使用
FROM ubuntu:24.04

# APTの実行時にインタラクティブなプロンプトを無効化
ENV DEBIAN_FRONTEND=noninteractive

# 2. Amazon Correttoとsbtのインストールに必要なツールを導入
RUN apt-get update && \
    apt-get install -y wget gpg curl && \
    rm -rf /var/lib/apt/lists/*

# 3. Amazon Corretto 11 JDKをインストール
RUN wget -O- https://apt.corretto.aws/corretto.key | gpg --dearmor -o /usr/share/keyrings/corretto-keyring.gpg && \
    echo "deb [signed-by=/usr/share/keyrings/corretto-keyring.gpg] https://apt.corretto.aws stable main" | tee /etc/apt/sources.list.d/corretto.list && \
    apt-get update && \
    apt-get install -y java-11-amazon-corretto-jdk && \
    rm -rf /var/lib/apt/lists/*

# 4. sbtをインストール (手順は以前と同じ)
RUN echo "deb https://repo.scala-sbt.org/scalasbt/debian all main" | tee /etc/apt/sources.list.d/sbt.list && \
    echo "deb https://repo.scala-sbt.org/scalasbt/debian /" | tee /etc/apt/sources.list.d/sbt_old.list && \
    curl -sL "https://keyserver.ubuntu.com/pks/lookup?op=get&search=0x2EE0EA64E40A89B84B2DF73499E82A75642AC823" | gpg --no-default-keyring --keyring gnupg-ring:/etc/apt/trusted.gpg.d/scalasbt-release.gpg --import && \
    chmod 644 /etc/apt/trusted.gpg.d/scalasbt-release.gpg && \
    apt-get update && \
    apt-get install -y sbt && \
    rm -rf /var/lib/apt/lists/*

ARG USERNAME

RUN groupadd $USERNAME && \
    useradd -m -s /bin/bash -g $USERNAME -G sudo $USERNAME && \
    echo "$USERNAME   ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers
USER $USERNAME
WORKDIR /home/$USERNAME
