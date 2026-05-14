# Usa a imagem oficial e mais recente do Prometheus
FROM prom/prometheus:latest

USER root
RUN apk add --no-cache gettext

# Copia o arquivo de configuração customizado para o caminho padrão dentro do container
COPY config/prometheus.yml /etc/prometheus/prometheus.yml.template

RUN chown -R nobody:nobody /etc/prometheus

USER nobody

# O comando abaixo processa o template e gera o arquivo final antes de iniciar o Prometheus
ENTRYPOINT ["/bin/sh", "-c", "envsubst < /etc/prometheus/prometheus.yml.template > /etc/prometheus/prometheus.yml && exec /bin/prometheus --config.file=/etc/prometheus/prometheus.yml --storage.tsdb.path=/prometheus"]
