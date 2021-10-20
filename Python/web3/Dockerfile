FROM python:3.10 as builder

## Virtualenv
RUN python -m venv /opt/venv
ENV PATH="/opt/venv/bin:$PATH"

## Install dependencies:
COPY requirements.txt .
RUN pip install --no-cache-dir --upgrade pip && \
    pip install --no-cache-dir -r requirements.txt

RUN bash -c 'echo -e "import solcx\nsolcx.install_solc(\"0.8.9\")" | python'
# uncomment for installing all solc versions:
# RUN bash -c 'echo -e "import solcx\nfor x in solcx.get_installable_solc_versions(): solcx.install_solc(x,True)" | python'

FROM python:3.10-slim-buster

## install some dev tools
RUN apt-get update && apt-get install -y \ 
    zsh \
    curl \
    git \
    nodejs \
    npm \
    && sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)" \
    && npm install -g ganache-cli \
    && apt-get purge -y npm git curl \
    && rm -rf /var/lib/apt/lists/* 

COPY --from=builder /root/.solcx /root/.solcx
COPY --from=builder /opt/venv /opt/venv

## Activate virtualenv
ENV PATH="/opt/venv/bin:$PATH"

WORKDIR /usr/src/app
CMD zsh