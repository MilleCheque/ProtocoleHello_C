# ProtocoleHello_C
Mise en œuvre de SAP et de sockets pour le développement d'une application client-serveur en C utilisant TCP
# TP Réseau – Communication Client-Serveur en TCP/IPv4

## Description

Ce projet est une implémentation en **C** d'une application **client-serveur** utilisant les **sockets TCP/IPv4**.

L'objectif est de comprendre le fonctionnement des communications réseau à travers :
- les **SAP (Service Access Points)** ;
- les **sockets TCP** ;
- les appels système de la programmation réseau ;
- l'implémentation d'un protocole applicatif simple nommé **HELLO**.

Le serveur initie toujours la communication, tandis que le client répond selon un automate défini.

---

## Objectifs pédagogiques

- Comprendre le rôle des SAP.
- Manipuler les structures `sockaddr`.
- Utiliser `getaddrinfo()`.
- Créer une socket TCP.
- Associer une socket à une adresse (`bind()`).
- Mettre un serveur en écoute (`listen()`).
- Accepter une connexion (`accept()`).
- Établir une connexion (`connect()`).
- Échanger des données avec `read()` et `write()`.
- Implémenter un protocole applicatif simple.

---

## Structure du projet

```
.
├── hello.h        # Fonctions communes et définition du protocole
├── hello-svr.c    # Serveur TCP
├── hello-clt.c    # Client TCP
└── README.md
```

---

## Le protocole HELLO

Le protocole est volontairement très simple.

### Messages

| PDU | Contenu | Taille |
|-----|----------|---------|
| HELLO | `"HELLO\0"` | 6 octets |
| OK | `"OK\0"` | 3 octets |
| FIN | `"FIN\0"` | 4 octets |

### Déroulement

```
Serveur                        Client

HELLO  ----------------------->

             <----------------- OK

FIN    ----------------------->
```

Le serveur envoie toujours le premier message.

---

## Architecture

```
                TCP

        +----------------+
        |    Serveur     |
        +----------------+
               ^
               |
           socket()
               |
            bind()
               |
           listen()
               |
           accept()
               |
        -----------------
               |
             write()
             read()
               |
        -----------------
               |
           close()


               TCP


        +----------------+
        |     Client     |
        +----------------+
               |
           socket()
               |
          connect()
               |
             read()
            write()
             read()
               |
            close()
```

---

## Les SAP (Service Access Point)

Chaque extrémité d'une communication TCP est identifiée par un couple :

```
(IP, Port)
```

Exemple :

```
Client

192.168.1.35 : 52148
        │
        │
        ▼
192.168.1.20 : 5000

Serveur
```

Dans le projet :

- `svrSAP` représente le SAP du serveur (côté client).
- `cltsSAP` représente le SAP utilisé pour le `bind()` du serveur.
- `cltSAP` représente le SAP du client obtenu après `accept()`.

---

## Compilation

Compiler le serveur :

```bash
gcc -Wall -Wextra -o hello-svr hello-svr.c
```

Compiler le client :

```bash
gcc -Wall -Wextra -o hello-clt hello-clt.c
```

---

## Exécution

### Lancer le serveur

```bash
./hello-svr 5000
```

Le serveur attend une connexion.

### Lancer le client

Depuis un autre terminal :

```bash
./hello-clt localhost 5000
```

ou

```bash
./hello-clt 127.0.0.1 5000
```

---

## Exemple d'exécution

### Serveur

```
serveur: envoyé "HELLO" : reçu "OK" : envoyé "FIN" : quitte
```

### Client

```
client: reçu "HELLO" : envoyé "OK" : reçu "FIN" : quitte
```

---

## Fonctions réseau utilisées

| Fonction | Rôle |
|-----------|------|
| `getaddrinfo()` | Résolution d'adresse IP |
| `socket()` | Création d'une socket |
| `bind()` | Association d'une socket à un SAP local |
| `listen()` | Mise en écoute |
| `accept()` | Acceptation d'une connexion cliente |
| `connect()` | Connexion au serveur |
| `read()` | Lecture de données |
| `write()` | Écriture de données |
| `close()` | Fermeture de la socket |

---

## Notions abordées

- Programmation réseau
- TCP
- IPv4
- SAP (Service Access Point)
- Socket
- Client-Serveur
- Communication synchrone
- Protocoles applicatifs
- Appels système UNIX

---

## Fonctionnement général

```
              socket()

                 │

        +-----------------+

        |    Serveur      |

        +-----------------+

                 │

              bind()

                 │

             listen()

                 │

             accept()

                 │

      HELLO  ─────────────►

                 │

        ◄─────────────  OK

                 │

      FIN    ───────────►

                 │

              close()
```

---

## Auteur
CISSE Daouda
Projet réalisé dans le cadre d'un TP de programmation réseau en C.

---
