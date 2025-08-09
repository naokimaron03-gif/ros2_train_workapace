# 公式のUbuntu 22.04イメージを土台として使用
FROM ubuntu:22.04

# インストール中の対話式プロンプトを無効化
ENV DEBIAN_FRONTEND=noninteractive

# sudoとユーザー管理ツールを含む、基本パッケージをインストール
RUN apt-get update && apt-get install -y sudo curl gnupg lsb-release locales build-essential python3-pip git

# 'vscode' という名前のユーザーを作成し、パスワードなしで管理者権限(sudo)を使えるようにする
RUN useradd -m vscode -s /bin/bash && \
    usermod -aG sudo vscode && \
    echo "vscode ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers

# ROS 2のリポジトリをシステムに追加
RUN curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -o /usr/share/keyrings/ros-archive-keyring.gpg
RUN echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(lsb_release -cs) main" | tee /etc/apt/sources.list.d/ros2.list > /dev/null

# ROS 2 Humble Desktop（RVizなどGUIツールを含む）をインストール
RUN apt-get update && apt-get install -y ros-humble-desktop

# ---ここから、vscodeユーザーとして作業を実行する---
USER vscode
WORKDIR /home/vscode

# ROS 2のビルドツールをインストール
RUN pip3 install colcon-common-extensions

# vscodeユーザーのターミナル設定ファイルに、ROS 2環境の読み込みコマンドを書き込む
RUN echo "source /opt/ros/humble/setup.bash" >> /home/vscode/.bashrc

# 最後に管理者ユーザーに戻って後処理
USER root
RUN rm -rf /var/lib/apt/lists/*
