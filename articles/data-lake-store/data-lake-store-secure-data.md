<properties 
   pageTitle="Zabezpieczanie danych przechowywanych w magazynie Lake danych Azure | Microsoft Azure" 
   description="Dowiedz się, jak zabezpieczyć dane w magazynie Lake danych Azure przy użyciu grup i listy kontroli dostępu" 
   services="data-lake-store" 
   documentationCenter="" 
   authors="nitinme" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="09/29/2016"
   ms.author="nitinme"/>

# <a name="securing-data-stored-in-azure-data-lake-store"></a>Zabezpieczanie danych przechowywanych w magazynie Lake danych Azure

Zabezpieczanie danych w magazynie Lake danych Azure to podejście trzech kroków.

1. Rozpoczyna się od utworzenia grupy zabezpieczeń w Azure Active Directory (AAD). Grupy zabezpieczeń są używane do wprowadzenia kontrola dostępu oparta na rolach (RBAC) Azure Portal. Aby uzyskać więcej informacji, zobacz [Oparta na rolach kontrola dostępu w programie Microsoft Azure](../active-directory/role-based-access-control-configure.md).

2. Przypisz grupy zabezpieczeń AAD do konta magazynu Lake danych Azure. Ta opcja kontroluje dostęp do konta magazynu Lake danych z portalu i zarządzanie operacji z poziomu portalu lub interfejsów API.

3. Przypisywanie AAD grupy zabezpieczeń programu Access control list (ACL) w systemie plików magazynu Lake danych.

4. Ponadto można także ustawić zakres adresów IP dla klientów, którzy mają dostęp do danych w magazynie Lake danych.

Ten artykuł zawiera instrukcje dotyczące sposobu używania Azure portal umożliwia wykonywanie zadań powyżej. Aby uzyskać szczegółowe informacje na temat sposobu magazynu Lake danych wykonuje zabezpieczeń na poziomie konta i dane zobacz [zabezpieczeń w magazynie Lake danych Azure](data-lake-store-security-overview.md). Dla poszerzony informacji na temat sposobu implementacji ACL w magazynie Lake danych Azure zobacz [Omówienie kontrola dostępu w magazynie Lake danych](data-lake-store-access-control.md).

## <a name="prerequisites"></a>Wymagania wstępne

Przed rozpoczęciem tego samouczka, musisz mieć następujące czynności:

- **Azure subskrypcji**. Zobacz [Azure pobrać bezpłatną wersję próbną](https://azure.microsoft.com/pricing/free-trial/).
- **Konto magazynu Lake danych Azure**. Aby uzyskać instrukcje, jak go utworzyć zobacz [Rozpoczynanie pracy z magazynu Lake danych Azure](data-lake-store-get-started-portal.md)

## <a name="create-security-groups-in-azure-active-directory"></a>Tworzenie grupy zabezpieczeń w usłudze Active Directory platformy Azure

Aby uzyskać instrukcje dotyczące sposobu tworzenia grup zabezpieczeń AAD oraz jak dodawać użytkowników do grupy zobacz [Zarządzanie grupami zabezpieczeń w usłudze Azure Active Directory](../active-directory/active-directory-accessmanagement-manage-groups.md).

## <a name="assign-users-or-security-groups-to-azure-data-lake-store-accounts"></a>Przypisywanie użytkowników lub grup zabezpieczeń z kontami magazynu Lake danych Azure

Po przypisaniu do magazynu Lake danych Azure konta użytkowników lub grupy zabezpieczeń, możesz kontrolować dostęp do operacji zarządzania na konto za pomocą Azure portal i interfejsów API Menedżera zasobów Azure. 

1. Otwórz konto Azure magazynu Lake danych. W okienku po lewej stronie kliknij przycisk **Przeglądaj**, kliknij pozycję **Magazyn Lake danych**, a następnie z karta magazynu Lake danych kliknij nazwę konta, do którego chcesz przypisać użytkowników lub grup zabezpieczeń.

2. W swojej karta konta magazynu Lake danych kliknij przycisk **Ustawienia**. Karta **Ustawienia** kliknij **użytkowników**.

    ![Przypisywanie grupy zabezpieczeń do konta magazynu Lake danych Azure] (./media/data-lake-store-secure-data/adl.select.user.icon.png "Przypisywanie grupy zabezpieczeń do konta magazynu Lake danych Azure")

3. Karta **użytkownika** domyślnie listy grupy **administratorów subskrypcji** jako właściciel. 

    ![Dodawanie użytkowników i ról] (./media/data-lake-store-secure-data/adl.add.group.roles.png "Dodawanie użytkowników i ról")
 
    Istnieją dwa sposoby Dodawanie grupy i przypisz odpowiednie role.

    * Dodać do konta użytkownika lub grupy, a następnie przypisać roli, lub
    * Dodawanie roli, a następnie Przypisz użytkowników i grupy do roli.

    W tej sekcji przyjrzymy się pierwszą metodę dodawania grupy, a następnie przypisywanie ról. Można wykonać podobne kroki w celu najpierw wybierz rolę, a następnie przypisać grupy do tej roli.
    
4. W karta **użytkowników** kliknij przycisk **Dodaj** , aby otworzyć karta **Dodaj dostęp** . W karta **Dodaj programu access** kliknij pozycję **Wybierz rolę**, a następnie wybierz rolę dla użytkownika lub grupy.

     ![Dodawanie roli użytkownika] (./media/data-lake-store-secure-data/adl.add.user.1.png "Dodawanie roli użytkownika")

    **Właściciel** i rola **współautora** zapewniają dostęp do różnych funkcji administracji na koncie lake danych. Dla użytkowników, którzy będą korzystać z danymi w lake danych możesz dodać je do roli **czytnika **. Zakres tych rolach jest ograniczone do operacji zarządzania związanych z klientem magazynu Lake danych Azure.

    Dla danych uprawnienia systemu plików poszczególnych operacji zdefiniować, co użytkownicy mogą robić. W związku z tym, użytkownika z roli czytników mogą tylko wyświetlać ustawienia administracyjne skojarzone z kontem, ale można potencjalnie odczytywanie i zapisywanie danych według przypisane uprawnienia systemu plików. Uprawnienia systemu plików magazynu Lake danych są opisane na stronie [Przypisywanie grupy zabezpieczeń ACL w systemie plików magazynu Lake danych Azure](#filepermissions).



5. W karta **dostęp Dodaj** kliknij **Dodawanie użytkowników** do otwarcia karta **Dodaj użytkowników** . Ta karta poszukaj utworzonego wcześniej w usługi Azure Active Directory grupy zabezpieczeń. Jeśli masz wiele grup wyszukiwanie od, użyj pola tekstowego u góry, aby odfiltrować nazwę grupy. Kliknij przycisk **Wybierz**.

    ![Dodawanie grupy zabezpieczeń] (./media/data-lake-store-secure-data/adl.add.user.2.png "Dodawanie grupy zabezpieczeń")

    Jeśli chcesz dodać grupy i użytkownika, którego nie ma na liście, może zaprosić je za pomocą ikony **zaprosić** i określając adres e-mail użytkownika lub grupy.

6. Kliknij **przycisk OK**. Powinien zostać wyświetlony grupy zabezpieczeń dodany, tak jak pokazano poniżej.

    ![Dodana grupa zabezpieczeń] (./media/data-lake-store-secure-data/adl.add.user.3.png "Dodana grupa zabezpieczeń")

7. Grupy zabezpieczeń i zawiera teraz dostępu do konta magazynu Lake danych Azure. Jeśli chcesz umożliwić dostęp do określonych użytkowników, możesz je dodać do grupy zabezpieczeń. Analogicznie jeśli chcesz odebrać dostęp do użytkownika, można je usunąć z grupy zabezpieczeń. Możesz również przypisać wiele grup zabezpieczeń konta. 

## <a name="filepermissions"></a>Przypisywanie użytkowników lub grupy zabezpieczeń jako ACL systemu plików magazynu Lake danych Azure

Przypisując użytkownika/grupy zabezpieczeń w systemie plików Azure Lake danych, możesz ustawić kontroli dostępu do danych przechowywanych w magazynie Lake danych Azure.

1. W swojej karta konta magazynu Lake danych kliknij pozycję **Eksplorator danych**.

    ![Tworzenie katalogów na koncie magazynu Lake danych] (./media/data-lake-store-secure-data/adl.start.data.explorer.png "Tworzenie katalogów na koncie Lake danych")

2. W karta **Eksplorator danych** kliknij plik lub folder, dla której chcesz skonfigurować listy kontroli dostępu, a następnie kliknij **programu Access**. Aby przypisać listy kontroli dostępu do pliku, należy kliknąć przycisk **dostępu** z karta **Podgląd pliku** .

    ![Ustawianie ACL w systemie plików Lake danych] (./media/data-lake-store-secure-data/adl.acl.1.png "Ustawianie ACL w systemie plików Lake danych")

3. Karta **programu Access** Wyświetla dostępu standardowego i niestandardowego programu access już przypisany do katalogu głównego. Kliknij ikonę **Dodaj** , aby dodać ACL Poziom niestandardowy.

    ![Lista standardowych i niestandardowych programu access] (./media/data-lake-store-secure-data/adl.acl.2.png "Lista standardowych i niestandardowych programu access")

    * **Dostępu standardowe** jest dostęp typu UNIX, którym możesz określić, Odczyt, zapis, wykonywanie (rwx) do trzech klas inną nazwę: właściciela grupy i innych osób.
    * **Niestandardowe dostępu** odpowiada ACL POSIX, który umożliwia ustawianie uprawnień dla określonych nazwany użytkowników lub grupy, a nie tylko pliku właściciela lub grupy. 
    
    Aby uzyskać więcej informacji zobacz [ACL HDFS](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html#ACLs_Access_Control_Lists). Aby uzyskać więcej informacji na implementacji ACL w magazynie Lake danych zobacz [Kontrola dostępu w magazynie Lake danych](data-lake-store-access-control.md).

4. Kliknij ikonę **Dodaj** , aby otworzyć karta **Dodawanie dostępu niestandardowe** . W tym karta kliknij **Wybierz użytkownika lub grupy**, a następnie **Wybierz użytkownika lub grupę** karta, poszukaj grupy zabezpieczeń, utworzony wcześniej w usługi Azure Active Directory. Jeśli masz wiele grup wyszukiwanie od, użyj pola tekstowego u góry, aby odfiltrować nazwę grupy. Kliknij grupę, którą chcesz dodać, a następnie kliknij przycisk **Wybierz**.

    ![Dodaj grupę] (./media/data-lake-store-secure-data/adl.acl.3.png "Dodaj grupę")

5. Kliknij pozycję **Wybierz uprawnienia**, wybierz uprawnienia i czy chcesz przypisać uprawnienia jako domyślne ACL dostępu do list ACL lub obie. Kliknij **przycisk OK**.

    ![Przypisywanie uprawnień do grupy] (./media/data-lake-store-secure-data/adl.acl.4.png "Przypisywanie uprawnień do grupy")

    Aby uzyskać więcej informacji na temat uprawnień w magazynie Lake danych i ACL domyślnego i dostępu Zobacz [Kontrola dostępu w magazynie Lake danych](data-lake-store-access-control.md).


6. W karta **Dodawanie dostępu niestandardowe** kliknij **przycisk OK**. Nowo dodanej grupy, skojarzone z nimi uprawnienia zostanie dodany do listy w **programie Access** karta.

    ![Przypisywanie uprawnień do grupy] (./media/data-lake-store-secure-data/adl.acl.5.png "Przypisywanie uprawnień do grupy")

    > [AZURE.IMPORTANT] W bieżącej wersji może mieć tylko 9 wpisy w obszarze **Dostęp niestandardowy**. Jeśli chcesz dodać więcej niż 9 użytkowników, należy utworzyć grupy zabezpieczeń, dodawanie użytkowników do grup zabezpieczeń, dodaj dostęp do tych grup zabezpieczeń dla konta magazynu Lake danych.

7. W razie potrzeby można także modyfikować uprawnień dostępu, po dodaniu grupy. Wyczyść lub zaznacz pole wyboru dla każdego typu uprawnienia (Odczyt, zapis, wykonywanie) według tego, czy chcesz usunąć lub przypisać uprawnienie do grupy zabezpieczeń. Kliknij przycisk **Zapisz** , aby zapisać zmiany, lub **pozycję Odrzuć,** Aby cofnąć zmiany.

## <a name="set-ip-address-range-for-data-access"></a>Ustaw zakres adresów IP, aby uzyskać dostęp do danych

Magazyn Lake danych Azure umożliwia dalsze blokowania dostępu do sklepu danych na poziomie sieci. Można włączyć zaporę, określić adres IP lub zdefiniować zakres adresów IP dla klientów korzystających z zaufanych. Po włączeniu tylko klientów, którzy mają adresy IP zdefiniowanego zakresu można połączyć w magazynie.

![Ustawienia zapory i dostęp IP] (./media/data-lake-store-secure-data/firewall-ip-access.png "Ustawienia zapory i adres IP")

## <a name="remove-security-groups-for-an-azure-data-lake-store-account"></a>Usuwanie grupy zabezpieczeń dla konta magazynu Lake danych Azure

Po usunięciu grupy zabezpieczeń z konta magazynu Lake danych Azure powoduje tylko zmianę dostęp do operacji zarządzania na konto za pomocą API Menedżera zasobów Azure i Azure Portal.

1. W swojej karta konta magazynu Lake danych kliknij przycisk **Ustawienia**. Karta **Ustawienia** kliknij **użytkowników**.

    ![Przypisywanie grupy zabezpieczeń z kontem Lake danych Azure] (./media/data-lake-store-secure-data/adl.select.user.icon.png "Przypisywanie grupy zabezpieczeń z kontem Lake danych Azure")

2. **Użytkownicy** karta kliknij grupę zabezpieczeń, który chcesz usunąć.

    ![Usuwanie grupy zabezpieczeń] (./media/data-lake-store-secure-data/adl.add.user.3.png "Usuwanie grupy zabezpieczeń")

3. W karta dla grupy zabezpieczeń kliknij przycisk **Usuń**.

    ![Grupa zabezpieczeń usunięte] (./media/data-lake-store-secure-data/adl.remove.group.png "Grupa zabezpieczeń usunięte")

## <a name="remove-security-group-acls-from-azure-data-lake-store-file-system"></a>Usuwanie grupy zabezpieczeń ACL z magazynu Lake danych Azure plików systemu Windows

Po usunięciu grupy zabezpieczeń ACL z plików systemu Windows Azure magazynu Lake danych, możesz zmienić dostęp do danych w magazynie Lake danych.

1. W swojej karta konta magazynu Lake danych kliknij pozycję **Eksplorator danych**.

    ![Tworzenie katalogów na koncie Lake danych] (./media/data-lake-store-secure-data/adl.start.data.explorer.png "Tworzenie katalogów na koncie Lake danych")

2. W karta **Eksplorator danych** kliknij plik lub folder, dla którego chcesz usunąć listy kontroli dostępu, a następnie w swojej karta konta kliknij ikonę programu **Access** . Aby usunąć list ACL dla pliku, należy kliknąć przycisk **dostępu** z karta **Podgląd pliku** .

    ![Ustawianie ACL w systemie plików Lake danych] (./media/data-lake-store-secure-data/adl.acl.1.png "Ustawianie ACL w systemie plików Lake danych")

3. W karta **programu Access** , w sekcji **Niestandardowy w programie Access** kliknij grupę zabezpieczeń, który chcesz usunąć. W karta **Dostęp niestandardowy** kliknij przycisk **Usuń** , a następnie kliknij **przycisk OK**.

    ![Przypisywanie uprawnień do grupy] (./media/data-lake-store-secure-data/adl.remove.acl.png "Przypisywanie uprawnień do grupy")


## <a name="see-also"></a>Zobacz też

- [Omówienie magazynu Lake danych Azure](data-lake-store-overview.md)
- [Skopiuj dane z obiektami blob miejsca do magazynowania Azure do magazynu Lake danych](data-lake-store-copy-data-azure-storage-blob.md)
- [Używanie analizy Lake Azure danych z magazynu Lake danych](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Usługa Azure HDInsight za pomocą magazynu Lake danych](data-lake-store-hdinsight-hadoop-use-portal.md)
- [Rozpoczynanie pracy z magazynu Lake danych przy użyciu programu PowerShell](data-lake-store-get-started-powershell.md)
- [Rozpoczynanie pracy z magazynu Lake danych przy użyciu zestawu SDK .NET](data-lake-store-get-started-net-sdk.md)
- [Dzienniki diagnostyczne dostępu dla magazynu Lake danych](data-lake-store-diagnostic-logs.md)
