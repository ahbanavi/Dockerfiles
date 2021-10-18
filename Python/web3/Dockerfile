FROM python:3.10 as builder

## Virtualenv
ENV VIRTUAL_ENV=/opt/venv
RUN python3 -m venv $VIRTUAL_ENV
ENV PATH="$VIRTUAL_ENV/bin:$PATH"

## Install dependencies:
RUN pip install --upgrade pip
RUN pip install --no-cache-dir black
RUN pip install --no-cache-dir cython==0.29.24
RUN pip install --no-cache-dir web3
RUN pip install --no-cache-dir py-solc-x

RUN bash -c 'echo -e "import solcx\nsolcx.install_solc(\"0.8.9\")" | python'
# uncomment for installing all solc versions:
# RUN bash -c 'echo -e "import solcx\nfor x in solcx.get_installable_solc_versions(): solcx.install_solc(x,True)" | python'

RUN python

FROM python:3.10-slim-buster

COPY --from=builder /root/.solcx /root/.solcx

## Activate virtualenv
COPY --from=builder /opt/venv /opt/venv
ENV PATH="/opt/venv/bin:$PATH"

WORKDIR /code
CMD bash