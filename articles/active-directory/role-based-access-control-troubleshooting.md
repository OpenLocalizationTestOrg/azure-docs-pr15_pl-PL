<properties
    pageTitle="Rozwiązywanie problemów z kontrola dostępu oparta na rolach | Microsoft Azure"
    description="Uzyskaj pomoc dotyczącą problemów lub pytania dotyczące zasobów kontrola dostępu oparta roli."
    services="azure-portal"
    documentationCenter="na"
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/12/2016"
    ms.author="kgremban"/>

# <a name="role-based-access-control-troubleshooting"></a>Rozwiązywanie kontrola dostępu oparta na rolach

## <a name="introduction"></a>Wprowadzenie

[Kontrola dostępu oparta na rolach](role-based-access-control-configure.md) jest zaawansowanych funkcji, która umożliwia delegowanie szerokiego dostępu do zasobów w Azure. Oznacza to, że poczujesz pewność udzielanie określoną osobę w prawo, aby użyć dokładnie potrzebują i już. Jednak czasami model zasobów dla zasobów Azure może być skomplikowana i może być trudne do zrozumienia dokładnie jakie będą mieli uprawnienia do.

Ten dokument zostanie wyświetlona informacja, czego mogą się spodziewać podczas korzystania z niektóre role w portalu Azure. Następujące trzy role obejmuje wszystkich typów zasobów:

- Właściciel  
- Trybu współautora  
- Czytnik  

Właściciele i współpracowników mieć pełny dostęp do środowiska zarządzania, ale współautora nie może udzielić dostępu do pozostałych użytkowników lub grup. Elementy interesujący nieco bardziej z rolą czytnika, jest to miejsce, w którym będzie poświęcić trochę czasu. Zobacz [artykuł wprowadzenie get kontrola dostępu oparta na rolach](role-based-access-control-configure.md) Aby uzyskać szczegółowe informacje na temat do udzielania dostępu.

## <a name="app-service-workloads"></a>Obciążeń pracą usługi aplikacji

### <a name="write-access-capabilities"></a>Możliwości zapisu

Jeśli udzielić użytkownikowi dostępu tylko do odczytu do aplikacji sieci web pojedynczy, niektóre funkcje są wyłączone, że nie mogą się spodziewać. Następujące funkcje zarządzania wymaga **Pisanie** dostępu do aplikacji sieci web (Współautor lub właściciel), a nie będzie dostępny w dowolnym scenariusz tylko do odczytu.

- Polecenia (Rozpocznij, Zatrzymaj, itp.)
- Zmienianie ustawień, takich jak ogólny układ, ustawień skali, ustawienia kopii zapasowych i ustawienia monitorowania
- Uzyskiwanie dostępu do publikowania poświadczenia i inne hasła, takie jak ustawienia aplikacji i parametry połączenia
- Przesyłanie strumieniowe dzienników
- Dzienniki diagnostyczne konfiguracji
- Console (wiersza polecenia)
- Aktywne i ostatnio używane wdrożeń (wdrożenia lokalnego cyfra ciągły)
- Szacowane wydatki
- Testy sieci Web
- Wirtualna sieć (widoczne tylko dla czytnika, jeśli wcześniej skonfigurowano wirtualnej sieci przez użytkownika z uprawnieniami do zapisu).

Jeśli nie masz dostępu do dowolnego z tych Kafelki, musisz skontaktować się z administratorem współautora dostęp do aplikacji sieci web.

### <a name="dealing-with-related-resources"></a>Sposób postępowania z zasoby pokrewne

Aplikacje sieci Web są skomplikowane obecności kilka różnych zasobów, które wzajemne oddziaływanie. Oto grupy typowe zasobów przy użyciu kilka witryn sieci Web:

![Grupa zasobów aplikacji sieci Web](./media/role-based-access-control-troubleshooting/website-resource-model.png)

W wyniku, jeżeli pełnomocnictwo dostęp do właśnie aplikacji sieci web wiele funkcji na karta Witryna sieci Web w portalu Azure zostanie wyłączona.

Te elementy wymagają **Pisanie** dostęp do **aplikacji usługi plan** , który odnosi się do witryny sieci Web:  

- Wyświetlanie aplikacji sieci web jest ceny warstwa (wolny lub standardowe)  
- Konfiguracja skali (liczba wystąpień, rozmiar maszyn wirtualnych, ustawienia autoscale)  
- Przydziałów (miejsca do magazynowania, przepustowość, Procesor)  

Te elementy wymagają **Pisanie** dostęp do całej **grupy zasobów** zawiera witryny sieci Web:  

- Certyfikaty SSL i powiązania (jest to ponieważ certyfikatów SSL są udostępniane między witrynami w tej samej grupy zasobów i lokalizacją geo)  
- Reguły alertów  
- Ustawienia Autoscale  
- Składniki wniosków aplikacji  
- Testy sieci Web  

## <a name="virtual-machine-workloads"></a>Obciążenia maszyn wirtualnych

Podobnie jak przy użyciu aplikacji web apps, niektóre funkcje na karta maszyn wirtualnych wymagają zapisu maszyn wirtualnych lub inne zasoby w grupie zasobów.

Maszyn wirtualnych odnoszą się do nazwy domeny, wirtualnych sieci, kont miejsca do magazynowania i reguł alertów.

Te elementy wymagają **maszyn wirtualnych** **Pisanie** programu access:

- Punkty końcowe  
- Adresy IP  
- Dysków  
- Rozszerzenia  

**Pisanie** dostęp do **maszyn wirtualnych**i **Grupa zasobów** (wraz z nazwy domeny), który znajduje się w tych wymagają:  

- Konfigurowanie dostępności  
- Ładowanie zestawu strategii  
- Reguły alertów  

Jeśli nie masz dostępu do dowolnego z tych Kafelki, musisz skontaktować się z administratorem współautora dostępu do grupy zasobów.

## <a name="see-more"></a>Zobacz więcej
- [Kontrola dostępu oparta roli](role-based-access-control-configure.md): rozpoczynanie pracy z RBAC w portalu Azure.
- [Wbudowane ról](role-based-access-built-in-roles.md): uzyskiwanie informacji o rolach, które standardowe w RBAC.
- [Role niestandardowe w Azure RBAC](role-based-access-control-custom-roles.md): Dowiedz się, jak tworzyć niestandardowe role do potrzeb użytkownika programu access.
- [Tworzenie dostępu zmienić raport historii](role-based-access-control-access-change-history-report.md): śledzenie informacji dotyczących Zmienianie przypisania ról w RBAC.
