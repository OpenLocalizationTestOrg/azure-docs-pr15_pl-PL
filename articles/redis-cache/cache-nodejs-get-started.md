<properties
    pageTitle="Jak używać Azure Redis w pamięci podręcznej z Node.js | Microsoft Azure"
    description="Rozpoczynanie pracy z Azure Redis pamięci podręcznej za pomocą Node.js i node_redis."
    services="redis-cache"
    documentationCenter=""
    authors="steved0x"
    manager="douge"
    editor="v-lincan"/>

<tags
    ms.service="cache"
    ms.devlang="nodejs"
    ms.topic="hero-article"
    ms.tgt_pltfrm="cache-redis"
    ms.workload="tbd"
    ms.date="10/25/2016"
    ms.author="sdanie"/>

# <a name="how-to-use-azure-redis-cache-with-nodejs"></a>Jak korzystać z Node.js Azure Redis w pamięci podręcznej

> [AZURE.SELECTOR]
- [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
- [PROGRAMU ASP.NET](cache-web-app-howto.md)
- [Node.js](cache-nodejs-get-started.md)
- [Java](cache-java-get-started.md)
- [Python](cache-python-get-started.md)

Azure Redis pamięci podręcznej umożliwia dostęp do bezpiecznego, dedykowane Redis pamięci podręcznej, zarządzane przez firmę Microsoft. Pamięć podręczna jest dostępna z dowolnej aplikacji w programie Microsoft Azure.

W tym temacie pokazano, jak rozpocząć pracę z pamięci podręcznej Azure Redis przy użyciu Node.js. Inny przykład użycia pamięci podręcznej Azure Redis z Node.js uzyskać [tworzenia Node.js aplikacji komunikować się z Socket.IO w witrynie sieci Web Azure](../app-service-web/web-sites-nodejs-chat-app-socketio.md).


## <a name="prerequisites"></a>Wymagania wstępne

Zainstaluj [node_redis](https://github.com/mranney/node_redis):

    npm install redis

Samouczku [node_redis](https://github.com/mranney/node_redis). Przykłady korzystania z innymi klientami Node.js zobacz dokumentację poszczególnych dla klientów Node.js wymieniony w artykule [Node.js Redis klientów](http://redis.io/clients#nodejs).

## <a name="create-a-redis-cache-on-azure"></a>Tworzenie pamięci podręcznej Redis Azure

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="retrieve-the-host-name-and-access-keys"></a>Pobieranie hosta klawiszy nazwy i access

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

## <a name="connect-to-the-cache-securely-using-ssl"></a>Nawiązywanie połączenia z pamięci podręcznej bezpieczne przy użyciu protokołu SSL

Najnowsze kompilacjach pakietu [node_redis](https://github.com/mranney/node_redis) obsługiwać do łączenia się z pamięci podręcznej Azure Redis przy użyciu protokołu SSL. W poniższym przykładzie pokazano sposób nawiązywania połączenia z pamięci podręcznej Azure Redis przy użyciu punktu końcowego SSL 6380. Zamienianie `<name>` o nazwie pamięci podręcznej i `<key>` z albo jako klucz podstawowy i pomocniczy opisane w poprzedniej sekcji [pobrać hosta kluczy nazwy i access](#retrieve-the-host-name-and-access-keys) .

     var redis = require("redis");
    
      // Add your cache name and access key.
    var client = redis.createClient(6380,'<name>.redis.cache.windows.net', {auth_pass: '<key>', tls: {servername: '<name>.redis.cache.windows.net'}});


## <a name="add-something-to-the-cache-and-retrieve-it"></a>Dodawanie zawartości do pamięci podręcznej i pobranie

W poniższym przykładzie pokazano, jak połączyć się z wystąpieniem Azure Redis w pamięci podręcznej i przechowywanie i pobieranie elementów z pamięci podręcznej. Aby uzyskać więcej przykładów Redis za pomocą klienta [node_redis](https://github.com/mranney/node_redis) zobacz [http://redis.js.org/](http://redis.js.org/).

     var redis = require("redis");
    
      // Add your cache name and access key.
    var client = redis.createClient(6380,'<name>.redis.cache.windows.net', {auth_pass: '<key>', tls: {servername: '<name>.redis.cache.windows.net'}});
    
    client.set("key1", "value", function(err, reply) {
            console.log(reply);
        });
    
    client.get("key1",  function(err, reply) {
            console.log(reply);
        });

Wynik:

    OK
    value


## <a name="next-steps"></a>Następne kroki

- [Włączanie diagnostyki pamięci podręcznej](cache-how-to-monitor.md#enable-cache-diagnostics) , dzięki czemu można [Monitorowanie](cache-how-to-monitor.md) kondycji pamięci podręcznej.
- W tym artykule oficjalnym [Redis dokumentacji](http://redis.io/documentation).



