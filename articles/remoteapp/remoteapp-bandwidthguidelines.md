<properties 
    pageTitle="Azure przepustowości sieci RemoteApp - ogólne wskazówki | Microsoft Azure"
    description="Opis wskazówki przepustowości Podstawowa sieć zbiory Azure RemoteApp i aplikacje."
    services="remoteapp"
    documentationCenter="" 
    authors="lizap" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="elizapo" />
    
# <a name="azure-remoteapp-network-bandwidth---general-guidelines-if-you-cant-test-your-own"></a>Azure przepustowość sieci RemoteApp - ogólne wskazówki (Jeśli nie można przetestować własnych)

> [AZURE.IMPORTANT]
> Azure RemoteApp jest są przerywane. W tym artykule [anons](https://go.microsoft.com/fwlink/?linkid=821148) , aby uzyskać szczegółowe informacje.

Jeśli nie masz czasu lub możliwość Uruchom [testy przepustowości sieci](remoteapp-bandwidthtests.md) Azure RemoteApp, Oto dość ogólne wskazówki umożliwiające oszacowanie przepustowości dla poszczególnych użytkowników.

Jeśli masz kilka poniższych scenariuszach, nie zaleca się nic mniejsze niż (lub równe) 10 MB/s jako przepustowość sieci MINIMUM nowoczesny podłączonego do Internetu aplikacji w środowisku zdalnego. (Chociaż, jak wspomniano, nie gwarantuje lepiej niż środowisko użytkownika średnia.)

## <a name="complex-powerpoint-simple-powerpoint"></a>PowerPoint złożone, prostych programu PowerPoint

Azure RemoteApp działa najlepiej w sieci LAN 100 MB. 10 MB/s profil sieci kiedy zakłócenia powyżej 120 ms jest większa niż 5%, użytkownik zostanie wyświetlony średnia środowiska. 1 MB/s, które różnych jest glaring — na przykład w pokazie slajdów użytkownik może być niewidoczny animowane przejścia na wszystkich ponieważ ramki są pomijane.

## <a name="internet-explorer-mixed-pdf-pdf-text"></a>Program Internet Explorer mieszane PDF, plik PDF, tekst

10 MB/s profil sieci jest zbliżony sieci LAN w większości aspektach. 1 MB/s zapewni OK środowisko, jednak czasami może występować pewne zakłócenia, gdy użytkownik przewija dokument, jeśli istnieją obrazy na ekranie.

## <a name="flash-video-youtube"></a>Wideo (YouTube) w formacie Flash

100 MB/s LAN zawiera uzyskać najlepsze wyniki, gdy 10 MB/s jest akceptowane (znaczenia firma Microsoft przechowywać z szybkość odtwarzania, ale funkcja podwyżki). 1 MB/s zakłócenia jest bardzo wysoki i zauważalne.

## <a name="word-typing-word-remote-input"></a>Pisania (Word zdalnego wejście) w programie Word
Jest to scenariusz zastosowania niskiej przepustowości. 256 KB/s udostępniamy tak dobrze interfejsu jako sieci LAN.

## <a name="learn-more"></a>Dowiedz się więcej
- [Szacowanie wykorzystania przepustowości sieci Azure RemoteApp](remoteapp-bandwidth.md)

- [Azure RemoteApp - jak przepustowość sieci i jakość środowiska pracy ze sobą?](remoteapp-bandwidthexperience.md)

- [Azure RemoteApp - tseting do wykorzystania przepustowości sieci z kilka typowych scenariuszy](remoteapp-bandwidthtests.md)