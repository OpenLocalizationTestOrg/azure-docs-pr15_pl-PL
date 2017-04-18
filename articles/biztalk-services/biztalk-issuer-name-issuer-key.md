<properties 
    pageTitle="Nazwa wystawcy i klucza wystawcy w usługach BizTalk | Microsoft Azure" 
    description="Dowiedz się, jak pobrać Nazwa wystawcy i klucza wystawcy Bus usług i kontroli dostępu (ACS) w usługach BizTalk. MABS, WABS" 
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




# <a name="biztalk-services-issuer-name-and-issuer-key"></a>Usługi BizTalk: Nazwa wystawcy i klucza wystawcy

Usług Azure BizTalk używa Nazwa wystawcy Bus usługi i klucza wystawcy oraz nazwa wystawcy kontrola dostępu i klucza wystawcy. W szczególności:

Zadanie | Które Nazwa wystawcy i klucza wystawcy
--- | ---
Wdrażanie aplikacji z programu Visual Studio | Nazwa wystawcy kontrola dostępu i klucza wystawcy
Konfigurowanie portalu usługi Azure BizTalk | Nazwa wystawcy kontrola dostępu i klucza wystawcy
Tworzenie przekaźniki LOB z usługami karty BizTalk w programie Visual Studio | Nazwa wystawcy Bus usługi i klucza wystawcy

W tym temacie przedstawiono czynności, aby pobrać Nazwa wystawcy i klucza wystawcy. 

## <a name="access-control-issuer-name-and-issuer-key"></a>Nazwa wystawcy kontrola dostępu i klucza wystawcy
Nazwa wystawcy kontrola dostępu i klucza wystawcy są używane przez następujące czynności:

- Aplikacja usługi BizTalk Azure utworzone w programie Visual Studio: aby Azure wdrażania aplikacji usługi BizTalk w programie Visual Studio, należy wprowadzić Nazwa wystawcy kontrola dostępu i klucza wystawcy. 
- Portal Azure usług BizTalk: Podczas tworzenia usługi BizTalk i otwórz Portal usługi BizTalk, nazwa wystawcy kontrola dostępu, a klucz wystawcy są automatycznie zarejestrowane dla wdrożeń o tych samych wartościach kontrola dostępu.

### <a name="to-copy-and-paste-the-access-control-issuer-name-and-issuer-key"></a>Aby skopiować i wkleić Nazwa wystawcy kontrola dostępu i klucza wystawcy

1. Zaloguj się do [portalu klasyczny Azure](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. W okienku nawigacji po lewej stronie wybierz **Usługi BizTalk**.
3. Wybierz pozycję usługi BizTalk. 
4. Na pasku zadań, wybierz pozycję **Informacje o połączeniu** . Namespace kontrola dostępu, domyślny wystawcy (Nazwa wystawcy) i domyślny klawisz (wystawcy) są wyświetlane i czy można kopiować i wklejać.  

Podsumowanie:  
Nazwa wystawcy = wystawcy domyślne  
Klucz wystawcy = domyślny klawisz


Możesz również zaznaczyć **Otwieranie portalu zarządzania ACS** do pobierania wartości kontrola dostępu:

1. Zaloguj się do [portalu klasyczny Azure](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. W okienku nawigacji po lewej stronie wybierz **Usługi BizTalk**.
3. Wybierz pozycję usługi BizTalk.
4. Wybierz przycisk informacje o połączeniu, a następnie wybierz pozycję **Otwórz Portal Zarządzanie ACS**.
5. W portalu w obszarze **Ustawienia usług**wybierz pozycję **Tożsamości usługi**. Spowoduje to wyświetlenie Twojej tożsamości usług, czyli wartość Nazwa wystawcy kontrolki programu Access. Wybierz łącze Twojej tożsamości usługi w taki sposób, aby wyświetlić hasła, czyli wartość klucza wystawcy. Można kopiować ich wartości.<br/><br/>
Na przykład **Usługa tożsamości**, zobacz "właściciel". "Właściciel" jest nazwy wystawcy kontroli dostępu. Po kliknięciu łącza "właściciel", zobacz **hasła**. Po kliknięciu łącza "Hasło", zobacz wartość. Ta wartość hasła jest klucza wystawcy kontroli dostępu.  

Podsumowanie:  
Nazwa wystawcy = nazwa tożsamość usługi  
Klucz wystawcy = wartość hasła

W okienku nawigacji po lewej stronie można też zaznaczyć **Usługi Active Directory** do pobierania wartości kontroli dostępu. 

> [AZURE.IMPORTANT]Po utworzeniu Namespace kontroli dostępu za pomocą **Usługi Active Directory**, tożsamość usługi **nie** jest automatycznie tworzony. Po obsługę usługi BizTalk, Namespace kontrola dostępu, tożsamość usługi o nazwie "właściciel" (Nazwa wystawcy) hasła (klawisz wystawcy), i symetrycznej klucza są tworzone automatycznie.<br /> 
[Jak: używanie ACS usługi zarządzania tożsamościami Konfigurowanie usługi](http://go.microsoft.com/fwlink/p/?LinkID=303942) zawiera więcej informacji na tożsamości usługi kontroli dostępu.


## <a name="service-bus-issuer-name-and-issuer-key"></a>Nazwa wystawcy Bus usługi i klucza wystawcy
Nazwa wystawcy Bus usługi i klucza wystawcy są używane przez usługi karty BizTalk. W projekcie BizTalk usług w programie Visual Studio możesz korzystać z usług karty BizTalk nawiązywania połączenia z lokalnym systemem z LOB (LOB). Aby połączyć, tworzenie przekazywania LOB i wprowadź swoje dane systemu LOB. W ten sposób, możesz również wprowadzić Nazwa wystawcy Bus usługi i klucza wystawcy.

### <a name="to-retrieve-the-service-bus-issuer-name-and-issuer-key"></a>Aby pobrać Nazwa wystawcy Bus usługi i klucza wystawcy

1. Zaloguj się do [portalu klasyczny Azure](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. W okienku nawigacji po lewej stronie wybierz **Usługę Bus**.
3. Wybierz obszar nazw. Na pasku zadań wybierz **Informacje o połączeniu**. Spowoduje to wyświetlenie **Domyślne wystawcy** (Nazwa wystawcy) i **Domyślny klucz** (klawisz wystawcy). Można kopiować ich wartości.  

Podsumowanie:  
Nazwa wystawcy = wystawcy domyślne  
Klucz wystawcy = domyślny klawisz

## <a name="next"></a>Następny
Dodatkowe usługi Azure BizTalk tematy:

-  [Instalowanie usługi Azure BizTalk SDK](http://go.microsoft.com/fwlink/p/?LinkID=241589)<br/>
-  [Samouczki: Usługi Azure BizTalk](http://go.microsoft.com/fwlink/p/?LinkID=236944)<br/>
-  [Jak mogę rozpocząć przy użyciu SDK usług BizTalk Azure](http://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>
-  [Usługi Azure BizTalk](http://go.microsoft.com/fwlink/p/?LinkID=303664)<br/>


## <a name="see-also"></a>Zobacz też
-  [Jak: Konfigurowanie usługi tożsamości za pomocą usługi zarządzania ACS](http://go.microsoft.com/fwlink/p/?LinkID=303942)<br/>
- [Usługi BizTalk: Deweloper, podstawowej, standardowe i wykres wersji Premium](http://go.microsoft.com/fwlink/p/?LinkID=302279)<br/>
- [Usługi BizTalk: Klasyczny portal Azure za pomocą inicjowania obsługi administracyjnej](http://go.microsoft.com/fwlink/p/?LinkID=302280)<br/>
- [Usługi BizTalk: Wykres stan inicjowania obsługi administracyjnej](http://go.microsoft.com/fwlink/p/?LinkID=329870)<br/>
- [BizTalk usługi: Karty pulpitu nawigacyjnego, monitorowanie i skali](http://go.microsoft.com/fwlink/p/?LinkID=302281)<br/>
- [BizTalk usługi: Kopia zapasowa i przywracanie](http://go.microsoft.com/fwlink/p/?LinkID=329873)<br/>
- [Usługi BizTalk: ograniczanie](http://go.microsoft.com/fwlink/p/?LinkID=302282)<br/>
 
