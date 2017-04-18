
<properties
    pageTitle="Zmienianie dzierżawy usługi Azure Active Directory w Azure RemoteApp | Microsoft Azure"
    description="Dowiedz się, jak zmienić dzierżawy usługi Azure Active Directory skojarzone z Azure RemoteApp"
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />



# <a name="change-the-azure-active-directory-tenant-in-azure-remoteapp"></a>Zmienianie dzierżawy usługi Azure Active Directory w Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp jest są przerywane. W tym artykule [anons](https://go.microsoft.com/fwlink/?linkid=821148) , aby uzyskać szczegółowe informacje.

Azure RemoteApp używa usługi Azure Active Directory (Azure AD), aby umożliwić dostęp użytkownika. Dzierżawy tylko Azure AD, którego można używać w Azure RemoteApp jest skojarzony z subskrypcją Azure. Skojarzony subskrypcji można sprawdzić na stronie **Ustawienia** w portalu. Przyjrzyj się kolumnie **katalogu** na karcie **Subskrypcje** .

> [AZURE.NOTE] Ta zmiana powiodła się należy najpierw usunąć wszystkich użytkowników z istniejącej dzierżawy usługi Azure Active Directory ze wszystkich kolekcji Azure RemoteApp. Aby to zrobić, przejdź do portalu Azure, przejdź na kartę **Azure RemoteApp** i Otwórz co zbioru Azure RemoteApp. Przejdź na kartę **użytkowników** i usuwanie użytkowników, które należą do Twojej bieżącej dzierżawy usługi Azure Active Directory. Powtórz dla wszystkich istniejących zbiorów Azure RemoteApp. Bez w ten sposób nie będzie można tworzyć lub poprawka zbiorów.

Jeśli chcesz użyć innego dzierżawy, wykonaj następujące kroki, aby zmienić skojarzenie z subskrypcji:

1. W portalu usunąć wszystkich użytkowników Azure AD, do których udzieleniu dostępu do kolekcji Azure RemoteApp. (O tym, jak to zrobić, zobacz uwagi powyżej kroki).


2. Ustawianie konta Microsoft (dawniej nazywanych Live ID) jako administrator usługi. (Nie wiesz, czy jesteś już administrator usługi? Możesz można znaleźć, klikając pozycję **Ustawienia -> Administratorzy**.) Teraz Oto jak zmienić to:
    1. Kliknij użytkownika w prawym górnym rogu, a następnie kliknij polecenie **Widok rachunku**.
    2. Kliknij pozycję subskrypcji. Następnie na nowej stronie, przewiń w dół i kliknij przycisk **Edytuj szczegóły subskrypcji** w prawo. (Sortowanie środkowym prawym dolnym rogu, jeśli można łatwiej znaleźć.)
    3. Wpisz nazwę konta Microsoft dla użytkownika, który ma być administratora usługi.

3. Teraz wylogować się z portalu, a następnie zaloguj się ponownie, używając innego konta Microsoft, którego określonego w poprzednim kroku.


4. Kliknij pozycję **Nowy -> aplikacji usług -> Aktywne katalogu -> katalog -> niestandardowy tworzenie**.
5. W **katalogu**wybierz pozycję **Użyj istniejącego katalogu**. Firma Microsoft ma być teraz wylogować się z portalu, więc należy wybrać pozycję **mogę wylogowano się teraz**.
6. Zaloguj się z powrotem do portalu jako administrator globalny katalogu, który chcesz dodać. (Jeśli użytkownik nie zostały już administratorem globalnym, będzie po round z Zaloguj się, a następnie zaloguj.)
7. Zostanie wyświetlony monit podczas logowania się Jeśli chcesz użyć istniejącej dzierżawy usługi AD przy użyciu swojej subskrypcji. Kliknij przycisk **Kontynuuj**, a następnie kliknij polecenie **Wyloguj się teraz**.
5. Zaloguj ponownie, a następnie wróć do **ustawień -> Subskrypcje**. Wybierz subskrypcję, a następnie kliknij **Edytuj katalogu**. Wybierz pozycję dzierżawy Azure AD, którego chcesz użyć.



Możesz teraz Użyj nowego Azure AD dzierżawy kontrolowanie dostępu do subskrypcji Azure i konfigurowanie dostępu użytkowników w Azure RemoteApp.
