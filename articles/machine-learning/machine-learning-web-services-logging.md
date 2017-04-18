<properties 
    pageTitle="Rejestrowanie dla usług sieci web maszynowego uczenia | Microsoft Azure" 
    description="Dowiedz się, jak włączyć rejestrowanie maszynowego uczenia usług sieci web. Rejestrowanie udostępnia dodatkowe informacje ułatwiające rozwiązywanie problemów za pośrednictwem interfejsów API." 
    services="machine-learning" 
    documentationCenter="" 
    authors="raymondlaghaeian" 
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data" 
    ms.date="10/05/2016"
    ms.author="raymondl;garye"/>

# <a name="enable-logging-for-machine-learning-web-services"></a>Włącz rejestrowanie dla usług sieci web uczenia komputera  

Ten dokument zawiera informacje dotyczące funkcji rejestrowania usług sieci Web klasyczny. Włączanie rejestrowania w usługach sieci Web udostępnia dodatkowe informacje, oprócz tylko numer błędu i wiadomości, które mogą ułatwić rozwiązywanie problemów z połączenia za pośrednictwem interfejsów API nauki komputera.  

Aby włączyć rejestrowanie w usługach sieci Web w portalu klasyczny Azure:   

1.  Zaloguj się do [portalu klasyczny Azure](https://manage.windowsazure.com/)
2.  W kolumnie po lewej stronie funkcje kliknij **Nauki komputera**.
3.  Kliknij obszar roboczy, następnie **Usług sieci WEB**.
4.  Na liście usług sieci Web kliknij nazwę usługi sieci Web.
5.  Na liście punkty końcowe kliknij nazwę punktu końcowego.
6.  Kliknij przycisk **Konfiguruj**.
7.  Ustaw **Poziom śledzenia diagnostyki** *Błąd* lub *Wszystkie*, a następnie kliknij przycisk **ZAPISZ**.

Aby włączyć rejestrowanie w portalu usługi sieci Web uczenia maszynowego Azure.

1. Zaloguj się do portalu [Usługi sieci Web uczenia maszynowego Azure](https://services.azureml.net) .
2. Kliknij pozycję usług sieci Web klasyczny.
3.  Na liście usług sieci Web kliknij nazwę usługi sieci Web.
4.  Na liście punkty końcowe kliknij nazwę punktu końcowego.
5.  Kliknij przycisk **Konfiguruj**.
6.  Ustaw **Rejestrowanie** *błędów* lub *Wszystkie*, a następnie kliknij przycisk **ZAPISZ**.

## <a name="the-effects-of-enabling-logging"></a>Włączanie rejestrowania efektów

Rejestrowanie jest włączone, wszystkie, którego narzędzia diagnostyczne i błędów z wybranego punktu końcowego jest zalogowany do konta magazynu platformy Azure połączone z obszarem roboczym użytkownika. Spowoduje to konto miejsca do magazynowania w portalu klasyczny Azure widokiem pulpitu nawigacyjnego (na dole sekcji szybkie skrócie) swój obszar roboczy.  

Dzienniki można wyświetlać za pomocą jednej z kilku narzędzi dostępnych Eksplorowanie konto Azure miejsca do magazynowania. Najprostszym może być po prostu przejdź do konta miejsca do magazynowania w portalu klasyczny Azure, a następnie kliknij pozycję **kontenerów**. Następnie można zobaczyć kontenera o nazwie **diagnostyki ml**. Ten kontener zawiera wszystkie informacje diagnostyczne dla wszystkich sieci web usługi punkty końcowe dla wszystkich obszarów roboczych skojarzone z tym kontem miejsca do magazynowania. 
 
## <a name="log-blob-detail-information"></a>Dziennik obiektów blob szczegółowych informacji

Poszczególnych obiektów blob w kontenerze przechowuje informacje diagnostyczne dla dokładnie jedną z następujących czynności:

-   Wykonanie metody — wsadowe  
-   Wykonanie metody żądanie odpowiedź  
-   Inicjowanie kontenera żądanie odpowiedź
  
Długość nazwy poszczególnych obiektów blob ma prefiksu następującą postać: 

{Identyfikator obszaru roboczego}-{usługi sieci Web identyfikator}-{identyfikator punktu końcowego} / {typ dziennika}  

Miejsce, w którym typ dziennika jest jednym z następujących wartości:  

- partii  
- wynik żądania  
- wynik inicjowania  