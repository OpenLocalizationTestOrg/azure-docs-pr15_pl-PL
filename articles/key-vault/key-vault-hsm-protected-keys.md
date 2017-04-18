<properties
    pageTitle="Jak wygenerować i przenieść chroniony HSM klawiszy dla magazynu klucza Azure | Microsoft Azure"
    description="W tym artykule umożliwia ułatwiają planowanie, Generuj, a następnie przełącz własne chroniony HSM klawisze do użytku z magazynu klucza Azure."
    services="key-vault"
    documentationCenter=""
    authors="cabailey"
    manager="mbaldwin"
    tags="azure-resource-manager"/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/24/2016"
    ms.author="cabailey"/>
#<a name="how-to-generate-and-transfer-hsm-protected-keys-for-azure-key-vault"></a>Jak wygenerować i przełączanie chroniony HSM kluczy dla magazynu klucza Azure

##<a name="introduction"></a>Wprowadzenie

W odniesieniu do zapewnienia dodanego korzystając z magazynu klucza Azure, można importować lub wygenerowania kluczy w modułach zabezpieczeń sprzętu (HSM), które nigdy nie pozostawić granicę HSM. W tym scenariuszu często nazywa się *przełączyć własny klucz*lub BYOK. HSM są FIPS 140-2 poziomu 2 sprawdzana poprawność. Azure klucza magazynu używa firmy Thales rodziny nShield HSM do ochrony kluczy.

Skorzystaj z informacji w tym temacie ułatwiają planowanie, Generuj, a następnie przełącz własne chroniony HSM klawisze do użytku z magazynu klucza Azure.

Ta funkcja nie jest dostępna dla Chin Azure. 

>[AZURE.NOTE] Aby uzyskać więcej informacji na temat magazynu klucza Azure zobacz [Co to jest Azure klucza magazynu?](key-vault-whatis.md)  
>
>Pobieranie samouczek Wprowadzenie, zawierającego tworzenia klucza magazynu kluczy chronionych HSM, zobacz [Rozpoczynanie pracy z magazynu klucza Azure](key-vault-get-started.md).

Więcej informacji na temat generowania i przenoszenia klawisza chroniony HSM w Internecie:

- Klucz jest wygenerować z roboczej trybu offline, zmniejszając powierzchni atakiem.

- Klucz jest zaszyfrowany przy użyciu klucza Exchange klucz (KEK), które pozostają zaszyfrowane aż są przenoszone do HSM magazynu klucza Azure. Zaszyfrowana wersja klucza pozostawia stację oryginalny.

- Przybornik ustawia właściwości klucza dzierżawy, która wiąże klucza na świecie zabezpieczeń Azure klucza magazynu. Dlatego po HSM magazynu klucza Azure odbieranie i odszyfrowywania klucza, tylko te HSM go używać. Nie można wyeksportować klucza. To powiązanie są realizowane przez HSM firmy Thales.

- Klucz Exchange klucz (KEK), który jest używany do szyfrowania klucza jest generowany wewnątrz HSM magazynu klucza Azure i nie można eksportować. HSM wymusić, że mogą być nie wyczyść wersji KEK poza HSM. Ponadto Przybornik zawiera poświadczenie z firmy Thales, że KEK nie jest eksportowany i został wygenerowany wewnątrz oryginalnego HSM, który został wytwarzane przez firmy Thales.

- Przybornik zawiera poświadczenie z firmy Thales, że na świecie zabezpieczeń Azure klucza magazynu również wygenerowany oryginalnego HSM, wytwarzane przez firmy Thales. Poświadczenie to potwierdza, że Microsoft korzysta z oryginalnego sprzętu.

- Firma Microsoft korzysta z osobnych KEKs i oddzielić względem zabezpieczeń w poszczególnych regionach geograficznych. Podział ten gwarantuje, że klucz może być używany tylko w centrach danych w regionie go szyfrowane. Na przykład klucz Europejski odbiorcy nie można używać w centrach danych w Ameryce Północnej i Azji.

##<a name="more-information-about-thales-hsms-and-microsoft-services"></a>Więcej informacji o usługach firmy Thales HSM i Microsoft

E zabezpieczeń firmy Thales jest wiodącym dostawcą globalnej szyfrowania danych i rozwiązania zabezpieczeń cyber usług finansowych, wysokiej technologii, produkcji, dla instytucji rządowych i sektory technologii. Za pomocą 40 roku śledzenia rekordu ochrony firmy i dla instytucji rządowych informacji: rozwiązań firmy Thales są używane przez cztery z pięciu dużych przedsiębiorstwach energii i aerospace. Ich rozwiązania są też używane przez 22 krajów NATO i zabezpieczanie ponad 80 procent transakcji płatności na całym świecie.

Microsoft występują we współpracy z firmy Thales do poprawy stanu clipart dla HSM. Ulepszenia te pozwalają na typowe korzystania z zalet usługi hostingowe bez zrzeczenia się kontroli nad kluczy. W szczególności te ulepszenia powiadomić firmę Microsoft Zarządzanie HSM, tak aby nie trzeba. Jako usługi w chmurze Azure klucza magazynu skale w górę w wezwaniu w celu spełnienia tych najwyższych wartościach organizacji zastosowania. W tym samym czasie klucza jest chroniony wewnątrz HSM firmy Microsoft: zachować kontrolę nad klucza cyklu życia, ponieważ wygenerowania klucza i przeniesienie go do HSM firmy Microsoft.

##<a name="implementing-bring-your-own-key-byok-for-azure-key-vault"></a>Implementacji wyświetlić własny klucz (BYOK) dla magazynu klucza Azure

Jeśli zostanie wygenerować własny klucz chroniony HSM, a następnie przełącz je do magazynu klucza Azure za pomocą następujące informacje i procedury — Przesuń swojego własnego scenariusza klucz (BYOK).


##<a name="prerequisites-for-byok"></a>Wymagania wstępne dotyczące BYOK

Zobacz poniższej tabeli do listy wymagania wstępne dotyczące wyświetlić własny klucz (BYOK) dla magazynu klucza Azure.

|Wymaganie|Więcej informacji|
|---|---|
|Subskrypcję usługi Azure|Aby utworzyć magazynu klucza Azure, należy subskrypcji usługi Azure: [Załóż bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/)|
|Warstwa usług Azure klucza magazynu Premium do obsługi chroniony HSM klawiszy|Aby uzyskać więcej informacji o warstwy usługi i możliwości Azure klucza magazynu zobacz witrynę sieci Web [Azure klucza magazynu ceny](https://azure.microsoft.com/pricing/details/key-vault/) .|
|HSM firmy Thales, kartach inteligentnych i oprogramowanie|Musi mieć dostęp do firmy Thales sprzętowego modułu zabezpieczeń i podstawowej znajomości operacyjnych firmy Thales HSM. Zobacz [Sprzętowego modułu zabezpieczeń firmy Thales](https://www.thales-esecurity.com/msrms/buy) na liście modeli zgodne lub aby zakupić HSM, jeśli nie masz.|
|Poniżej sprzętu i oprogramowania:<ol><li>Offline x64 roboczej minimalne operacji systemu Windows 7 i firmy Thales oprogramowania nShield, co najmniej wersji 11.50.<br/><br/>Jeśli stacji działa system Windows 7, musisz [zainstalować program Microsoft .NET Framework 4,5](http://download.microsoft.com/download/b/a/4/ba4a7e71-2906-4b2d-a0e1-80cf16844f5f/dotnetfx45_full_x86_x64.exe).</li><li>Pracy, który jest połączony z Internetem i minimalna systemie operacyjnym Windows systemu Windows 7.</li><li>Dysk USB lub inne urządzenie przenośne magazynu, który ma co najmniej 16 MB wolnego miejsca.</li></ol>|Ze względów bezpieczeństwa zaleca się pierwszy pracy nie jest połączony z siecią. Jednak to zalecenie nie jest programowy wymuszane.<br/><br/>Należy zauważyć, że instrukcje występujące po tym, w stacji nosi nazwę stację Rozłączono.</p></blockquote><br/>Ponadto w przypadku dzierżawy klucz sieci produkcyjnej, zalecamy używanie roboczej drugi, oddzielnych pobrać przybornik i przekaż klucz dzierżawy. Jednak podczas testowania, można użyć tej samej stacji roboczej jako pierwsza.<br/><br/>Należy zauważyć, że instrukcje występujące po tym, w drugim stacji nosi nazwę stację podłączonego do Internetu.</p></blockquote><br/>|

##<a name="generate-and-transfer-your-key-to-azure-key-vault-hsm"></a>Generowanie i przesyłać klucza HSM magazynu klucza Azure

Generowanie i przesyłać klucza HSM magazynu klucza Azure będzie wykonaj następujące czynności pięć:

- [Krok 1: Przygotowanie pracy podłączonego do Internetu](#step-1-prepare-your-internet-connected-workstation)
- [Krok 2: Przygotowywanie Rozłączono pracy](#step-2-prepare-your-disconnected-workstation)
- [Krok 3: Generowanie klucza](#step-3-generate-your-key)
- [Krok 4: Przygotowanie klucza przesyłania](#step-4-prepare-your-key-for-transfer)
- [Krok 5: Przesyłać klucza magazynu klucza Azure](#step-5-transfer-your-key-to-azure-key-vault)

## <a name="step-1-prepare-your-internet-connected-workstation"></a>Krok 1: Przygotowanie pracy podłączonego do Internetu
W tym kroku pierwszego zrobić z poniższych procedur w miejscu pracy, który jest połączony z Internetem.


###<a name="step-11-install-azure-powershell"></a>Krok 1.1: Instalowanie programu PowerShell Azure

Ze roboczej podłączonego do Internetu Pobierz i zainstaluj modułu programu PowerShell usługi Azure, która zawiera polecenia cmdlet, aby zarządzać Azure klucza magazynu. Wymaga to minimalna wersja 0.8.13.

Instrukcje dotyczące instalacji zobacz [jak zainstalować i skonfigurować Azure programu PowerShell](../powershell-install-configure.md).

###<a name="step-12-get-your-azure-subscription-id"></a>Krok 1.2: Uzyskaj identyfikator subskrypcji Azure

Rozpocznij sesję programu PowerShell Azure i zaloguj się do konta Azure za pomocą następującego polecenia:

        Add-AzureAccount
W oknie podręcznym przeglądarki wprowadź Azure nazwa użytkownika i hasło. Następnie użyj polecenia [Get-AzureSubscription](https://msdn.microsoft.com/library/azure/dn790366.aspx) :

        Get-AzureSubscription
Z zestawu wyników zlokalizuj identyfikator subskrypcji, którą chcesz użyć do magazynu klucza Azure. Ten identyfikator subskrypcji jest potrzebna później.

Nie zamykaj okna Azure programu PowerShell.

###<a name="step-13-download-the-byok-toolset-for-azure-key-vault"></a>Krok 1.3: Pobierz narzędzia BYOK dla magazynu klucza Azure

Przejdź do witryny Microsoft Download Center i [Pobierz narzędzia Azure klucza magazynu BYOK](http://www.microsoft.com/download/details.aspx?id=45345) dla regionu geograficznego lub wystąpienia Azure. Określ nazwę pakiet do pobrania i jego odpowiedni skrót pakietu 256 Agent kondycji systemu za pomocą następujące informacje:

---

**Ameryka Północna:**

KeyVault-BYOK-narzędzia Zjednoczone States.zip

305F44A78FEB750D1D478F6A0C345B097CD5551003302FA465C73D9497AB4A03

---

**Europa:**

KeyVault-BYOK-narzędzia Europe.zip

C73BB0628B91471CA7F9ADFCE247561C6016A5103EF1A315D49C3EA23AFC0B9C

---

**Kraje Azji:**

KeyVault-BYOK-narzędzia AsiaPacific.zip

BE9A84B6C76661929F9FDAD627005D892B3B8F9F19F351220BB4F9C356694192

---

**Ameryka Łacińska:**

KeyVault-BYOK-narzędzia LatinAmerica.zip
    
9E8EE11972DECE8F05CD898AF64C070C375B387CED716FDCB788544AE27D3D23

---

**Japonia:**

KeyVault-BYOK-narzędzia Japan.zip

E6B88C111D972A02ABA3325F8969C4E36FD7565C467E9D7107635E3DDA11A8B2

---

**Australia:**

KeyVault-BYOK-narzędzia Australia.zip

7660D7A675506737857B14F527232BE51DC269746590A4E5AB7D50EDD220675D

---

[**Azure dla instytucji rządowych:**](https://azure.microsoft.com/features/gov/)

KeyVault-BYOK-narzędzia USGovCloud.zip

53801A3043B0F8B4A50E8DC01A935C2BFE61F94EE027445B65C52C1ACC2B5E80

---

**Kanada:**

KeyVault-BYOK-narzędzia Canada.zip

A42D9407B490E97693F8A5FA6B60DC1B06B1D1516EDAE7C9A71AA13E12CF1345

---

**Niemcy:**

KeyVault-BYOK-narzędzia Germany.zip

4795DA855E027B2CA8A2FF1E7AE6F03F772836C7255AFC68E576410BDD28B48E

---
**Indie:**

KeyVault-BYOK-narzędzia India.zip

26853511EB767A33CF6CD880E78588E9BBE04E619B17FBC77A6B00A5111E800C

---

Aby sprawdzić poprawność integralności narzędzi BYOK pobrany, z sesji programu Azure PowerShell należy użyć polecenia cmdlet [Get-FileHash](https://technet.microsoft.com/library/dn520872.aspx) .

    Get-FileHash KeyVault-BYOK-Tools-*.zip

Przybornik zawiera następujące czynności:

- Pakiet klucza klucza programu Exchange (KEK), który zawiera nazwisko zaczynające się **BYOK-KEK - pkg-.**
- Pakiet świata zabezpieczeń, który zawiera nazwisko zaczynające się **BYOK-SecurityWorld - pkg-.**
- Skrypt python o nazwie v**erifykeypackage.py.**
- Nazwana **KeyTransferRemote.exe** i skojarzone dll plików wiersza polecenia plik wykonywalny.
- Pakiet redystrybucyjny Visual C++, o nazwie **vcredist_x64.exe.**

Skopiuj pakiet dysk USB lub innych magazynu przenośnego.

##<a name="step-2-prepare-your-disconnected-workstation"></a>Krok 2: Przygotowywanie Rozłączono pracy

Ten krok drugi wykonaj poniższe procedury na komputerze, który nie jest połączony z siecią (Internet lub sieci wewnętrznej).


###<a name="step-21-prepare-the-disconnected-workstation-with-thales-hsm"></a>Krok 2.1: Przygotowywanie stację Rozłączono z firmy Thales HSM

Zainstalować to oprogramowanie nCipher (firmy Thales) na komputerze z systemem Windows, a następnie dołączyć HSM firmy Thales na tym komputerze.

Upewnij się, że w obszarze Narzędzia firmy Thales znajdują się w ścieżce (**%nfast_home%\bin** i **%nfast_home%\python\bin**). Na przykład wpisz poniższe dane:

        set PATH=%PATH%;”%nfast_home%\bin”;”%nfast_home%\python\bin”

Aby uzyskać więcej informacji zobacz Podręcznik użytkownika dostępny w pakiecie HSM firmy Thales.

###<a name="step-22-install-the-byok-toolset-on-the-disconnected-workstation"></a>Krok 2.2: Instalowanie narzędzi BYOK na stację Rozłączono

Skopiuj pakiet narzędzi BYOK z dysk USB lub innych magazynu przenośnego, a następnie wykonaj następujące czynności:

1. Wyodrębnij pliki z pakietu pobranego do dowolnego folderu.
2. W tym folderze uruchom vcredist_x64.exe.
3. Postępuj zgodnie z instrukcjami do instalacji elementy uruchomieniowe Visual C++ Visual Studio 2013 r.

##<a name="step-3-generate-your-key"></a>Krok 3: Generowanie klucza

W tym kroku trzecim wykonaj poniższe procedury na stację Rozłączono.

###<a name="step-31-create-a-security-world"></a>Krok 3.1: Tworzenie świata zabezpieczeń

Uruchom wiersz polecenia, a następnie uruchom program nowego świata firmy Thales.

    new-world.exe --initialize --cipher-suite=DLf1024s160mRijndael --module=1 --acs-quorum=2/3

Ten program tworzy plik **Świata zabezpieczeń** % NFAST_KMDATA%\local\world, odnoszącą się do folderu Settings\User zarządzania C:\ProgramData\nCipher\Key. Za pomocą różnych wartości dla kworum, ale w naszym przykładzie zostanie wyświetlony monit o wprowadzenie trzy karty puste i numery PIN dla każdego z nich. Następnie w dowolnej dwie karty przekazać pełny dostęp do świata zabezpieczeń. Te karty są zamieniane na **Administrator karty ustawić** dla nowego świata zabezpieczeń.

Następnie wykonaj następujące czynności:

- Utwórz kopię zapasową pliku świata. Zabezpieczanie i ochrony plików świecie, karty administratora i ich PIN i upewnij się, że nikt pojedynczy ma dostęp do więcej niż jednej karty.

###<a name="step-32-validate-the-downloaded-package"></a>Krok 3,2: Sprawdzanie poprawności pobrany pakiet

Ten krok jest opcjonalny, ale zalecane tak, aby sprawdzić poprawność następujące czynności:

- Exchange klucz, który znajduje się w Przybornik został wygenerowany z oryginalnego HSM firmy Thales.
- Skrót świata zabezpieczeń, w której znajduje się w Przybornik został wygenerowany w oryginalnego HSM firmy Thales.
- Nie można eksportować jest klucz programu Exchange.

>[AZURE.NOTE]Aby sprawdzić poprawność pobrany pakiet, HSM musi być połączony, włączony i może zawierać świata zabezpieczeń na nim (taki jak przedstawiony na właśnie utworzonego).

Aby sprawdzić poprawność pobrany pakiet:

1.  Uruchom skrypt verifykeypackage.py przez jedną z następujących czynności, zależnie od regionu geograficznego i wystąpienie Azure wiązanie:
    - Na terenie Ameryki Północnej:

            python verifykeypackage.py -k BYOK-KEK-pkg-NA-1 -w BYOK-SecurityWorld-pkg-NA-1
    - Aby uzyskać Europa:

            python verifykeypackage.py -k BYOK-KEK-pkg-EU-1 -w BYOK-SecurityWorld-pkg-EU-1
    - Dla Azji:

            python verifykeypackage.py -k BYOK-KEK-pkg-AP-1 -w BYOK-SecurityWorld-pkg-AP-1
    - Aby uzyskać Ameryka Łacińska:

            python verifykeypackage.py -k BYOK-KEK-pkg-LATAM-1 -w BYOK-SecurityWorld-pkg-LATAM-1
    - W Japonii:

            python verifykeypackage.py -k BYOK-KEK-pkg-JPN-1 -w BYOK-SecurityWorld-pkg-JPN-1
    - Dla Australii:

            python verifykeypackage.py -k BYOK-KEK-pkg-AUS-1 -w BYOK-SecurityWorld-pkg-AUS-1
    - [Azure dla instytucji rządowych](https://azure.microsoft.com/features/gov/), który używa wystąpienie Rządu Stanów Zjednoczonych Azure:

            python verifykeypackage.py -k BYOK-KEK-pkg-USGOV-1 -w BYOK-SecurityWorld-pkg-USGOV-1
    - Dla Kanady:

            python verifykeypackage.py -k BYOK-KEK-pkg-CANADA-1 -w BYOK-SecurityWorld-pkg-CANADA-1
    - Niemcy:

            python verifykeypackage.py -k BYOK-KEK-pkg-GERMANY-1 -w BYOK-SecurityWorld-pkg-GERMANY-1
    - Aby uzyskać Indie:

            python verifykeypackage.py -k BYOK-KEK-pkg-INDIA-1 -w BYOK-SecurityWorld-pkg-INDIA-1
    >[AZURE.TIP]Oprogramowanie firmy Thales zawiera python u %NFAST_HOME%\python\bin

2.  Upewnij się, czy jest widoczny poniżej, które wskazuje sprawdzeniu poprawności: **wynik: SUKCESU**

Skrypt sprawdza łańcuch osoby podpisującej maksymalnie firmy Thales klucza głównego. Skrót ten klucz główny jest osadzony w skrypt i jej wartość powinna być **59178a47 de508c3f 291277ee 184f46c4 f1d9c639**. Możesz też potwierdzić tę wartość oddzielnie, odwiedź witrynę [firmy Thales witryny sieci Web](http://www.thalesesec.com/).

Teraz możesz utworzyć nowy klucz.

###<a name="step-33-create-a-new-key"></a>Krok 3.3: Tworzenie nowego klucza

Wygenerować klucz za pomocą programu **generatekey** firmy Thales.

Uruchom następujące polecenie, aby wygenerować klucz:

    generatekey --generate simple type=RSA size=2048 protect=module ident=contosokey plainname=contosokey nvram=no pubexp=

Po uruchomieniu tego polecenia za pomocą następujące instrukcje:

- Parametr *ochrony* musi wynosić wartość **moduł**, jak pokazano. Spowoduje to utworzenie klucz chroniony modułu. Zestaw narzędzi BYOK nie obsługuje chronionych OCS klawiszy.

- Zamień wartość *contosokey* **ident** i **plainname** wartości ciągu. Aby zminimalizować ogólne koszty administracyjne i zmniejszyć ryzyko błędów, zaleca się używać tej samej wartości dla obu. Wartość **ident** może zawierać tylko liczby, kreski i małe litery.

- Pubexp pozostanie puste (ustawienie domyślne) w tym przykładzie, ale można określić określonej wartości. Aby uzyskać więcej informacji zobacz dokumentację firmy Thales.

To polecenie tworzy plik klucza plikach w folderze %NFAST_KMDATA%\local pod nazwą, zaczynając od **key_simple_**, a po nim **ident** , który został określony w poleceniu. Na przykład: **key_simple_contosokey**. Ten plik zawiera zaszyfrowany klucz.

Wykonaj kopię zapasową tego plikach pliku klucza w bezpiecznym miejscu.

>[AZURE.IMPORTANT] Podczas przenoszenia później klucz do magazynu klucza Azure, Microsoft nie można wyeksportować ten klucz powrót do, aby stał się niezwykle ważne bezpiecznie kopii zapasowej Twój świat klucz i zabezpieczeń. Wskazówki i najważniejsze wskazówki dotyczące tworzenia kopii zapasowej klucza, skontaktuj się z firmy Thales.

Teraz możesz przystąpić do przesyłać klucza Azure klucza magazynu.

##<a name="step-4-prepare-your-key-for-transfer"></a>Krok 4: Przygotowanie klucza przesyłania

W tym kroku czwartym wykonaj poniższe procedury na stację Rozłączono.

###<a name="step-41-create-a-copy-of-your-key-with-reduced-permissions"></a>Krok 4.1: Utwórz kopię klucza z uprawnieniami ograniczona

Aby ograniczyć uprawnienia do klucza, w wierszu polecenia, uruchom jedną z następujących czynności, zależnie od regionu geograficznego i wystąpienie Azure:

- Na terenie Ameryki Północnej:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-NA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-NA-1
- Aby uzyskać Europa:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-EU-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-EU-1
- Dla Azji:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AP-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AP-1
- Aby uzyskać Ameryka Łacińska:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-LATAM-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-LATAM-1
- W Japonii:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-JPN-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-JPN-1
- Dla Australii:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AUS-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AUS-1
- [Azure dla instytucji rządowych](https://azure.microsoft.com/features/gov/), który używa wystąpienie Rządu Stanów Zjednoczonych Azure:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-USGOV-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-USGOV-1
- Dla Kanady:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-CANADA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-CANADA-1
- Niemcy:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-GERMANY-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-GERMANY-1
- Aby uzyskać Indie:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-INDIA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-INDIA-1


Po uruchomieniu tego polecenia Zastąp *contosokey* o tej samej wartości określonych w **3.3 kroku: Tworzenie nowego klucza** z kroku [Generuj klucza](#step-3-generate-your-key) .

Zostanie wyświetlony monit o podłączeniu karty zabezpieczeń świata administratora.

Jeśli polecenie wykonuje, zostanie wyświetlony **wynik: SUKCESU** i kopię klucza z uprawnieniami ograniczona znajdują się w pliku o nazwie key_xferacId_<contosokey>.

###<a name="step-42-inspect-the-new-copy-of-the-key"></a>Krok 4.2: Sprawdzanie nową kopię klucza

Opcjonalnie można uruchomić firmy Thales narzędzia, aby potwierdzić minimalne uprawnienia dla nowego klucza:

- aclprint.py:

        "%nfast_home%\bin\preload.exe" -m 1 -A xferacld -K contosokey "%nfast_home%\python\bin\python" "%nfast_home%\python\examples\aclprint.py"
- kmfile-dump.exe:

        "%nfast_home%\bin\kmfile-dump.exe" "%NFAST_KMDATA%\local\key_xferacld_contosokey"
Po uruchomieniu polecenia Zastąp contosokey o tej samej wartości określonych w **3.3 kroku: Tworzenie nowego klucza** z kroku [Generuj klucza](#step-3-generate-your-key) .

###<a name="step-43-encrypt-your-key-by-using-microsofts-key-exchange-key"></a>Krok 4.3: Szyfrowanie klucza przy użyciu programu Exchange klucz firmy Microsoft

Uruchom jedną z następujących poleceń, w zależności od usługi regionu geograficznego lub wystąpienia Azure:

- Na terenie Ameryki Północnej:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-NA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-NA-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- Aby uzyskać Europa:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-EU-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-EU-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- Dla Azji:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AP-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AP-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- Aby uzyskać Ameryka Łacińska:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-LATAM-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-LATAM-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- W Japonii:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-JPN-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-JPN-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- Dla Australii:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AUS-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AUS-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- [Azure dla instytucji rządowych](https://azure.microsoft.com/features/gov/), który używa wystąpienie Rządu Stanów Zjednoczonych Azure:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-USGOV-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-USGOV-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- Dla Kanady:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-CANADA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-CANADA-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- Niemcy:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-GERMANY-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-GERMANY-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- Aby uzyskać Indie:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-INDIA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-INDIA-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey


Po uruchomieniu tego polecenia za pomocą następujące instrukcje:

- Zamień *contosokey* identyfikator, którego użyto do wygenerowania klucza w **3.3 kroku: Tworzenie nowego klucza** z kroku [Generuj klucza](#step-3-generate-your-key) .

- Zamień *SubscriptionID* identyfikator Azure subskrypcji, która zawiera usługi klucza magazynu. Pobrać tę wartość wcześniej w **1.2 kroku: Uzyskaj identyfikator subskrypcji Azure** z kroku [przygotowanie pracy podłączonego do Internetu](#step-1-prepare-your-internet-connected-workstation) .

- Zamień *ContosoFirstHSMKey* etykietę, która służy do nazwy pliku danych wyjściowych.

Gdy to zakończy się pomyślnie, zostanie wyświetlona **wynik: SUKCESU** i nowy plik w bieżącym folderze, który ma następującą nazwę: .byok TransferPackage -*ContosoFirstHSMkey*

###<a name="step-44-copy-your-key-transfer-package-to-the-internet-connected-workstation"></a>Krok 4.4: Kopiowanie pakietu przeniesienia klucza do stacji podłączonego do Internetu

Aby skopiować plik docelowy w poprzednim kroku (KeyTransferPackage ContosoFirstHSMkey.byok) do pracy podłączonego do Internetu za pomocą dysk USB lub innych magazynu przenośnego.

##<a name="step-5-transfer-your-key-to-azure-key-vault"></a>Krok 5: Przesyłać klucza magazynu klucza Azure

W tym kroku końcowego na stację podłączonego do Internetu użyć polecenia cmdlet [AzureKeyVaultKey Dodaj](https://msdn.microsoft.com/library/azure/dn868048\(v=azure.300\).aspx) do przekazania pakietu klucza transfer kopiowane z stację Rozłączono do HSM magazynu klucza Azure:

    Add-AzureKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMkey' -KeyFilePath 'c:\TransferPackage-ContosoFirstHSMkey.byok' -Destination 'HSM'

Jeśli przekazywanie zakończyło się pomyślnie, zostanie wyświetlona wyświetlane właściwości dodanego klucza.


##<a name="next-steps"></a>Następne kroki

Za pomocą tego klucza chroniony HSM można teraz w swojej klucza magazynu. Aby uzyskać więcej informacji zobacz sekcję **Czy chcesz używać sprzętu zabezpieczeń moduły** samouczka [wprowadzenie Azure klucza magazynu](key-vault-get-started.md) .
