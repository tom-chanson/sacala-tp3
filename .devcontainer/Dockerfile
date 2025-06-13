# See here for image contents: https://github.com/microsoft/vscode-dev-containers/tree/v0.236.0/containers/java/.devcontainer/base.Dockerfile

# [Choice] Java version (use -bullseye variants on local arm64/Apple Silicon): 11, 17, 11-bullseye, 17-bullseye, 11-buster, 17-buster
ARG VARIANT="17-bullseye"
FROM mcr.microsoft.com/vscode/devcontainers/java:${VARIANT}

ARG SPARK_VERSION=3.5.6

ENV SPARK_DIST="spark-${SPARK_VERSION}-bin-hadoop3"
ENV SPARK_HOME=/opt/spark
ENV SPARK_USER=spark

ENV JUPYTER_HOME=/opt/jupyter
ENV VENV_DIR="${JUPYTER_HOME}/.venv"

COPY --chmod=755 spark /etc/init.d/spark

# Install Spark
RUN wget -nv https://dlcdn.apache.org/spark/spark-${SPARK_VERSION}/${SPARK_DIST}.tgz && \
    tar -xzf ${SPARK_DIST}.tgz && \
    mv ${SPARK_DIST} ${SPARK_HOME} && \
    rm ${SPARK_DIST}.tgz && \
    adduser --system --group --no-create-home --shell /usr/sbin/nologin ${SPARK_USER} && \
    update-rc.d spark defaults && \
    update-rc.d spark enable 

# Install Jupyter and Toree
RUN apt update -y && \
    apt install -y --no-install-recommends \
    python3-pip python3-venv && \
    python3 -m venv ${VENV_DIR} && \
    ${VENV_DIR}/bin/pip install --upgrade pip && \
    ${VENV_DIR}/bin/pip install toree jupyter && \
    ${VENV_DIR}/bin/jupyter toree install --spark_home=${SPARK_HOME} --interpreters=Scala,SQL

