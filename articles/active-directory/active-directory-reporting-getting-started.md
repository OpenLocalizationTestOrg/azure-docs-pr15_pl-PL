<properties
   pageTitle="Azure Active Directory raportowania: Wprowadzenie | Microsoft Azure"
   description="Wyświetlanie listy różnych dostępnych raportów w raportach usługi Azure Active Directory"
   services="active-directory"
   documentationCenter=""
   authors="dhanyahk"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="03/07/2016"
   ms.author="dhanyahk"/>

# <a name="getting-started-with-azure-active-directory-reporting"></a>Wprowadzenie do programu Azure Active Directory raportowania

## <a name="what-it-is"></a>Co to jest

Azure Active Directory (Azure AD) zawiera zabezpieczeń, działania i raporty inspekcji dla katalogu. Poniżej przedstawiono listę dostępnych raportów:

### <a name="security-reports"></a>Raporty dotyczące zabezpieczeń

- Dodatki logowania z nieznanych źródeł
- Dodatki logowania po wielu błędów
- Dodatki logowania z wielu obszarów geograficznych
- Dodatki logowania z adresów IP przy użyciu podejrzane działania
- Nieprawidłowe działanie logowania
- Dodatki znak z prawdopodobnie zakażonych urządzeń
- Użytkownicy z działaniem anomalous logowania

### <a name="activity-reports"></a>Raporty aktywności

- Użycie aplikacji: Podsumowanie
- Użycie aplikacji: szczegółowe
- Pulpit nawigacyjny aplikacji
- Konto błędy inicjowania obsługi administracyjnej
- Urządzenia poszczególnych użytkowników
- Poszczególnych użytkowników aktywności
- Raport aktywności grup
- Raport aktywności rejestracji resetowania hasła
- Działania do resetowania hasła

### <a name="audit-reports"></a>Raporty inspekcji

- Raport inspekcji katalogu

> [AZURE.TIP] Więcej dokumentacji na raportowanie Azure AD zapoznaj się z [Wyświetlanie raportów dostępu i użytkowania](active-directory-view-access-usage-reports.md).



## <a name="how-it-works"></a>Jak to działa


### <a name="reporting-pipeline"></a>Potok raportowania

Proces raportowania składa się z trzech głównych kroków. Każdorazowo użytkownik rejestruje lub wykonany uwierzytelniania, są następujące akcje:

- Najpierw uwierzytelnieniu użytkownika (pomyślnie lub pomyślnie), a wynik jest przechowywany w bazach danych usługi Azure Active Directory.
- W regularnych odstępach, wszystkie ostatnie logowania dodatki są przetwarzane. W tym momencie bezpieczeństwa i algorytmy aktywności anomalous wyszukiwania wszystkich logowania ostatnie dodatki dla podejrzane działania.
- Po przetworzeniu raporty są zapisywane, przechowywanych w pamięci podręcznej, a następnie w portalu klasyczny Azure.

### <a name="report-generation-times"></a>Raportowanie czasu generowania

Z powodu dużej liczby uwierzytelnienia i zaloguj dodatki przetwarzanych przez platformy Azure AD, ostatnio rejestrowaniu przetwarzane są, średnio o jedną godzinę. Czasami może potrwać do 8 godzin przetwarzania najnowsze dodatki logowania.

Sprawdzając tekstu pomocy w górnej części każdy raport można znaleźć ostatnio przetworzone logowania.

![Tekst pomocy w górnej części każdej raportu](./media/active-directory-reporting-getting-started/reportingWatermark.PNG)

> [AZURE.TIP] Więcej dokumentacji na raportowanie Azure AD zapoznaj się z [Wyświetlanie raportów dostępu i użytkowania](active-directory-view-access-usage-reports.md).



## <a name="getting-started"></a>Wprowadzenie


### <a name="sign-into-the-azure-classic-portal"></a>Zaloguj się do portalu klasyczny Azure

Najpierw musisz zalogować się do [portal Azure klasyczny](https://manage.windowsazure.com) jako globalny lub administrator zgodności. Możesz również musi być administratorem usługi Azure subskrypcji lub Współtworzenie lub za pomocą "Dostęp do Azure AD" Azure subskrypcji.

### <a name="navigate-to-reports"></a>Przejdź do raportów

Wyświetlanie raportów, przejdź do karty raporty w górnej części katalogu.

Jeśli jest używany po raz pierwszy wyświetlania raportów, musisz zgadza się okno dialogowe, aby można było wyświetlać raporty. To, aby upewnić się, że jest dopuszczalne dla administratorów w Twojej organizacji, aby wyświetlić te dane, które można uznać prywatne informacje w niektórych krajach.

![Okno dialogowe](./media/active-directory-reporting-getting-started/dialogBox.png)

### <a name="explore-each-report"></a>Eksplorowanie każdy raport

Przejdź do każdego raport, aby wyświetlić dane są zbierane i przetwarzane dodatki logowania. Można znaleźć [listę wszystkich raportów w tym miejscu](active-directory-reporting-guide.md).

![Wszystkie raporty](./media/active-directory-reporting-getting-started/reportsMain.png)

### <a name="download-the-reports-as-csv"></a>Pobieranie raportów w postaci CSV

Każdy raport można pobrać jako pliku CSV (wartości rozdzielane przecinkami). Możesz użyć tych plików w programie Excel, PowerBI lub analizy innych programów do dalszej analizy danych.

Aby pobrać każdy raport jako CSV, przejdź do raportu i kliknij przycisk "Pobierz" u dołu.

![Przycisk pobierania](./media/active-directory-reporting-getting-started/downloadButton.png)

> [AZURE.TIP] Więcej dokumentacji na raportowanie Azure AD zapoznaj się z [Wyświetlanie raportów dostępu i użytkowania](active-directory-view-access-usage-reports.md).





## <a name="next-steps"></a>Następne kroki

### <a name="customize-alerts-for-anomalous-sign-in-activity"></a>Dostosowywanie alertów dla anomalous logowania w działaniu

Przejdź do karty "Konfigurowanie" katalogu.

Przewiń do sekcji "Powiadomienia".

Włączanie lub wyłączanie sekcji "Wyślij pocztą E-mail powiadomienia o rejestrowaniu Anomalous".

![W sekcji powiadomienia](./media/active-directory-reporting-getting-started/notificationsSection.png)

### <a name="integrate-with-the-azure-ad-reporting-api"></a>Integracja z usługą Azure Active Directory raportowania interfejsu API

Zobacz [wprowadzenie z interfejsem API raportów](active-directory-reporting-api-getting-started.md).

### <a name="engage-multi-factor-authentication-on-users"></a>Prowadzenie uwierzytelnianie wieloskładnikowe na użytkowników

Wybierz użytkownika w raporcie.

Kliknij przycisk "Włącz MFA" u dołu ekranu.

![Uwierzytelnianie wieloskładnikowe u dołu ekranu](./media/active-directory-reporting-getting-started/mfaButton.png)

> [AZURE.TIP] Więcej dokumentacji na raportowanie Azure AD zapoznaj się z [Wyświetlanie raportów dostępu i użytkowania](active-directory-view-access-usage-reports.md).




## <a name="learn-more"></a>Dowiedz się więcej


### <a name="audit-events"></a>Zdarzenia inspekcji

Dowiedz się więcej o tym, które zdarzenia są inspekcji w katalogu w [Azure Active Directory raportowania inspekcji zdarzeń](active-directory-reporting-audit-events.md).

### <a name="api-integration"></a>Interfejs API integracji

Zobacz [wprowadzenie z interfejsem API raportów](active-directory-reporting-api-getting-started.md) i [dokumentacji interfejsu API](https://msdn.microsoft.com/library/azure/mt126081.aspx).

### <a name="get-in-touch"></a>Kontaktowanie się

Wiadomości e-mail [aadreportinghelp@microsoft.com](mailto:aadreportinghelp@microsoft.com) opinii, pomocy lub może być pytania.

> [AZURE.TIP] Więcej dokumentacji na raportowanie Azure AD zapoznaj się z [Wyświetlanie raportów dostępu i użytkowania](active-directory-view-access-usage-reports.md).
