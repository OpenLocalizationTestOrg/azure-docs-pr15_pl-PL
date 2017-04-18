<properties
   pageTitle="Omówienie zabezpieczeń w magazynie Lake danych | Microsoft Azure"
   description="Zrozumieć, jak magazyn Lake danych Azure jest bardziej bezpiecznego magazynu danych duży"
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
   ms.date="08/02/2016"
   ms.author="nitinme"/>

# <a name="security-in-azure-data-lake-store"></a>Zabezpieczenia w magazynie Lake danych Azure

Wiele jednostek są korzystanie z danych duży analizy wniosków firm ułatwić podejmowanie decyzji inteligentne. Organizacja może być środowiska złożone i regulowanym, zwiększenie liczby różnych użytkowników. Ważne jest w przedsiębiorstwie upewnić się, że ważnych danych biznesowych są przechowywane bezpieczniejsze, z odpowiedni poziom dostępu przyznane dla poszczególnych użytkowników. Magazyn Lake danych Azure ułatwiających spełnianie wymagań zabezpieczeń. Ten artykuł zawiera informacje o możliwości zabezpieczeń magazynu Lake danych, w tym:

* Uwierzytelnianie
* Autoryzacja
* Izolacji sieci
* Ochrona danych
* Inspekcja

## <a name="authentication-and-identity-management"></a>Zarządzanie uwierzytelniania i tożsamości

Uwierzytelnianie polega na zweryfikowaniu tożsamość użytkownika w wyniku interakcji użytkownika z magazynu Lake danych lub z dowolnej usługi połączony z magazynu Lake danych. Do uwierzytelniania i Zarządzanie tożsamościami magazynu Lake danych korzysta z [Usługi Azure Active Directory](../active-directory/active-directory-whatis.md), pełna tożsamości i dostępu do zarządzania chmury rozwiązanie, które ułatwia zarządzanie użytkownikami i grupami.

Każdej subskrypcji Azure mogą być skojarzone z wystąpieniem usługi Azure Active Directory. Tylko użytkownicy i tożsamości usługi, które zdefiniowano za pomocą usługi Azure Active Directory można uzyskać dostępu do konta magazynu Lake danych za pomocą portalu Azure narzędzi wiersza polecenia, lub za pośrednictwem aplikacji klienckich organizacji tworzy przy użyciu Azure Data Lake sklepu SDK. Kluczowe zalety w charakterze mechanizmu kontroli scentralizowany dostęp za pomocą usługi Azure Active Directory są następujące:

* Uproszczone zarządzanie tożsamościami cyklu życia. Tożsamość użytkownika lub usługą (tożsamość głównej usługi) można szybko tworzyć i szybko odwołany po prostu usuwając lub wyłączając konta w katalogu.

* Uwierzytelnianie wieloskładnikowe. [Uwierzytelnianie wieloskładnikowe](../multi-factor-authentication/multi-factor-authentication.md) zapewnia dodatkową warstwę zabezpieczeń dla transakcji i dodatki logowania użytkownika.

* Uwierzytelnianie od dowolnego klienta za pośrednictwem standardowej protokołu open, takich jak OAuth lub OpenID.

* Federacja z usługami katalogowymi przedsiębiorstwa i dostawcy tożsamości w chmurze.

## <a name="authorization-and-access-control"></a>Autoryzacja i kontroli dostępu

Po usługi Azure Active Directory uwierzytelnia użytkownika, aby umożliwić użytkownikowi dostęp magazynu Lake danych Azure, kontrolki autoryzacji dostęp do uprawnień do magazynu Lake danych. Magazyn Lake danych oddziela autoryzacji działań związanych z konta i związanych z danymi w następujący sposób:

* [Kontrola dostępu oparta na rolach](../active-directory/role-based-access-control-what-is.md) (RBAC) przewidziane przez Azure Zarządzanie kontami
* ACL POSIX uzyskiwania dostępu do danych w sklepie


### <a name="rbac-for-account-management"></a>RBAC do zarządzania kontami

Cztery podstawowe role są domyślnie definiowane dla magazynu Lake danych. Role zezwolić na różnych operacji na koncie magazynu Lake danych za pośrednictwem Azure portal, poleceń cmdlet środowiska PowerShell oraz interfejsy API pozostałych. Role właściciela i współautora można wykonywać różne funkcje administracji na koncie. Rola czytnika można przypisać użytkownikom, którzy wchodzić w interakcje tylko z danymi.

![Role RBAC] (./media/data-lake-store-security-overview/rbac-roles.png "Role RBAC")

Należy zauważyć, że chociaż role są przypisywane do zarządzania kontami, niektóre role wpływa na dostęp do danych. Należy użyć ACL kontrolowanie dostępu do działania, które użytkownik może wykonywać w systemie plików. W poniższej tabeli przedstawiono podsumowanie zarządzania prawami i prawa dostępu do danych dla role domyślne.

| Role                    | Zarządzanie prawami               | Prawa dostępu do danych | Wyjaśnienie |
| ------------------------ | ------------------------------- | ------------------ | ----------- |
| Nie rolę         | Brak                            | Objętych ACL    | Użytkownik nie umożliwia Azure portal lub poleceń cmdlet środowiska PowerShell Azure Przeglądanie magazynu Lake danych. Użytkownik może skorzystać z narzędzia wiersza polecenia tylko.
| Właściciel  | Wszystkie  | Wszystkie  | Rola właściciela jest administratora. Ta rola można zarządzać wszystko i ma pełny dostęp do danych.
| Czytnik   | Tylko do odczytu  | Objętych ACL    | Rola czytnika można wyświetlać wszystkie elementy dotyczące zarządzania kontami, takich jak której użytkownik jest przypisany do roli. Rola czytnika nie wprowadź odpowiednie zmiany.   |
| Trybu współautora              | Wszystko poza Dodawanie i usuwanie ról | Objętych ACL    | Rola współautora można zarządzać niektórych aspektów konta, takiego jak wdrożenia, tworzenie i Zarządzanie alertami. Rola współautora nie można dodawać lub usuwać role.
| Administrator dostępu użytkowników | Dodawanie i usuwanie ról            | Objętej ACL    | Rola administratora użytkowników programu Access można zarządzać użytkownikowi dostępu do konta. |

Aby uzyskać instrukcje zobacz [Przypisywanie użytkowników lub grup zabezpieczeń z kontami magazynu Lake danych](data-lake-store-secure-data.md#assign-users-or-security-groups-to-azure-data-lake-store-accounts).

### <a name="using-acls-for-operations-on-file-systems"></a>Za pomocą list ACL dla operacji w systemie plików

Magazyn Lake danych jest system plików hierarchicznych takich jak pliku usługi Hadoop Distributed System (HDFS) i obsługuje [ACL POSIX](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html#ACLs_Access_Control_Lists). Każdy z nich steruje odczytu (r), zapisu (w) i wykonywania (x) uprawnień do zasobów dla roli właściciela, w grupie właściciele i inni użytkownicy i grupy. W Lake sklepu publicznej podglądu danych (bieżącej wersji) ACL są włączone w folderze głównym, podfoldery i pojedyncze pliki. ACL, które dotyczą folderu głównego dotyczą również wszystkie pliki i foldery podrzędne.

Firma Microsoft zaleca Definiowanie ACL dla wielu użytkowników przy użyciu [grupy zabezpieczeń](../active-directory/active-directory-accessmanagement-manage-groups.md). Dodawanie użytkowników do grupy zabezpieczeń, a następnie przypisać list ACL pliku lub folderu do tej grupy zabezpieczeń. Jest to przydatne, gdy chcesz udostępnić niestandardowe, ponieważ jest ograniczone do dodawania maksymalnie dziewięciu pozycje dostęp niestandardowy. Aby uzyskać więcej informacji na temat lepiej bezpiecznego dane przechowywane w magazynie Lake danych przy użyciu grup zabezpieczeń usługi Azure Active Directory zobacz [Przypisywanie użytkowników lub grupy zabezpieczeń ACL w systemie plików magazynu Lake danych Azure](data-lake-store-secure-data.md#filepermissions).

![Lista standardowych i niestandardowych programu access] (./media/data-lake-store-security-overview/adl.acl.2.png "Lista standardowych i niestandardowych programu access")

## <a name="network-isolation"></a>Izolacji sieci

Korzystanie z magazynu Lake danych ułatwiają kontrolowanie dostępu do sklepu danych na poziomie sieci. Można ustalić zapory i zdefiniować zakres adresów IP dla klientów korzystających z zaufanych. Zakres adresów IP tylko klientów, którzy mają adresy IP zdefiniowanego zakresu można się łączyć z magazynu Lake danych.

![Ustawienia zapory i dostęp IP] (./media/data-lake-store-security-overview/firewall-ip-access.png "Ustawienia zapory i adres IP")

## <a name="data-protection"></a>Ochrona danych

Organizacje chcesz pewność, że ich kluczowych danych biznesowych są bezpieczne w całej jej cyklu życia. W przypadku danych podczas przesyłania magazynu Lake danych korzysta z protokołu zabezpieczeń TLS (Transport Layer) standardowych zabezpieczania danych, który powoduje przejście między klientem a magazynu Lake danych.

Ochrona danych danych w pozostałych będzie dostępna w przyszłych wersjach.

## <a name="auditing-and-diagnostic-logs"></a>Dzienniki inspekcji i diagnostyczne

Możesz użyć dzienniki inspekcji lub diagnostycznych, w zależności od tego, czy szukasz dzienników działań związanych z zarządzaniem lub działań związanych z danymi.

*  Działań związanych z zarządzaniem przy użyciu interfejsów API Menedżera zasobów Azure i są udostępnione w portalu Azure za pomocą dzienników inspekcji.
*  Działań związanych z danymi przy użyciu interfejsów API pozostałych WebHDFS i są udostępnione w portalu Azure za pomocą dzienników diagnostycznych.

### <a name="auditing-logs"></a>Dzienniki inspekcji

Aby zachować zgodność z przepisami, organizacji może potrwać ścieżek odpowiednie inspekcji, gdy należy wyświetlić do określonych zdarzeń. Magazynu Lake danych ma wbudowane monitorowania i inspekcji i rejestruje wszystkie działania związane z zarządzaniem konta.

Do wykonywania inspekcji zarządzania kontem wyświetlanie i Wybieranie kolumn, które chcesz się zalogować. Możesz także wyeksportować dzienników inspekcji do magazynu Azure.

![Dzienniki inspekcji] (./media/data-lake-store-security-overview/audit-logs.png "Dzienniki inspekcji")

### <a name="diagnostic-logs"></a>Dzienniki diagnostyczne

Można ustawić ścieżek inspekcji dostęp do danych w portalu Azure (w ustawieniach diagnostyczne) i utworzyć konto magazyn obiektów Blob platformy Azure miejsce, w którym są przechowywane pliki dziennika.

![Dzienniki diagnostyczne] (./media/data-lake-store-security-overview/diagnostic-logs.png "Dzienniki diagnostyczne")

Po skonfigurowaniu ustawień diagnostycznych, możesz wyświetlić dzienniki, na karcie **Dzienniki diagnostyczne** .

Aby uzyskać więcej informacji na temat pracy z dzienników diagnostycznych z magazynu Lake danych Azure zobacz [Dzienniki diagnostyczne dostępu dla magazynu Lake danych](data-lake-store-diagnostic-logs.md).

## <a name="summary"></a>Podsumowanie

Przedsiębiorstwa zażądać platformy chmury analizy danych, która jest bezpieczny i łatwy w użyciu. Azure magazynu Lake danych ma na celu adres, z którego umieść te wymagania za pośrednictwem Zarządzanie tożsamościami i uwierzytelniania za pośrednictwem integracji usługi Azure Active Directory, opartego na ACL, izolacji sieci, szyfrowania danych podczas przesyłania i w (już w przyszłości) i inspekcja.

Jeśli chcesz wyświetlić nowe funkcje w magazynie Lake danych, przysłać nam swoją opinię na [forum możliwości Sklepu Lake danych](https://feedback.azure.com/forums/327234-data-lake).

## <a name="see-also"></a>Zobacz też

- [Omówienie magazynu Lake danych Azure](data-lake-store-overview.md)
- [Rozpoczynanie pracy z magazynu Lake danych](data-lake-store-get-started-portal.md)
- [Zabezpieczanie danych w magazynie Lake danych](data-lake-store-secure-data.md)
