# <img src="https://cdn-icons-png.flaticon.com/512/174/174881.png" alt="Wordpress icon" align=left width=35 height=35 style="margin-right: 10px">Wordpress docker setup

Questa repository contiene un docker-compose che permette di effettuare il setup di uno stack composto da Wordpress (con apache2) e Mysql su Docker

## Utlizzo

L'intero stack può essere inizializzato semplicemente con il comando.
La flag `-d`, opzionale, lancia il tutto in modalità detached, senza bloccare il terminale.

```bash
docker compose up -d
```

Entrambi i container saranno esposti alla macchina host attraverso le porte **8080** (Wordpress) e **3306** (MySql)

## 🗂 Struttura

```yaml
.
├── docker         # cartella con il Dockerfile custom di Wordpress
├── plugins        # cartella con i plugins che saranno visibili a Wordpress
├── themes         # cartella con i temi che saranno visibili a Wordpress
├── .gitignore     # file .gitignore
├── .env.example   # configurazione di default dello stack
├── README.md      # THIS FILE
```

## 🧾 Requisiti

- [Docker](https://hub.docker.com/search/?type=edition&offering=community)

> `NOTA:` deve essere disponibile il comando `docker compose`. Le ultime versioni di Docker lo hanno integrato, altrimenti deve essere installato a parte

> `NOTA:` sebbene qualsiasi combinazione di versioni Wordpress-MySql sia possibile, alcune di queste potrebbero presentare incompatibilità fra di loro o con temi e plugins, da risolvere manualemente

> `NOTA:` lo stack funziona anche su Windows, ma posizionare le cartelle nel filesystem NTFS rallenta moltissimo l'applicazione. È consigliabile usare Linux o posizionare il tutto su WSL

## ⚙️ Configurazione

Le configurazioni esposte dello stack possono essere modificate semplicemente creando un file _.env_ ed inserendo le configurazioni di cui si vuole fare l'override rispetto ai valori di default presenti in _.env.example_.

```properties
# versione del container di wordpress
WORDPRESS_VERSION=5.9.1-php8.0-apache
# nome del database mysql
WORDPRESS_DB_NAME=wordpress
# host del db. Di default, è il nome del service nel docker-compose
WORDPRESS_DB_HOST=mysql
# utente con accesso al db
WORDPRESS_DB_USER=user
# password dell'utente con accesso al db
WORDPRESS_DB_PASSWORD=password
# prefisso applicato alle tabelle create da wordpress
WORDPRESS_TABLE_PREFIX=wp_
# se attivare la funzione di deug nel wp-config
WORDPRESS_DEBUG=true
# se attivare la funzione di deug-log nel wp-config
WORDPRESS_DEBUG_LOG=true
# se attivare la funzione di deug-display nel wp-config
WORDPRESS_DEBUG_DISPLAY=true

# versione del container di mysql
MYSQL_VERSION=latest
# utente con accesso al db
MYSQL_USER=user
# password dell'utente con accesso al db
MYSQL_PASSWORD=password
# nome del database
MYSQL_DATABASE=wordpress
# se accettare o meno password vuote
MYSQL_ALLOW_EMPTY_PASSWORD=no
```

## ▶️ Build & delete

### Build

Per lanciare l'intero stack, si utilizza il comando. \
La flag `-d`, opzionale, lancia il tutto in modalità detached, senza bloccare il terminale.

```bash
docker compose up -d
```

Nel caso si fosse apportato un cambiamento, dopo aver eliminato lo stack precedente, è opportuno forzare il rebuild delle immagini aggiungendo l'apposita flag:

```bash
docker compose up -d --build
```

ed eliminare i volumi creati in precedenza con

```bash
docker volume prune
```

### Delete

Per fermare ed eliminare lo stack, utilizzare il comando

```bash
docker compose down
```

I volumi, e quindi i dati, non saranno eliminati, se non manualmente eseguendo il comando 

```bash
docker volume prune
```

## ➕ Aggiungere temi e plugins

Per aggiungere temi e plugins è sufficiente posizionare gli stessi nell'apposita cartella, _plugins_ o _themes_.

## 🛠 Modificare il container

È possibile otternere una shell all'interno di entrambi i container attraverso il comando

```bash
docker exec -it wordpress-debug /bin/bash
# o
docker exec -it mysql /bin/bash
```

Se si utilizza VsCode, è consigliabile installare le estensioni [Docker](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-docker) e [Remote - Containers](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers) per ottenere la possibilità di aprire VsCode direttamente all'interno del container.