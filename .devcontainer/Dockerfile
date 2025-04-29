# ───────────────────────────────────────────────────────
# 1. ベースイメージ
# ───────────────────────────────────────────────────────
FROM mcr.microsoft.com/devcontainers/javascript-node:1-20-bullseye

# ───────────────────────────────────────────────────────
# 2. 必要パッケージのインストール（root権限）
# ───────────────────────────────────────────────────────
USER root

RUN apt-get update \
 && apt-get install -y --no-install-recommends \
      sudo \
      locales \
      curl \
      unzip \
      git \
      openssh-client \
      python3-pip \
 && rm -rf /var/lib/apt/lists/*

# ───────────────────────────────────────────────────────
# 3. 日本語ロケールの生成＆設定
# ───────────────────────────────────────────────────────
RUN sed -i 's|# *\(ja_JP\.UTF-8 UTF-8\)|\1|' /etc/locale.gen \
 && locale-gen \
 && update-locale LANG=ja_JP.UTF-8

ENV LANG=ja_JP.UTF-8 \
    LANGUAGE=ja_JP:ja \
    LC_ALL=ja_JP.UTF-8

# AWS CLI v2 のダウンロード＆インストール
RUN curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "/tmp/awscliv2.zip" \
 && unzip /tmp/awscliv2.zip -d /tmp \
 && /tmp/aws/install \
 && rm -rf /tmp/awscliv2.zip /tmp/aws

# SAM CLI のインストール
RUN pip3 install --no-cache-dir aws-sam-cli

# ───────────────────────────────────────────────────────
# 6. 非 root 用ユーザー devuser の作成
#    UID/GID を 9000 に変更
# ───────────────────────────────────────────────────────
ARG USERNAME=devuser
ARG USER_UID=9000
ARG USER_GID=9000

# グループが既にある場合は無視、ユーザーがなければ作成
RUN groupadd --gid $USER_GID $USERNAME 2>/dev/null || true \
 && id -u $USERNAME 2>/dev/null || useradd --uid $USER_UID --gid $USER_GID -m -s /bin/bash $USERNAME \
 && echo "$USERNAME ALL=(ALL) NOPASSWD:ALL" > /etc/sudoers.d/$USERNAME \
 && chmod 0440 /etc/sudoers.d/$USERNAME

# ───────────────────────────────────────────────────────

# ───────────────────────────────────────────────────────
# 8. デフォルトユーザーを devuser に切り替え
# ───────────────────────────────────────────────────────
USER ${USERNAME}
