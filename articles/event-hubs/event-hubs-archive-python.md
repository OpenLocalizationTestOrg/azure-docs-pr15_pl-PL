<properties
    pageTitle="Azure Instruktaż zdarzenia koncentratory archiwum | Microsoft Azure"
    description="Przykładowy korzystającego z Azure Python SDK w celu zademonstrowania przy użyciu funkcji archiwum koncentratory zdarzenia."
    services="event-hubs"
    documentationCenter=""
    authors="djrosanova"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="darosa;sethm"/>

# <a name="event-hubs-archive-walkthrough-python"></a>Zdarzenia archiwum koncentratory Instruktaż: Python

Archiwum koncentratory zdarzenie jest nową funkcję koncentratorów zdarzenia, umożliwiające automatycznie prezentowania transmisji danych w Twoim Centrum wydarzenia z klientem magazyn obiektów Blob platformy Azure wybranych przez użytkownika. Ułatwia przeprowadzanie przetwarzanie partii na przesyłanie strumieniowe dane w czasie rzeczywistym. Ten artykuł zawiera opis sposobu używania archiwum koncentratory wydarzenia z Python. Aby uzyskać więcej informacji o archiwum koncentratory zdarzenia zobacz [artykuł Omówienie](event-hubs-archive-overview.md).

W przykładzie użyto Azure Python SDK w celu zademonstrowania przy użyciu funkcji archiwum. Sender.py wyśle odbiorcy symulowany telemetrycznego środowiska koncentratory zdarzenia w formacie JSON. Centrum zdarzenie jest skonfigurowany do używania funkcji archiwum zapisać te dane do blob miejsca do magazynowania partiami. Archivereader.py następnie odczytuje tych obiektów blob i tworzy plik Dołącz na urządzenie i zapisuje dane do plików CSV.

Co to osiągnąć

1.  Tworzenie konta usługi Magazyn obiektów Blob platformy Azure i kontenera obiektów blob, to za pomocą portalu Azure

2.  Tworzenie nazw Centrum zdarzenia, za pomocą portalu Azure

3.  Tworzenie Centrum zdarzeń za pomocą funkcji archiwum włączone, za pomocą portalu Azure

4.  Wysyłanie danych do koncentratora zdarzeń za pomocą skryptu Python

5.  Czytanie plików z archiwum i procesu ich przy użyciu innego Python skryptu

Wymagania wstępne

1.  Python 2.7.x

2.  Subskrypcję usługi Azure

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="create-an-azure-storage-account"></a>Tworzenie konta magazynu platformy Azure

1.  Zaloguj się do [portalu Azure][].

2.  W okienku nawigacji po lewej stronie portalu kliknij przycisk Nowy, a następnie kliknij danych + miejsca do magazynowania, a następnie kliknij konto miejsca do magazynowania.

3.  Wypełnij pola w karta konta miejsca do magazynowania, a następnie kliknij przycisk **Utwórz**.

    ![][1]

4.  Gdy pojawi się komunikat **Wdrożeń zakończyła się pomyślnie** , kliknij na nowe konto miejsca do magazynowania i karta **Essentials** kliknij **obiektów blob**. Gdy zostanie otwarta karta **obiektów Blob usługi** , kliknij przycisk **+ kontener** u góry. Nadaj nazwę kontenera **archiwum**, a następnie zamknij karta **Blob usługi** .

5.  Karta po lewej stronie kliknij **klawisze dostępu** i skopiuj nazwę konta magazynu i wartości **klucz1**. Zapisz te wartości w pliku programu Notatnik albo tymczasową lokalizację.

[AZURE.INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

## <a name="create-a-python-script-to-send-events-to-your-event-hub"></a>Utworzyć skrypt Python w celu wysyłania zdarzeń do Twoim Centrum zdarzenia

1.  Otwórz Edytor Python Ulubione, takich jak [Kod Visual Studio][].

2.  Utwórz skrypt o nazwie **sender.py**. Ten skrypt wyśle 200 zdarzeń do Twoim Centrum zdarzenia. Są to proste odczyt środowiska wysłane w formacie JSON.

3.  Wklej następujący kod do sender.py:

    ```
    import uuid
    import datetime
    import random
    import json
    from azure.servicebus import ServiceBusService
    
    sbs = ServiceBusService(service_namespace='INSERT YOUR NAMESPACE NAME', shared_access_key_name='RootManageSharedAccessKey', shared_access_key_value='INSERT YOUR KEY')
    devices = []
    for x in range(0, 10):
        devices.append(str(uuid.uuid4()))
    
    for y in range(0,20):
        for dev in devices:
            reading = {'id': dev, 'timestamp': str(datetime.datetime.utcnow()), 'uv': random.random(), 'temperature': random.randint(70, 100), 'humidity': random.randint(70, 100)}
            s = json.dumps(reading)
            sbs.send\_event('myhub', s)
        print y
    ```
4.  Zaktualizuj poprzedniego kodu w celu użycia swojej nazwy nazw i wartości klucza, które uzyskane podczas tworzenia nazw koncentratory zdarzenia.

## <a name="create-a-python-script-to-read-your-archive-files"></a>Utworzyć skrypt Python, aby odczytać pliki archiwum

1.  Wypełnij karta, a następnie kliknij przycisk **Utwórz**.

2.  Utwórz skrypt o nazwie **archivereader.py**. Ten skrypt odczytuje pliki archiwum i tworzenie pliku na urządzenie jest zapisanie danych tylko w przypadku tego urządzenia.

3.  Wklej następujący kod do archivereader.py:

    ```
    import os
    import string
    import json
    import avro.schema
    from avro.datafile import DataFileReader, DataFileWriter
    from avro.io import DatumReader, DatumWriter
    from azure.storage.blob import BlockBlobService
    
    def processBlob(filename):
        reader = DataFileReader(open(filename, 'rb'), DatumReader())
        dict = {}
        for reading in reader:
            parsed\_json = json.loads(reading["Body"])
            if not 'id' in parsed\_json:
                return
            if not dict.has\_key(parsed\_json['id']):
            list = []
            dict[parsed\_json['id']] = list
        else:
            list = dict[parsed\_json['id']]
            list.append(parsed\_json)
        reader.close()
        for device in dict.keys():
            deviceFile = open(device + '.csv', "a")
            for r in dict[device]:
                deviceFile.write(", ".join([str(r[x]) for x in r.keys()])+'\\n')

    def startProcessing(accountName, key, container):
        print 'Processor started using path: ' + os.getcwd()
        block\_blob\_service = BlockBlobService(account\_name=accountName, account\_key=key)
        generator = block\_blob\_service.list\_blobs(container)
        for blob in generator:
            if blob.properties.content\_length != 0:
                print('Downloaded a non empty blob: ' + blob.name)
                cleanName = string.replace(blob.name, '/', '\_')
                block\_blob\_service.get\_blob\_to\_path(container, blob.name, cleanName)
                processBlob(cleanName)
                os.remove(cleanName)
            block\_blob\_service.delete\_blob(container, blob.name)
    startProcessing('YOUR STORAGE ACCOUNT NAME', 'YOUR KEY', 'archive')
    ```

4.  Pamiętaj wkleić odpowiednie wartości nazwę konta magazynu i klucza w wywołaniu `startProcessing`.

## <a name="run-the-scripts"></a>Uruchamianie skryptów

1.  Otwórz okno wiersza polecenia, zawierającego Python w jego ścieżkę, a następnie uruchom te polecenia, aby zainstalować pakiety wstępne Python:

    ```
    pip install azure-storage
    pip install azure-servicebus
    pip install avro
    ```
  
    Jeśli masz starszej wersji programu albo Magazyn azure lub azure może być konieczne używanie **— uaktualnienie** opcji

    Ponadto może być konieczne uruchom następujące polecenie (nie są niezbędne w większości systemów):

    ```
    pip install cryptography
    ```

2.  Zmień katalog na wszędzie tam, gdzie został zapisany sender.py i archivereader.py, a tego polecenia:

    ```
    start python sender.py
    ```
    
    Zostanie uruchomiony nowy proces Python uruchamianie nadawcy.

3. Teraz Poczekaj kilka minut archiwum do uruchomienia. Do swojego oryginalnego okna polecenia wpisz następujące polecenie:

    ```
    python archivereader.py
    ```

Ten procesor archiwum korzysta z lokalnego katalogu pobrać wszystkie obiekty BLOB z konta kontenera magazynu. Będzie przetwarzał, które nie są puste, napisz wyniki jako pliki CSV do katalogu lokalnego.

## <a name="next-steps"></a>Następne kroki

Możesz więcej informacji o koncentratory zdarzenia, odwiedź witrynę z poniższych łączy:

- [Omówienie koncentratory zdarzenia archiwizowanie][]
- Ukończono [przykładową aplikację, która korzysta z koncentratorów zdarzenia][].
- Przykładowy [skali się zdarzenia przetwarzania z koncentratorów zdarzenia][] .
- [Omówienie koncentratory zdarzenia][]
 

[Azure portal]: https://portal.azure.com/
[Omówienie koncentratory zdarzenia archiwizowanie]: event-hubs-archive-overview.md
[1]: ./media/event-hubs-archive-python/event-hubs-python1.png
[About Azure storage accounts]: https://azure.microsoft.com/en-us/documentation/articles/storage-create-storage-account/
[Kod Visual Studio]: https://code.visualstudio.com/
[Omówienie koncentratory zdarzenia]: event-hubs-overview.md
[przykładową aplikację używa koncentratory zdarzenia]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Możliwość skalowania zdarzenia przetwarzania z koncentratorów zdarzenia]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
