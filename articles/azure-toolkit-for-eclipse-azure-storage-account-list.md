<properties
    pageTitle="Listy kont Azure miejsca do magazynowania"
    description="Zarządzanie przy użyciu narzędzi Azure dla Zaćmienie ustawień konta miejsca do magazynowania"
    services=""
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="multiple"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/dn205108.aspx -->

# <a name="azure-storage-account-list"></a>Listy kont Azure miejsca do magazynowania #

Konta magazynu platformy Azure włączyć lokalizacji pobierania może być używany dla JDK, serwera aplikacji i składniki dowolnego, a także do przechowywania stan podczas korzystania z pamięci podręcznej. Zaćmienie przechowuje listę kont znane miejsca do magazynowania, które są dostępne dla projektów w obszarze roboczym Zaćmienie. Aby otworzyć okno dialogowe **Konta miejsca do magazynowania** , która jest używana do zarządzania tę listę w Zaćmienie, kliknij menu **okno**, kliknij polecenie **Preferencje**, rozwiń **Azure**, a następnie kliknij **Kont miejsca do magazynowania**.

Poniżej przedstawiono okno dialogowe **Konta miejsca do magazynowania** .

![][ic719496]

To okno dialogowe można również otworzyć z łącza **kont** w oknach dialogowych używające kont miejsca do magazynowania, takich jak następujące czynności:

* Karta **JDK** okna dialogowego **Konfiguracji serwera** .
* Karta **serwera** okna dialogowego **Konfiguracji serwera** .
* Okno dialogowe **Dodawanie składników** .
* Okno dialogowe właściwości **buforowanie** .

## <a name="to-import-your-storage-accounts-using-a-publish-settings-file"></a>Aby zaimportować konta miejsca do magazynowania za pomocą pliku ustawienia publikowania ##

1. W oknie dialogowym **Konta miejsca do magazynowania** kliknij pozycję **Importuj z pliku ustawienia publikowania**.
2. (Pomiń ten krok, jeśli masz już zapisany plik ustawień Publikuj na komputerze lokalnym). W oknie dialogowym **Informacje o subskrypcji importu** kliknij pozycję **Pobierz plik ustawień publikowania**. Jeśli nie masz jeszcze zalogowany do konta usługi Azure, zostanie wyświetlony monit o zalogowanie się. Następnie zostanie wyświetlony monit Zapisz Azure publikowanie plik ustawień. (Możesz zignorować otrzymane instrukcje wyświetlane na stronie logowania - ich są dostarczane przez Azure portal i są przeznaczone dla użytkowników programu Visual Studio) Zapisz go na komputerze lokalnym.
3. Nadal w oknie dialogowym **Informacje o subskrypcji importu** kliknij przycisk **Przeglądaj** , wybierz plik z ustawieniami publikowania lokalnie zapisany wcześniej i kliknij przycisk **Otwórz**.
4. Kliknij **przycisk OK** , aby zamknąć okno dialogowe **Importuj informacje o subskrypcji** .

## <a name="to-create-a-new-storage-account"></a>Aby utworzyć nowe konto miejsca do magazynowania ##

1. W oknie dialogowym **Konta miejsca do magazynowania** kliknij przycisk **Dodaj**.
2. W oknie dialogowym **Dodawanie konta miejsca do magazynowania** kliknij przycisk **Nowy**.
3. W oknie dialogowym **Nowe konto miejsca do magazynowania** określ następujące wartości:
    * Nazwę konta magazynu.
    * Lokalizacja konta miejsca do magazynowania.
    * Opis rachunku miejsca do magazynowania.
    * Subskrypcja, do której należy konto miejsca do magazynowania.
4. Kliknij **przycisk OK** , aby zamknąć okno dialogowe **Nowe konto miejsca do magazynowania** .

Może potrwać kilka minut można utworzyć konta miejsca do magazynowania. Po jego utworzeniu, kliknij **przycisk OK** , aby zamknąć okno dialogowe **Dodawanie konta miejsca do magazynowania** i nowego konta miejsca do magazynowania, zostaną dodane do listy kont dostępnego miejsca.

## <a name="to-add-an-existing-storage-account-to-the-list"></a>Aby dodać istniejące konto miejsca do magazynowania do listy ##

1. Jeśli nie masz już konto Azure miejsca do magazynowania, utworzyć, wykonując kroki opisane w sekcji **Tworzenie nowej sekcji konta miejsca do magazynowania** powyżej. (Można też kliknąć, można utworzyć nowe konto miejsca do magazynowania w [Portalu zarządzania Azure][].)
2. W oknie dialogowym **Konta miejsca do magazynowania** kliknij przycisk **Dodaj**.
3. W oknie dialogowym **Dodawanie konta miejsca do magazynowania** wprowadź wartości **Nazwa** i **Klawisz dostępu**. Klucz nazwę i dostępu do konta musi być istniejące konto Azure miejsca do magazynowania. Sekcja **miejsca do magazynowania** [Portalu zarządzania Azure][] umożliwia wyświetlanie nazwy kont miejsca do magazynowania i klawiszy. Usługi okno dialogowe **Dodawanie konta miejsca do magazynowania** będzie wyglądać podobnie do następującej.

    ![][ic719497]

4. Kliknij **przycisk OK** , aby zamknąć okno dialogowe **Dodawanie konta miejsca do magazynowania** .

## <a name="to-modify-a-storage-account-to-use-a-new-access-key"></a>Aby zmodyfikować konto miejsca do magazynowania, za pomocą nowego klucza dostępu ##

1. W oknie dialogowym **Konta miejsca do magazynowania** kliknij pozycję konta miejsca do magazynowania, który chcesz edytować, a następnie kliknij przycisk **Edytuj**.
2. W oknie dialogowym **Edytowanie klucz dostępu do konta miejsca do magazynowania** Zmień wartość **Klawisz dostępu** .
3. Kliknij **przycisk OK** , aby zamknąć okno dialogowe **Edytowanie klucz dostępu do konta miejsca do magazynowania** .

## <a name="to-remove-a-storage-account-from-the-list-maintained-in-eclipse"></a>Aby usunąć konto miejsca do magazynowania z listy w Zaćmienie ##

1. W oknie dialogowym **Konta miejsca do magazynowania** kliknij pozycję konta miejsca do magazynowania, który chcesz edytować, a następnie kliknij przycisk **Usuń**.
2. Kliknij **przycisk OK** , gdy zostanie wyświetlony monit o usunięcie konta miejsca do magazynowania.

>[AZURE.NOTE] Usunięcie konta miejsca do magazynowania za pośrednictwem okna dialogowego **Konta miejsca do magazynowania** tylko usuwane z listy kont miejsca do magazynowania widoczny w Zaćmienie. Nie powoduje usunięcia konta miejsca do magazynowania z subskrypcji usługi Azure. Ponadto konto miejsca do magazynowania można ponownie wyświetlone na liście po Zaćmienie ładuje Szczegóły subskrypcji.

## <a name="see-also"></a>Zobacz też ##

[Azure zestaw narzędzi dla programu Eclipse][]

[Instalowanie narzędzi Azure dla programu Eclipse][] 

[Tworzenie aplikacji Witaj świecie dla Azure w Zaćmienie][]

Aby uzyskać więcej informacji na temat Azure za pomocą języka Java zobacz [Centrum deweloperów języka Java Azure][].

<!-- URL List -->

[Centrum deweloperów języka Java Azure]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure zestaw narzędzi dla programu Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Portal Azure zarządzania]: http://go.microsoft.com/fwlink/?LinkID=512959
[Tworzenie aplikacji Witaj świecie dla Azure w Zaćmienie]: http://go.microsoft.com/fwlink/?LinkID=699533
[Instalowanie narzędzi Azure dla programu Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546
[What's New in the Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699552

<!-- IMG List -->

[ic719496]: ./media/azure-toolkit-for-eclipse-azure-storage-account-list/ic719496.png
[ic719497]: ./media/azure-toolkit-for-eclipse-azure-storage-account-list/ic719497.png
