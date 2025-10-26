FROM python:3.10-slim
iWORKDIR /app

# Install AWS CLI to authenticate with CodeArtifact
RUN apt-get update && apt-get install -y awscli

# Copy your source code and dependencies file
COPY requirements.txt .
COPY app.py .

# Environment arguments
ARG AWS_REGION=us-east-2
ARG AWS_ACCOUNT_ID=832871077374
ARG CODEARTIFACT_DOMAIN=python-domain
ARG CODEARTIFACT_REPO=python-libs

# Fetch CodeArtifact token and authenticate pip
RUN set -ex && \
    export CODEARTIFACT_AUTH_TOKEN=$(aws codeartifact get-authorization-token \
      --domain $CODEARTIFACT_DOMAIN \
      --domain-owner $AWS_ACCOUNT_ID \
      --region $AWS_REGION \
      --query authorizationToken --output text) && \
    pip config set global.index-url https://aws:$CODEARTIFACT_AUTH_TOKEN@$CODEARTIFACT_DOMAIN-$AWS_ACCOUNT_ID.d.codeartifact.$AWS_REGION.amazonaws.com/pypi/$CODEARTIFACT_REPO/simple/ && \
    pip install --upgrade pip && \
    pip install -r requirements.txt

CMD ["python", "app.py"]

