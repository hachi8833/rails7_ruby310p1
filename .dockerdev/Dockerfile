ARG RUBY_VERSION
# 後述
FROM ruby:$RUBY_VERSION-slim-buster

ARG NODE_MAJOR
ARG YARN_VERSION

# 共通の依存関係
RUN apt-get update -qq \
  && DEBIAN_FRONTEND=noninteractive apt-get install -yq --no-install-recommends \
    build-essential \
    gnupg2 \
    curl \
    less \
    git \
  && apt-get clean \
  && rm -rf /var/cache/apt/archives/* \
  && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* \
  && truncate -s 0 /var/log/*log

# NodeJSをsources listに追加
RUN curl -sL https://deb.nodesource.com/setup_$NODE_MAJOR.x | bash -

# Yarnをsources listに追加
RUN curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - \
  && echo 'deb http://dl.yarnpkg.com/debian/ stable main' > /etc/apt/sources.list.d/yarn.list

# 依存関係をインストール
COPY ./.dockerdev/Aptfile /tmp/Aptfile
RUN apt-get update -qq && DEBIAN_FRONTEND=noninteractive apt-get -yq dist-upgrade && \
  DEBIAN_FRONTEND=noninteractive apt-get install -yq --no-install-recommends \
    sqlite3 \
    libsqlite3-dev \
    nodejs \
    yarn=$YARN_VERSION-1 \
    $(grep -Ev '^\s*#' /tmp/Aptfile | xargs) && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* && \
    truncate -s 0 /var/log/*log

# bundlerとPATHを設定
ENV LANG=C.UTF-8\
  GEM_HOME=/bundle\
  BUNDLE_JOBS=4\
  BUNDLE_RETRY=3
ENV BUNDLE_PATH $GEM_HOME
ENV BUNDLE_APP_CONFIG=$BUNDLE_PATH\
  BUNDLE_BIN=$BUNDLE_PATH/bin
ENV PATH /app/bin:$BUNDLE_BIN:$PATH

# RubyGemsをアップグレードして必要なバージョンのbundlerをインストール
RUN gem update --system &&\
    mkdir -p /app

WORKDIR /app