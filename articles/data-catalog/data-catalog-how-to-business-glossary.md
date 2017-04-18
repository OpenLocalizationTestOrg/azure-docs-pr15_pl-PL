<properties
    pageTitle="Jak skonfigurować słownik firm dla podlega znakowania | Microsoft Azure"
    description="Artykule wyróżnianie słownik biznesowych w wykazie danych Azure do definiowania i używania wspólnego słownika firm do niego znacznik zarejestrowany aktywów danych."
    services="data-catalog"
    documentationCenter=""
    authors="steelanddata"
    manager="NA"
    editor=""
    tags=""/>
<tags
    ms.service="data-catalog"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.workload="data-catalog"
    ms.date="09/21/2016"
    ms.author="maroche"/>

# <a name="how-to-set-up-the-business-glossary-for-governed-tagging"></a>Jak skonfigurować słownik firm podlega znakowania

## <a name="introduction"></a>Wprowadzenie

Azure wykaz danych zawiera funkcje Odnajdowanie źródeł danych, umożliwiające użytkownikom łatwe odnajdowanie i opis źródła danych, które są potrzebne do przeprowadzenia analizy i podejmowanie decyzji. Tych funkcji odnajdowania wprowadź największy wpływ, gdy użytkownicy mogą znaleźć i opis najszerszym zakresie dostępnych źródeł danych.

Znakowanie jest jedną funkcję wykazu danych, które ułatwia lepsze zrozumienie danych składników majątku. Znakowanie umożliwia użytkownikom skojarzyć słów kluczowych środka trwałego lub kolumnę, która z kolei ułatwia odnajdowanie trwałego za pośrednictwem wyszukiwania lub przeglądania, i umożliwia użytkownikom łatwiej zrozumieć kontekstu i przeznaczenie środka.

Jednak znakowanie czasami może powodować problemy z własnych. Niektóre problemy, które mogą zostać wprowadzone przez znakowania należą:

1.  Jeśli jesteś użytkownikiem za pomocą skrótów na niektóre składniki majątku i rozwinięty tekst w innych podczas znakowania. Ten niespójności przeszkadza odnajdowania środków trwałych, mimo że został zamiarem Aby oznakować składniki majątku z ten sam znacznik.
2.  Znaczniki, które oznaczają różne w różnych kontekstach. Na przykład tag o nazwie "Zysk" w zbiorze danych klienta może oznaczać przychodu przez klienta, ale ten sam znacznik na zestaw danych sprzedaży kwartalnej może oznaczać kwartalnych przychodu firmy.  

W celu, rozwiązania tych i innych podobnych wyzwania, wykazu danych zawiera słownik firm.

Słownik firm wykaz danych umożliwia organizacjom dokumentu biznesowymi o kluczowym terminy i definicje tworzenie wspólnego słownika firm. Ten zarządzania umożliwia spójności użycia danych w całej organizacji. Po terminy są definiowane w słowniku firm, mogą one przypisane do danych składniki majątku w katalogu, używając to samo podejście jako znakowania, umożliwiając _podlega znakowania_.

> [AZURE.NOTE] Funkcje opisane w tym artykule są dostępne tylko w standardowej wersji programu Azure wykazu danych. Bezpłatna wersja nie oferuje możliwości, dla której działalność znakowania lub słownik firm.

## <a name="glossary-availability-and-privileges"></a>Dostępność słownik i uprawnienia

*Słownik firm jest dostępna w standardowej wersji programu Azure wykazu danych. Bezpłatne Edition wykazu danych nie zawiera słownika.*

Słownik biznesowych są dostępne za pomocą opcji "Słownik" w menu nawigacji portalu wykazu danych.  

![Uzyskiwanie dostępu do słownika firm](./media/data-catalog-how-to-business-glossary/01-portal-menu.png)


Członkowie roli Administratorzy słownik i administratorów wykaz danych można utworzyć, edytowanie i usuwanie słownik w słowniku firm. Wszyscy użytkownicy wykazu danych można wyświetlać definicje terminów, a może oznaczać zasobami za pomocą słownik.

![Dodawanie nowego terminu — słownik](./media/data-catalog-how-to-business-glossary/02-new-term.png)


## <a name="creating-glossary-terms"></a>Tworzenie słownik

Administratorzy wykazu danych i Administratorzy słownik, można utworzyć nowy słownik, klikając pozycję na nowy termin "przycisk, aby utworzyć słownik z następujące pola:

* Definicja firm terminu
* Opis, który rejestruje przeznaczone lub reguł biznesowych dotyczących zawartości i kolumny
* Wykaz uczestników, którzy najbardziej o termin
* Terminu nadrzędnego, który definiuje hierarchii, w której są zorganizowane terminów


## <a name="glossary-term-hierarchies"></a>Glosariusz terminów hierarchii

Słownik wykazu danych biznesowych umożliwia Opisz słownictwa firm jako hierarchia terminów. Dzięki temu organizacjom tworzenie klasyfikacji terminów lepiej odpowiada ich taksonomii firm.

Nazwa terminu musi być unikatowa na danym poziomie hierarchii — zduplikowane nazwy są niedozwolone. Istnieje limit liczby poziomów w hierarchii, ale hierarchii jest często bardziej zrozumiały po trzy poziomy lub mniej.

Korzystanie z hierarchii w słowniku firm jest opcjonalne. Pozostawiając puste pole terminu nadrzędnego na słownik utworzy listy płaskie (non hierarchicznych) warunków w słowniku.  

## <a name="tagging-assets-with-glossary-terms"></a>Znakowanie zasobami za pomocą słownik

Po zdefiniowaniu słownik w wykazie występowania znakowanie składników majątku jest zoptymalizowana pod Aby przeszukiwać słowniku jako użytkownik wpisze ich znacznika. Portalu wykazu danych zawiera listę zgodnych słownik, użytkownik może wybierać. Jeśli użytkownik wybierze słownik z listy jest dodawany do elementu jako tag (alias Słownik tag). Aby utworzyć nowy znacznik, wpisując terminu, który nie znajduje się w słowniku (alias również wybrać użytkownika tag użytkownika).

![Trwały danych oznakowano znaczników jednego użytkownika i dwa tagi — słownik](./media/data-catalog-how-to-business-glossary/03-tagged-asset.png)

> [AZURE.NOTE] Tagi użytkownika są tylko rodzaj znacznika obsługiwane w bezpłatnej wersji wykazu danych.

### <a name="hover-behavior-on-tags"></a>Zachowanie umieść wskaźnik myszy na znaczników
W portalu wykazu danych dwa typy znaczników różnią się wizualnie, z różnych hover zachowania. Gdy użytkownik znajduje się nad znacznika użytkownika mogą je wyświetlać tekst znacznika i użytkownika lub użytkowników, którzy został dodany znacznik. Gdy użytkownik znajduje się nad znacznika słownik, też wyświetlić definicji słownik i łącze, aby otworzyć Słownik firm, aby wyświetlić pełną definicję terminu.

### <a name="search-filters-for-tags"></a>Filtry wyszukiwania znaczników
Zarówno znaczniki słownik znaczniki użytkownika są można wyszukiwać i mogą być stosowane jako filtry w wynikach wyszukiwania.

## <a name="summary"></a>Podsumowanie
Słownik firm wykazu danych Azure i której działalność znakowania społecznościowego, który umożliwia, możliwe danych składniki majątku zidentyfikował, zarządzania i w sposób ciągły. Słownik firm może podwyższanie szkoleń słownictwo firm wśród użytkowników w organizacji i obsługuje zrozumiałej metadane do przechwytywane, wykonywanie wykrywanie zasobów i opis błyskawicznie.

## <a name="see-also"></a>Zobacz też

- [Interfejsu API usługi REST dokumentacji firmy — słownik](https://msdn.microsoft.com/library/mt708855.aspx)
