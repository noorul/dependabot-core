FROM ghcr.io/dependabot/dependabot-updater-core
RUN apt-get update \
  && apt-get upgrade -y \
  && apt-get install -y --no-install-recommends \
    gnupg2 sudo wget

ARG ERLANG_MAJOR_VERSION=25

RUN echo "deb http://binaries2.erlang-solutions.com/ubuntu/ jammy-esl-erlang-25 contrib" >> /etc/apt/sources.list
RUN wget https://binaries2.erlang-solutions.com/GPG-KEY-pmanager.asc \
  && sudo apt-key add GPG-KEY-pmanager.asc

RUN apt-get update \
  && apt-get install -y --no-install-recommends esl-erlang

# Install Elixir
# https://github.com/elixir-lang/elixir/releases
ARG ELIXIR_VERSION=v1.16.3
ARG ELIXIR_CHECKSUM=e8e81771bc6acd62a2c1bf1b31c3aa3d0a469415de3b243b99f3e2e2d639f5ea
RUN curl -sSLfO https://github.com/elixir-lang/elixir/releases/download/${ELIXIR_VERSION}/elixir-otp-${ERLANG_MAJOR_VERSION}.zip \
  && echo "$ELIXIR_CHECKSUM  elixir-otp-${ERLANG_MAJOR_VERSION}.zip" | sha256sum -c - \
  && unzip -d /usr/local/elixir -x elixir-otp-${ERLANG_MAJOR_VERSION}.zip \
  && rm -f elixir-otp-${ERLANG_MAJOR_VERSION}.zip
ENV PATH="$PATH:/usr/local/elixir/bin"

USER dependabot

COPY --chown=dependabot:dependabot hex/helpers /opt/hex/helpers
ENV MIX_HOME="/opt/hex/mix"
# https://github.com/hexpm/hex/releases
ENV HEX_VERSION="2.0.6"
RUN bash /opt/hex/helpers/build

COPY --chown=dependabot:dependabot hex $DEPENDABOT_HOME/hex
COPY --chown=dependabot:dependabot common $DEPENDABOT_HOME/common
COPY --chown=dependabot:dependabot updater $DEPENDABOT_HOME/dependabot-updater
