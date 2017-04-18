
<properties
    pageTitle="Rozwiązywanie problemów z dynamiczne członkostwa grup | Microsoft Azure"
    description="Porady dotyczące rozwiązywania problemów dla dynamiczne członkostwa dla grupy w Azure AD."
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""
    />

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="curtand"/>


# <a name="troubleshooting-dynamic-memberships-for-groups"></a>Rozwiązywanie problemów z dynamicznego członkostwa dla grup

**Po skonfigurowaniu reguły w grupie, ale zaktualizowane nie członkostwa w grupie**<br/>Upewnij się, że ustawienie **Włącz delegowane zarządzania grupy** jest ustawiona na **Tak** na karcie **Konfiguruj** . To ustawienie zostanie wyświetlony tylko wtedy, gdy jest zalogowany jako użytkownik, któremu przypisano licencji usługi Azure Active Directory Premium. Sprawdź wartości atrybutów użytkownika w regule: istnieją użytkownicy, które spełniają regułę?

**Po skonfigurowaniu reguły, ale teraz usunąć członków istniejącej reguły**<br/>Jest to oczekiwane zachowanie. Istniejący członkowie grupy zostaną usunięte, gdy reguła jest włączona lub zmienione. Użytkownicy zwrócone przez oceny reguły są dodawane jako członków do grupy.     

**Nie widzę, że natychmiast po Dodawanie lub zmienianie reguły, dlaczego nie zmienia się członkostwo?**<br/>Oceny członkostwa w dedykowanej odbywa się okresowo w tle asynchroniczne. Jak długo trwa proces zależy od liczby użytkowników w katalogu i rozmiar grupy utworzonych w wyniku reguły. Zazwyczaj katalogów w małej liczby użytkownicy widzą zmian członkostwa w grupach mniej niż kilka minut. Katalogi z dużą liczbą użytkowników może potrwać 30 minut lub dłużej do wypełnienia.

Te artykuły zawiera dodatkowe informacje dotyczące usługi Azure Active Directory.

* [Zarządzanie dostępem do zasobów z grupami usługi Azure Active Directory](active-directory-manage-groups.md)
* [Artykuł indeks do zarządzania aplikacjami w usłudze Azure Active Directory](active-directory-apps-index.md)
* [Co to jest Azure Active Directory?](active-directory-whatis.md)
* [Integrowanie tożsamości lokalnych z usługi Azure Active Directory](active-directory-aadconnect.md)
