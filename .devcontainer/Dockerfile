# ベースイメージに Node.js ＆基本ツールを含む Dev Containers イメージを利用
FROM mcr.microsoft.com/vscode/devcontainers/javascript-node:0-18

# root 権限でパッケージ更新と必要ツールのインストール
USER root
RUN apt-get update \
    && apt-get install -y --no-install-recommends \
         curl \
         unzip \
    && rm -rf /var/lib/apt/lists/*

# AWS CLI v2 のダウンロード＆インストール
RUN curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "/tmp/awscliv2.zip" \
    && unzip /tmp/awscliv2.zip -d /tmp \
    && /tmp/aws/install --update \
    && rm -rf /tmp/aws /tmp/awscliv2.zip

# SAM CLI が必要なら（オプション）
RUN pip3 install --no-cache-dir aws-sam-cli

# 開発用ユーザーに戻す
USER vscode

# ビルドログで確認できるようにバージョン出力
RUN aws --version && aws sso login help
