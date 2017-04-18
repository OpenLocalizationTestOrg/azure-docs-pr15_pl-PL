
<properties 
    pageTitle="Szacowanie wykorzystania przepustowości sieci Azure RemoteApp | Microsoft Azure"
    description="Informacje na temat wymagań dotyczących przepustowości sieci zbiory Azure RemoteApp i aplikacje."
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

# <a name="estimate-azure-remoteapp-network-bandwidth-usage"></a>Szacowanie wykorzystania przepustowości sieci Azure RemoteApp 

> [AZURE.IMPORTANT]
> Azure RemoteApp jest są przerywane. W tym artykule [anons](https://go.microsoft.com/fwlink/?linkid=821148) , aby uzyskać szczegółowe informacje.

Azure RemoteApp używa protokołu RDP (Remote Desktop) do komunikowania się między aplikacjami działającymi w chmurze Azure i użytkowników. Ten artykuł zawiera podstawowe wskazówki, używanego do Szacowanie wykorzystania tej sieci i potencjalnie Szacowanie wykorzystania przepustowości sieci na Azure RemoteApp użytkownika.

Szacowanie wykorzystania przepustowości dla poszczególnych użytkowników jest bardzo złożony i wymaga korzystania z wielu aplikacji jednocześnie w scenariuszach wielozadaniowość miejsce, w którym mogą mieć wpływ na aplikacje innych osób wydajności według ich zapotrzebowania na przepustowość sieci. Nawet typ klienta pulpitu zdalnego (na przykład klient Mac i HTML5 klienta) może prowadzić do różnych przepustowości wyników. Aby ułatwić pracę tych komplikacji, firma Microsoft będzie podzielić scenariusze użycia kilka typowych kategorii, aby odtworzyć rzeczywistych scenariuszy. (Jeśli rzeczywisty scenariusz jest oczywiście różnych kategorii i różni się przez użytkownika)

Zanim zostały opisane dalej - musi należy zauważyć, że przyjmie RDP zapewnia warto doskonałe środowisko dla większości scenariuszy zastosowania w sieciach opóźnienie poniżej 120 ms i przepustowość ponad 5 MB — to jest oparty na możliwość firmy RDP dynamicznie zmieniać przy użyciu dostępna przepustowość sieci i przepustowość szacowany aplikacji. W tym artykule wykracza poza tymi "w większości przypadków użycia" Aby przyjrzeć się krawędzi, gdzie scenariusze zacząć operacji unwind i środowisko użytkownika rozpocznie się zmniejszyć.

Zalecenia według planu bazowego, a co firma Microsoft nie zostały uwzględnione w naszym szacuje teraz zapoznaj się z następujących artykułów, aby uzyskać szczegóły, w tym kwestie do rozważenia.

- [Jak przepustowość sieci i jakość środowiska pracy ze sobą?](remoteapp-bandwidthexperience.md)
- [Testowanie do wykorzystania przepustowości sieci z kilka typowych scenariuszy](remoteapp-bandwidthtests.md)
- [Szybkie wskazówki, jeśli nie masz czasu lub możliwość testowanie](remoteapp-bandwidthguidelines.md)


## <a name="what-are-we-not-including"></a>Co firma Microsoft nie zawierają?

Po przejrzeniu proponowanego badania i Nasze zalecenia ogólne (i niewątpliwie ogólne), należy pamiętać, istnieje kilka kwestii, które możemy pomijać. Na przykład komplikacji środowisko użytkownika dostarczony przez asymetrycznym rodzaj przekazywania a Pobierz przepustowości. Asymetrycznym rodzaj większość sieci Wi-Fi będzie ponadto wpływają na wydajność i wrażenie środowisko użytkownika. Interakcyjne scenariuszach podrzędne ruch może priorytety poniżej poprzednie, który może zwiększyć liczbę utracone ramki audio lub wideo i w związku z tym wpływ na użytkownika widzenia przesyłanie strumieniowe środowiska. Można uruchamiać własne doświadczenia w celu zobacz Co to jest dobrym przypadków użycia określonych i sieci.

Mimo że omówimy przekierowywania możemy nie uwzględnia wpływ na przepustowość ruch sieciowy spowodowany dołączonych urządzeń, takich jak miejsca do magazynowania, drukarek, skanerów, kamer internetowych i innych urządzeń USB. Efekt tych urządzeń zazwyczaj tymczasowo wzrósł wymagania dotyczące przepustowości i zniknie po wykonaniu zadania. Jednak jeśli często wykonywane, że żądanie przepustowości może być bardzo zauważalne.

Możemy również nie są omawiane jak jeden użytkownik może mieć wpływ na innych użytkowników w tej samej sieci. Na przykład jeden użytkownik używające 4K wideo w sieci 100 MB/s znacznie może wpłynąć na innych użytkowników w tej samej sieci wykonać to samo zadanie. Niestety otrzymuje stopniowo trudniejsze ustalić wpływ zastosowania równoczesne, aby nadać typowych lub obejmujący wszystkie zalecenia dotyczące sposobu system wykonuje w agregacji. Wszystko, co mówi można to, że technologii Protocol (protokół), aby uzyskać najlepsze wykorzystanie dostępna przepustowość sieci, ale ma swoje ograniczenia.