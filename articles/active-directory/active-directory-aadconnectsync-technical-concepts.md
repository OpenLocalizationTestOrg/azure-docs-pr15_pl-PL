<properties
    pageTitle="Azure AD Connect synchronizacją: pojęcia techniczne | Microsoft Azure"
    description="W tym artykule wyjaśniono techniczne koncepcji Azure AD Connect synchronizacji."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="markusvi;andkjell"/>


# <a name="azure-ad-connect-sync-technical-concepts"></a>Azure AD Connect synchronizacją: pojęcia techniczne
W tym artykule przedstawiono podsumowanie tematu [Architektura opis](active-directory-aadconnectsync-technical-concepts.md).

Azure AD Connect synchronizacją opiera się platformy pełne metadirectory synchronizacji.
Poniższe sekcje wprowadzenie pojęcia metadirectory synchronizacji.
Oparciu MIIS, ILM i FIM, Azure Active Directory Services synchronizacji zapewnia platformę następnego połączenia ze źródłami danych, synchronizowanie danych między źródłami danych, a także inicjowania obsługi administracyjnej i cofanie ubezpieczeń tożsamości.

![Pojęcia techniczne](./media/active-directory-aadconnectsync-technical-concepts/scenario.png)

W poniższych sekcjach przedstawiono szczegółowe informacje dotyczące następujących aspektów usługi synchronizacji FIM:

- Łącznik
- Atrybut przepływu
- Obszar łączników
- Metaverse
- Inicjowanie obsługi administracyjnej

## <a name="connector"></a>Łącznik

Modułów kodu, w których są używane do komunikacji z połączonych katalogu są nazywane łączników (wcześniej nazywanego agenci zarządzania (MAs)).

Te są zainstalowane na komputerze z systemem Azure AD Connect synchronizacji.
Łączniki umożliwiają bez wykorzystania agentów do konwersacji przy użyciu protokołów system zdalny zamiast Polegaj na wdrożenie czynników specjalistyczne. Oznacza to, że może się zmniejszyć ryzyko i czas wdrażania, zwłaszcza w przypadku, gdy zajmujących się krytyczne aplikacji i systemów.

Na powyższym obrazie łącznik jest synonimem obszar łączników, ale obejmuje całą komunikację z systemem zewnętrznym.

Łącznik jest odpowiedzialny za wszystkie importowania i eksportowania funkcji do systemu i zwolnienia deweloperów z konieczności zrozumienie sposobu łączenia się z każdego systemu oryginalnie podczas dostosowywania przekształcenia danych przy użyciu deklaracyjnych inicjowania obsługi administracyjnej.

Importu i eksportu tylko wówczas, gdy planowany, umożliwiające dalsze izolacji z zmian w ramach systemu, ponieważ zmiany nie są automatycznie propagowane do podłączonego źródła danych. Ponadto deweloperów może również tworzyć własne łączniki do łączenia się z praktycznie dowolnych źródeł danych.

## <a name="attribute-flow"></a>Atrybut przepływu

Metaverse jest skonsolidowany widok wszystkich połączonych tożsamości sąsiadujące spacje łącznik. Na powyższym rysunku przepływu atrybut jest przedstawiona przez linie ze strzałkami dla przepływu przychodzące i wychodzące. Atrybut przepływ jest proces kopiowania lub Przekształcanie danych z jednego systemu i wszystkich atrybutów przepływów (przychodzących i wychodzących).

Atrybut przepływ występuje między obszar łączników i metaverse bi techniką podczas planowania do uruchomienia operacji synchronizacji (pełnej lub zmiana).

Atrybut przepływu przeprowadzana wyłącznie po tych synchronizacji są uruchamiane. Atrybut przepływów są definiowane w regułach synchronizacji. Te mogą być ruchu przychodzącego (ISR na powyższym obrazie) lub ruchu wychodzącego (OSR na powyższym obrazie).

## <a name="connected-system"></a>System połączonego

System połączonego (czyli połączonego katalog) jest odwołując się do systemu zdalnego narzędzie Azure AD Connect synchronizacją jest połączony i czytania i pisania danych tożsamości, do i z.

## <a name="connector-space"></a>Obszar łączników

Dla każdego źródła danych połączonych jest reprezentowane jako podzbiór filtrowanych obiekty i atrybuty w obszarze łącznik.
Umożliwia do działania lokalnie, bez konieczności kontakt system zdalny podczas synchronizowania obiektów usługi synchronizacji i ogranicza interakcji w celu importowanie i eksportuje tylko.

Jeśli źródła danych i łącznik ma możliwość wyświetlania listy zmian (importowanie delta), operacyjne efektywności zwiększany znacznie jako tylko zmiany wprowadzone od czasu ostatniego cyklu ankieta są wymieniane. Obszar łączników uwalniają źródło danych połączonych z propagowanie automatycznie przez wymaganie czy w harmonogramie łącznik importuje i eksportuje zmiany. Dodano ubezpieczenia udziela wyróżniane podczas testowania, wyświetlanie podglądu i Potwierdzanie następnej aktualizacji.

## <a name="metaverse"></a>Metaverse

Metaverse jest skonsolidowany widok wszystkich połączonych tożsamości sąsiadujące spacje łącznik.

Gdy tożsamości są połączone ze sobą, Urząd przypisano dla różnych atrybutów za pośrednictwem Importowanie przepływu mapowania, obiektu metaverse centralnej zaczyna agregacji informacji z wielu systemów. Z tego przepływu atrybut obiektu mapowania zawierać informacje do systemów ruchu wychodzącego.

Obiekty są tworzone podczas system autorytatywne projektów je do metaverse. Zaraz po usunięciu wszystkich połączeń obiektu metaverse zostanie usunięty.

Obiekty w metaverse nie można edytować bezpośrednio. Wszystkie dane w obiekcie musi zamieszczanie za pośrednictwem przepływu atrybut. Metaverse zachowuje trwałych łączników z każdego miejsca łącznika. Te łączniki nie wymagają ponownej oceny dla każdego synchronizacja. Oznacza to, narzędzie Azure AD Connect synchronizacją nie ma Znajdź pasujące obiektu zdalnego zawsze. Pozwala to uniknąć potrzeby kosztów czynników uniemożliwić wprowadzanie zmian atrybutów, które zwykle są odpowiedzialne za korelacji obiektów.

Podczas odkrywanie nowych źródeł danych, które mogą mieć wcześniej istniejących obiektów, które muszą być zarządzane, narzędzie Azure AD Connect synchronizacją używa procesu o nazwie reguły sprzężenia ma być obliczona potencjalnych kandydatów służącą do nawiązania połączenia.
Po utworzeniu połączenia ocena nie wystąpi, a przepływ normalny atrybut mogą występować między zdalnego połączonego źródła danych i metaverse.

## <a name="provisioning"></a>Inicjowanie obsługi administracyjnej

Podczas projektów Źródło autorytatywne nowy obiekt do metaverse nowy obiekt miejsca łącznika można tworzony w innej łącznik reprezentujący podrzędne połączone źródło danych.

Założenia potwierdza łącze, a przepływ atrybut kontynuowaniem bi techniką.

Gdy reguły określa, że nowy obiekt miejsca łącznik musi być utworzony, nazywany inicjowania obsługi administracyjnej. Jednak ponieważ tej operacji tylko odbywa się w obszarze łącznika, go nie przenoszenia do źródła danych połączonych do momentu odbywa się eksportu.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Azure AD Sync połączenia: Opcje dostosowywania synchronizacji](active-directory-aadconnectsync-whatis.md)
* [Integrowanie tożsamości lokalnych z usługi Azure Active Directory](active-directory-aadconnect.md)

<!--Image references-->
[1]: ./media/active-directory-aadsync-technical-concepts/ic750598.png
