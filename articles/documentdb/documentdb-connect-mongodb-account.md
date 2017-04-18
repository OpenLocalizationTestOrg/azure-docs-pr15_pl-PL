<properties 
    pageTitle="Nawiązywanie połączenia z kontem DocumentDB z obsługą protokołu MongoDB | Microsoft Azure" 
    description="Dowiedz się, jak połączyć z kontem DocumentDB z obsługą protokołu MongoDB teraz dostępna do podglądu. Nawiązywanie połączenia przy użyciu parametrów połączenia z MongoDB." 
    keywords="Parametry połączenia mongodb"
    services="documentdb" 
    authors="AndrewHoh" 
    manager="jhubbard" 
    editor="" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/23/2016" 
    ms.author="anhoh"/>

# <a name="how-to-connect-to-a-documentdb-account-with-protocol-support-for-mongodb"></a>Jak utworzyć połączenie z kontem DocumentDB z obsługą protokołu MongoDB

Informacje o sposobie łączenia się z kontem Azure DocumentDB z obsługą protokołu MongoDB przy użyciu standardowy format identyfikatora URI ciągu połączenia MongoDB.  

## <a name="get-the-accounts-connection-string-information"></a>Uzyskaj informacje parametrów połączenia dla konta

1. W nowym oknie Zaloguj się do usługi [Azure Portal](https://portal.azure.com).
2. Na pasku **Nawigacyjnym z lewej strony** karta konta kliknij przycisk **Parametry połączenia**. Aby przejść do **Karta konta**, na Jumpbar kliknij przycisk **Więcej usług**, kliknij **DocumentDB (NoSQL)** , a następnie wybierz konto DocumentDB z obsługą protokołu MongoDB.

    ![Zrzut ekranu: karta wszystkie ustawienia](./media/documentdb-connect-mongodb-account/SettingsBlade.png)

3. Karta **Informacje parametrów połączenia** zostanie otwarty i zawiera wszystkie informacje niezbędne do nawiązania połączenia z kontem za pomocą sterownika dla MongoDB, łącznie z parametrów połączenia wstępnie skonstruowanych.

    ![Zrzut ekranu: karta Parametry połączenia](./media/documentdb-connect-mongodb-account/ConnectionStringBlade.png)

## <a name="connection-string-requirements"></a>Wymagania dotyczące parametrów połączenia

Należy pamiętać, że DocumentDB obsługuje standard MongoDB format ciągu połączenia identyfikator URI, wystarczy wykonać kilka wymagań: konta DocumentDB wymagają uwierzytelnianie i bezpieczna komunikacja za pośrednictwem protokołu SSL.  W związku z tym format ciągu połączenia jest:

    mongodb://username:password@host:port/[database]?ssl=true

Miejsce, w którym wartości tego ciągu są dostępne w karta Parametry połączenia, jak pokazano powyżej.

- Nazwa użytkownika (wymagany)
    - Nazwa konta DocumentDB
- Hasło (wymagany)
    - Hasło konta DocumentDB
- Host (wymagany)
    - Konto FQDN DocumentDB
- Port (wymagany)
    - 10250
- Bazy danych (opcjonalnie)
    - Domyślne bazę danych używaną przez połączenie
- SSL = true (wymagany)

Na przykład można rozważyć konta wyświetlane w powyższych informacji parametry połączenia.  Jest prawidłowe parametry połączenia:
    
    mongodb://contoso123:<password@contoso123.documents.azure.com:10250/mydatabase?ssl=true

## <a name="connecting-with-the-c-driver-for-mongodb"></a>Nawiązywanie połączenia za pomocą sterownika C# dla MongoDB
Jak już wspomniano wszystkie konta DocumentDB wymagają zarówno uwierzytelnianie, jak i bezpieczna komunikacja za pośrednictwem protokołu SSL. Gdy format identyfikatora URI ciągu połączenia MongoDB obsługuje protokół ssl = parametr ciągu kwerendy PRAWDA, Praca z MongoDB C# sterownik wymaga użycia obiektu MongoClientSettings podczas tworzenia MongoClient.  Podane powyżej informacje o koncie, następujący fragment kodu przedstawiono sposób nawiązania połączenia z kontem i pracować z bazą danych "Zadania".

            MongoClientSettings settings = new MongoClientSettings();
            settings.Server = new MongoServerAddress("contoso123.documents.azure.com", 10250);
            settings.UseSsl = true;
            settings.SslSettings = new SslSettings();
            settings.SslSettings.EnabledSslProtocols = SslProtocols.Tls12;

            MongoIdentity identity = new MongoInternalIdentity("Tasks", "contoso123");
            MongoIdentityEvidence evidence = new PasswordEvidence("<password>");

            settings.Credentials = new List<MongoCredential>()
            {
                new MongoCredential("SCRAM-SHA-1", identity, evidence)
            };
            MongoClient client = new MongoClient(settings);
            var database = client.GetDatabase("Tasks",);
    

## <a name="next-steps"></a>Następne kroki


- Dowiedz się, jak [używać MongoChef](documentdb-mongodb-mongochef.md) przy użyciu konta DocumentDB z protokołem obsługę MongoDB.
- Poznaj DocumentDB z obsługą protokołu MongoDB [próbki](documentdb-mongodb-samples.md).

 
