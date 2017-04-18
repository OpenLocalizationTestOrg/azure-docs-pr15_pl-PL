<properties 
    pageTitle="Więcej informacji na temat ograniczania w usługach BizTalk | Microsoft Azure" 
    description="Informacje na temat ograniczania progi i powodujące zachowania środowisko uruchomieniowe usługi BizTalk. Ograniczanie jest oparty na użycie pamięci i liczba wiadomości. MABS, WABS" 
    services="biztalk-services" 
    documentationCenter="" 
    authors="MandiOhlinger" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="biztalk-services" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="mandia"/>





# <a name="biztalk-services-throttling"></a>Usługi BizTalk: ograniczanie

Usług Azure BizTalk wykonuje usługi ograniczania na podstawie dwóch warunków: użycie pamięci i liczba jednoczesne wiadomości przetwarzania. W tym temacie wymieniono ograniczania progi i opisano zachowanie środowisko uruchomieniowe, gdy wystąpi ograniczania warunku.

## <a name="throttling-thresholds"></a>Ograniczanie progi

W poniższej tabeli wymieniono ograniczania źródłowa i progi:

||Opis|Dolna wartość progowa|Progowa|
|---|---|---|---|
|Pamięci|% sumy system pamięci dostępne PageFileBytes. <p><p>Suma PageFileBytes dostępne jest około 2 razy pamięć RAM systemu.|60%|70%|
|Przetwarzanie wiadomości|Liczba wiadomości przetwarzania jednocześnie|40 * liczba rdzeni|100 * Liczba rdzeni|

Po osiągnięciu progu wysoki ograniczyć zaczyna się usługi Azure BizTalk. Ograniczanie stopnie po osiągnięciu progu niski. Na przykład usługi jest użycie pamięci 65%. W takim przypadku nie ograniczyć usługę. Usługi zostanie uruchomiony przy użyciu pamięci 70%. W tej sytuacji usługa ogranicza i będzie nadal występował ograniczyć do momentu usługa używa 60% (dolna wartość progowa) pamięci.

Usług Azure BizTalk śledzi ograniczania status (stan normalny a stanie ograniczonej) i ograniczania czasu trwania.


## <a name="runtime-behavior"></a>Zachowanie

Gdy usługi Azure BizTalk umieścisz ograniczania stan, są następujące:

- Ograniczenie jest dla każdego wystąpienia roli. Na przykład:<br/>
Ograniczanie jest RoleInstanceA. RoleInstanceB nie jest ograniczania. W tej sytuacji wiadomości w RoleInstanceB są przetwarzane zgodnie z oczekiwaniami. Wiadomości w RoleInstanceA zostaną odrzucone i zakończyć się niepowodzeniem z następujący komunikat o błędzie:<br/><br/>
**Serwer jest zajęty. Spróbuj ponownie.**<br/><br/>
- Wszystkie źródła pobieraj nie ankieta i Pobierz wiadomości. Na przykład:<br/>
Potok pobiera wiadomości z zewnętrznego źródła FTP. Wystąpienie roli, wykonując pobieraj otrzymuje do ograniczania stanu. W tej sytuacji proces zatrzymuje pobieranie dodatkowych wiadomości, aż wystąpienie roli przestanie ograniczania.
- Odpowiedź jest wysyłana do klienta, klienta można Prześlij wiadomość.
- Należy zaczekać, aż ograniczania został rozwiązany. W szczególności należy zaczekać, aż osiągnięciu progu niski.

## <a name="important-notes"></a>Ważne notatki
- Nie można wyłączyć ograniczania.
- Nie można zmodyfikować ograniczania progi.
- Ograniczanie jest zaimplementowana systemowe.
- Serwer bazy danych SQL Azure też ma wbudowane ograniczania.

## <a name="additional-azure-biztalk-services-topics"></a>Dodatkowe tematy usługi BizTalk Azure

-  [Instalowanie usługi Azure BizTalk SDK](http://go.microsoft.com/fwlink/p/?LinkID=241589)<br/>
-  [Samouczki: Usługi Azure BizTalk](http://go.microsoft.com/fwlink/p/?LinkID=236944)<br/>
-  [Jak mogę rozpocząć przy użyciu SDK usług BizTalk Azure](http://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>
-  [Usługi Azure BizTalk](http://go.microsoft.com/fwlink/p/?LinkID=303664)<br/>

## <a name="see-also"></a>Zobacz też
- [Usługi BizTalk: Deweloper, podstawowej, standardowe i wykres wersji Premium](http://go.microsoft.com/fwlink/p/?LinkID=302279)<br/>
- [Usługi BizTalk: Klasyczny portal Azure za pomocą inicjowania obsługi administracyjnej](http://go.microsoft.com/fwlink/p/?LinkID=302280)<br/>
- [Usługi BizTalk: Wykres stan inicjowania obsługi administracyjnej](http://go.microsoft.com/fwlink/p/?LinkID=329870)<br/>
- [BizTalk usługi: Karty pulpitu nawigacyjnego, monitorowanie i skali](http://go.microsoft.com/fwlink/p/?LinkID=302281)<br/>
- [BizTalk usługi: Kopia zapasowa i przywracanie](http://go.microsoft.com/fwlink/p/?LinkID=329873)<br/>
- [Usługi BizTalk: Nazwa wystawcy i klucza wystawcy](http://go.microsoft.com/fwlink/p/?LinkID=303941)<br/>
 
