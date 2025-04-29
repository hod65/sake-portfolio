# ベースイメージはお好みですが、Node.js などの開発環境付きイメージがおすすめです
FROM mcr.microsoft.com/vscode/devcontainers/javascript-node:0-20

# root 権限でパッケージ更新＆必要ツールをインストール
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

# （オプション）SAM CLI を使う場合
RUN pip3 install --no-cache-dir aws-sam-cli

# 開発ユーザーに戻す
USER vscode

# 確認用（ビルド時に出力されます）
RUN aws --version && aws sso login help
