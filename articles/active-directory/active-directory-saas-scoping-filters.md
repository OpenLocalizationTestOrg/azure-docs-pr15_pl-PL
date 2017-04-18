<properties
    pageTitle="Aplikacja opartej na atrybutach inicjowania obsługi administracyjnej z zakresu filtry | Microsoft Azure"
    description="Dowiedz się, jak zapobiec obiektów w aplikacji, które obsługują użytkownika automatycznego inicjowania obsługi administracyjnej w rzeczywistości jest obsługi administracyjnej Jeśli obiekt nie spełnia wymagań biznesowych za pomocą filtrów zakresu."
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


# <a name="attribute-based-app-provisioning-with-scoping-filters"></a>Z zakresu filtry obsługi administracyjnej aplikacji opartej na atrybutach

Jest celem w tej sekcji wyjaśniono, jak skorzystać z filtrów zakresu do definiowania reguły oparte na atrybut, określające, którzy użytkownicy będą obsługi administracyjnej, do aplikacji.





## <a name="clauses-and-scope-groups"></a>Klauzule i grupach zakresu


![Określanie zakresu filtru][1] 




Filtry zakresu są definiowane przez jedną lub więcej **grup zakresów**, z których każdy będzie przytrzymaj jeden lub więcej **klauzul**. Aby wyświetlić klauzule dla określonego zakresu grupy, należy ją rozwinąć, klikając strzałkę znajdującą się po lewej stronie nazwy grupy.

**Klauzula** Określa, którzy użytkownicy mogą przechodzić przez filtr zakresu określić atrybuty każdego użytkownika. Na przykład może być jednej klauzuli, która wymaga równe Nowy Jork, co oznacza, że tylko użytkownicy Nowy Jork będzie obsługi administracyjnej do aplikacji atrybut "Państwo" użytkownika.

![Określanie zakresu nazwę grupy][2] 



Każda **Grupa zakres** zaczyna się jedną obowiązkowe **klauzuli**, jak pokazano powyżej zrzut ekranu. W tej klauzuli po prostu informujący, że użytkownik najpierw musi mieć przypisane do aplikacji, zanim zostanie ona potraktowana przez filtry zakresu. W tej klauzuli nie można usunąć lub zmodyfikować.

Możesz dodać nowe klauzule lub nowych grup zakres, naciskając odpowiedni przycisk. Każda grupa zakres można nadać nazwę, edytując jego właściwość **Zakres nazwę grupy** .





## <a name="how-scoping-filters-are-evaluated"></a>Jak są obliczane w określanie zakresu filtrów

Podczas inicjowania obsługi administracyjnej, testowania każdej przydzielonych użytkownika przed zakresu filtry w celu ustalenia, jeśli użytkownik wymaga dostęp do aplikacji. Możesz myśleć każdej klauzuli jako test, który należy przekazać w kolejności dla użytkownika uniknąć wprowadzenie odfiltrowane. 

Jeśli masz wiele zdefiniowanych grupach zakresu, każdy użytkownik musi mieć co najmniej jeden z nich w celu uzyskania dostępu do aplikacji. W grupach zakresu, użytkownik musi mieć co jedną klauzulę do przekazywania tej grupy w określonym zakresie. 

Innymi słowy, możesz myśleć o grupach zakresu jako czy razem lub możesz myśleć klauzul w nich jako i będzie ze sobą. Na przykład można rozważyć Filtr zakresu poniżej:


![Określanie zakresu nazwę grupy][2]  


Według ten filtr zakresu użytkownicy muszą spełniać następujące kryteria, aby być przygotowana:

1. Ta osoba musi mieć przypisane do aplikacji.

2. Muszą one pracować w dziale inżynierskie

3. Muszą być pracy w San Francisco lub Kanadzie.


##<a name="related-articles"></a>Artykuły pokrewne

- [Artykuł indeks do zarządzania aplikacjami w usłudze Azure Active Directory](active-directory-apps-index.md)
- [Automatyzowanie użytkownika ubezpieczenia i cofanie ubezpieczeń aplikacjom władz akredytacji bezpieczeństwa](active-directory-saas-app-provisioning.md)
- [Dostosowywanie mapowań atrybutów dla użytkownika inicjowania obsługi administracyjnej.](active-directory-saas-customizing-attribute-mappings.md)
- [Wyrażeń do mapowania atrybutów](active-directory-saas-writing-expressions-for-attribute-mappings.md)
- [Konto inicjowania obsługi administracyjnej powiadomienia](active-directory-saas-account-provisioning-notifications.md)
- [Aby włączyć automatyczne inicjowania obsługi administracyjnej użytkowników i grup z usługi Azure Active Directory do aplikacji przy użyciu SCIM](active-directory-scim-provisioning.md)
- [Lista samouczki dotyczące integracja aplikacji władz akredytacji bezpieczeństwa](active-directory-saas-tutorial-list.md)

<!--Image references-->
[1]: ./media/active-directory-saas-scoping-filters/ic782811.png
[2]: ./media/active-directory-saas-scoping-filters/ic782812.png
[3]: ./active-directory-saas-scoping-filters/ic782813.png
