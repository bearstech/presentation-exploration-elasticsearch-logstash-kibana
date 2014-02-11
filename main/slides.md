# La triplette Elasticsearch-Logstash-Kibana

!SLIDE

## ES n'est pas un moteur de recherche

C'est *aussi* un moteur de recherche.

Elasticsearch est une base clef/valeur, distribuée, avec une interface REST et des index efficaces.

ElasticSearch stocke des réponses qu'il construit, et les assemble au dernier moment.

!SLIDE

## Logstash traduit le blougui blouga des logs

Déjà vu. Tout le monde l'a déjà fait avec des scripts maison, pour un résultat aléatoire.

Logstash a quelques armes secrètes : la lecture des dates, des expressions régulières lisibles, de la résolution IP, du parsing de user-agent et des threads qui fonctionnent.

Attention, il peut lire à peu n'importe quoi et écrire dans encore plus de formats.

!SLIDE

## Kibana est un joli tableau de bord

Kibana a enlevé tout ce qui ne servait à rien : le serveur, pour se brancher directement sur Elasticsearch. Tout en JavaScript, Angular assure la modularité, Flot les graphiques.

Kibana utilise un format simplissime qui permet de le nourrir un peu comme on veut.

L'interface permet d'explorer les données, puis de proposer des tableaux de bords.