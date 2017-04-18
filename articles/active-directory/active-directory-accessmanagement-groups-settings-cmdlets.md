<properties
    pageTitle="Azure cmdlet usługi Active Directory do konfigurowania ustawień grupy | Microsoft Azure"
    description="Jak zarządzać ustawieniami dla grup przy użyciu poleceń cmdlet usługi Azure Active Directory."
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/22/2016"
    ms.author="curtand"/>


# <a name="azure-active-directory-cmdlets-for-configuring-group-settings"></a>Azure cmdlet usługi Active Directory do konfigurowania ustawień grupy

Można skonfigurować następujące ustawienia dla grupy ujednolicony w katalogu:

1.  Klasyfikacje: rozdzielaną średnikami listę kategorii, które użytkownicy mogą konfigurować w grupie. Przykłady będzie "Formacie", "Tajny" i "Ściśle tajne".

2.  Adres URL wskazówki dotyczące użycia: adres URL wskazywanego przez użytkowników z warunkami użytkowania używania Unified grup określone przez organizację. Ten adres URL pojawi się w interfejsie użytkownika, w której użytkownicy używanie grup.

3.  Grupowanie tworzenia włączone: czy brak, niektórych lub wszystkich użytkowników mogą tworzyć Unified grupy. Jeśli skonfigurowano na, wszyscy użytkownicy mogą tworzyć grupy. Gdy jest ustawiony na wyłączone, użytkownicy nie można utworzyć grupy. Gdy wyłączone, można również określić grupy zabezpieczeń, w której użytkownicy nadal mogą tworzyć grupy.

Te ustawienia są skonfigurowane przy użyciu ustawień i SettingsTemplate obiektów. Początkowo są niewidoczne obiekty ustawienia w katalogu. Oznacza to, że skonfigurowano katalogu z ustawieniami domyślnymi. Aby zmienić ustawienia domyślne, użytkownik utworzy nowy obiekt ustawienia przy użyciu szablonu ustawienia. Szablony ustawienia są definiowane przez firmę Microsoft.

Moduł zawierający cmdlet używane dla tych operacji z [witryny Microsoft Connect](http://connect.microsoft.com/site1164/Downloads/DownloadDetails.aspx?DownloadID=59185)może zostać pobrana.

## <a name="create-settings-at-the-directory-level"></a>Tworzenie ustawień na poziomie katalogu

Te kroki Tworzenie ustawień na poziomie katalogu, które dotyczą wszystkich grup pakietu Office w katalogu.

1. Jeśli nie wiesz, które SettingTemplate używać, to polecenie cmdlet zwraca listę szablonów ustawienia:

    `Get-MsolAllSettingTemplate`

    ![Lista szablonów ustawienia](./media/active-directory-accessmanagement-groups-settings-cmdlets/list-of-templates.png)

2. Aby dodać adres URL wytyczne dotyczące użycia, należy najpierw pobrać obiektu SettingsTemplate, który definiuje wartość adres URL wytyczne zastosowania; oznacza to, że szablon Group.Unified:

    `$template = Get-MsolSettingTemplate –TemplateId 62375ab9-6b52-47ed-826b-58e47e0e304b`

3. Następnie utwórz nowy obiekt Ustawienia oparty na tym szablonie:

    `$setting = $template.CreateSettingsObject()`

4. Następnie zaktualizuj wartości wytyczne zastosowania:

    `$setting["UsageGuidelinesUrl"] = "<https://guideline.com>"`

5. Na koniec zastosować ustawienia:

    `New-MsolSettings –SettingsObject $setting`

    ![Dodaj adres URL wytyczne dotyczące użycia](./media/active-directory-accessmanagement-groups-settings-cmdlets/add-usage-guideline-url.png)

Poniżej przedstawiono z ustawieniami określonymi w Group.Unified SettingsTemplate.

 **Ustawienie**                          | **Opis**                                                                                             
--------------------------------------|-----------------------------------------------
 <ul><li>ClassificationList<li>Typ: ciąg<li>Wartość domyślna: ""                  | Plik rozdzielany przecinkami lista klasyfikacji prawidłowe wartości, które można stosować do grup Unified.                
 <ul><li>EnableGroupCreation<li>Typ: logiczna<li>Domyślne: PRAWDA              | Flaga wskazująca, czy możliwości tworzenia grup ujednolicony jest dozwolone w katalogu.                               
 <ul><li>GroupCreationAllowedGroupId<li>Typ: ciąg<li>Wartość domyślna: ""         | Identyfikator GUID grupy zabezpieczeń, która będzie mógł tworzyć grupy Unified nawet wtedy, gdy EnableGroupCreation == false.
 <ul><li>UsageGuidelinesUrl<li>Typ: ciąg<li>Wartość domyślna: ""                  | Łącze do wytyczne korzystania z grupy.                                                                       

## <a name="read-settings-at-the-directory-level"></a>Więcej ustawień na poziomie katalogu

Te kroki odczytać ustawień na poziomie katalogu, które dotyczą wszystkich grup pakietu Office w katalogu.

1. Czytanie wszystkich istniejących ustawień katalogu:

    `Get-MsolAllSettings`

2. Przeczytaj wszystkie ustawienia dla konkretnej grupy:

    `Get-MsolAllSettings -TargetType Groups -TargetObjectId <groupObjectId>`

3. Przeczytaj określonego katalogu ustawienia przy użyciu SettingId GUID:

    `Get-MsolSettings –SettingId dbbcb0ea-a6ff-4b44-a1f3-9d7cef74984c`

    ![Ustawienia identyfikator GUID](./media/active-directory-accessmanagement-groups-settings-cmdlets/settings-id-guid.png)

## <a name="update-settings-at-the-directory-level"></a>Aktualizowanie ustawień na poziomie katalogu

Poniższe czynności, zaktualizuj ustawienia na poziomie katalogu, które dotyczą wszystkich grup pakietu Office w katalogu.

1. Pobierz istniejący obiekt ustawienia:

    `$setting = Get-MsolSettings –SettingId dbbcb0ea-a6ff-4b44-a1f3-9d7cef74984c`

2. Pobierz wartość, którą chcesz zaktualizować:

    `$value = $Setting.GetSettingsValue()`

3. Aktualizowanie wartości:

    `$value["AllowToAddGuests"] = "false"`

4. Zaktualizuj ustawienia:

    `Set-MsolSettings –SettingId dbbcb0ea-a6ff-4b44-a1f3-9d7cef74984c –SettingsValue $value`

## <a name="remove-settings-at-the-directory-level"></a>Usuwanie ustawień na poziomie katalogu

W tym kroku usuwa ustawienia na poziomie katalogu, które dotyczą wszystkich grup pakietu Office w katalogu.

    `Remove-MsolSettings –SettingId dbbcb0ea-a6ff-4b44-a1f3-9d7cef74984c`

## <a name="cmdlet-syntax-reference"></a>Informacje dotyczące poleceń cmdlet składni

Więcej dokumentację Azure Active Directory programu PowerShell można znaleźć w [Poleceń cmdlet Azure Active Directory](http://go.microsoft.com/fwlink/p/?LinkId=808260).

## <a name="settingstemplate-object-reference-groupunified-settingstemplate-object"></a>Odwołanie do obiektu SettingsTemplate (Group.Unified SettingsTemplate obiekt)

- "Nazwa": "EnableGroupCreation", "typ": "Typu System.Boolean", "wartość domyślna": "prawda", "opis": "Flagę logiczną wskazującą, czy funkcji tworzenia grupy ujednolicony przebiega zgodnie z."

- "Nazwa": "GroupCreationAllowedGroupId", "typ": "System.Guid", "wartość domyślna": "", "opis": "Identyfikator GUID grupy zabezpieczeń, który jest whitelisted do tworzenia grup ujednolicony."

- "Nazwa": "ClassificationList", "typ": "System.String", "wartość domyślna": "", "opis": "Rozdzielany przecinkami lista klasyfikacji prawidłowe wartości, które można stosować do grup ujednolicony."

- "Nazwa": "UsageGuidelinesUrl", "typ": "System.String", "wartość domyślna": "", "opis": "Łącze z wytycznymi zastosowania grupy."

Nazwa | Typ | Wartość domyślna | Opis
----------  | ----------  | ---------  | ----------
"EnableGroupCreation"  | Typu "System.Boolean"  | wartość "prawda"  | "Flagę logiczną wskazującą, czy funkcji tworzenia grupy ujednolicony przebiega zgodnie z."
"GroupCreationAllowedGroupId"  | "System.Guid"  | ""  | "Identyfikator GUID grupy zabezpieczeń, który jest whitelisted do tworzenia grup Unified".
"ClassificationList"  | "System.String"  | ""  | "Rozdzielany przecinkami lista klasyfikacji prawidłowe wartości, które można stosować do grup Unified."
"UsageGuidelinesUrl"  | "System.String"  | ""  | "Łącze z wytycznymi zastosowania grupy."

## <a name="next-steps"></a>Następne kroki

Więcej dokumentację Azure Active Directory programu PowerShell można znaleźć w [Poleceń cmdlet Azure Active Directory](http://go.microsoft.com/fwlink/p/?LinkId=808260).

Dodatkowe instrukcje z Menedżera programów Microsoft Jong de Piotr jest dostępna w [Blogu grup Piotr firmy](http://robsgroupsblog.com/blog/configuring-settings-for-office-365-groups-in-azure-ad).

* [Zarządzanie dostępem do zasobów z grupami usługi Azure Active Directory](active-directory-manage-groups.md)

* [Integrowanie tożsamości lokalnych z usługi Azure Active Directory](active-directory-aadconnect.md)
