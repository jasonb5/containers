ARG BASE=condaforge/mambaforge:latest
FROM ${BASE}

RUN groupadd -g 1000 jupyter && \
    useradd -g jupyter -m -u 1000 jupyter && \
    mamba install -y jupyterlab

WORKDIR /home/jupyter

COPY entrypoint.sh /entrypoint.sh

RUN chown -R root:jupyter /opt/conda && \
    chown jupyter:jupyter /entrypoint.sh

USER jupyter

ENTRYPOINT ["/entrypoint.sh"]
