<properties
    pageTitle="Azure wprowadzenie magazynu klucza stos | Microsoft Azure"
    description="Dowiedz się, jak Azure stos klucza magazynu zarządza klucze i hasła"
    services="azure-stack"
    documentationCenter=""
    authors="rlfmendes"
    manager="natmack"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/26/2016"
    ms.author="ricardom"/>

# <a name="introduction-to-key-vault-in-azure-stack"></a>Wprowadzenie do klucza magazynu w stos Azure #

## <a name="before-you-start"></a>Przed rozpoczęciem

W tym artykule przyjęto założenie, następujące czynności:

- Czytnik ma dostęp do subskrypcji, która ma Azure klucza magazynu włączone.
- Zestaw SDK programu PowerShell Azure jest skonfigurowanych.
- Dla wersji TP2 wszystkie operacje skierowaną dzierżawy mogą być wykonywane tylko z programu PowerShell.

## <a name="key-vault-basics"></a>Klucz magazynu — informacje podstawowe

Klucz magazynu w stos Azure pomaga chronić klucze szyfrowania i za pomocą haseł, które aplikacji i usług w chmurze. Przy użyciu klucza magazynu, można zaszyfrować klucze i hasła (na przykład kluczy uwierzytelniania, klawiszy konta miejsca do magazynowania, klucze szyfrowania danych, pliki PFX i hasła).

Klucz magazynu upraszcza proces zarządzania kluczami i pozwala zachować kontrolę klawiaturowych, których dostępu i szyfrowania danych. Deweloperów można Utwórz klawisze służące do projektowania i testowania w minutach, a następnie bezproblemowo migrowanie ich do produkcji klawiszy. Administratorzy zabezpieczeń może udzielić (i odwoływanie) uprawnienia do klawiszy, stosownie do potrzeb.

Każda z subskrypcją usługi Azure stos tworzenie i używanie magazynami najważniejszych. Mimo że klucz magazynu korzyści deweloperów i administratorów zabezpieczeń, można zaimplementowana i zarządzane przez administratora, który zarządza inne usługi Azure stosu dla organizacji. Na przykład ten administrator może Zaloguj się przy użyciu subskrypcji usługi Azure stos utworzyć magazynu dla organizacji, w której chcesz przechowywania kluczy, a następnie jest odpowiedzialny za tych zadań operacyjnych:

- Tworzenie lub importowanie klucz lub hasło
- Odwoływanie lub usunąć klucz lub hasło
- Zezwolić użytkownikom i aplikacjom uzyskiwanie dostępu do klucza magazynu, aby mogli następnie zarządzanie lub wykorzystywania klucze i hasła
- Konfigurowanie użycia klucza (na przykład podpisywania lub szyfrowania)

Ten administrator można udostępnia deweloperów identyfikatory URI, aby nawiązać połączenie z ich aplikacji i podaj administratora zabezpieczeń informacji rejestrowania użycia klucza.

Deweloperów można również zarządzać klucze bezpośrednio przy użyciu interfejsów API. Aby uzyskać więcej informacji zobacz Przewodnik programisty klucza magazynu.

## <a name="scenarios"></a>Scenariusze

Poniższa tabela przedstawia scenariuszy, gdzie klucza magazynu mogą pomóc potrzeb deweloperów i administratorów zabezpieczeń:


### <a name="developer-for-an-azure-stack-application"></a>Deweloper aplikacji stos Azure

**Problem**: Chcę, aby zapisać aplikację dla stosu Azure, który używa kluczy podpisywania i szyfrowania, ale chcę ich zewnętrznych z mojej aplikacji tak, aby rozwiązanie nadaje się do aplikacji, która jest rozproszone geograficznie.

**Instrukcja**: klawisze są przechowywane w magazynu i wywoływane przez identyfikator URI w razie potrzeby.


### <a name="developer-for-software-as-a-service-saas"></a>Deweloper oprogramowania jako usługa (SaaS)

**Problemu:** Nie chcesz odpowiedzialności ani zobowiązań potencjalnych kluczy dzierżawy Moi klienci i hasła.

**Instrukcja:** Klientów można zaimportować własne klucze stos Azure i zarządzać nimi. Chcę klientom własności i zarządzanie nimi klucze, tak aby I może skupić się na tym, co zrobić, najlepiej, która udostępnia podstawowe funkcje oprogramowania.


### <a name="chief-security-officer-cso"></a>Dyrektor zabezpieczeń (CSO)

**Problemu:** Chcę upewnić się, że mojej organizacji jest w kontrolce klucza cyklu życia i można monitorować użycia klucza.

**Instrukcja** Klucz magazynu została zaprojektowana tak, dzięki czemu Microsoft Zobacz lub nie Wyodrębnianie kluczy.  Jeśli aplikacji będzie potrzebny do wykonywania operacji szyfrowania za pomocą klawiszy klientów, magazynu klucz powinien się tym zająć imieniu aplikacji. Aplikacja nie widzi klawiszy klientów.  Mimo że firma Microsoft korzysta z wielu usług Azure stos i zasobów, chcę zarządzać kluczami z jednej lokalizacji w stos Azure. Magazyn zawiera jeden interfejs, niezależnie od tego, ile magazynami masz w stos Azure, regiony ich pomocy technicznej i aplikacji, które z nich korzystać.

## <a name="next-steps"></a>Następne kroki

[Rozpoczynanie pracy z magazynu klucza](azure-stack-kv-getting-started.md)
