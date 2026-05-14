# Estágio 1: Baixa a imagem oficial do Prometheus apenas para extrair os binários
FROM prom/prometheus:latest AS prometheus_base

# Estágio 2: Cria a imagem final usando o Alpine
FROM alpine:latest

USER root
RUN apk add --no-cache gettext

# Copia os binários e diretórios oficiais do Prometheus do primeiro estágio
COPY --from=prometheus_base /bin/prometheus /bin/prometheus
COPY --from=prometheus_base /bin/promtool /bin/promtool

# Cria o diretório de storage e ajusta as permissões (o Alpine já tem o usuário nobody)
RUN mkdir -p /etc/prometheus /prometheus && chown -R nobody:nobody /etc/prometheus /prometheus

# Copia o arquivo de configuração customizado
COPY config/prometheus.yml /etc/prometheus/prometheus.yml.template

USER nobody

# O comando abaixo processa o template e gera o arquivo final antes de iniciar o Prometheus
ENTRYPOINT ["/bin/sh", "-c", "envsubst < /etc/prometheus/prometheus.yml.template > /etc/prometheus/prometheus.yml && exec /bin/prometheus --config.file=/etc/prometheus/prometheus.yml --storage.tsdb.path=/prometheus"]
