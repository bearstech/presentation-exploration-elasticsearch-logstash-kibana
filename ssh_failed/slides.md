# Analyse de journaux perdus au fond de syslog

!SLIDE

Les serveurs loguent plein de choses qui pourront avoir une utilité, plus tard.

Ces informations, en tant que tel ont peu de sens, et donc peu d'utilités. Elles construisent par contre une histoire, qui elle, est intéressante.


!SLIDE

## Le problème

De manière systématique et aveugle, des pénibles vont essayer de se loguer en SSH sur vos machines. SSH empêche les attaques par dictionnaire, en imposant un laps de temps entre chaque tentative.

!SLIDE

## Syslog

UNIX propose depuis des temps immémoriaux de centraliser et de router des logs avec Syslog.

L'outil n'est pas parfait, mais il permet simplement de router tout ce qui touche à l'authentification à Logstash, qui fera ensuite le tri pour ne traiter que ce qui touche aux échecs SSH.

	$ cat /etc/rsyslog.d/logstash.conf
	auth.* @@127.0.0.1:10515

En français, ça donne : envoies tout ce qui parle d'authentification en TCP, en localhost, sur le port 10515.

!SLIDE

## Logstash

### Input
```
input {
	syslog {
		host => "127.0.0.1"
			port => 10515
			type => "auth"
	}
}
```
!SLIDE

### Filter

```
filter {
	if [type] == "auth" and [program] == "sshd" {
		grok { match => {
				"message" => "Failed password for( invalid user)? %{DATA:user} from %{IP:ip} port %{INT:port:int} ssh2"
			}
			add_tag => "ssh_failed"
		}
	}
	if "ssh_failed" in [tags] {
		geoip { source => "ip" }
		dns { reverse => ["ip"] }
	}
}
```
!SLIDE

### Output

```
output {
	if "ssh_failed" in [tags] {
		elasticsearch_http {
			host => "localhost"
		}
	}
}
```
!SLIDE

## Analyse

Le nombre de tentatives est-il important?

D'où viennent les pénibles?

Quel utilisateur est-il utilisé?
