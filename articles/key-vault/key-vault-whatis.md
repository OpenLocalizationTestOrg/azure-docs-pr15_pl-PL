<properties
    pageTitle="Co to jest Azure klucza magazynu? | Microsoft Azure"
    description="Azure magazynu klucza ułatwia ochronę informacji klucze szyfrowania i hasła używane przez aplikacje w chmurze i usługi. Przy użyciu magazynu klucza Azure, klientów można zaszyfrować klucze i hasła (na przykład kluczy uwierzytelniania, klawiszy konta miejsca do magazynowania, klucze szyfrowania danych. Pliki PFX i hasła) za pomocą klawiszy, które są chronione moduły sprzętu (HSM)."
    services="key-vault"
    documentationCenter=""
    authors="cabailey"
    manager="mbaldwin"
    tags="azure-resource-manager"/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/10/2016"
    ms.author="cabailey"/>



# <a name="what-is-azure-key-vault"></a>Co to jest Azure klucza magazynu?

Azure magazynu klucza jest dostępna w większości regionów. Aby uzyskać więcej informacji zobacz [Klucz magazynu ceny strony](https://azure.microsoft.com/pricing/details/key-vault/).

## <a name="introduction"></a>Wprowadzenie

Azure magazynu klucza ułatwia ochronę informacji klucze szyfrowania i hasła używane przez aplikacje w chmurze i usługi. Przy użyciu klucza magazynu, można zaszyfrować klucze i hasła (na przykład kluczy uwierzytelniania, klawiszy konta miejsca do magazynowania, klucze szyfrowania danych. Pliki PFX i hasła) za pomocą klawiszy, które są chronione moduły sprzętu (HSM). W odniesieniu do zapewnienia dodanego można importować lub wygenerowania kluczy w HSM. Jeśli wybierzesz w tym celu procesów Microsoft kluczy w FIPS 140-2 poziomu 2 sprawdzanej HSM (sprzętu i oprogramowania układowego).  

Klucz magazynu upraszcza proces zarządzania kluczami i umożliwia zachowanie kontroli nad klawisze dostępu, które szyfrowania danych. Deweloperów można Utwórz klawisze służące do projektowania i testowania w minutach, a następnie bezproblemowo migrowanie ich do produkcji klawiszy. Administratorzy zabezpieczeń może udzielić (i odwoływanie) uprawnienia do klawiszy, stosownie do potrzeb.

Użyj poniższej tabeli, aby lepiej zrozumieć, jak klucza magazynu mogą ułatwić stosownie do potrzeb użytkowników i administratorów zabezpieczeń.





| Rola        | Opis problemu           | Rozwiązany przez Azure klucza magazynu  |
| ------------- |-------------|-----|
| Deweloper aplikacji Azure      | "Chcę, aby zapisać aplikację dla Azure używaną klawiszy na potrzeby podpisywania i szyfrowania, ale chcę, aby te klawisze, aby być zewnętrznych z mojej aplikacji, tak aby rozwiązanie nadaje się do aplikacji, która jest rozproszone geograficznie. <br/><br/>Mają także następujące klucze i hasła, które mają być chronione, bez konieczności pisania kodu, samodzielnie. Również mają następujące klucze i hasła, można łatwo dla mnie do używania z moich aplikacji z optymalną wydajność." | Klawisze √ są przechowywane w magazynu i wywoływane przez identyfikator URI w razie potrzeby.<br/><br/> Klucze √ są zabezpieczone przez Azure, przy użyciu standardowych algorytmów, długość klucza i moduły sprzętu (HSM).<br/><br/> Klucze √ są przetwarzane w HSM, które znajdują się w tym samym Azure centrach danych jako aplikacje. Zapewnia większą niezawodność i ograniczenie opóźnienie niż Jeśli klucze znajdują się w innej lokalizacji, takiej jak lokalnego.|
| Deweloper oprogramowania jako usługa (SaaS)      |"Nie chcę odpowiedzialności ani zobowiązań potencjalnych kluczy dzierżawy Moi klienci i hasła. <br/><br/>Chcę klientom własności i zarządzanie nimi klucze, tak aby I może skupić się na tym, co zrobić, najlepiej, która udostępnia podstawowe funkcje oprogramowania." | Klienci √ można importować własne klucze Azure i zarządzać nimi. Gdy aplikacja władz akredytacji bezpieczeństwa wymaga do wykonywania operacji szyfrowania za pomocą klawiszy swoich klientów, klucza magazynu czy te operacje w imieniu aplikacji. Aplikacja nie widzi klawiszy klientów.|
| Pełnomocnik ochrony główny (CSO) | "Chcę dowiedzieć się, że nasze aplikacje są zgodne z FIPS 140-2 poziom 2 HSM dla bezpiecznego zarządzania kluczami. <br/><br/>Chcę upewnić się, że mojej organizacji znajduje się w formancie klucza cyklu życia i można monitorować użycia klucza. <br/><br/>Mimo że firma Microsoft korzysta z wielu usług Azure i zasoby, chcę i zarządzać kluczami z jednej lokalizacji w Azure."     |√ HSM są FIPS 140-2 poziomu 2 sprawdzana poprawność.<br/><br/>√ Klucza magazynu dzięki temu Microsoft Zobacz lub nie Wyodrębnianie kluczy.<br/><br/>√ w czasie rzeczywistym rejestrowania użycia klucza.<br/><br/>√ Magazyn zawiera jeden interfejs, bez względu na to magazynami ile masz w Azure, regiony ich pomocy technicznej i aplikacji, które z nich korzystać. |


Każda z subskrypcją usługi Azure tworzenie i używanie magazynami najważniejszych. Mimo że klucz magazynu korzyści deweloperów i administratorów zabezpieczeń, może zaimplementowana i zarządzane przez administratora w organizacji, kto zarządza inne usługi Azure dla organizacji. Na przykład ten administrator może zalogować się przy użyciu subskrypcji usługi Azure utworzyć magazynu dla organizacji, w której chcesz przechowywania kluczy, a następnie za zadań operacyjnych, takich jak:

+ Tworzenie lub importowanie klucz lub hasło
+ Odwoływanie lub usunąć klucz lub hasło
+ Zezwolić użytkownikom i aplikacjom uzyskiwanie dostępu do klucza magazynu, aby mogli następnie zarządzanie lub wykorzystywania klucze i hasła
+ Konfigurowanie użycia klucza (na przykład podpisywania lub szyfrowania)
+ Monitorowanie użycia klucza

Ten administrator może następnie podaj deweloperów identyfikatory URI, aby nawiązać połączenie z ich aplikacji i podaj administratora zabezpieczeń informacji rejestrowania użycia klucza. 

   ![Omówienie Azure klucza magazynu][1]

Deweloperzy można również zarządzać klucze bezpośrednio przy użyciu interfejsów API. Aby uzyskać więcej informacji zobacz [Przewodnik programisty klucza magazynu](key-vault-developers-guide.md).

## <a name="next-steps"></a>Następne kroki

Pobieranie samouczek wprowadzenie przez administratora zobacz [Rozpoczynanie pracy z magazynu klucza Azure](key-vault-get-started.md).

Aby uzyskać więcej informacji o korzystaniu z rejestrowania dla magazynu klucza zobacz [Azure klucza magazynu rejestrowania](key-vault-logging.md).

Aby uzyskać więcej informacji o korzystaniu z magazynu klucza Azure klucze i hasła zobacz [temat kluczy, certyfikaty i hasła](https://msdn.microsoft.com/library/azure/dn903623\(v=azure.1\).aspx).


<!--Image references-->
[1]: ./media/key-vault-whatis/AzureKeyVault_overview.png
