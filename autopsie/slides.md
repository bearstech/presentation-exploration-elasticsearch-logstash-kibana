!SLIDE
# Comprendre le cheminement d'HTTP

!SLIDE

## HTTP

HTTP est un format bien normalisé, orienté texte (avant sa version 2.0).

HTTP est prévu pour être empilé, les proxys sont inévitables.

HTTP est partout, et dans REST en particulier.

!SLIDE

## Visualiser et comprendre

Vérifier que le routage se passe comme prévu.

Ça plante, mais qui est le responsable?

Cette application utilise des webservices, c'est peut être la cause de ses ralentissements?

!SLIDE

## Empiler les proxys

Du cache avec Varnish.

De la haute disponibilité avec HAproxy.

Le classique serveur web avec Nginx ou Apache.

Le serveur d'application, tout au fond.

!SLIDE

## Journeaux applicatifs

Des logs, quoi.

Pratique, mais on est plus dans la monde du chacun pour soi, ils parlent de leur tranche, pas du gateau en entier.

Les logs peuvent facilement être verbeux, ou simplement ennuyeux.

!SLIDE
## PCAP

PCAP est l'extraction de la partie visualisation de paquets de _tcpdump_.

PCAP est partout, dans Wireshark et tous ses clones (tcpflow, Justniffer…).

PCAP permet de surveiller sans modifier, mais PCAP peut perdre des paquets.

!SLIDE

## dpkt et pypcap

Python est beaucoup utilisé par les gens qui veulent savoir ce qui passe dans le tuyau (pour ensuite y pousser des choses bizzares, pour voir ce que ça fait).

Scapy est l'outil de référence pour analyser et forger.

Là, on veut juste regarder de l'HTTP : pypcap pour attraper les trames réseaux, dpkt pour reconstruire la couche protocole.

!SLIDE

## Filtrer le flot d'information

Autopsie se contente de filtrer le minimum d'informations (via les filtres de pcap), puis au niveau du protocole.

En live, ou depuis un snapshot au format de tcpdump.

Il rassemble chaque requète et avec sa réponse, en mesurant des choses.

Ces informations sont ensuite confiés à un autre outil : Kibana.

!SLIDE

## Logstash

Autopsie parle directement à Logstash, via son protocole tout simple en TCP.

Logstash peut en plus enrichir les informations (user agent, geoip…) et il se charge de transférer les données dans Elasticsearch, indispensable pour visualiser avec Kibana.

!SLIDE

## Kibana

