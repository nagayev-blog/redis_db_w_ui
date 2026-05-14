docker-compose.yml для установки redis и UI для управления на стеке portainer + traefik

в env указать 4 параметра
HOST=redis-prod.example.com
REDIS_PASSWORD=сгенерируй_длинный_пароль_минимум_32_символа
REDIS_MAXMEMORY=4gb
INSIGHT_AUTH=admin:$$apr1$$xxxxxx$$xxxxxxxxxxxxxxxxxxxxxx

HOST - dns имя по которому будет располагаться сервис, убедиться перед установкой, что запись успешно резолвится (это требуется для корректного получения сертификата)
REDIS_PASSWORD — обязательно длинный (Redis с TLS — но всё равно). Сгенерируй например openssl rand -base64 32.
REDIS_MAXMEMORY — под объём RAM сервера. Для RAG не ставь allkeys-lru, иначе индекс может разъехаться — оставлено noeviction.
INSIGHT_AUTH — basicauth для UI. Получи через htpasswd -nb admin твой_пароль, в результате удвой все $ → $$.

UI будет доступен по адресу https://ui.redis-prod.example.com
для входа потребуется INSIGHT_AUTH
Далее при подключении экземпляра redis указать параметры
Host: redis (имя сервиса — они в одной docker-сети redis_internal)
Port: 6379
Password: твой REDIS_PASSWORD
TLS: не включай (внутри docker-сети ходят без TLS)
