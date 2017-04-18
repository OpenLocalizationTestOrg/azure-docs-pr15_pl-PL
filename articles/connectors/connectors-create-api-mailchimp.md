<properties
pageTitle="MailChimp | Microsoft Azure"
description="Tworzenie aplikacji logika z usługą Azure aplikacji. MailChimp jest usługa władz akredytacji bezpieczeństwa, która umożliwia firmom zautomatyzować działań marketingowych wiadomości e-mail, w tym wysyłanie marketingowych wiadomości e-mail, wiadomości automatyczną i docelowej kampanii i zarządzanie nimi."
services="logic-apps"
documentationCenter=".net,nodejs,java"  
authors="msftman"
manager="erikre"
editor=""
tags="connectors" />

<tags
ms.service="logic-apps"
ms.devlang="multiple"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="integration"
ms.date="08/18/2016"
ms.author="deonhe"/>

# <a name="get-started-with-the-mailchimp-connector"></a>Wprowadzenie do łącznika MailChimp

MailChimp jest usługa władz akredytacji bezpieczeństwa, która umożliwia firmom zautomatyzować działań marketingowych wiadomości e-mail, w tym wysyłanie marketingowych wiadomości e-mail, wiadomości automatyczną i docelowej kampanii i zarządzanie nimi.


>[AZURE.NOTE] Tą wersją artykułu dotyczy wersji schematu 2015-08-01-podgląd aplikacji logicznych.

Można rozpocząć pracę, tworząc aplikację logiczny teraz, zobacz [Tworzenie aplikacji logicznych](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Wyzwalacze i akcje

Łącznik MailChimp może być używany jako akcji; ma trigger(s). Wszystkie łączniki obsługuje danych w formatach XML i JSON.

 Łącznik MailChimp występują następujące akcje i/lub trigger(s) dostępne:

### <a name="mailchimp-actions"></a>Akcje MailChimp
Możesz wykonać następujące akcje:

|Akcja|Opis|
|--- | ---|
|[newcampaign](connectors-create-api-mailchimp.md#newcampaign)|Utwórz nową kampanię oparty na typie kampanii ustawienia kampanii (wiersz tematu, tytuł, from_name i reply_to) i lista adresatów|
|[NewList](connectors-create-api-mailchimp.md#newlist)|Tworzenie nowej listy na swoim koncie MailChimp|
|[Dodawanie członka](connectors-create-api-mailchimp.md#addmember)|Dodaj lub zaktualizuj członka listy|
|[Usuwanie członka](connectors-create-api-mailchimp.md#removemember)|Usuwanie członka z listy.|
|[updatemember](connectors-create-api-mailchimp.md#updatemember)|Aktualizowanie informacji o określonej listy elementów członkowskich|
### <a name="mailchimp-triggers"></a>MailChimp wyzwalaczy
Można wykrywać te zdarzenia:

|Wyzwalacza | Opis|
|--- | ---|
|Kiedy członek został dodany do listy|Uaktywnia przepływ pracy został dodany nowy element członkowski do listy|
|Po utworzeniu nowej listy|Nowa lista zostanie utworzona uaktywnia przepływu pracy|


## <a name="create-a-connection-to-mailchimp"></a>Tworzenie połączenia z MailChimp
Aby utworzyć logiki aplikacji z MailChimp, należy najpierw utworzyć **połączenie** następnie należy podać dane dla następujących właściwości:

|Właściwość| Wymagane|Opis|
| ---|---|---|
|Tokenu|Tak|Poświadczenia MailChimp|

>[AZURE.INCLUDE [Steps to create a connection to MailChimp](../../includes/connectors-create-api-mailchimp.md)]

>[AZURE.TIP] To połączenie jest używane w innych aplikacjach logika.

## <a name="reference-for-mailchimp"></a>Odwołanie do MailChimp
Dotyczy wersji: 1.0

## <a name="newcampaign"></a>newcampaign
Nowej kampanii: Tworzenie nowej kampanii oparty na typie kampanii ustawienia kampanii (wiersz tematu, tytuł, from_name i reply_to) i lista adresatów

```POST: /campaigns```

| Nazwa| Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|newCampaignRequest| |tak|Treść|Brak|Obiekt JSON do wysłania w treści z parametrami nowe żądanie kampanii|

#### <a name="response"></a>Odpowiedź

|Nazwa|Opis|
|---|---|
|200|Ok|
|400|Nieprawidłowe żądanie|
|401|Brak autoryzacji|
|403|Dostęp zabroniony|
|404|Nie można odnaleźć|
|500|Wewnętrzny błąd serwera. Wystąpił nieznany błąd|
|domyślne|Operacja nie powiodła się.|


## <a name="newlist"></a>NewList
Nowa lista: Tworzenie nowej listy na swoim koncie MailChimp

```POST: /lists```

| Nazwa| Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|newListRequest| |tak|Treść|Brak|Obiekt JSON do wysłania w treści z parametrami nowe żądanie kampanii|

#### <a name="response"></a>Odpowiedź

|Nazwa|Opis|
|---|---|
|200|Ok|
|400|Nieprawidłowe żądanie|
|401|Brak autoryzacji|
|403|Dostęp zabroniony|
|404|Nie można odnaleźć|
|500|Wewnętrzny błąd serwera. Wystąpił nieznany błąd|
|domyślne|Operacja nie powiodła się.|


## <a name="addmember"></a>Dodawanie członka
Dodawanie członka do listy: Dodaj lub zaktualizuj członka listy

```POST: /lists/{list_id}/members```

| Nazwa| Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|list_id|ciąg|tak|Ścieżka|Brak|Unikatowy identyfikator listy|
|newMemberInList| |tak|Treść|Brak|Obiekt JSON do wysłania w treści przy użyciu nowych informacji członka|

#### <a name="response"></a>Odpowiedź

|Nazwa|Opis|
|---|---|
|200|Ok|
|400|Nieprawidłowe żądanie|
|401|Brak autoryzacji|
|403|Dostęp zabroniony|
|404|Nie można odnaleźć|
|500|Wewnętrzny błąd serwera. Wystąpił nieznany błąd|
|domyślne|Operacja nie powiodła się.|


## <a name="removemember"></a>Usuwanie członka
Usuwanie członka z listy: usuwanie członka z listy.

```DELETE: /lists/replacemailwithhash/{list_id}/members/{member_email}```

| Nazwa| Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|list_id|ciąg|tak|Ścieżka|Brak|Unikatowy identyfikator listy|
|member_email|ciąg|tak|Ścieżka|Brak|Adres e-mail uczestnika do usunięcia|

#### <a name="response"></a>Odpowiedź

|Nazwa|Opis|
|---|---|
|200|Ok|
|400|Nieprawidłowe żądanie|
|401|Brak autoryzacji|
|403|Dostęp zabroniony|
|404|Nie można odnaleźć|
|500|Wewnętrzny błąd serwera. Wystąpił nieznany błąd|
|domyślne|Operacja nie powiodła się.|


## <a name="updatemember"></a>updatemember
Zaktualizuj informacje o użytkowniku: aktualizowanie informacji o określonej listy elementów członkowskich

```PATCH: /lists/replacemailwithhash/{list_id}/members/{member_email}```

| Nazwa| Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|list_id|ciąg|tak|Ścieżka|Brak|Unikatowy identyfikator listy|
|member_email|ciąg|tak|Ścieżka|Brak|Adres e-mail unikatowe członka, aby zaktualizować|
|updateMemberInListRequest| |tak|Treść|Brak|Obiekt JSON do wysłania w treści z informacjami o zaktualizowanych członka|

#### <a name="response"></a>Odpowiedź

|Nazwa|Opis|
|---|---|
|200|Ok|
|400|Nieprawidłowe żądanie|
|401|Brak autoryzacji|
|403|Dostęp zabroniony|
|404|Nie można odnaleźć|
|500|Wewnętrzny błąd serwera. Wystąpił nieznany błąd|
|domyślne|Operacja nie powiodła się.|


## <a name="onmembersubscribed"></a>OnMemberSubscribed
Kiedy członek został dodany do listy: uruchamia przepływ pracy, gdy nowy element członkowski został dodany do listy

```GET: /trigger/lists/{list_id}/members```

| Nazwa| Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|list_id|ciąg|tak|Ścieżka|Brak|Unikatowy identyfikator listy|

#### <a name="response"></a>Odpowiedź

|Nazwa|Opis|
|---|---|
|200|Ok|
|202|Zaakceptowane|
|400|Nieprawidłowe żądanie|
|401|Brak autoryzacji|
|403|Dostęp zabroniony|
|404|Nie można odnaleźć|
|500|Wewnętrzny błąd serwera. Wystąpił nieznany błąd|
|domyślne|Operacja nie powiodła się.|


## <a name="oncreatelist"></a>OnCreateList
Po utworzeniu nowej listy: wyzwalane przepływ pracy po utworzeniu nowej listy

```GET: /trigger/lists```

Nie ma żadnych parametrów dla tego połączenia
#### <a name="response"></a>Odpowiedź

|Nazwa|Opis|
|---|---|
|200|Ok|
|202|Zaakceptowane|
|400|Nieprawidłowe żądanie|
|401|Brak autoryzacji|
|403|Dostęp zabroniony|
|404|Nie można odnaleźć|
|500|Wewnętrzny błąd serwera. Wystąpił nieznany błąd|
|domyślne|Operacja nie powiodła się.|


## <a name="object-definitions"></a>Definicje obiektów

### <a name="newcampaignrequest"></a>NewCampaignRequest


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|Typ|ciąg|Tak |
|Adresaci|nie zdefiniowano|Tak |
|Ustawienia|nie zdefiniowano|Tak |
|variate_settings|nie zdefiniowano|Brak |
|Śledzenie|nie zdefiniowano|Brak |
|rss_opts|nie zdefiniowano|Brak |
|social_card|nie zdefiniowano|Brak |



### <a name="recipient"></a>Adresat


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|list_id|ciąg|Tak |
|segment_opts|nie zdefiniowano|Brak |



### <a name="settings"></a>Ustawienia


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|subject_line|ciąg|Tak |
|Tytuł|ciąg|Brak |
|from_name|ciąg|Tak |
|reply_to|ciąg|Tak |
|use_conversation|wartość logiczna|Brak |
|to_name|ciąg|Brak |
|folder_id|Liczba całkowita|Brak |
|uwierzytelnianie|wartość logiczna|Brak |
|auto_footer|wartość logiczna|Brak |
|inline_css|wartość logiczna|Brak |
|auto_tweet|wartość logiczna|Brak |
|auto_fb_post|Tablica|Brak |
|fb_comments|wartość logiczna|Brak |



### <a name="variatesettings"></a>Variate_Settings


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|winner_criteria|ciąg|Brak |
|czas_oczekiwania|Liczba całkowita|Brak |
|test_size|Liczba całkowita|Brak |
|subject_lines|Tablica|Brak |
|send_times|Tablica|Brak |
|from_names|Tablica|Brak |
|reply_to_addresses|Tablica|Brak |



### <a name="tracking"></a>Śledzenie


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|zostanie otwarta|wartość logiczna|Brak |
|html_clicks|wartość logiczna|Brak |
|text_clicks|wartość logiczna|Brak |
|goal_tracking|wartość logiczna|Brak |
|ecomm360|wartość logiczna|Brak |
|google_analytics|ciąg|Brak |
|clicktale|ciąg|Brak |
|usługi SalesForce|nie zdefiniowano|Brak |
|highrise|nie zdefiniowano|Brak |
|kapsułka|nie zdefiniowano|Brak |



### <a name="rssopts"></a>RSS_Opts


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|feed_url|ciąg|Brak |
|częstość|ciąg|Brak |
|constrain_rss_img|ciąg|Brak |
|Harmonogram|nie zdefiniowano|Brak |



### <a name="socialcard"></a>Social_Card


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|image_url|ciąg|Brak |
|Opis|ciąg|Brak |
|Tytuł|ciąg|Brak |



### <a name="segmentopts"></a>Segment_Opts


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|saved_segment_id|Liczba całkowita|Brak |
|Uwzględnij|ciąg|Brak |



### <a name="salesforce"></a>Usługi SalesForce


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|kampanii|wartość logiczna|Brak |
|notatki|wartość logiczna|Brak |



### <a name="highrise"></a>Highrise


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|kampanii|wartość logiczna|Brak |
|notatki|wartość logiczna|Brak |



### <a name="capsule"></a>Kapsułka


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|notatki|wartość logiczna|Brak |



### <a name="schedule"></a>Harmonogram


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|Godzina|Liczba całkowita|Brak |
|daily_send|nie zdefiniowano|Brak |
|weekly_send_day|ciąg|Brak |
|monthly_send_date|Liczba|Brak |



### <a name="dailysend"></a>Daily_Send


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|niedziela|wartość logiczna|Brak |
|poniedziałek|wartość logiczna|Brak |
|Wtorek|wartość logiczna|Brak |
|Środa|wartość logiczna|Brak |
|czwartek|wartość logiczna|Brak |
|piątek|wartość logiczna|Brak |
|sobota|wartość logiczna|Brak |



### <a name="campaignresponsemodel"></a>CampaignResponseModel


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|Identyfikator|ciąg|Brak |
|Typ|ciąg|Brak |
|create_time|ciąg|Brak |
|archive_url|ciąg|Brak |
|Stan|ciąg|Brak |
|emails_sent|Liczba całkowita|Brak |
|send_time|ciąg|Brak |
|CONTENT_TYPE|ciąg|Brak |
|Adresat|Tablica|Brak |
|Ustawienia|nie zdefiniowano|Brak |
|variate_settings|nie zdefiniowano|Brak |
|Śledzenie|nie zdefiniowano|Brak |
|rss_opts|nie zdefiniowano|Brak |
|ab_split_opts|nie zdefiniowano|Brak |
|social_card|nie zdefiniowano|Brak |
|report_summary|nie zdefiniowano|Brak |
|delivery_status|nie zdefiniowano|Brak |
|_links|Tablica|Brak |



### <a name="absplitopts"></a>AB_Split_Opts


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|split_test|ciąg|Brak |
|pick_winner|ciąg|Brak |
|wait_units|ciąg|Brak |
|czas_oczekiwania|Liczba całkowita|Brak |
|split_size|Liczba całkowita|Brak |
|from_name_a|ciąg|Brak |
|from_name_b|ciąg|Brak |
|reply_email_a|ciąg|Brak |
|reply_email_b|ciąg|Brak |
|subject_a|ciąg|Brak |
|subject_b|ciąg|Brak |
|send_time_a|ciąg|Brak |
|send_time_b|ciąg|Brak |
|send_time_winner|ciąg|Brak |



### <a name="reportsummary"></a>Report_Summary


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|zostanie otwarta|Liczba całkowita|Brak |
|unique_opens|Liczba całkowita|Brak |
|open_rate|Liczba|Brak |
|kliknięcia|Liczba całkowita|Brak |
|subscriber_clicks|Liczba|Brak |
|click_rate|Liczba|Brak |



### <a name="deliverystatus"></a>Delivery_Status


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|włączone|wartość logiczna|Brak |
|can_cancel|wartość logiczna|Brak |
|Stan|ciąg|Brak |
|emails_sent|Liczba całkowita|Brak |
|emails_canceled|Liczba całkowita|Brak |



### <a name="link"></a>Łącze


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|ReL|ciąg|Brak |
|Odwołanie|ciąg|Brak |
|Metoda|ciąg|Brak |
|targetSchema|ciąg|Brak |
|schematu|ciąg|Brak |



### <a name="newlistrequest"></a>NewListRequest


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|Nazwa|ciąg|Tak |
|Skontaktuj się z|nie zdefiniowano|Tak |
|permission_reminder|ciąg|Tak |
|use_archive_bar|wartość logiczna|Brak |
|campaign_defaults|nie zdefiniowano|Tak |
|notify_on_subscribe|ciąg|Brak |
|notify_on_unsubscribe|ciąg|Brak |
|email_type_option|wartość logiczna|Tak |
|widoczność|ciąg|Brak |



### <a name="contact"></a>Kontakt


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|Firma|ciąg|Tak |
|Adres1|ciąg|Tak |
|2|ciąg|Brak |
|Miasto|ciąg|Tak |
|Województwo|ciąg|Tak |
|ZIP|ciąg|Tak |
|kraj|ciąg|Tak |
|Telefon|ciąg|Tak |



### <a name="campaigndefaults"></a>Campaign_Defaults


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|from_name|ciąg|Tak |
|from_email|ciąg|Tak |
|Temat|ciąg|Brak |
|język|ciąg|Tak |



### <a name="createnewlistresponsemodel"></a>CreateNewListResponseModel


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|Identyfikator|ciąg|Tak |
|Nazwa|ciąg|Tak |
|Skontaktuj się z|nie zdefiniowano|Tak |
|permission_reminder|ciąg|Tak |
|use_archive_bar|wartość logiczna|Brak |
|campaign_defaults|nie zdefiniowano|Tak |
|notify_on_subscribe|ciąg|Brak |
|notify_on_unsubscribe|ciąg|Brak |
|date_created|ciąg|Brak |
|list_rating|Liczba całkowita|Brak |
|email_type_option|wartość logiczna|Tak |
|subscribe_url_short|ciąg|Brak |
|subscribe_url_long|ciąg|Brak |
|beamer_address|ciąg|Brak |
|widoczność|ciąg|Brak |
|Moduły|Tablica|Brak |
|statystyki|nie zdefiniowano|Brak |
|_links|Tablica|Brak |



### <a name="stats"></a>Statystyki


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|member_count|Liczba całkowita|Brak |
|unsubscribe_count|Liczba całkowita|Brak |
|cleaned_count|Liczba całkowita|Brak |
|member_count_since_send|Liczba całkowita|Brak |
|unsubscribe_count_since_send|Liczba całkowita|Brak |
|cleaned_count_since_send|Liczba całkowita|Brak |
|campaign_count|Liczba całkowita|Brak |
|campaign_last_sent|Liczba całkowita|Brak |
|merge_field_count|Liczba całkowita|Brak |
|avg_sub_rate|Liczba|Brak |
|avg_unsub_rate|Liczba|Brak |
|target_sub_rate|Liczba|Brak |
|open_rate|Liczba|Brak |
|click_rate|Liczba|Brak |
|last_sub_date|ciąg|Brak |
|last_unsub_date|ciąg|Brak |



### <a name="getlistsresponsemodel"></a>GetListsResponseModel


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|list|Tablica|Brak |
|total_items|Liczba całkowita|Brak |



### <a name="newmemberinlistrequest"></a>NewMemberInListRequest


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|email_type|ciąg|Brak |
|Stan|ciąg|Tak |
|merge_fields|nie zdefiniowano|Brak |
|zainteresowania|ciąg|Brak |
|język|ciąg|Brak |
|VIP|wartość logiczna|Brak |
|Lokalizacja|nie zdefiniowano|Brak |
|adres_email|ciąg|Tak |



### <a name="firstandlastname"></a>FirstAndLastName


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|FNAME|ciąg|Brak |
|LNAME|ciąg|Brak |



### <a name="location"></a>Lokalizacja


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|szerokości|Liczba|Brak |
|długość geograficzna|Liczba|Brak |



### <a name="memberresponsemodel"></a>MemberResponseModel


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|Identyfikator|ciąg|Brak |
|adres_email|ciąg|Brak |
|unique_email_id|ciąg|Brak |
|email_type|ciąg|Brak |
|Stan|ciąg|Brak |
|merge_fields|nie zdefiniowano|Brak |
|zainteresowania|ciąg|Brak |
|statystyki|nie zdefiniowano|Brak |
|ip_signup|ciąg|Brak |
|timestamp_signup|ciąg|Brak |
|ip_opt|ciąg|Brak |
|timestamp_opt|ciąg|Brak |
|member_rating|Liczba całkowita|Brak |
|last_changed|ciąg|Brak |
|język|ciąg|Brak |
|VIP|wartość logiczna|Brak |
|email_client|ciąg|Brak |
|Lokalizacja|nie zdefiniowano|Brak |
|last_note|nie zdefiniowano|Brak |
|list_id|ciąg|Brak |
|_links|Tablica|Brak |



### <a name="lastnote"></a>Last_Note


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|note_id|Liczba całkowita|Brak |
|created_at|ciąg|Brak |
|created_by|ciąg|Brak |
|Uwaga|ciąg|Brak |



### <a name="getallmembersresponsemodel"></a>GetAllMembersResponseModel


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|elementy członkowskie|Tablica|Brak |
|list_id|ciąg|Brak |
|total_items|Liczba całkowita|Brak |



### <a name="object"></a>Obiekt






### <a name="updatememberinlistrequest"></a>UpdateMemberInListRequest


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|adres_email|ciąg|Brak |
|email_type|ciąg|Brak |
|Stan|ciąg|Tak |
|merge_fields|nie zdefiniowano|Brak |
|zainteresowania|ciąg|Brak |
|język|ciąg|Brak |
|VIP|wartość logiczna|Brak |
|Lokalizacja|nie zdefiniowano|Brak |



### <a name="getmembersresponsemodel"></a>GetMembersResponseModel


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|elementy członkowskie|Tablica|Brak |
|list_id|ciąg|Brak |
|total_items|Liczba całkowita|Brak |



### <a name="adduserresponsemodel"></a>AddUserResponseModel


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|Identyfikator|ciąg|Tak |
|adres_email|ciąg|Tak |
|unique_email_id|ciąg|Brak |
|email_type|ciąg|Brak |
|Stan|ciąg|Brak |
|merge_fields|nie zdefiniowano|Tak |
|zainteresowania|ciąg|Brak |
|statystyki|nie zdefiniowano|Brak |
|ip_signup|ciąg|Brak |
|timestamp_signup|ciąg|Brak |
|ip_opt|ciąg|Brak |
|timestamp_opt|ciąg|Brak |
|member_rating|Liczba całkowita|Brak |
|last_changed|ciąg|Brak |
|język|ciąg|Brak |
|VIP|wartość logiczna|Brak |
|email_client|ciąg|Brak |
|Lokalizacja|nie zdefiniowano|Brak |
|last_note|nie zdefiniowano|Brak |
|list_id|ciąg|Brak |
|_links|Tablica|Brak |


## <a name="next-steps"></a>Następne kroki
[Tworzenie aplikacji warunków logicznych](../app-service-logic/app-service-logic-create-a-logic-app.md)
