# docker_teaser_network

docker-compose up -d
Для Windows оставляем всё как есть.

Для Linux заменить 
 - "host.docker.internal:host-gateway"
на
 - "host.docker.internal:$DOCKER_GATEWAY_HOST"
В переменные окружения добавить $DOCKER_GATEWAY_HOST:
   export DOCKER_GATEWAY_HOST=$(ip addr show | grep "\binet\b.*\bdocker0\b" | awk '{print $2}' | cut -d '/' -f 1)

Переименовать env файлЖ
    cp .env.example .env

Если всё прошло удачно, то мы должны увидеть примерно следующее:
docker-compose ps:
![img.png](img.png)

src является корневой папкой для всех проектов, которые работают внутри образа