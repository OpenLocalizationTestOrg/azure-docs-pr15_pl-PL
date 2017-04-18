<properties
    pageTitle="Azure serwera w porównaniu z chmury MFA | Microsoft Azure"
    description="Wybierz pozycję uwierzytelnianie wieloskładnikowe secutiry rozwiązanie w prawo, aby użytkownik, oraz jakie am i próby zabezpieczania i gdzie są moje użytkowników znajduje się.  Następnie wybierz pozycję chmurki, serwer MFA lub usług AD FS."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="yossib"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/14/2016"
    ms.author="kgremban"/>

#<a name="choose-the-azure-multi-factor-authentication-solution-for-you"></a>Wybierz rozwiązanie uwierzytelnianie wieloskładnikowe Azure

Ponieważ nie istnieją podtypy kilku z Azure uwierzytelnianie wieloskładnikowe (MFA), firma Microsoft musi odpowiedzieć na kilka pytań ustalanie wersję, która jest pisane z wielkiej litery korzystać.  Dostępne są następujące zagadnienia:

-   [Co to jest uznany za bezpiecznego](#what-am-i-trying-to-secure)
-   [Gdzie znajdują się użytkowników](#where-are-the-users-located)
- [Jakie funkcje potrzebują?](#what-featured-do-i-need)

W poniższych sekcjach przedstawiono wskazówki dotyczące ustalania każdej z tych odpowiedzi.

## <a name="what-am-i-trying-to-secure"></a>Co to jest uznany za bezpiecznego?

Aby określić rozwiązanie Weryfikacja dwuetapowa poprawne, najpierw firma Microsoft musi odpowiedzi na pytanie: co możesz próby zabezpieczania z drugą metodę uwierzytelniania.  Jest to aplikacja, która znajduje się w Azure?  Dostęp zdalny do systemu lub?  Określając, co możemy próbujesz bezpiecznego, firma Microsoft odpowiedzi na pytanie, gdzie uwierzytelnianie wieloskładnikowe musi być włączona.  


Co to są próbujesz bezpiecznego| Uwierzytelnianie wieloskładnikowe w chmurze|Uwierzytelnianie wieloskładnikowe serwera
------------- | :-------------: | :-------------: |
Aplikacje firmy Microsoft|● |● |
Aplikacje władz akredytacji bezpieczeństwa w galerii aplikacji|● |● |
Są publikowane w usłudze Azure AD aplikacji Proxy aplikacji usług IIS|● |● |
Nie są publikowane w usłudze Azure AD aplikacji Proxy aplikacji usług IIS | |● |
Dostęp zdalny, takich jak sieci VPN, RDG| |● |



## <a name="where-are-the-users-located"></a>Gdzie znajdują się użytkowników

Następnie spojrzenie na miejsce, w którym znajdują się naszych użytkowników pozwala określić właściwe rozwiązanie problemu, aby użyć, w chmurze lub przy użyciu serwera MFA lokalnego.



Lokalizacja użytkownika| Uwierzytelnianie wieloskładnikowe w chmurze|Uwierzytelnianie wieloskładnikowe serwera
------------- | :-------------: | :-------------: |
Azure Active Directory|● | |
Azure AD i lokalnych AD Federacji za pomocą usług AD FS|● |● |
Azure AD i lokalnych AD przy użyciu narzędzia DirSync, Azure AD Sync Azure AD Connect — nie synchronizacji haseł|● |● |
Azure AD i lokalnych AD przy użyciu narzędzia DirSync, Azure AD Sync Azure AD Connect - z synchronizacji haseł|● | |
W lokalnej usłudze Active Directory| |● |

## <a name="what-features-do-i-need"></a>Jakie funkcje potrzebują?

Poniższa tabela jest porównanie funkcji, które są dostępne z uwierzytelnianie wieloskładnikowe w chmurze i serwer uwierzytelniania wieloskładnikowego.

 | Uwierzytelnianie wieloskładnikowe w chmurze | Uwierzytelnianie wieloskładnikowe serwera
------------- | :-------------: | :-------------: |
Powiadomienie o aplikacji dla urządzeń przenośnych jako drugi czynnik | ● | ● |
Kod weryfikacyjny aplikacji dla urządzeń przenośnych jako drugi czynnik | ● | ●
Rozmowa telefoniczna jako drugi czynniki | ● | ●
Jednokierunkowe SMS jako drugi czynniki | ● | ●
Dwukierunkowa SMS jako drugi czynniki |  | ●
Tokeny sprzętowe jako drugi czynniki |  | ●
Hasła aplikacji dla klientów, które nie obsługują MFA | ● |  
Administrator kontrolę nad metod uwierzytelniania | ● | ●
Tryb numeru PIN |  | ●
Alert oszustwa | ● | ●
Raporty MFA | ● | ●
Obejście jednorazowego |  | ●
Niestandardowe powitania dla połączenia telefoniczne | ● | ●
Identyfikator rozmówcy można dostosowywać dla połączenia telefoniczne | ● | ●
Adresy IP zaufanych | ● | ●
Pamiętaj MFA zaufanych urządzeń  | ● |  
Dostępu warunkowego | ● | ●
Pamięć podręczna |  | ●

Teraz, gdy mamy ustaleniu, czy mają być używane uwierzytelnianie wieloskładnikowe chmury lub MFA lokalnego serwera, firma Microsoft może rozpoczęcie konfigurowania i uwierzytelnianie wieloskładnikowe Azure. **Wybierz ikonę, która reprezentuje rozwiązania!**

<center>




[![Cloud](./media/multi-factor-authentication-get-started/cloud2.png)](multi-factor-authentication-get-started-cloud.md)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[![Proofup](./media/multi-factor-authentication-get-started/server2.png)](multi-factor-authentication-get-started-server.md)  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
</center>
