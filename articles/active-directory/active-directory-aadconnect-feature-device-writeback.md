<properties
    pageTitle="Narzędzie Azure AD Connect: Włączanie zapisu urządzenia | Microsoft Azure"
    description="Jak włączyć zapisu urządzenia za pomocą narzędzie Azure AD Connect szczegóły tego dokumentu"
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"  
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/29/2016"
    ms.author="billmath"/>

# <a name="azure-ad-connect-enabling-device-writeback"></a>Narzędzie Azure AD Connect: Włączanie zapisu urządzenia

>[AZURE.NOTE] Subskrypcję usługi Azure AD Premium jest wymagana do zapisu urządzenia.

Poniższą dokumentację zawiera informacje o tym, jak włączyć funkcję zapisu urządzenia w narzędzie Azure AD Connect. Zapisu urządzenie jest używane w następujących sytuacjach:

- Włączanie dostępu warunkowego według urządzenia, aby ADFS (2012 R2 lub nowszy) chroniony aplikacji (uzależnionej zaufania firmy).

Zapewnia dodatkowe zabezpieczenia i zapewnienia dostępu do aplikacji udzielane tylko zaufanych urządzeń. Aby uzyskać więcej informacji na dostępie warunkowym zobacz [Zarządzanie ryzykiem z dostępem warunkowym](active-directory-conditional-access.md) i [Konfigurowanie warunkowego dostępu do lokalnego przy użyciu Azure Active Directory Rejestracja urządzenia](https://msdn.microsoft.com/library/azure/dn788908.aspx).

>[AZURE.IMPORTANT]
<li>Urządzenia musi znajdować się w tym samym las jako użytkowników. Ponieważ urządzenia musi zapisywane w jeden las, ta funkcja nie jest obecnie obsługiwany wdrożenia z wieloma lasami użytkownika.</li>
<li>Tylko jedno urządzenie rejestracji konfiguracji obiektów można dodawać do las usługi Active Directory w lokalnej. Ta funkcja nie jest zgodny z topologii miejsce, w którym jest synchronizowane w lokalnej usłudze Active Directory do wielu katalogów Azure AD.</li>

## <a name="part-1-install-azure-ad-connect"></a>Część 1: Zainstaluj Azure AD nawiązywanie połączenia
1. Zainstaluj narzędzie Azure AD Connect przy użyciu niestandardowe lub ustawienia ekspresowe. Firma Microsoft zaleca zaczynać się wszyscy użytkownicy i grupy pomyślnie zsynchronizowany przed włączeniem zapisu urządzenia.

## <a name="part-2-prepare-active-directory"></a>Część 2: Przygotowywanie usługi Active Directory
Wykonaj następujące czynności, aby przygotować się przy użyciu urządzenia wystornowania.

1.  Z komputera, na którym zainstalowano narzędzie Azure AD Connect uruchamianie programu PowerShell w trybie podwyższonym poziomem uprawnień.

2.  Jeśli nie zainstalowano modułu programu PowerShell usługi Active Directory, zainstaluj go przy użyciu następującego polecenia:

    `Install-WindowsFeature –Name AD-Domain-Services –IncludeManagementTools`

3. Jeśli nie zainstalowano modułu programu PowerShell Azure Active Directory, następnie Pobierz i zainstaluj go z [Azure Active Directory modułu dla programu Windows PowerShell (wersja 64-bitowa)](http://go.microsoft.com/fwlink/p/?linkid=236297). Ten składnik zawiera zależność na Asystenta logowania w, która jest instalowana z Azure AD Connect.

4.  Przy użyciu poświadczeń administratora przedsiębiorstwa uruchom następujące polecenia, a następnie Zakończ działanie programu PowerShell.

    `Import-Module 'C:\Program Files\Microsoft Azure Active Directory Connect\AdPrep\AdSyncPrep.psm1'`

    `Initialize-ADSyncDeviceWriteback {Optional:–DomainName [name] Optional:-AdConnectorAccount [account]}`

Poświadczenia administratora przedsiębiorstwa są wymagane, ponieważ są potrzebne zmiany nazw konfiguracji. Administrator domeny nie będą mieć wystarczających uprawnień.

![PowerShell umożliwiających zapisu urządzenia](./media/active-directory-aadconnect-feature-device-writeback/powershell.png)

Opis:

- Jeśli już istnieje, tworzy i konfiguruje nowe kontenerów i obiektów CN = konfiguracji rejestracji urządzenia, CN = usługi, CN = Konfiguracja [nazwy domeny las].
- Jeśli już istnieje, tworzy i konfiguruje nowe kontenerów i obiektów CN = RegisteredDevices, [nazwy domeny domeny]. Obiekty urządzeń zostanie utworzony w tym kontenerze.
- Ustawia uprawnienia na konto Azure AD łącznik, aby zarządzać urządzeniami na usługi Active Directory.
- Tylko musi obsługiwać na jeden las, nawet jeśli są instalowane narzędzie Azure AD Connect na wiele lasów.

Parametry:

- Nazwa_domeny: Domena usługi Active Directory miejsce, w którym będą tworzone obiekty urządzeń. Uwaga: Wszystkie urządzenia dla danego las usługi Active Directory zostanie utworzony w jednej domenie.
- AdConnectorAccount: Konto usługi Active Directory narzędzie Azure AD Connect posłuży do zarządzania obiektami w katalogu. To jest konto używane przez narzędzie Azure AD Connect synchronizacji, aby nawiązać połączenie AD. Jeśli został zainstalowany za pomocą ustawienia ekspresowe, to konto prefiksem MSOL_.

## <a name="part-3-enable-device-writeback-in-azure-ad-connect"></a>Część 3: Włącz urządzenie zapisu w narzędzie Azure AD Connect
Poniższa procedura umożliwia włączanie zapisu urządzenia w narzędzie Azure AD Connect.

1.  Ponownie uruchom Kreatora instalacji. Wybierz pozycję **Dostosowywanie opcji synchronizacji** ze strony dodatkowe zadania, a następnie kliknij przycisk **Dalej**.
![Instalacja niestandardowa Dostosowywanie opcji synchronizacji](./media/active-directory-aadconnect-feature-device-writeback/devicewriteback2.png)
2.  Na stronie funkcje opcjonalne zapisu urządzenia będą już być wyszarzone. Należy notatkę, że jeśli Azure AD Connect przygotowywania kroki nie są zapisu złożonym urządzenia będą wyszarzone Pomniejsz na stronie funkcje opcjonalne. Zaznacz pole obok zapisu urządzenia, a następnie kliknij przycisk **Dalej**. Jeśli pole wyboru nadal jest wyłączony, zobacz temat [Rozwiązywanie problemów z sekcji](#the-writeback-checkbox-is-still-disabled).
![Instalacja niestandardowa opcjonalne funkcje zapisu urządzenia](./media/active-directory-aadconnect-feature-device-writeback/devicewriteback3.png)
3.  Na stronie zapisu podanym domeny zostanie wyświetlona jako las zapisu urządzenie domyślne.
![Niestandardowe las docelowy zapisu urządzenia instalacji](./media/active-directory-aadconnect-feature-device-writeback/devicewriteback4.png)
4.  Wykonywanie instalacji kreatora bez żadnych zmian dodatkowa konfiguracja. W razie potrzeby odwołują się do [instalacji niestandardowej: narzędzie Azure AD Connect.](./connect/active-directory-aadconnect-get-started-custom.md)

## <a name="enable-conditional-access"></a>Włączanie dostępu warunkowego
Szczegółowe instrukcje, aby włączyć w tym scenariuszu są dostępne w ramach [konfigurowania warunkowego dostępu do lokalnego przy użyciu Azure Active Directory Rejestracja urządzenia](https://msdn.microsoft.com/library/azure/dn788908.aspx).

## <a name="verify-devices-are-synchronized-to-active-directory"></a>Sprawdź, czy urządzenia są synchronizowane z usługą Active Directory
Teraz można działa poprawnie zapisu urządzenia. Należy pamiętać, że może potrwać do 3 godziny dla obiektów urządzenie ma być zapisywane wstecz AD.  Aby sprawdzić, czy urządzenia są synchronizowanego prawidłowo, wykonaj następujące czynności po zakończeniu reguły synchronizacji:

1.  Uruchamianie Centrum administracyjne usługi Active Directory.
2.  Rozwijanie RegisteredDevices w domenie jest federacji.
![Centrum administracyjne usługi Active Directory zarejestrowana urządzeń](./media/active-directory-aadconnect-feature-device-writeback/devicewriteback5.png)
3.  Bieżący zarejestrowanych urządzenia będą wyświetlane dostępne.
![Centrum administracyjne usługi Active Directory zarejestrowana listy urządzeń](./media/active-directory-aadconnect-feature-device-writeback/devicewriteback6.png)

## <a name="troubleshooting"></a>Rozwiązywanie problemów

### <a name="the-writeback-checkbox-is-still-disabled"></a>Pole wyboru zapisu nadal jest wyłączony.
Jeśli pole wyboru do zapisu urządzenia nie jest włączona, mimo że wykonano powyższe kroki, robiąc tak przeprowadzi Cię przez proces co to jest sprawdzanie Kreatora instalacji przed pole jest włączone.

Najpierw pierwszy:

- Upewnij się, że co najmniej jeden las ma 2012R2 Windows Server. Typ obiektu urządzenie musi istnieć.
- Jeśli Kreator instalacji jest już uruchomiony, wszelkie zmiany nie można wykryć. W tym przypadku Kończenie pracy Kreatora instalacji i uruchom go ponownie.
- Upewnij się, że konto, które zapewniają skryptu inicjowanie jest faktycznie odpowiednich użytkowników używane przez łącznik usługi Active Directory. Aby to sprawdzić, wykonaj następujące czynności:
    - Z start menu Otwórz **Usługi synchronizacji**.
    - Otwórz kartę **łączników** .
    - Znajdź łącznik typu usług domenowych Active Directory i wybierz ją.
    - W obszarze **Akcje**wybierz pozycję **Właściwości**.
    - Przejdź do **nawiązać las Active Directory**. Sprawdź, czy nazwy domeny i użytkownika określona w tym Dopasowanie ekranu na rachunku określonym do skryptu.
![Łącznik konta w Menedżerze usługi synchronizacji](./media/active-directory-aadconnect-feature-device-writeback/connectoraccount.png)

Sprawdź konfigurację w usłudze Active Directory:
- Sprawdź, czy usługi rejestracji urządzeń znajduje się w lokalizacji poniżej (CN = DeviceRegistrationService, CN = usługi rejestracji urządzenia, CN = konfiguracji rejestracji urządzenia, CN = usługi, CN = Konfiguracja) w kontekście nazewnictwa konfiguracji.

![Rozwiązywanie problemów, DeviceRegistrationService w konfiguracji nazw](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot1.png)

- Sprawdź, czy jest tylko jeden obiekt konfiguracji przez wyszukiwanie nazw konfiguracji. Jeśli istnieje więcej niż jedno, Usuń duplikat.

![Rozwiązywanie problemów, Wyszukaj zduplikowane obiekty](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot2.png)

- Upewnij się, atrybut msDS — DeviceLocation jest dostępny i ma wartość na obiekt usługi rejestracji urządzenia. Odnośnik tej lokalizacji i upewnij się, że jest zainstalowany z typu obiektu msDS-DeviceContainer.

![Rozwiązywanie problemów, msDS DeviceLocation](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot3.png)

![Rozwiązywanie problemów, klasy obiektu RegisteredDevices](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot4.png)

- Sprawdź, czy konto używane przez łącznik usługi Active Directory ma wymagane uprawnienia w kontenerze zarejestrowane urządzenia, znaleziony w poprzednim kroku. Jest to oczekiwanych uprawnień w tym kontenerze:

![Rozwiązywanie problemów, sprawdź uprawnienia w kontenerze](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot5.png)

- Weryfikowanie konta usługi Active Directory ma uprawnienia do CN = konfiguracji rejestracji urządzenia, CN = usługi, CN = obiekt konfiguracji.

![Rozwiązywanie problemów, sprawdź uprawnienia konfiguracji rejestracji urządzenia](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot6.png)

## <a name="additional-information"></a>Dodatkowe informacje
- [Zarządzanie ryzykiem z dostępem warunkowym](active-directory-conditional-access.md)
- [Aby skonfigurować warunkowego dostępu do lokalnego przy użyciu Azure Active Directory Rejestracja urządzenia](https://msdn.microsoft.com/library/azure/dn788908.aspx)

## <a name="next-steps"></a>Następne kroki
Dowiedz się więcej o [integrowaniu lokalnego tożsamości z usługą Azure Active Directory](active-directory-aadconnect.md).
