<properties
    pageTitle="Dostosowywanie mapowań atrybutów | Microsoft Azure"
    description="Dowiedz się, jakie mapowań atrybutów władz akredytacji bezpieczeństwa aplikacji usługi Azure Active Directory są, jak można dostosować je do rozwiązania potrzeb firmy."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="markusvi"/>


# <a name="customizing-attribute-mappings"></a>Dostosowywanie mapowania atrybutów


Microsoft Azure AD umożliwia przypisywanie użytkowników do innych firm władz akredytacji bezpieczeństwa aplikacji, takich jak usługi Salesforce, usługi Google Apps i innych osób. Jeśli masz przypisywanie użytkowników dla aplikacji władz akredytacji bezpieczeństwa włączone innej firmy, portalu zarządzania Azure określa jego atrybuty w formularzu konfiguracji o nazwie "mapowanie atrybutu".

Istnieje wstępnie zestaw atrybutów mapowań między obiektami użytkownika Azure AD i każdej aplikacji władz akredytacji bezpieczeństwa użytkownika. Niektóre aplikacje Zarządzanie innych typów obiektów, takich jak grupy lub kontakty. <br> 
Możesz dostosować domyślne mapowania atrybutu według potrzeb firmy. Oznacza to, możesz zmienić lub usunąć istniejące mapowania atrybut lub utworzyć nowe mapowania atrybutu.

W portalu Azure AD możesz korzystać z tej funkcji, klikając pozycję atrybuty na pasku narzędzi aplikacji władz akredytacji bezpieczeństwa.

> [AZURE.NOTE] Łącze **atrybuty** jest dostępna tylko jeśli masz włączone dla aplikacji władz akredytacji bezpieczeństwa przypisywanie użytkowników. 


![Usługi SalesForce][1] 


Po kliknięciu atrybuty na pasku narzędzi na liście bieżące mapowania skonfigurowane na potrzeby aplikacji władz akredytacji bezpieczeństwa.

Następujące zrzut ekranu przedstawia przykładową to:



![Usługi SalesForce][2]  


W powyższym przykładzie widać atrybut **Imię** zarządzanych obiektu w usług Salesforce zostanie wypełniona wartość **Imię** połączone Azure AD obiektu.

Jeśli albo chcesz dostosować mapowania atrybut lub jeśli chcesz przywrócić niestandardowych ustawień powrót do konfiguracji domyślnej, możesz to zrobić, klikając powiązanych przycisk na pasku narzędzi w dolnej części aplikacji.


![Usługi SalesForce][3]  


Istnieją mapowania atrybutów, które są wymagane przez aplikację władz akredytacji bezpieczeństwa działał poprawnie. W tabeli atrybutów mapowania atrybutów powiązanych **Tak** być jako wartość atrybutu **wymagane** . Jeśli wymagane jest mapowanie atrybutu, nie można usunąć. W tym przypadku **Usuwanie** funkcja jest niedostępna.

Aby zmodyfikować istniejące mapowanie atrybut, po prostu wybierz opcję mapowanie, a następnie kliknij przycisk **Edytuj**. Spowoduje to wyświetlenie strony okno dialogowe umożliwiające modyfikowanie mapowania zaznaczony atrybut.


![Edytowanie mapowanie atrybutu][4]  



## <a name="understanding-attribute-mapping-types"></a>Opis mapowania typów atrybutów


Z mapowania atrybutów sterowania jak atrybuty są wypełniane w innej strony władz akredytacji bezpieczeństwa aplikacji. Istnieją cztery różnych typów mapowania obsługiwane:

- **Bezpośrednie** — atrybut docelowy zostanie wypełniona wartość atrybutu obiektu połączonego w Azure AD.


- **Stała** — atrybut docelowy znajduje się określonym ciągiem znaków, wprowadzonych.


- **Wyrażenie** — atrybut docelowy jest wypełnione na podstawie wyniku wyrażenia skrypt podobne. Aby uzyskać więcej informacji zobacz [Pisania wyrażeń do mapowania atrybutów w usłudze Azure Active Directory](active-directory-saas-writing-expressions-for-attribute-mappings.md).


- **Brak** — atrybut docelowy jest lewej za w niezmienionej postaci. Jednak jeśli atrybut docelowy jest kiedykolwiek pusta, będzie można wypełnić z wartością domyślną, określonym przez użytkownika.



Oprócz te cztery typy mapowania podstawowe atrybut mapowania atrybutu niestandardowego obsługiwał koncepcji przypisanie wartości **domyślnej** . Przypisanie wartości domyślne gwarantuje, że atrybut docelowy jest wypełniona wartości, które istnieje żadna wartość w Azure AD ani na obiekt docelowy.

Firma Microsoft Azure AD zapewnia dobry stosowania procesu synchronizacji. W środowisku zainicjowana tylko obiekty wymagających aktualizacji są przetwarzane cyklu synchronizacji. Aktualizowanie mapowania atrybutów ma wpływ na wydajność cyklu synchronizacji. Jest to, ponieważ jest aktualizacja Konfiguracja mapowania atrybutu wymaga wszystkich obiektów zarządzanych, można reevaluated. Z tego powodu jest zalecane za najbardziej skuteczne rozwiązanie do zachowania liczby kolejne zmiany do mapowania atrybutu co najmniej.


##<a name="related-articles"></a>Artykuły pokrewne

- [Artykuł indeks do zarządzania aplikacjami w usłudze Azure Active Directory](active-directory-apps-index.md)
- [Automatyzowanie użytkownika inicjowania obsługi administracyjnej i cofanie ubezpieczeń do aplikacji władz akredytacji bezpieczeństwa](active-directory-saas-app-provisioning.md)
- [Wyrażeń do mapowania atrybutów](active-directory-saas-writing-expressions-for-attribute-mappings.md)
- [Określanie zakresu filtry dla użytkownika inicjowania obsługi administracyjnej.](active-directory-saas-scoping-filters.md)
- [Aby włączyć automatyczne inicjowania obsługi administracyjnej użytkowników i grup z usługi Azure Active Directory do aplikacji przy użyciu SCIM](active-directory-scim-provisioning.md)
- [Konto inicjowania obsługi administracyjnej powiadomienia](active-directory-saas-account-provisioning-notifications.md)
- [Lista samouczki dotyczące integracja aplikacji władz akredytacji bezpieczeństwa](active-directory-saas-tutorial-list.md)


<!--Image references-->
[1]: ./media/active-directory-saas-customizing-attribute-mappings/ic765497.png
[2]: ./media/active-directory-saas-customizing-attribute-mappings/ic775419.png
[3]: ./media/active-directory-saas-customizing-attribute-mappings/ic775420.png
[4]: ./media/active-directory-saas-customizing-attribute-mappings/ic775421.png
