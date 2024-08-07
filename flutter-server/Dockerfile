FROM ubuntu:20.04

LABEL maintainer hogehoge

ENV TZ=Asia/Tokyo
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

RUN apt-get update && apt-get install -y \
        autoconf \
        bash \
        bzip2 \
        curl \
        file \
        wget \
        unzip \
        gnupg \
        software-properties-common \
        vim \
        git \
        clang \
        cmake \
        ninja-build \
        pkg-config \
        libgtk-3-dev

# Install Node.js (Choose either 18.x or 20.x based on your preference)
RUN curl -fsSL https://deb.nodesource.com/setup_20.x | bash -
RUN apt-get install -y nodejs

# Flutter
RUN echo "deb http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google.list \
    && wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add - \
    && add-apt-repository ppa:maarten-fonville/android-studio
RUN apt-get update && \
    apt-get -y -q install \
    xz-utils libglu1-mesa openjdk-8-jdk google-chrome-stable android-studio \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Android
WORKDIR /usr/local
RUN mkdir -p Android/sdk
ENV ANDROID_SDK_ROOT /usr/local/Android/sdk
RUN mkdir -p .android && touch .android/repositories.cfg
RUN wget -O sdk-tools.zip https://dl.google.com/android/repository/sdk-tools-linux-4333796.zip
RUN unzip sdk-tools.zip && rm sdk-tools.zip
RUN mv tools Android/sdk/tools

# Update SDK Tools
RUN wget -O commandlinetools-linux.zip https://dl.google.com/android/repository/commandlinetools-linux-7583922_latest.zip \
    && unzip commandlinetools-linux.zip -d Android/sdk \
    && rm commandlinetools-linux.zip

RUN mv Android/sdk/cmdline-tools Android/sdk/tools/latest \
    && mkdir -p Android/sdk/cmdline-tools \
    && mv Android/sdk/tools/latest Android/sdk/cmdline-tools/

RUN cd Android/sdk/cmdline-tools/latest/bin && yes | ./sdkmanager --licenses
RUN cd Android/sdk/cmdline-tools/latest/bin && ./sdkmanager "build-tools;29.0.2" "platform-tools" "platforms;android-29" "sources;android-29"
ENV PATH "$PATH:/usr/local/Android/sdk/platform-tools"

# Dart
RUN sh -c 'curl https://storage.googleapis.com/download.dartlang.org/linux/debian/dart_stable.list > /etc/apt/sources.list.d/dart_stable.list'
RUN apt-get update && \
    apt-get -y -q install \
    dart \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Download Flutter SDK
RUN git clone https://github.com/flutter/flutter.git
ENV PATH "$PATH:/usr/local/flutter/bin"

# Flutter Web
RUN flutter config --enable-web

RUN mkdir /flutter
WORKDIR /flutter


