# Estágio 1: Baixa a imagem oficial do Alertmanager apenas para extrair os binários
FROM prom/alertmanager:latest AS alertmanager_base

# Estágio 2: Cria a imagem final usando o Alpine
FROM alpine:latest

USER root
# Instala o gettext para disponibilizar o envsubst
RUN apk add --no-cache gettext

# Copia os binários essenciais do Alertmanager do primeiro estágio
COPY --from=alertmanager_base /bin/alertmanager /bin/alertmanager
COPY --from=alertmanager_base /bin/amtool /bin/amtool

# Cria os diretórios de configuração e storage, e ajusta as permissões (o Alpine já tem o usuário nobody)
RUN mkdir -p /etc/alertmanager /alertmanager && chown -R nobody:nobody /etc/alertmanager /alertmanager

# Copia o seu arquivo de configuração customizado para o caminho de template
COPY config/alertmanager.yml /etc/alertmanager/alertmanager.yml.template

USER nobody

# O comando abaixo processa o template e gera o arquivo final antes de iniciar o Alertmanager
ENTRYPOINT ["/bin/sh", "-c", "envsubst < /etc/alertmanager/alertmanager.yml.template > /etc/alertmanager/alertmanager.yml && exec /bin/alertmanager --config.file=/etc/alertmanager/alertmanager.yml --storage.path=/alertmanager"]
