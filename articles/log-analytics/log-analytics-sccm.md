<properties
    pageTitle="Łączenie Menedżer konfiguracji z dziennika analizy | Microsoft Azure"
    description="W tym artykule przedstawiono czynności, aby połączyć Menedżer konfiguracji dziennika analizy i rozpoczynanie analizy danych."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/29/2016"
    ms.author="banders"/>

# <a name="connect-configuration-manager-to-log-analytics"></a>Menedżer konfiguracji nawiązać połączenie analizy dziennika

Można nawiązać Menedżera konfiguracji centrum systemu do analizy dziennika w usługi OMS synchronizacji urządzenia zbioru danych. Dzięki temu dane z wdrożenia Menedżer konfiguracji są dostępne w usługi OMS.

Istnieje kilka czynności wymagane do nawiązania Menedżer konfiguracji usługi OMS, więc w tym miejscu jest szybkie uwalnianie całego procesu:

1. W portalu zarządzania Azure zarejestrować Menedżer konfiguracji jako aplikacji dla aplikacji sieci Web i/lub interfejs API sieci Web i upewnij się, że masz identyfikator klienta i klucz tajny klienta z rejestracji z usługi Azure Active Directory. Zobacz [Używanie portalu do tworzenia aplikacji usługi Active Directory i głównej, która ma dostęp do zasobów usługi](../resource-group-create-service-principal-portal.md) Aby uzyskać szczegółowe informacje dotyczące wykonywania tej czynności.
2. W Azure portalu zarządzania, [Podaj Menedżer konfiguracji (aplikacji sieci web zarejestrowanych) z uprawnieniami dostępu do usługi OMS](#provide-configuration-manager-with-permissions-to-oms).
3. W Menedżerze konfiguracji, [Dodawanie połączenia za pomocą Kreatora dodawania połączenia usługi OMS](#add-an-oms-connection-to-configuration-manager).
4. W Menedżerze konfiguracji można [zaktualizować właściwości połączenia](#update-oms-connection-properties) czy klucz tajny hasła lub klienta kiedykolwiek wygaśnie zostaną utracone.
5. Na podstawie informacji z portalem usługi OMS, [Pobierz i zainstaluj program Microsoft Agent monitorowania](#download-and-install-the-agent) na komputerze z systemem połączenia usługi Menedżer konfiguracji punktu Rola systemu witryny. Agent wysyła dane Menedżer konfiguracji usługi OMS.
6. W przypadku usługi OMS [Importowanie zbiorów za pomocą Menedżera konfiguracji](#import-collections) jako grupy komputera.
7. W przypadku usługi OMS wyświetlać danych za pomocą Menedżera konfiguracji jako [grupy komputera](log-analytics-computer-groups.md).

Więcej o nawiązywaniu połączeń Menedżer konfiguracji z usługi OMS [synchronizacji](https://technet.microsoft.com/library/mt757374.aspx)dane za pomocą Menedżera konfiguracji w pakiecie zarządzania operacji firmy Microsoft.



## <a name="provide-configuration-manager-with-permissions-to-oms"></a>Menedżer konfiguracji udostępnić uprawnienia do usługi OMS

W poniższej procedurze opisano Portal Azure zarządzania uprawnieniami, aby uzyskać dostęp do usługi OMS. W szczególności musi udzielić *roli współautora* dla użytkowników w grupie zasobów. Z kolei, która umożliwia Portal Azure zarządzania nawiązać Menedżer konfiguracji usługi OMS.

>[AZURE.NOTE] Należy określić uprawnienia do usługi OMS dla Menedżer konfiguracji. W przeciwnym razie otrzymasz komunikat o błędzie podczas korzystania z Kreatora konfiguracji w Menedżerze konfiguracji.


1. Otwórz [Azure portal](https://portal.azure.com/) , a następnie kliknij przycisk **Przeglądaj** > **Usługi dziennika analizy (OMS)** , aby otworzyć karta usługi dziennika analizy (OMS).  
2. Na karta **Usługi dziennika analizy (OMS)** kliknij przycisk **Dodaj** , aby otworzyć karta **Usługi OMS obszaru roboczego** .  
  ![Karta usługi OMS](./media/log-analytics-sccm/sccm-azure01.png)
3. Na karta **Usługi OMS obszaru roboczego** wprowadź następujące informacje, a następnie kliknij **przycisk OK**.
  - **Obszar roboczy usługi OMS**
  - **Subskrypcji**
  - **Grupa zasobów**
  - **Lokalizacja**
  - **Cennik warstwy**  
    ![Karta usługi OMS](./media/log-analytics-sccm/sccm-azure02.png)  

    >[AZURE.NOTE] Powyższy przykład umożliwia utworzenie nowej grupy zasobów. Grupa zasobów jest używany tylko do dostarczania Menedżer konfiguracji z uprawnieniami do obszaru roboczego usługi OMS w tym przykładzie.

4. Kliknij przycisk **Przeglądaj,** > **grup zasobów** , aby otworzyć karta **grup zasobów** .
5. W karta **grup zasobów** kliknij grupę zasobów utworzonym powyżej, aby otworzyć &lt;Nazwa grupy zasobów&gt; karta Ustawienia.  
  ![Karta Ustawienia grupy zasobów](./media/log-analytics-sccm/sccm-azure03.png)
6. W &lt;Nazwa grupy zasobów&gt; karta Ustawienia, kliknij opcję Kontrola dostępu (IAM), aby otworzyć &lt;Nazwa grupy zasobów&gt; karta użytkowników.  
  ![Karta Użytkownicy grupa zasobów](./media/log-analytics-sccm/sccm-azure04.png)  
7. W &lt;Nazwa grupy zasobów&gt; karta użytkowników, kliknij przycisk **Dodaj** , aby otworzyć karta **Dodaj dostęp** .
8. W karta **Dodaj programu access** kliknij pozycję **Wybierz rolę**, a następnie wybierz rola **współautora** .  
  ![Wybierz rolę](./media/log-analytics-sccm/sccm-azure05.png)  
9. Kliknij pozycję **Dodawaj użytkowników**, wybierz użytkownika, Menedżer konfiguracji, kliknij przycisk **Wybierz**, a następnie kliknij **przycisk OK**.  
  ![Dodawanie użytkowników](./media/log-analytics-sccm/sccm-azure06.png)  


## <a name="add-an-oms-connection-to-configuration-manager"></a>Dodawanie połączenia usługi OMS do Menedżera konfiguracji

Aby dodać połączenie usługi OMS, środowiska Menedżer konfiguracji musi być [punkt połączenia usługi](https://technet.microsoft.com/library/mt627781.aspx) skonfigurowane do trybu online.

1. W obszarze roboczym **administracji** programu Menedżer konfiguracji wybierz **Łącznik usługi OMS**. Spowoduje to otwarcie **Kreatora dodawania połączenia usługi OMS**. Wybierz przycisk **Dalej**.

2. Na ekranie **Ogólne** upewnij się, że wykonaniu poniższych akcji i czy możesz szczegółów poszczególnych elementów, a następnie wybierz przycisk **Dalej**.
  1. W portalu zarządzania Azure została zarejestrowana Menedżer konfiguracji jako aplikacji dla aplikacji sieci Web i/lub interfejs API sieci Web i że masz [identyfikator klienta z rejestracji](../active-directory/active-directory-integrating-applications.md).
  2. W portalu zarządzania Azure została utworzona aplikacja klucz tajny zarejestrowanych aplikacji w usłudze Azure Active Directory.  
  3. W portalu zarządzania Azure podane aplikacji sieci web zarejestrowanych z uprawnieniami dostępu do usługi OMS.  
  ![Połączenie do strony głównej Kreatora usługi OMS](./media/log-analytics-sccm/sccm-console-general01.png)

3. Na ekranie **Usługi Azure Active Directory** skonfigurowanie ustawień połączenia do usługi OMS, dostarczając **dzierżawy** , **Identyfikator klienta** i **Kluczy tajny klienta** , a następnie wybierz przycisk **Dalej**.  
  ![Połączenie do strony usługi OMS Kreatora usługi Azure Active Directory](./media/log-analytics-sccm/sccm-wizard-tenant-filled03.png)

4. Jeśli wszystkie inne procedury są wykonywane pomyślnie, informacji wyświetlanych na ekranie **Konfiguracji połączenia usługi OMS** będą automatycznie wyświetlane na tej stronie. Informacje dotyczące ustawień połączenia będą widoczne dla **subskrypcji Azure** , **Grupa zasobów Azure** i **Obszar roboczy pakietu zarządzania operacji**.  
  ![Połączenie do strony usługi OMS Kreatora usługi OMS połączenia](./media/log-analytics-sccm/sccm-wizard-configure04.png)

5. Kreator łączy usługę przy użyciu informacje, które zostały wprowadzania. Zaznacz zbiory urządzenia, które chcesz zsynchronizować z usługi OMS, a następnie kliknij przycisk **Dodaj**.  
  ![Wybierz kolekcje](./media/log-analytics-sccm/sccm-wizard-add-collections05.png)

6. Sprawdź ustawienia połączenia na ekranie **Podsumowanie** , a następnie wybierz przycisk **Dalej**. Na ekranie **postępu** jest wyświetlany stan połączenia, a następnie należy **wykonane**.

>[AZURE.NOTE] Usługi OMS musisz połączyć się z witryną Warstwa górna w hierarchii. Jeśli łączenie usługi OMS do podstawowej witryny autonomicznej, a następnie dodaj witryny administracji centralnej w środowisku, konieczne będzie usunięcie i ponowne utworzenie połączenia usługi OMS w nowej hierarchii.

Po połączeniu Menedżer konfiguracji usługi OMS, dodawanie lub usuwanie zbiorów, a wyświetlanie właściwości połączenia usługi OMS.

## <a name="update-oms-connection-properties"></a>Aktualizowanie właściwości połączenia usługi OMS

Jeśli klucz tajny hasła lub klienta kiedykolwiek wygasa lub zostaje utracony, musisz ręcznie zaktualizować usługi OMS właściwości połączenia.

1. W Menedżerze konfiguracji przejdź do **Usług w chmurze** , a następnie wybierz **Łącznik usługi OMS** , aby otworzyć stronę **Właściwości połączenia usługi OMS** .
2. Na tej stronie kliknij kartę **Usługi Azure Active Directory** , aby wyświetlić Twojej **dzierżawy**, **Identyfikator klienta**, **klienta tajnego klucza wygaśnięcia**. **Zweryfikuj** swój **klucz tajny klienta** , jeśli wygasła.


## <a name="download-and-install-the-agent"></a>Pobieranie i instalowanie agenta

1. W portalu usługi OMS [Pobieranie pliku konfiguracji agenta z usługi OMS](log-analytics-windows-agents.md#download-the-agent-setup-file-from-oms).
2. Aby zainstalować i skonfigurować agenta na komputerze z systemem roli system witryny punktu połączenia Menedżer konfiguracji usługi, użyj jednej z następujących metod:
  - [Instalowanie agenta przy użyciu konfiguracji](log-analytics-windows-agents.md#install-the-agent-using-setup)
  - [Instalowanie agenta przy użyciu wiersza polecenia](log-analytics-windows-agents.md#install-the-agent-using-the-command-line)
  - [Instalowanie agenta przy użyciu DSC w automatyzacji Azure](log-analytics-windows-agents.md#install-the-agent-using-dsc-in-azure-automation)


## <a name="import-collections"></a>Importowanie zbiorów

Po połączenie usługi OMS dodane do Menedżera konfiguracji i zainstalowany agent na komputerze z systemem połączenia usługi Menedżer konfiguracji punktu Rola systemu witryny, następnym krokiem jest importowanie zbiorów za pomocą Menedżera konfiguracji w usługi OMS jako grupy komputera.

Po włączeniu czasowej informacji o członkostwie zbioru są pobierane co 3 godziny, aby zapewnić aktualność członkostwa zbioru. Możesz wyłączyć czasowej w dowolnym momencie.

1. W portalu usługi OMS kliknij przycisk **Ustawienia**.
2. Kliknij kartę **Grupy komputera** , a następnie kliknij kartę **SCCM** .
3. Wybierz pozycję **Menedżer konfiguracji importowanie zbioru członkostwa** , a następnie kliknij przycisk **Zapisz**.  
  ![Grupy komputerów — karta SCCM](./media/log-analytics-sccm/sccm-computer-groups01.png)

## <a name="view-data-from-configuration-manager"></a>Wyświetlanie danych za pomocą Menedżera konfiguracji

Po utworzeniu połączenie usługi OMS dodane do Menedżera konfiguracji i zainstalowany na komputerze z systemem roli system witryny punktu połączenia Menedżer konfiguracji usługi agenta, dane z agentem są wysyłane do usługi OMS. W usługi OMS kolekcji Menedżer konfiguracji są wyświetlane jako [grupy komputerów](log-analytics-computer-groups.md). W obszarze **Ustawienia**umożliwia przeglądanie grup na stronie **Menedżera konfiguracji** w obszarze **Grupy komputerów** .

Po zaimportowaniu kolekcje widać wykryto ilu komputerach z kolekcji członkostwa. Można też wyświetlić wielu kolekcji, które zostały zaimportowane.

![Grupy komputerów — karta SCCM](./media/log-analytics-sccm/sccm-computer-groups02.png)

Po kliknięciu jedną zostanie otwarty wyszukiwania zawierający wszystkie zaimportowane grupy albo wszystkich komputerach, które należą do każdej grupy. [Przeszukiwanie dziennika](log-analytics-log-searches.md)można uruchomić bardziej szczegółowej analizy danych Menedżer konfiguracji.

## <a name="next-steps"></a>Następne kroki

- Aby wyświetlić szczegółowe informacje na temat danych Menedżer konfiguracji za pomocą [Wyszukiwania dziennika](log-analytics-log-searches.md) .
