<properties
    pageTitle="Jak używać Azure Redis w pamięci podręcznej z Python | Microsoft Azure"
    description="Rozpoczynanie pracy z Azure Redis pamięci podręcznej za pomocą Python"
    services="redis-cache"
    documentationCenter=""
    authors="steved0x"
    manager="douge"
    editor="v-lincan"/>

<tags
    ms.service="cache"
    ms.devlang="python"
    ms.topic="hero-article"
    ms.tgt_pltfrm="cache-redis"
    ms.workload="tbd"
    ms.date="08/16/2016"
    ms.author="sdanie"/>

# <a name="how-to-use-azure-redis-cache-with-python"></a>Jak korzystać z Python Azure Redis w pamięci podręcznej

> [AZURE.SELECTOR]
- [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
- [PROGRAMU ASP.NET](cache-web-app-howto.md)
- [Node.js](cache-nodejs-get-started.md)
- [Java](cache-java-get-started.md)
- [Python](cache-python-get-started.md)

W tym temacie pokazano, jak rozpocząć pracę z pamięci podręcznej Azure Redis przy użyciu Python.


## <a name="prerequisites"></a>Wymagania wstępne

Zainstaluj [redis Kopiuj](https://github.com/andymccurdy/redis-py).


## <a name="create-a-redis-cache-on-azure"></a>Tworzenie pamięci podręcznej Redis Azure

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="retrieve-the-host-name-and-access-keys"></a>Pobieranie hosta klawiszy nazwy i access

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]


## <a name="enable-the-non-ssl-endpoint"></a>Włączanie punkt końcowy bez użycia protokołu SSL

Niektórzy klienci Redis nie obsługuje protokołu SSL i domyślnie [port SSL nie jest wyłączone dla nowych wystąpień Azure Redis w pamięci podręcznej](cache-configure.md#access-ports). Podczas pisania tego dokumentu klienta [Korekta redis](https://github.com/andymccurdy/redis-py) nie obsługuje protokołu SSL. 

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-non-ssl-port.md)]


## <a name="add-something-to-the-cache-and-retrieve-it"></a>Dodawanie zawartości do pamięci podręcznej i pobranie


    >>> import redis
    >>> r = redis.StrictRedis(host='<name>.redis.cache.windows.net',
          port=6380, db=0, password='<key>', ssl=True)
    >>> r.set('foo', 'bar')
    True
    >>> r.get('foo')
    b'bar'


Zamienianie `<name>` przy użyciu Twojej nazwy pamięci podręcznej i `key` przy użyciu klucza dostępu.


<!--Image references-->
[1]: ./media/cache-python-get-started/redis-cache-new-cache-menu.png
[2]: ./media/cache-python-get-started/redis-cache-cache-create.png
