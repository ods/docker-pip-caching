FROM python:3.8

ARG PIP_INDEX_URL
ARG PIP_TRUSTED_HOST
ENV PIP_INDEX_URL ${PIP_INDEX_URL:-https://pypi.org/simple}
ENV PIP_TRUSTED_HOST ${PIP_TRUSTED_HOST}

RUN pip install -U pip
RUN pip install git+https://github.com/ods/devpi-builder.git@wheel_inspect_1_6_0
