FROM alpine:latest

ENV TERRAFORM_VERSION=0.12.9
ENV TERRAFORM_PROVIDER_AZURERM=2.8.0
ENV TERRAFORM_PROVIDER_CLOUDFLARE=2.6.0
ENV TERRAFORM_PROVIDER_PINGDOM=1.1.1
ENV TERRAFORM_PROVIDER_K8S=0.7.6
ENV TERRAFORM_PROVIDER_VAULT=2.10.0

RUN apk add --update bash curl openssl

ADD https://releases.hashicorp.com/terraform/${TERRAFORM_VERSION}/terraform_${TERRAFORM_VERSION}_linux_amd64.zip ./

RUN unzip terraform_${TERRAFORM_VERSION}_linux_amd64.zip -d /bin
RUN rm -f terraform_${TERRAFORM_VERSION}_linux_amd64.zip


RUN ["addgroup", "-S", "myuser"]
RUN ["adduser", "-S", "-D", "-h", "/home/myuser", "-G", "myuser", "myuser"]
RUN ["mkdir", "-p", "/home/myuser/terraform.d/plugins/linux_amd64/"]

ADD https://releases.hashicorp.com/terraform-provider-azurerm/${TERRAFORM_PROVIDER_AZURERM}/terraform-provider-azurerm_${TERRAFORM_PROVIDER_AZURERM}_linux_amd64.zip ./
RUN unzip terraform-provider-azurerm_${TERRAFORM_PROVIDER_AZURERM}_linux_amd64.zip -d /home/myuser/terraform.d/plugins/linux_amd64/
RUN rm -f terraform-provider-azurerm_${TERRAFORM_PROVIDER_AZURERM}_linux_amd64.zip

ADD https://releases.hashicorp.com/terraform-provider-cloudflare/${TERRAFORM_PROVIDER_CLOUDFLARE}/terraform-provider-cloudflare_${TERRAFORM_PROVIDER_CLOUDFLARE}_linux_amd64.zip ./
RUN unzip terraform-provider-cloudflare_${TERRAFORM_PROVIDER_CLOUDFLARE}_linux_amd64.zip -d /home/myuser/terraform.d/plugins/linux_amd64/
RUN rm -f terraform-provider-cloudflare_${TERRAFORM_PROVIDER_CLOUDFLARE}_linux_amd64.zip

ADD https://releases.hashicorp.com/terraform-provider-vault/${TERRAFORM_PROVIDER_VAULT}/terraform-provider-vault_${TERRAFORM_PROVIDER_VAULT}_linux_amd64.zip ./
RUN unzip terraform-provider-vault_${TERRAFORM_PROVIDER_VAULT}_linux_amd64.zip -d /home/myuser/terraform.d/plugins/linux_amd64/
RUN rm -f terraform-provider-vault_${TERRAFORM_PROVIDER_VAULT}_linux_amd64.zip

ADD https://github.com/russellcardullo/terraform-provider-pingdom/releases/download/v${TERRAFORM_PROVIDER_PINGDOM}/terraform-provider-pingdom_v${TERRAFORM_PROVIDER_PINGDOM}_linux_amd64_static ./
RUN mv terraform-provider-pingdom_v${TERRAFORM_PROVIDER_PINGDOM}_linux_amd64_static /home/myuser/terraform.d/plugins/linux_amd64/terraform-provider-pingdom_v${TERRAFORM_PROVIDER_PINGDOM}

ADD https://github.com/banzaicloud/terraform-provider-k8s/releases/download/v${TERRAFORM_PROVIDER_K8S}/terraform-provider-k8s_${TERRAFORM_PROVIDER_K8S}_linux_amd64.tar.gz ./
RUN tar -xzf terraform-provider-k8s_${TERRAFORM_PROVIDER_K8S}_linux_amd64.tar.gz terraform-provider-k8s
RUN mv terraform-provider-k8s /home/myuser/terraform.d/plugins/linux_amd64/terraform-provider-k8s_v${TERRAFORM_PROVIDER_K8S}
RUN rm -f terraform-provider-k8s_${TERRAFORM_PROVIDER_K8S}_linux_amd64.tar.gz

RUN chown myuser:myuser -R /home/myuser
RUN chmod +x -R /home/myuser/terraform.d/plugins/linux_amd64/
USER myuser
WORKDIR /home/myuser/