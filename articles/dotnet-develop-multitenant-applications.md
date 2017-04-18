<properties
    pageTitle="Wzór aplikacji sieci Web wielu dzierżawy | Microsoft Azure"
    description="Znajdowanie architektura omówienia i wzorców projektu, w których opisano, jak zaimplementować aplikacji sieci web wielu dzierżawy Azure."
    services=""
    documentationCenter=".net"
    authors="wadepickett" 
    manager="wpickett"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/05/2015"
    ms.author="wpickett"/>

# <a name="multitenant-applications-in-azure"></a>Multitenant aplikacji platformy Azure

Multitenant aplikacja jest zasobów współużytkowanych, która zezwala na kilku użytkowników lub "dzierżaw" wyświetlić aplikacji, jakby było własne. Typowy scenariusz, który pozwala na multitenant aplikacji jest jednym w którym wszyscy użytkownicy aplikacji może być wymagane Dostosowywanie sposobu pracy użytkownika, ale w przeciwnym razie samej podstawowe wymagania. Przykłady dużych multitenant aplikacji usługi Office 365, usługi Outlook.com i visualstudio.com.

Z perspektywy dostawcę aplikacji zalety multitenancy głównie dotyczą efektywności działania i kosztów. Jedną wersję aplikacji można potrzeb wielu dzierżaw i klientów, umożliwiając konsolidacji systemu zadań administracyjnych, takich jak monitorowanie, dostosowywanie wydajności oprogramowania konserwacji i kopie zapasowe danych.

Poniżej zamieszczono listę najważniejsze cele i wymagania z perspektywy dostawcy.

- **Obsługi**: muszą mieć możliwość obsługi administracyjnej dzierżaw nowej aplikacji.  W przypadku multitenant aplikacji z dużą liczbą dzierżaw jest zazwyczaj konieczne zautomatyzować ten proces, umożliwiając samodzielne inicjowania obsługi administracyjnej.
- **Łatwość**: konieczne jest uaktualnienie aplikacji i wykonywać inne zadania konserwacyjne, podczas jego używania wielu dzierżaw.
- **Monitorowanie**: muszą mieć możliwość monitorowania stosowania przez cały czas zidentyfikowanie problemów i ich rozwiązaniu. Ta opcja uwzględnia monitorowania, jak każdy dzierżawy jest przy użyciu aplikacji.

Właściwie wprowadzony aplikacji multitenant zapewnia następujące korzyści użytkownikom.

- **Izolacji**: działania poszczególnych dzierżaw nie ma wpływu na korzystanie z aplikacji przez innych dzierżaw. Dzierżaw nie ma dostępu dane innych osób. Wydaje się do dzierżawy, tak jakby mają wyłączności korzystanie z aplikacji.
- **Dostępność**: dzierżaw poszczególnych ma aplikacji był stale dostępny, być może gwarancje zdefiniowane w Umowa dotycząca poziomu usług. Ponownie działań innych dzierżaw nie narusza dostępności aplikacji.
- **Skalowalność**: aplikacja skale do zapotrzebowania poszczególnych dzierżaw. Obecność i działań innych dzierżaw nie narusza wydajność aplikacji.
- **Koszty**: koszty są mniejsze niż uruchomienie aplikacji dedykowane, pojedyncze dzierżawy, ponieważ wielu dzierżawy umożliwia udostępnianie zasobów.
- **Customizability**. Możliwość dostosowywania aplikacji dla poszczególnych dzierżawy na różne sposoby, takie jak dodawanie lub usuwanie funkcji, zmienianie kolorów i logo lub nawet dodając własne kod lub skrypt.

Krótko mówiąc istnieje wiele zagadnienia, które należy wziąć pod uwagę do obsługi wysoce skalowalna, wiążą się też liczbę cele i wymagania, które są wspólne dla wielu aplikacji multitenant. Niektóre mogą nie być istotne w określonych scenariuszach, a znaczenie poszczególnych cele i wymagania będą różne dla każdego scenariusz. Jako dostawca multitenant aplikacji możesz również zostanie przydzielony cele i wymagania, takich jak spotkania dzierżaw cele i wymagania, rentowność rozliczeń, wielu poziomów usług, inicjowania obsługi administracyjnej, łatwość monitorowania i automatyzacji.

Aby uzyskać więcej informacji na dodatkowe zagadnienia multitenant aplikacji zobacz [hostingu aplikację wielu dzierżawy Azure][]. Aby uzyskać informacji na temat typowych wzorców Architektura danych aplikacji bazy danych (władz akredytacji bezpieczeństwa) oprogramowania jako usługi wielu dzierżawy zobacz [Desenie projektu dla wielu dzierżawy władz akredytacji bezpieczeństwa aplikacji z bazy danych SQL Azure](./sql-database/sql-database-design-patterns-multi-tenancy-saas-applications.md). 

Azure udostępnia wiele funkcji, które umożliwiają kluczowe problemy napotykane podczas projektowania multitenant systemu.

**Izolacji**

- Dzierżaw części witryny sieci Web przez nagłówków hosta z lub bez komunikacji SSL
- Dzierżaw części witryny sieci Web przez parametry kwerendy
- Usługi sieci Web w role pracownika
    - Role pracownika. które zwykle procesu dane do wewnętrznej bazy danych aplikacji.
    - Role w sieci Web, które zwykle pełnić rolę serwera sieci Web dla aplikacji.

**Miejsca do magazynowania**

Zarządzanie danymi, takich jak bazy danych SQL Azure lub miejsce do magazynowania Azure usług, takich jak usługa tabeli, która udostępnia usługi magazynu dużych ilości danych niestrukturalne i usługa obiektów Blob, która zapewnia usługi przechowywania dużych ilości tekstu niestrukturalne lub dane binarne, takie jak klipy wideo, audio i obrazów.

- Zabezpieczanie danych Multitenant w bazie danych SQL odpowiednie logowania do programu SQL Server dla dzierżawy.
- Za pomocą tabel Azure dla aplikacji zasobów, określając zasadę dostępu na poziomie kontenera, możesz to zrobić możliwość dostosowania uprawnienia bez konieczności problemu nowego adresu URL dla zasobów chronione za pomocą podpisów udostępniania.
- Azure kolejek dla kolejek Azure zasoby aplikacji są często używane do procesu przetwarzania dysk imieniu dzierżaw, ale może być również do rozpowszechniania pracy potrzebną do inicjowania obsługi administracyjnej lub zarządzanie.
- Kolejek Bus usług dla zasobów aplikacji, które umieszcza pracy do udostępnionego usługę, możesz użyć pojedynczej kolejki miejsce, w którym każdemu nadawcy dzierżawy tylko uprawnieniami (jak określony na podstawie oświadczeniach wystawiony przez ACS) aby przekazać do tej kolejki, podczas gdy tylko odbiorców w usłudze mieć uprawnienie do pobierać z kolejki dane pochodzące z wielu dzierżaw.


**Połączenia i usług zabezpieczeń**

- Azure Bus usługi, infrastruktura wiadomości, która znajduje się między aplikacjami, co pozwala im do wymiany wiadomości w sposób luźno powiązanych ulepszone skali i elastyczność.

**Usługi sieci**

Azure udostępnia kilka usługi sieciowe obsługi uwierzytelniania i poprawić zarządzania hostowanej aplikacji. Te usługi są następujące:

- Azure umożliwia wirtualną sieć możesz dodawać i zarządzanie wirtualnych sieci prywatnych (VPN) platformy Azure także bezpiecznie połączyć je razem z lokalną infrastrukturę informatyczną.
- Menedżer ruch sieci wirtualnej umożliwia równoważenia obciążenia przychodzących ruchu w wielu usługach Azure hostowanej czy używasz w tym samym centrum danych lub wielu różnych centrach danych na całym świecie.
- Azure Active Directory (Azure AD) to nowoczesny, oparte na pozostałych usługa, która zapewnia możliwości kontroli dostępu i Zarządzanie tożsamościami aplikacje w chmurze. Używanie Azure AD dla zasobów aplikacji Azure AD do zapewnia łatwy sposób uwierzytelniania i autoryzacji użytkowników do uzyskania dostępu do usług i aplikacji sieci web umożliwienie funkcje uwierzytelniania i autoryzacji, aby uwzględnić poza kodu.
- Udostępnia Azure Bus usługi bezpiecznego wiadomości i distributed możliwość przepływu danych dla hybrydowych aplikacjami, takimi jak komunikacji między Azure hostowanej aplikacji i lokalnej aplikacji i usług, bez konieczności złożonych infrastruktury zapory i zabezpieczeń. Za pomocą usługi Bus przekazywania dla zasobów aplikacji usług, które są dostępne jako punkty końcowe mogą należeć do dzierżawy (na przykład hostowanej poza systemu, takie jak lokalna) lub mogą być usług obsługi administracyjnej specjalnie dla dzierżawy (ponieważ przesyłania danych poufnych, specyficzne dla dzierżawy w ich).



**Obsługa administracyjna zasobów**

Azure udostępnia na kilka sposobów świadczenia nowych dzierżaw dla aplikacji. W przypadku multitenant aplikacji z dużą liczbą dzierżaw jest zazwyczaj konieczne zautomatyzować ten proces, umożliwiając samodzielne inicjowania obsługi administracyjnej.

- Role pracownika umożliwiają dostarczanie i do świadczenia na dzierżawcę zasobów (na przykład gdy nowej dzierżawy znaki na ekranie lub anuluje), zbieranie metryki pomiaru korzystania i zarządzania nimi skali według określonego harmonogramu lub w odpowiedzi na skrzyżowania progi kluczowe wskaźniki wydajności. Tej samej roli może być również przesunąć aktualizacji i aktualizacje do rozwiązania.
- Azure blob mogą być używane do obsługi administracyjnej obliczeń lub wstępnie zainicjowany zasoby miejsca do magazynowania dla nowych dzierżaw zapewniając zasady dostępu na poziomie kontenera ochrony do uruchamiania usługi pakietów obrazów wirtualnego dysku twardego i inne zasoby.
- Opcje obsługi administracyjnej bazy danych SQL zasobów dla dzierżawy obejmują:

    -   DDL w skryptów lub osadzonego jako zasoby w zestawy
    -   Program SQL Server 2008 R2 DAC pakiety wdrożony programowy.
    -   Kopiowanie z referencyjne bazy danych
    -   Używanie bazy danych importu i eksportu do obsługi administracyjnej nowych baz danych z pliku.



<!--links-->

[Obsługa aplikacji wielu dzierżawy Azure]: http://msdn.microsoft.com/library/hh534480.aspx
[Designing Multitenant Applications on Azure]: http://msdn.microsoft.com/library/windowsazure/hh689716
