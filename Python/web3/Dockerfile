FROM python:3.10 as builder

## Virtualenv
ENV VIRTUAL_ENV=/opt/venv
RUN python3 -m venv $VIRTUAL_ENV
ENV PATH="$VIRTUAL_ENV/bin:$PATH"

## Install dependencies:
RUN pip install --upgrade pip
RUN pip install --no-cache-dir black
RUN pip install --no-cache-dir web3
RUN pip install --no-cache-dir py-solc-x

FROM python:3.10-slim-buster

## Activate virtualenv
COPY --from=builder /opt/venv /opt/venv
ENV PATH="/opt/venv/bin:$PATH"

WORKDIR /code
CMD bash