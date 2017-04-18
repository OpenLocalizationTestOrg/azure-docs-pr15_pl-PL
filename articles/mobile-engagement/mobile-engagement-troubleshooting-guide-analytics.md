<properties 
   pageTitle="Azure zaangażowania urządzeń przenośnych Podręcznik — analizy rozwiązywania problemów" 
   description="Rozwiązywanie problemów z analizy, monitorowanie segmentacji i pulpitu nawigacyjnego w zaangażowania Mobile Azure" 
   services="mobile-engagement" 
   documentationCenter="" 
   authors="piyushjo" 
   manager="dwrede" 
   editor=""/>

<tags
   ms.service="mobile-engagement"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="mobile-multiple"
   ms.workload="mobile" 
   ms.date="08/19/2016"
   ms.author="piyushjo"/>

# <a name="troubleshooting-guide-for-analytics-monitoring-segmentation-and-dashboard-issues"></a>Rozwiązywanie problemów z przewodnik w sprawie analizy, monitorowanie segmentacji i pulpitu nawigacyjnego

Poniżej przedstawiono możliwe problemy mogą się pojawić w sposób zaangażowania Mobile Azure zbiera dane dotyczące aplikacji, urządzeń i użytkowników.

## <a name="missingdelayed-information"></a>Informacje o braku opóźnione

### <a name="issue"></a>Problem
- Informacje o jest opóźnione w wyświetlane w analizy, segmentacji lub pulpitu nawigacyjnego.
- Monitorowanie brakuje informacji.
- Brakuje informacji analizy, segmentacji lub pulpitu nawigacyjnego.
- Naciśnięcie segmentacji ograniczenia.

### <a name="causes"></a>Powoduje, że

- Korzystając z interfejsu API analizy Monitor API, i interfejsu API segmentów identyczność jakichkolwiek danych możesz brakuje interfejsu użytkownika jest widoczny za pośrednictwem interfejsów API.
- Jeśli SDK zaangażowania Mobile Azure nie jest poprawnie zintegrowany aplikacji nie będą mogli wyświetlanie informacji w analizy, segmentacji, monitorowanie i pulpity nawigacyjne.
- Segmenty nie można zmienić po ich utworzeniu, segmenty można tylko "Sklonowany" (skopiowanie) lub "zniszczone" (usunięte). Segmenty może zawierać tylko 10 kryteriów.
- Najlepszym sposobem sprawdzenia brakujące informacje z monitorowania jest konfiguracji testowanie urządzenia, odinstaluj i/lub ponownie zainstaluj aplikację na testowanie urządzenia.
- Informacje są odświeżane co 24 godziny, analizy, segmentacji lub pulpitów nawigacyjnych.
- Informacje w nowych segmentów mogą nie być wyświetlane do 24 godzin po ich utworzeniu, nawet jeśli segmentu jest oparty na poprzednich informacji.
- Filtrowanie danych analizy w interfejsie użytkownika będą widoczne we wszystkich przykładach tego typu niezależnie od wersji aplikacji (np. "awarii" filtrowane według nazwy zostanie wyświetlona w wersji 1 i 2 aplikacji).
- Termin analizy zależy od daty od użytkowników ustawień urządzenia, dzięki czemu użytkownika, którego telefon ma nieprawidłowe daty można wyświetlane w danym okresie problem.
- Nie po stronie serwera, które dane są rejestrowane, korzystając z przycisku próba "" umieszcza, dane tylko są rejestrowane rzeczywistą wypychanych kampanii.

## <a name="cant-locate-items-in-ui"></a>Nie można odnaleźć elementów w interfejsie użytkownika

### <a name="issue"></a>Problem
- Nie można utworzyć segmenty oparte na niektórych wbudowana lub informacje o aplikacji niestandardowej znakowanie kryteria.
- Nie można odnaleźć niektórych wbudowane i informacje o aplikacji niestandardowej znakowanie kryteriów w analizy, monitorowanie i pulpity nawigacyjne.
- Nie może zinterpretować dane w analizy, monitorowanie, segmentacji lub pulpitów nawigacyjnych.

### <a name="causes"></a>Powoduje, że

- Wbudowana niektórych elementów i znaczników informacje o aplikacji są dostępne jako kryteriów wypychanych tylko, ale mogą nie zostać dodane do segmentu widoczne analizy, monitorowanie i pulpitu nawigacyjnego. 
- Wbudowanych elementów i znaczników informacje o aplikacji, których nie można dodawać do segmentu należy do listy konfiguracji kierowanie kryteriów w każdej kampanii pełnią tę samą funkcję, jak według segmentu.
- Zobacz menu kontekstowe w sekcjach analizy, monitorowanie segmentacji i pulpity nawigacyjne zaangażowania Mobile Azure, elementy interfejsu użytkownika, aby uzyskać dodatkową pomoc i informacje.

## <a name="crash-troubleshooting"></a>Ulec awarii Rozwiązywanie problemów

### <a name="issue"></a>Problem
- Powoduje awarię aplikacji, znajdujących się w analizy, monitorowanie lub pulpitu nawigacyjnego.

### <a name="causes"></a>Powoduje, że

- Rozwiązywać problemy z aplikacji ulega awarii w analizy, monitorowanie lub pulpitu nawigacyjnego upewnij się sprawdzić informacje o znanych problemach z poprzednich wersji zestawu SDK wersji.
- Aby rozwiązać awarii aplikacji, należy wykonać wydarzenia z testowanie urządzenia z zainstalowaną aplikacją i wyszukiwanie swój identyfikator urządzenia, w sekcji "Monitorowania — zdarzeń" interfejsu użytkownika zaangażowania Mobile Azure. Następnie wykonać zdarzenia, które powoduje aplikacji ulec awarii i wyszukiwanie dodatkowych informacji w sekcji "Monitor — awarii" interfejsu użytkownika zaangażowania Mobile Azure. 

 
