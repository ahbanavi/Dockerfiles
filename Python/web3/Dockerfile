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
RUN pip install --no-cache-dir python-dotenv
RUN pip install --no-cache-dir eth-brownie

RUN bash -c 'echo -e "import solcx\nsolcx.install_solc(\"0.8.9\")" | python'
# uncomment for installing all solc versions:
# RUN bash -c 'echo -e "import solcx\nfor x in solcx.get_installable_solc_versions(): solcx.install_solc(x,True)" | python'

FROM python:3.10-slim-buster

## install some dev tools
RUN apt update && apt upgrade && apt install -y zsh curl git nodejs npm
RUN npm install -g ganache-cli
RUN sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"

COPY --from=builder /root/.solcx /root/.solcx

## Activate virtualenv
COPY --from=builder /opt/venv /opt/venv
ENV PATH="/opt/venv/bin:$PATH"

WORKDIR /code
CMD zsh