<properties
   pageTitle="Klasyfikacja danych Azure | Microsoft Azure"
   description="Ten artykuł zawiera wprowadzenie do podstawy klasyfikacji danych i wyróżnienie jej wartość, szczególnie w kontekście chmury przetwarzania i za pomocą Microsoft Azure"
   services="security"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/16/2016"
   ms.author="yurid"/>

# <a name="data-classification-for-azure"></a>Klasyfikacja danych Azure

Ten artykuł zawiera wprowadzenie do podstawy klasyfikacji danych i wyróżnienie jej wartość, szczególnie w kontekście chmury przetwarzania i za pomocą Microsoft Azure. 

## <a name="data-classification-fundamentals"></a>Podstawy klasyfikacji danych

Klasyfikacja pomyślnego dane w organizacji wymaga znajomości szerokich potrzeb danej organizacji i dokładne zrozumienie miejsce, w którym znajdują się aktywów danych.  
 
Istnieją dane w jednym z trzech stanów podstawowe: 

- W stanie spoczynku 
- W procesie 
- Podczas przesyłania 
 
Wszystkie trójstanowy wymagają unikatowych rozwiązań technicznych klasyfikacji danych, ale zastosowaniu zasad klasyfikacji danych powinien być taki sam dla każdej z nich. Dane, które będą klasyfikowane jako poufne musi pozostać poufne na pozostałych w procesie i podczas przesyłania. 
 
Dane można także strukturalne i niestrukturalne. Typowe klasyfikacji procesów danych strukturalnych w bazach danych i arkuszy kalkulacyjnych są mniej złożone i czasochłonnym niż dane niestrukturalne, takie jak dokumenty, kod źródłowy i poczty e-mail. 

> [AZURE.TIP] Aby uzyskać więcej informacji na temat możliwości Azure i najważniejsze wskazówki dotyczące danych szyfrowania przeczytaj [Azure danych szyfrowania najważniejsze wskazówki](azure-security-data-encryption-best-practices.md)

Na ogół organizacji będą mieć więcej danych niestrukturalne niż danych strukturalnych. Niezależnie od tego, czy dane strukturalne i niestrukturalne jest ważne zarządzać charakter danych. Po prawidłowo, klasyfikacji danych ułatwia, upewnij się, że poufne lub tajne dane, które zasoby są zarządzane z nadzorem większa niż aktywów danych, które są traktowane jako publicznej lub bezpłatne do rozpowszechniania. 

### <a name="controlling-access-to-data"></a>Kontrolowanie dostępu do danych 

Uwierzytelniania i autoryzacji często mylić ze sobą, a ich role błędnie interpretowane. W rzeczywistości są bardzo różne, jak pokazano na poniższym rysunku.  

![Dostęp do danych i kontroli](./media/azure-security-data-classification/azure-security-data-classification-fig1.png)

### <a name="authentication"></a>Uwierzytelnianie 

Uwierzytelnianie zazwyczaj składa się z co najmniej dwie części: nazwa użytkownika lub Identyfikatora użytkownika do identyfikowania użytkownika i tokenu, takich jak hasło, aby potwierdzić, że poświadczenia nazwy użytkownika jest prawidłowy. Proces nie umożliwia uwierzytelnionemu użytkownikowi dostęp do wszelkich elementów lub usług; sprawdza, że użytkownik jest kto mówią, że są one.   

> [AZURE.TIP] [Usługi Azure Active Directory](../active-directory/active-directory-whatis.md) zapewnia usługi opartej na chmurze tożsamości, które umożliwiają uwierzytelniania i autoryzacji użytkowników. 

### <a name="authorization"></a>Autoryzacja
 
Autoryzacja polega na udostępnianiu uwierzytelniony użytkownik możliwości dostępu do aplikacji, zestaw danych, plik danych lub innego obiektu. Przypisywanie uwierzytelnionych użytkowników prawa do używania, modyfikowanie lub usuwanie elementów, które mogą uzyskiwać dostęp do wymaga uwagi do klasyfikacji danych. 

Autoryzacja pomyślnego wymaga stosowania mechanizmu do sprawdzania poprawności potrzeb poszczególnym użytkownikom dostęp do plików i informacji w zależności od kombinacji roli, zasady zabezpieczeń i zasad ryzyka zagadnienia. Na przykład dane z określonej aplikacji (LOB): LOB nie może być konieczne są dostępne dla wszystkich pracowników, a mały podzbiór pracowników prawdopodobnie wymaga dostępu do plików zasobów ludzkich (h). Ale organizacjom sterowania uzyskiwać dostęp do danych jako także kiedy i jak, skutecznego systemu uwierzytelniania użytkowników muszą znajdować się w miejscu. 

> [AZURE.TIP] w programie Microsoft Azure upewnij się, je wykorzystać kontrola dostępu Azure Role-Based (RBAC) aby udzielić tylko ilość programu access, użytkownicy muszą wykonać swoje zadania. Aby uzyskać więcej informacji, przeczytaj [przypisania roli Użyj zarządzać dostępem do zasobów usługi Azure Active Directory](../active-directory/role-based-access-control-configure.md) . 

### <a name="roles-and-responsibilities-in-cloud-computing"></a>Role i obowiązki w chmurze, przetwarzania danych 

Chociaż może pomóc w chmurze dostawców zarządzania ryzykiem, użytkownicy musieli upewnij się, że klauzul danych i wymuszania właściwie jest wykonywane w celu zapewnienia odpowiedniego poziomu usług zarządzania danymi.  
 
Obowiązki klasyfikacji danych zależy model usługi cloud, który znajduje się w miejscu, jak pokazano na poniższym rysunku. Infrastruktura usługi (IaaS), platformy usług (PaaS) i oprogramowania jako usługa (władz akredytacji bezpieczeństwa) są następujące trzy modele usługi cloud podstawowego. Implementacja mechanizmy klasyfikacji danych również zależy zależność od i oczekiwań dostawcy chmury. 

![Role](./media/azure-security-data-classification/azure-security-data-classification-fig2.png)

Mimo że odpowiadają klasyfikacji danych, dostawców chmurze należy ustawić pisanych zobowiązań wyjaśniono, jak będzie bezpiecznego i zachować prywatność danych klientów, przechowywane w chmurze ich.  

- Wymagania dotyczące **dostawców IaaS** są ograniczone do zapewnianie, że środowisko wirtualne umożliwia dodawanie możliwości klasyfikacji danych i wymagania dotyczące zgodności klienta. Dostawców IaaS mają mniejsze ról w klasyfikacji danych, ponieważ tylko potrzebne upewnić się, że dane klientów adresów spełnianie wymagań dotyczących. Jednak dostawców musi mając pewność, że ich środowiskach wirtualnych adresów wymagań klasyfikacji danych oprócz zabezpieczanie centrach danych.
- Obowiązki **dostawców PaaS** mogą być mieszane, ponieważ platformy można używać w podejściu warstwami w celu zapewnienia bezpieczeństwa dla narzędzia klasyfikacji. Dostawcy PaaS może być odpowiedzialny za uwierzytelniania i ewentualnie kilka reguł autoryzacji i należy podać zabezpieczeń i możliwości klasyfikacji danych z nimi warstwach aplikacji. Podobnie jak dostawców IaaS dostawców PaaS konieczne upewnij się, że platformy spełnia wymagania klasyfikacji wszelkie odpowiednie dane.
- **Dostawców władz akredytacji bezpieczeństwa** często są traktowane jako część łańcucha autoryzacji i będzie potrzebny do upewnij się, że dane przechowywane w władz akredytacji bezpieczeństwa aplikacji mogą być sterowane przez typ klasyfikacji. Aplikacje władz akredytacji bezpieczeństwa można dla aplikacji FIRMOWYCH oraz ich potrzeb charakter służyć do uwierzytelniania i autoryzacji danych, która jest używana i przechowywane. 

## <a name="classification-process"></a>Proces klasyfikacji 

Wiele organizacji, które rozumie potrzebę klasyfikacji danych i chcesz ją wdrożyć buźkę wezwanie podstawowe: czego zacząć?

Jeden skuteczne i prosty sposób implementacji klasyfikacji danych jest wyboru planu, wykonania, za pomocą, działają modelu z [MOF](https://technet.microsoft.com/solutionaccelerators/dd320379.aspx). Poniższa ilustracja wykresy z zadaniami, które są wymagane do pomyślnie wykonania klasyfikacji danych w modelu.  

1. **Planowanie**. Zidentyfikuj zasoby danych depozytariusza danych wdrażać program klasyfikacji i opracowywanie profilów ochrony. 
2. **DO**. Po uzgodnione zasady klasyfikacji danych, wdrażanie programu i implementowanie technologie wymuszania stosownie do potrzeb dane poufne.  
3. **Sprawdzanie**. Sprawdź i sprawdź poprawność raportów, aby upewnić się, że narzędzia i metody są skuteczne adresowania zasady klasyfikacji. 
4. **ACT**. Sprawdź stan dostęp do danych i przeglądanie plików i dane, które wymagają poprawek przyjęcia zmian przy użyciu metody podziału i poprawek i adres nowych czynników ryzyka.  

![Planowanie,, sprawdzanie, działania](./media/azure-security-data-classification/azure-security-data-classification-fig3.png)
 
###<a name="select-a-terminology-model-that-addresses-your-needs"></a>Wybierz model terminologia, którego dotyczy potrzeb
 
Istnieje kilka typów procesów klasyfikacji danych, w tym ręczne procesy, procesy opartych na lokalizacji klasyfikowania danych na podstawie lokalizacji użytkownika lub systemu aplikacji procesów, takich jak klasyfikacji specyficzny dla bazy danych, a automatycznego procesy używane przez różne technologie, niektóre z nich są opisane w sekcji "Ochrona danych poufnych" w dalszej części tego artykułu.  
 
W tym artykule przedstawiono dwóch modeli terminologia ogólnych, które są oparte na modelach dobrze zajęte i zachowane branżowe. Tych modeli terminologia one zapewnić trzy poziomy wrażliwości klasyfikacji, są wyświetlane w poniższej tabeli.  

> [AZURE.NOTE] Podczas klasyfikowania plik lub zasób, który łączy dane, które zwykle będzie można podzielić na różnych poziomach, na najwyższym poziomie klasyfikacji Prezentuj powinien ustanowić ogólnej klasyfikacji. Na przykład plik zawierający dane poufne i ograniczonych powinny być klasyfikowane jako ograniczony.  

| **Uwzględnianie**   | **Model terminologia 1**   | **Terminologia modelu 2** |
|--------------------|---------------------------|-------------------------|
| Duży               | Poufne              | Z ograniczeniami              |
| Średnia             | Tylko do użytku wewnętrznego     | Wielkość liter               |
| Niska                | Publiczna                    | Bez ograniczeń            |

#### <a name="confidential-restricted"></a>Poufne (ograniczone) 

Informacje, które będą klasyfikowane jako poufny lub zastrzeżony zawiera dane, które mogą być losowych jeden lub więcej osób i/lub organizacji, jeśli złamany lub utracone. Takie informacje często znajduje się na zasadzie "wiedzieć" i mogą zawierać: 

- Dane osobowe, w tym informacji osobistych, takich jak ubezpieczenia społecznego lub numery identyfikacyjne krajowe numery passport, numerów kart kredytowych, sterownika liczby licencji, rejestr medyczny i numery identyfikacyjne zasad kondycji ubezpieczenia.  
- Rekordy finansowych, z uwzględnieniem numery kont finansowych, takich jak sprawdzanie lub numery kont inwestycji. 
- Materiały firm, takich jak dokumenty lub dane, które są unikatowe lub specyficzne własności intelektualnej.  
- Dane prawne, włączając potencjalne materiały attorney uprawnieniach. 
- Dane uwierzytelniania, włączając kluczy prywatnych kryptograficzny, pary hasła nazwa użytkownika lub innych sekwencji identyfikacyjne, takie jak pliki prywatne klucza biometryczne. 

Dane, które będą klasyfikowane jako poufne często występują przepisami i wymagania dotyczące zgodności dla przetwarzania danych. 

#### <a name="for-internal-use-only-sensitive"></a>Tylko użytku wewnętrznego (ważne)
 
Informacje, które będą klasyfikowane jako wrażliwości średnie zawiera pliki i dane, które nie będzie miało wpływ trudnych na osoby i/lub organizacji, jeśli utraconych lub zniszczone. Te informacje mogą być: 

- Adres e-mail, z których większość można usunąć lub distributed nie powodując kryzysu (bez skrzynki pocztowe lub wiadomości e-mail od osób, które są oznaczone w poufne klasyfikacji).  
- Dokumentów i plików, które nie zawierają dane poufne.
 
Ogólnie rzecz biorąc klasyfikacji zawiera wszystkie elementy, które nie są poufne. Klasyfikacja ta może zawierać większość danych biznesowych, ponieważ większość plików, które są zarządzane lub codzienną można zaklasyfikować jako liter. Z wyjątkiem danych jest przekształcana w grupę publiczną lub poufne wszystkie dane w obrębie organizacji firm można zaklasyfikować jako poufne domyślnie. 

#### <a name="public-unrestricted"></a>Publicznej (bez ograniczeń)
 
Informacje, które będą klasyfikowane jako publicznej zawiera danych i pliki, które nie są krytyczne dla potrzeb biznesowych lub działania. Klasyfikacji może również zawierać dane, który został wydany celowo ogółowi społeczeństwa do użytku, na przykład materiałów marketingowych lub naciśnij klawisz Anonsy. Ponadto klasyfikacji mogą zawierać dane, takie jak wiadomości e-mail przed spamem przechowywane przez usługi poczty e-mail. 

### <a name="define-data-ownership"></a>Definiowanie własności danych
 
Należy ustalić wyczyść custodial łańcuch własności dla wszystkich trwałych danych. W poniższej tabeli przedstawiono role własności innych danych w zakresie klasyfikacji danych i ich odpowiednich praw.  

| **Rola**        | **Tworzenie**    | **Modyfikowanie i usuwanie**   | **Pełnomocnik**  | **Odczyt**    | **Archiwum i przywracanie**   |
|-----------------|---------------|---------------------|---------------|-------------|-----------------------|
| Właściciel           | X             | X                   | X             | X           | X                     |
| Powiernik       |               |                     | X             |             |                       |
| Administrator   |               |                     |               |             | X                     |
| Użytkownika\*          |               | X                   |               | X           |                       |
**Użytkownicy udziela dodatkowych praw, takie jak edytowanie i usuwanie przez depozytariusza* 

> [AZURE.NOTE] w tej tabeli nie umożliwia Pełna lista ról i praw, ale jedynie przedstawiciela próbki. 

**Właściciel elementów zawartości danych** jest oryginalny Kreator danych, kto może delegować własności i przypisywanie depozytariusza. Po utworzeniu pliku właściciela powinno być możliwe przypisywanie klasyfikacji, co oznacza, że odpowiedzialność, aby dowiedzieć się, co należy zrobić klasyfikowane jako poufne na podstawie zasad organizacji. Wszystkie dane właściciela zawartości danych może być sklasyfikowane automatycznej dla tylko użytku wewnętrznego (wielkość liter) chyba że są odpowiedzialne za posiadanie lub tworzenia typów danych poufnych (ograniczone). Często po dane są klasyfikowane zostanie zmieniona roli właściciela. Na przykład właściciel może utworzyć bazę danych informacji niejawnych i Rezygnacja z ich praw do powiernik danych.  

> [AZURE.NOTE] Właściciele zawartości danych często używane z różnych usług, urządzeń i multimediów, niektóre z nich osobiste, a niektóre z nich należy do organizacji. Wyczyść pole wyboru organizacji zasad może pomóc w upewnij się, że zastosowania urządzeń przenośnych i inteligentnych urządzeń jest zgodnie z wytycznymi klasyfikacji danych.  

**Powiernik trwałego danych** jest przypisywana przez właściciela zawartości (lub ich pełnomocnik) do zarządzania zawartości według umowy z właścicielem zawartości lub zgodnie z wymaganiami stosownych zasad. Najlepiej, jeśli roli Opiekun może być realizowana w systemie automatycznego. Powiernik zawartości zapewnia, że konieczne uzyskiwanie dostępu do formantów są dostarczane i jest odpowiedzialny za zarządzanie i ochrona aktywów delegowane do ich care. Obowiązki powiernik zawartości może zawierać:  

- Ochrona zawartości zgodnie z kierunku właściciela zawartości lub zgadza właściciel elementów zawartości 
- Zapewnianie zgodności zasad klasyfikacji 
- Informowanie właściciele trwałego o wszelkich zmianach uzgodnione kontrolek i/lub procedury ochrony przed te zmiany wpływają 
- Raportowanie do właściciela zawartości o zmianach lub usuwania obowiązki powiernik zawartości 
- **Administrator** odpowiada użytkownik, który jest odpowiedzialny za utrzymanie integralności, ale nie są one danych składników majątku właściciela, powiernik lub użytkownika. W rzeczywistości wiele ról administratora dostępne usługi zarządzania kontenera danych bez konieczności dostępu do danych. Rola administratora zawiera wykonywanie kopii zapasowych i przywracania danych, utrzymywanie ewidencji środków trwałych i wybranie pozycji, podczas uzyskiwania oraz obsługi urządzeń i miejsca do magazynowania domu składniki majątku. 
- Użytkownik zasobów zawiera każda osoba, która ma uprawnienia dostępu do danych lub pliku. Przydział programu Access jest często delegowane przez właściciela w celu powiernik zawartości.  

### <a name="implementation"></a>Implementacja
  
Uwagi dotyczące zarządzania Zastosuj do wszystkich metod klasyfikacji. Następujące zagadnienia muszą zawierać szczegółowe informacje o tym, kto, co, gdzie, kiedy i dlaczego zbiór danych chcesz być używane, uzyskać do nich dostęp, zmianie lub usunięte. Wszystkie zarządzanie zasobami musi odbywać się opis sposobu organizacji widoków jego czynników ryzyka, ale mogą być stosowane metody proste, zgodnie z definicją w procesie klasyfikacji danych. Uwagi dodatkowe klasyfikacji danych obejmować wprowadzenie nowych aplikacji i narzędzi i zarządzanie zmianami po metoda klasyfikacji.  

### <a name="reclassification"></a>Podziału
 
Przeklasyfikowaniu lub zmiana stanu klasyfikacji zbiór danych należy wtedy, gdy użytkownik lub system Określa zmienił trwałego danych profilu ważność lub ryzyka. Ten nakładu jest ważne w celu zapewnienia, że stan Klasyfikacja jest nadal bieżące i prawidłowe. Większość zawartości, która nie została sklasyfikowana ręcznie można automatycznie klasyfikowane lub na podstawie zużycia przez powiernik danych lub właściciela danych. 

### <a name="manual-data-reclassification"></a>Dane ręcznego podziału
 
Najlepiej, jeśli ten nakładu może zapewnić szczegóły zmiany zostały przechwycone, inspekcji. Najbardziej prawdopodobną przyczyną ręcznego podziału jest ze względu na charakter lub rekordów na papierze lub wymaganie przeglądanie danych, który został pierwotnie nieprawidłowo klasyfikowana. Ponieważ w tym dokumencie są uwzględnione klasyfikacji danych i przenoszenie danych w chmurze, wysiłków ręcznego podziału wymaga uwagi na podstawie przez przypadek, a przeglądu zarządzania ryzykiem będzie idealne rozwiązanie do wymagań klasyfikacji adres. Ogólnie rzecz biorąc, według nakładu czy należy rozważyć, czy zasad organizacji o co powinien być sklasyfikowane domyślny stan klasyfikacji (wszystkie dane i plików poufnych, ale nie do poufnych), a następnie wykonaj wyjątki wysokiego ryzyka danych. 

### <a name="automatic-data-reclassification"></a>Automatyczne danych podziału
 
Automatyczne danych podziału używa tej samej reguły ogólne jako klasyfikacji ręcznego. Wyjątkiem jest to, że zautomatyzowanego rozwiązania można zapewnić obserwowane i zastosowane zgodnie z potrzebami reguły. Klasyfikacja danych jest możliwe w ramach klasyfikacji wymuszania zasad dotyczących danych, które można wymusić, jeśli dane są przechowywane w użyciu, a podczas przesyłania przy użyciu technologii autoryzacji.

- Oparte na aplikacji. Za pomocą niektórych aplikacji domyślnie ustawia poziom klasyfikacji. Na przykład dane z oprogramowanie zarządzania (klientami CRM) relacjami z klientami, HR i narzędzia do zarządzania rekordami kondycji są poufne domyślnie. 
- Opartych na lokalizacji. Lokalizacja danych ułatwiają identyfikowanie charakter danych. Na przykład dane, które są przechowywane przez godzinę lub działu finansowe jest częściej charakter poufny.  
 
### <a name="data-retention-recovery-and-disposal"></a>Przechowywanie danych, odzyskiwania i usuwania 

Odzyskiwanie danych i usuwania, takich jak podziału danych jest aspektem zarządzanie zasobami danych. Zasady odzyskiwanie danych i usuwania czy zdefiniowanych przez zasad przechowywania danych i wykonywane w taki sam sposób jak podziału danych; według nakładu może być wykonywane przez ról powiernik i administrator jako zadania wspólnej.  

Brak zasad przechowywania danych może oznaczać utratą danych lub niepowodzenie zgodnie z wymaganiami odnajdowania przepisami i prawnych. Większość organizacji, które nie mają zasad przechowywania danych jasno zdefiniowane zwykle użyć domyślnych zasad przechowywania "zachowywanie wszystkich". Zasady przechowywania ma jednak dodatkowe ryzyka w chmurze scenariusze usług. 

Na przykład zasad przechowywania danych dla dostawcy usług w chmurze można uznać jak "obowiązywania subskrypcji" (o ile usługę zapłacono dla dane są zachowywane). Umowa płatności do przechowywania nie może dotyczyć zasady przechowywania firmy lub przepisami. Definiowanie zasad dotyczących danych poufnych można upewnij się, że dane są przechowywane i usunięte oparte na temat najlepszych rozwiązań. Ponadto można tworzyć zasad archiwizacji w celu sformalizować zrozumienia dane, które powinny być usuwane i kiedy. 

Zasad przechowywania danych zwracają wymaganych przepisami i wymagania dotyczące zgodności, a także wymagań przechowywania prawne firmy. Sklasyfikowanych danych może powodować pytania na temat przechowywania czas trwania i wyjątki danych, który został zapisany z dostawcą; w większości przypadków dla danych, które nie zostały poprawnie sklasyfikowane są takie kwestie. 

> [AZURE.TIP] Dowiedz się więcej o zasady przechowywania danych Azure i bardziej, czytając [Umowy subskrypcji usługi Online firmy Microsoft](https://azure.microsoft.com/support/legal/subscription-agreement/)

## <a name="protecting-confidential-data"></a>Ochrona danych poufnych
  
Dane są sklasyfikowane, znajdowanie i implementowania sposoby ochrony danych poufnych stanie się integralną częścią dowolnej strategię wdrożenia ochrony danych. Ochrona danych poufnych wymaga dodatkowych uwagę na sposób przechowywania danych i przekazywane w konwencjonalny architektury, a także w chmurze. 

Ta sekcja zawiera podstawowe informacje o niektórych technologii automatyzujących wymuszania działań, na przykład w celu ochrony danych, które zostało sklasyfikowane jako poufne. 
 
Jak pokazano na poniższym rysunku, te technologie mogą być rozmieszczone jako lokalnego lub rozwiązań opartych na chmurze — lub w sposób hybrydowego z niektóre z nich wdrożeniu lokalnym i niektóre w chmurze. (Niektóre technologie, takie jak szyfrowanie i Zarządzanie prawami obejmować urządzenia użytkownika.)  

![Technologie](./media/azure-security-data-classification/azure-security-data-classification-fig4.png)

### <a name="rights-management-software"></a>Oprogramowanie zarządzania prawami dostępu  

Zapobieganie utracie danych jednego rozwiązaniem jest oprogramowanie zarządzania prawami. W przeciwieństwie do metod, które próbują przerwać przepływ informacji w punktach wyjścia w organizacji oprogramowanie zarządzania prawami działa na poziomach głębokości w ramach technologii miejsca do magazynowania danych. Dokumenty są szyfrowane i kontrolować, kto może je odszyfrować używa kontroli dostępu, zdefiniowane w rozwiązanie sterowania uwierzytelniania, takich jak usługa katalogowa.  

> [AZURE.TIP] za pomocą usługi Azure Rights Management (Azure RMS) jako rozwiązania ochrony informacji do ochrony danych w różnych scenariuszach. Przeczytaj [Co to jest usługi Azure Rights Management?](https://docs.microsoft.com/rights-management/understand-explore/what-is-azure-rms) uzyskać więcej informacji na temat tego rozwiązania Azure.

Oto niektóre z zalet oprogramowanie zarządzania prawami dostępu: 

- Chronionym informacji poufnych. Użytkowników możesz chronić swoje dane bezpośrednio przy użyciu aplikacji obsługujących zarządzanie prawami. Żadne dodatkowe czynności są wymagane — tworzenie dokumentów, wysyłanie wiadomości e-mail oraz publikowania danych oferują obsługi ochrony spójnych danych. 
- Ochrona przechodzi z danymi. Klienci pozostają kontrolę nad kto ma dostęp do swoich danych, czy w chmurze, istniejącej infrastruktury informatycznej lub na komputerze użytkownika. Organizacje mogą być szyfrować dane, a także ograniczać dostęp zgodnie z ich wymagań. 
- Domyślne zasady ochrony informacji. Administratorów i użytkowników, można użyć standardowego zasad dla wielu typowe scenariusze biznesowe, takie jak "Firmy poufne — tylko do odczytu" i "Nie przekazuj." Bogatego zestawu zastosowania prawa są obsługiwane, takich jak Odczyt, kopiowanie, drukowanie, zapisywanie edycji i przesyłanie dalej, aby umożliwić elastyczność podczas definiowania niestandardowe prawa użytkowania. 

> [AZURE.TIP] dane w magazynie Azure można chronić przy użyciu [Szyfrowanie usługi Azure miejsca do magazynowania](../storage/storage-service-encryption.md) danych w pozostałych. Za pomocą [Szyfrowania dysku Azure](azure-security-disk-encryption.md) chronić danych znajdujących się na dyskach wirtualnych używane dla maszyn wirtualnych Azure.

### <a name="encryption-gateways"></a>Szyfrowanie bram

Szyfrowanie bram działać w własne warstwy do świadczenia usług szyfrowania przez przekierowanie dostęp do danych w chmurze. Ta metoda nie należy mylić z tym wirtualną sieć prywatną (VPN). Szyfrowanie bramy umożliwia przezroczystości warstwy do rozwiązania oparte na chmurze.   

Bram szyfrowania można udostępnić sposoby zarządzania i bezpiecznego dane, które zostało sklasyfikowane jako informacje poufne, ponieważ do szyfrowania danych podczas przesyłania, a także dane spoczynku.  
 
Bram szyfrowania są umieszczane w przepływie danych między urządzeniami użytkownika i dane aplikacji centra świadczenie usług szyfrowania i odszyfrowywania. Te rozwiązania, takie jak sieci VPN, znajdują się głównie rozwiązania lokalnego. Są one przeznaczone do udostępnić kontrolę nad klucze szyfrowania, co pomaga zmniejszyć ryzyko wprowadzania danych i klucza zarządzanie przy użyciu jednego dostawcę innej firmy. Takiego rozwiązania są zaprojektowane, podobnie jak szyfrowania współpracuje i przezroczysty między użytkownikami a usługą. 

> [AZURE.TIP] Azure ExpressRoute umożliwia rozszerzanie sieci lokalnej w chmurze firmy Microsoft przez dedykowane połączenie prywatne. Aby uzyskać więcej informacji na temat tej funkcji, przeczytaj [Omówienie kwestii technicznych ExpressRoute](../expressroute/expressroute-introduction.md) . Inny opcje krzyżowe lokalnej łączność między sieci lokalnej i [Azure jest VPN witryny do witryny](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md).

### <a name="data-loss-prevention"></a>Zapobieganie utracie danych 
Utracie danych (nazywane wycieku danych) jest ważne, a zapobiegania utracie danych zewnętrznych za pomocą złośliwego i przypadkowe wewnętrznych jest najważniejsze dla wielu organizacjach.  
 
Technologie zapobieganie (DLP) utracie danych ułatwiają, upewnij się, że rozwiązań, takich jak usługi poczty e-mail nie wysyłaj dane, które zostało sklasyfikowane jako poufne. Organizacje mogą korzystać z funkcji DLP w istniejących produktów, aby zapobiec utracie danych. Zasady, które można łatwo tworzyć od podstaw lub przy użyciu szablonu podanego przez dostawcę oprogramowania za pomocą takich funkcji.  
 
Technologie DLP mogą wykonywać szczegółowej analizy zawartości za pośrednictwem dopasowania słów kluczowych, dopasowania słownika obliczeń przy użyciu wyrażeń regularnych i innych zawartości badania wykrywanie zawartości, która narusza zasad DLP organizacyjnych. Na przykład DLP może pomóc w uniknięciu utratę następujące typy danych: 

- Ubezpieczenia społecznego i numery identyfikacyjne krajowe 
- Informacje dotyczące banku 
- Numery kart kredytowych  
- Adresy IP 

Niektóre technologie DLP zapewniają również możliwość pominięcia konfiguracji DLP (na przykład, jeśli organizacja wymaga przekazywania informacji numer PESEL procesor płac). Ponadto użytkownik może skonfigurować DLP, tak aby użytkownicy będą powiadamiani przed próbą nawet do wysyłania informacji poufnych, które nie powinny być przekazywane. 

> [AZURE.TIP] za pomocą usługi Office 365 DLP możliwości ochrony dokumentów. Przeczytaj [kontrolek zgodności usługi Office 365: ochrona przed utratą danych](https://blogs.office.com/2013/10/28/office-365-compliance-controls-data-loss-prevention/) uzyskać więcej informacji.

## <a name="see-also"></a>Zobacz też

- [Najważniejsze wskazówki szyfrowania danych Azure](azure-security-data-encryption-best-practices.md)
- [Najważniejsze wskazówki dotyczące zabezpieczeń sterowania Azure Zarządzanie tożsamościami i access](azure-security-identity-management-best-practices.md)
- [Blog zespołu Azure zabezpieczeń](http://blogs.msdn.com/b/azuresecurity/)
- [Centrum zabezpieczeń firmy Microsoft](https://technet.microsoft.com/library/dn440717.aspx)

