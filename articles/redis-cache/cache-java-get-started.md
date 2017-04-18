<properties
   pageTitle="Jak używać Azure Redis w pamięci podręcznej z języka Java | Microsoft Azure"
    description="Rozpoczynanie pracy z Azure Redis pamięci podręcznej za pomocą języka Java"
    services="redis-cache"
    documentationCenter=""
    authors="steved0x"
    manager="douge"
    editor=""/>

<tags
    ms.service="cache"
    ms.devlang="java"
    ms.topic="hero-article"
    ms.tgt_pltfrm="cache-redis"
    ms.workload="tbd"
    ms.date="08/24/2016"
    ms.author="sdanie"/>

# <a name="how-to-use-azure-redis-cache-with-java"></a>Jak używać Azure Redis w pamięci podręcznej z języka Java

> [AZURE.SELECTOR]
- [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
- [PROGRAMU ASP.NET](cache-web-app-howto.md)
- [Node.js](cache-nodejs-get-started.md)
- [Java](cache-java-get-started.md)
- [Python](cache-python-get-started.md)

Azure zapewnia Redis pamięci podręcznej dostęp do dedykowany Redis pamięci podręcznej, zarządzane przez firmę Microsoft. Pamięć podręczna jest dostępna z dowolnej aplikacji w programie Microsoft Azure.

W tym temacie pokazano, jak rozpocząć pracę z pamięci podręcznej Azure Redis przy użyciu języka Java.

## <a name="prerequisites"></a>Wymagania wstępne

[Jedis](https://github.com/xetorthio/jedis) - klient Java Redis

Samouczku Jedis, ale możesz użyć dowolnego klienta Java wymieniony w artykule [http://redis.io/clients](http://redis.io/clients).

## <a name="create-a-redis-cache-on-azure"></a>Tworzenie pamięci podręcznej Redis Azure

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="retrieve-the-host-name-and-access-keys"></a>Pobieranie hosta klawiszy nazwy i access

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]


## <a name="enable-the-non-ssl-endpoint"></a>Włączanie punkt końcowy bez użycia protokołu SSL

Niektórzy klienci Redis nie obsługuje protokołu SSL i domyślnie [port SSL nie jest wyłączone dla nowych wystąpień Azure Redis w pamięci podręcznej](cache-configure.md#access-ports). Podczas pisania tego dokumentu klienta [Jedis](https://github.com/xetorthio/jedis) nie obsługuje protokołu SSL. 

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-non-ssl-port.md)]




## <a name="add-something-to-the-cache-and-retrieve-it"></a>Dodawanie zawartości do pamięci podręcznej i pobranie

    package com.mycompany.app;
    import redis.clients.jedis.Jedis;
    import redis.clients.jedis.JedisShardInfo;

    /* Make sure you turn on non-SSL port in Azure Redis using the Configuration section in the Azure Portal */
    public class App
    {
      public static void main( String[] args )
      {
        /* In this line, replace <name> with your cache name: */
        JedisShardInfo shardInfo = new JedisShardInfo("<name>.redis.cache.windows.net", 6379);
        shardInfo.setPassword("<key>"); /* Use your access key. */
        Jedis jedis = new Jedis(shardInfo);
        jedis.set("foo", "bar");
        String value = jedis.get("foo");
      }
    }


## <a name="next-steps"></a>Następne kroki

- [Włączanie diagnostyki pamięci podręcznej](https://msdn.microsoft.com/library/azure/dn763945.aspx#EnableDiagnostics) , dzięki czemu można [Monitorowanie](https://msdn.microsoft.com/library/azure/dn763945.aspx) kondycji pamięci podręcznej.
- W tym artykule oficjalnym [Redis dokumentacji](http://redis.io/documentation).

