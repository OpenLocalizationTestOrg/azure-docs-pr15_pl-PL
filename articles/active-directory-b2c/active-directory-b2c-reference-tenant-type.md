<properties
    pageTitle="Azure Active Directory B2C: Dzierżaw skali produkcji a Podgląd B2C | Microsoft Azure"
    description="Temat typy Azure Active Directory B2C dzierżaw"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/30/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-production-scale-vs-preview-b2c-tenants"></a>Azure Active Directory B2C: Dzierżaw skali produkcji a Podgląd B2C

Jeśli planujesz pisanie aplikacji produkcji na B2C usługi Azure Active Directory (Azure AD), musisz mieć pewność, że masz prawo dzierżawy "typ", aby przejść na żywo. Aby sprawdzić, jakie masz, wykonaj następujące kroki Przejdź [do karta funkcje B2C](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) portalu Azure i wygląd w obszarze **Typ dzierżawy**.

## <a name="summary"></a>Podsumowanie

Azure AD B2C obsługuje aplikacje produkcji tylko w **skali produkcji** B2C dzierżaw w Ameryce Północnej.

| Typ dzierżawy | Krajów/regionów | Ogólnie dostępne? |
| ----------- | -------------- | --------------------- |
| **Skala produkcji dzierżawy** | Ameryce Północnej krajów/regionów | Tak |
| **Skala produkcji dzierżawy** | Wszystkich krajów/regionów z wyjątkiem Ameryka Północna | Brak |
| **Podgląd dzierżawy** | Rekordy wszystkich krajów i regionów | Brak |

> [AZURE.NOTE]
Azure AD B2C dzierżaw (dla klientów indywidualnych) są obecnie niedostępne w kilku krajów lub regionów, których dzierżaw Azure AD (dla pracowników) są dostępne. W tym artykule poniższych sekcjach, aby uzyskać więcej informacji.

## <a name="production-scale-b2c-tenant-in-north-america"></a>Skala produkcji B2C dzierżawy w Ameryce Północnej

Jeśli [utworzona dzierżawy usługi B2C](active-directory-b2c-get-started.md) w Ameryce Północnej, to znaczy w jednym z następujących krajów i regionów: Zjednoczone Zjednoczone, Kanada, Kostaryka, Dominikana, Salwador, Gwatemala, Meksyk, Panama, Portoryko Trynidad i Tobago i **Typ dzierżawy** interfejsu użytkownika administratora B2C mówi **skali produkcji**, dzierżawy usługi można używać w przypadku aplikacji produkcji.

> [AZURE.NOTE]
Skala produkcji dzierżaw są w stanie skalowania 100s miliony tożsamości dla klientów indywidualnych na dzierżawcę.

![Zrzut ekranu przedstawiający dzierżawy skali produkcji](./media/active-directory-b2c-reference-tenant-type/production-scale-b2c-tenant.png)

## <a name="preview-b2c-tenant-in-any-countryregion"></a>Wyświetlanie podglądu B2C dzierżawy w dowolnym kraju/regionu

Jeśli utworzono dzierżawy B2C okresie Podgląd Azure AD B2C, prawdopodobnie Twojej **dzierżawy wpisz** komunikat **Podgląd dzierżawy**. Jeśli jest to możliwe, należy użyć dzierżawy usługi wyłącznie do opracowywania i testowania, a nie dla aplikacji produkcji.

> [AZURE.IMPORTANT]
Istnieje nie ścieżkę migracji z poziomu widoku Podgląd dzierżawy B2C do dzierżawy B2C skali produkcji. Należy zauważyć, że są znane problemy, usunięcie dzierżawy Podgląd B2C i ponownie utworzyć dzierżawy B2C skali produkcji z tą samą nazwą domeny. Musisz utworzyć dzierżawy B2C skali produkcji z inną nazwę domeny.

![Zrzut ekranu: Podgląd dzierżawy](./media/active-directory-b2c-reference-tenant-type/preview-b2c-tenant.png)

## <a name="production-scale-b2c-tenant-outside-of-north-america"></a>Skala produkcji B2C dzierżawy rejonów spoza Ameryki Północnej

Azure AD B2C nie jest obecnie-ogólnodostępne rejonów spoza Ameryki Północnej. Jednak można utworzyć i używać dzierżaw skali produkcji do projektowania i testowania celów, w jednym z następujących krajów i regionów: Algieria, Austria, Azerbejdżan, Bahrajn, Białoruś, Belgia, Bułgaria, Chorwacja, Cypr, Czechy, Dania, Egipt, Estonia, Finlandia, Francja, Niemcy, Grecja, Węgry, Islandia, Irlandia, Izrael, Włochy, Jordania, Kazachstan, Kenii, Kuwejt, Lativa, Liban, Liechtenstein, Lituania, Luksemburg, była Jugosłowiańska Republika Macedonii, Malta, Czarnogóra, Maroko, Holandia, Nigeria, Norwegia , Oman, Pakistan, Polska, Portugalia, Katar, Rumunia, Rosja, Arabia Saudyjska, Serbia, Słowacja, Słowenia, Republika Południowej Afryki, Hiszpania, Szwecja, Szwajcaria, Tunezja, Turcja, Ukraina, Zjednoczone Emiraty Arabskie i Zjednoczone Królestwo.

Po Azure AD B2C informuje o ogólnodostępną w powyżej krajów i regionów, można użyć tych dzierżaw skali produkcji i live przejdź z aplikacji produkcji bez utraty danych.

## <a name="availability-of-b2c-tenants"></a>Dostępność B2C dzierżaw

Dzierżaw B2C są obecnie niedostępne w następujących krajów i regionów: Afganistan, Argentyna, Australia, Brazylia, Chile, Kolumbia, Ekwador, Hongkong SAR, Indie, Indonesia, Irak, Japonia, Korea, Malezja, Nowa Zelandia, Paragwaj, Peru, Filipiny, Singapur, Sri Lanki, Tajwan, Thailand (ไทย), Urugwaj oraz Wenezuela. Planujemy w celu uwzględnienia ich w przyszłości.
