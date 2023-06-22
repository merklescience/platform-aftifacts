FROM debian:bookworm-slim
ENV TERRAFORM_VERSION=1.5.1
ENV KUBECTL_VERSION=1.27.3

RUN apt-get update && apt-get install --no-install-recommends -y \
  wget \
  curl \
  gettext-base \
  unzip \
  jq \
  python3 \
  apt-transport-https \
  ca-certificates \
  gnupg \
  # cleaning up unused files
  && apt-get purge -y --auto-remove -o APT::AutoRemove::RecommendsImportant=false \
  && rm -rf /var/lib/apt/lists/*

RUN wget --quiet https://releases.hashicorp.com/terraform/${TERRAFORM_VERSION}/terraform_${TERRAFORM_VERSION}_linux_amd64.zip \
  && unzip terraform_${TERRAFORM_VERSION}_linux_amd64.zip \
  && mv terraform /usr/local/bin \
  && rm terraform_${TERRAFORM_VERSION}_linux_amd64.zip \
  && rm -rf /tmp/* \
  && rm -rf /var/tmp/*

RUN curl -LO https://dl.k8s.io/release/v${KUBECTL_VERSION}/bin/linux/amd64/kubectl \
  && chmod +x ./kubectl \
  && mv ./kubectl /usr/local/bin/kubectl

RUN echo "deb [signed-by=/usr/share/keyrings/cloud.google.gpg] https://packages.cloud.google.com/apt cloud-sdk main" | tee -a /etc/apt/sources.list.d/google-cloud-sdk.list \
  && curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key --keyring /usr/share/keyrings/cloud.google.gpg add - \
  && apt-get update && apt-get install -y google-cloud-cli \
  # cleaning up unused files
  && apt-get purge -y --auto-remove -o APT::AutoRemove::RecommendsImportant=false \
  && rm -rf /var/lib/apt/lists/*

RUN apt-get update \
  && apt-get install -y google-cloud-sdk-gke-gcloud-auth-plugin \
  # cleaning up unused files
  && apt-get purge -y --auto-remove -o APT::AutoRemove::RecommendsImportant=false \
  && rm -rf /var/lib/apt/lists/*
