## Créer un network monitoring
docker network create monitoring_network

## Personnaliser le fichier de configuration prometheus avec toutes les données que vous souhaitez analyser (target metrics)
nano ./prometheus/prometheus.yml

## Créer un container prometheus avec le fichier de config
docker run -d --name prometheus --network monitoring_network -v ./prometheus/prometheus.yml:/etc/prometheus/prometheus.yml -p 9090:9090 prom/prometheus

## Personnaliser le fichier alert avec toutes les alertes que vous souhaitez mettre en place (penser à personnaliser avec le nom de vos job)
nano ./prometheus/alert.yml

## Copier le fichier alert.yml dans le container prometheus
docker cp prometheus/alert.yml prometheus:/etc/prometheus/

## Redémarrer prometheus
docker restart prometheus

## Personaliser le fichier de config alertmanager avec vos crédentiels SMTP
nano ./alertmanager/alertmanager.yml

## Créer un container alertmanager avec le fichier de config
docker run -d --name alertmanager --network monitoring_network -v ./alertmanager/alertmanager.yml:/etc/alertmanager/alertmanager.yml -p 9093:9093 prom/alertmanager

## Créer un container grafana
docker run -d --name grafana --network monitoring_network -p 3000:3000 grafana/grafana

## Configure grafana