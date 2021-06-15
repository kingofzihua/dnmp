### .env
```dotenv
# mariadb 10
MARIADB_VERSION=10
MARIADB_HOST_PORT=3306
MARIADB_ROOT_PASSWORD=123456
MARIADB_CONF_FILE=./services/mariadb/mysql.cnf
```

### docker-compose.yml
```yml
  mariadb:
    image: mariadb:${MARIADB_VERSION}
    container_name: mariadb
    ports:
      - '${MARIADB_DB_PORT:-3306}:3306'
    volumes:
      - ${MARIADB_CONF_FILE}:/etc/mysql/conf.d/mysql.cnf:ro
      - ${DATA_DIR}/mariadb:/var/lib/mysql:rw
    restart: always
    networks:
      - default
    environment:
      MYSQL_ROOT_PASSWORD: '${MARIADB_ROOT_PASSWORD}'
      MYSQL_ALLOW_EMPTY_PASSWORD: 'yes'
      TZ: "$TZ"
    healthcheck:
      test: ["CMD", "mysqladmin", "ping"]
```

### /services/mariadb/mysql.cnf

```ini
[client]
port                    = 3306
default-character-set   = utf8mb4


[mysqld]
user                    = mysql
port                    = 3306
sql_mode                = NO_ENGINE_SUBSTITUTION,STRICT_TRANS_TABLES

default-storage-engine  = InnoDB
default-authentication-plugin   = mysql_native_password
character-set-server    = utf8mb4
collation-server        = utf8mb4_unicode_ci
init_connect            = 'SET NAMES utf8mb4'

disable-log-bin
skip-character-set-client-handshake
explicit_defaults_for_timestamp

slow_query_log
long_query_time         = 3
slow-query-log-file     = /var/lib/mysql/mysql.slow.log
log-error               = /var/lib/mysql/mysql.error.log

default-time-zone       = '+8:00'

[mysql]
default-character-set   = utf8mb4
```