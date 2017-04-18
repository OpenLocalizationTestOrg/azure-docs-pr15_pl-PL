<properties
   pageTitle="Zarządzanie jednostek administracyjnych w usługi Azure Active Directory"
   description="Za pomocą jednostek administracyjnych dla bardziej szczegółowego delegowanie uprawnień w usłudze Active Directory platformy Azure"
   services="active-directory"
   documentationCenter=""
   authors="curtand"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/23/2016"
   ms.author="curtand"/>

# <a name="administrative-units-management-in-azure-ad---public-preview"></a>Zarządzanie jednostek administracyjnych w Azure AD - Public Preview

Ten artykuł zawiera opis jednostki administracyjne — nowe kontenera usługi Azure Active Directory zasobów, które mogą być używane delegowania uprawnień administracyjnych na podzbiór użytkowników i stosowanie zasad grupy użytkowników. Usługi Azure Active Directory jednostek administracyjnych włączyć Administratorzy central umożliwia delegowanie uprawnień administratorom regionalne lub ustawienie zasad na poziomie szczegółowego.

Jest to przydatne w przypadku organizacji z niezależnych działy, na przykład dużych university, który składa się z wielu szkoły autonomiczne (szkoły firm, szkoły inżynieria i tak dalej), niezależne od siebie. Takie przegrody mieć własne Administratorzy IT, którzy kontrolowania dostępu, zarządzanie użytkownikami i ustawić zasady specjalnie dla ich podział. Administratorzy Central ma być w stanie udzielić tych oddziałów uprawnień administratorów przez użytkowników w ich określonego działów. Dokładniej w tym przykładzie administratora centralnego można, na przykład utworzyć jednostkę administracyjne dla określonego szkoły (firm szkoły) i wypełnij go tylko użytkownicy biznesowi szkoły. Innymi słowy, administrator centralnej można dodać szkoły firm personelem INFORMATYCZNYM do roli zakresu, następnie przydziel personelem INFORMATYCZNYM uprawnień administracyjnych szkoły firm tylko w jednostce administracyjnej szkoły firm.

> [AZURE.IMPORTANT]
> Można tworzyć i używać jednostek administracyjnych, tylko wtedy, gdy włączysz Azure Active Directory Premium. Aby uzyskać więcej informacji zobacz [Wprowadzenie do Azure AD Premium](active-directory-get-started-premium.md).

Z punktu widzenia administratora centralnego jednostka administracyjnych jest obiekt katalogu, które mogą zostać utworzone i wypełnione z zasobami. **W tej wersji tych zasobów można uzyskiwać dostęp tylko użytkownicy.** Po utworzeniu i wypełnieniu jednostki administracyjnej może służyć jako zakres Aby ograniczyć udzielone uprawnienia tylko na zasoby zawarte w danej jednostce administracyjnej.

## <a name="managing-administrative-units"></a>Zarządzanie jednostki administracyjne

W tej wersji preview można tworzyć i zarządzać jednostek administracyjnych za pomocą polecenia cmdlet Azure Active Directory modułu dla programu Windows PowerShell.

Aby uzyskać więcej informacji na temat wymagania dotyczące oprogramowania i zainstalować moduł usługi Azure AD i uzyskać informacji na temat polecenia cmdlet Azure AD modułu zarządzania jednostek administracyjnych, łącznie ze składni, opisy parametrów i przykładami zobacz [Zarządzanie Azure AD przy użyciu programu Windows PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx).


## <a name="next-steps"></a>Następne kroki
[Azure wersji usługi Active Directory](active-directory-editions.md)
