<properties
    pageTitle="Rozwiązywanie problemów z Microsoft Azure stos | Microsoft Azure"
    description="Azure stosem do rozwiązywania problemów."
    services="azure-stack"
    documentationCenter=""
    authors="heathl17"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/13/2016"
    ms.author="helaw"/>

# <a name="microsoft-azure-stack-troubleshooting"></a>Rozwiązywanie problemów z Microsoft Azure stos

Niniejszy dokument zawiera typowe informacje dotyczące rozwiązywania problemów dla stosu Azure.

Aby uzyskać najlepsze wyniki upewnij się, że środowisko wdrażania spełnia wszystkie [wymagania](azure-stack-deploy.md) i [przygotowań](azure-stack-run-powershell-script.md) przed rozpoczęciem instalacji. 

Zalecenia dotyczące rozwiązywania problemów, które zostały opisane w tej sekcji pochodzących z różnych źródeł i może lub nie może rozwiązać problemu określonego. Jeśli masz problem nie upewnij się sprawdzić, na [Forum w witrynie MSDN stos Azure](https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack) dalszej pomocy technicznej i dodatkowe informacje.

Przykłady kodu są dostarczane, tak jak są i nie można zagwarantować oczekiwanych wyników. W tej sekcji podlega częste zmiany i aktualizacje są wykonane ulepszenia produktu.

  

## <a name="known-issues"></a>Znane problemy

 - Może zostać wyświetlony następujących-zakończenie błędów podczas wdrażania, które wpływają na pomyślnego wdrożenia:
     - "Termin"C:\WinRM\Start-Logging.ps1"nie został rozpoznany"
     - "Wywołania EceAction: nie można indeksować do tablicy null" 
     - "InvokeEceAction: nie można powiązać argument parametru"Komunikat", ponieważ jest pusty ciąg."
 - Może zostać wyświetlony wdrożenia się nie powieść w kroku 60.61.93 się komunikat o błędzie "Aplikacja identyfikatorem" "nie znaleziono identyfikatora URI". To zachowanie jest ze względu na sposób, które aplikacje są zarejestrowane w usłudze Azure Active Directory.  Jeśli wystąpi ten błąd, Kontynuuj ponowne uruchomienie [skryptu instalacji](azure-stack-rerun-deploy.md) z kroku 60.61.93 aż do wdrożenia.
 - Zobaczysz, że **Ustawianie dostępności** zasobów na rynku są widoczne w obszarze kategorii **virtualMachine ARM** — wygląd jest tylko kosmetycznych problem dotyczący.
 - Podczas tworzenia nowej maszyny wirtualnej w portalu w kroku **podstawy** opcja magazynowania domyślnie SSD.  Dysk twardy, lub **rozmiar** kroku wdrażania maszyn wirtualnych, należy zmienić to ustawienie, nie zobaczą rozmiary maszyn wirtualnych dostępne wybierz i kontynuować wdrożenia. 
 - Pojawi się moduły programu AzureRM PowerShell już nie są instalowane domyślnie na MAS CON01 maszyn wirtualnych (w TP1 to został o nazwie ClientVM). To zachowanie jest zamierzone, ponieważ istnieje alternatywna metoda [zainstalować te moduły](azure-stack-connect-powershell.md)i połączyć.  
 - Zobaczysz, że dostawcy zasobów **Microsoft.Insights** nie jest automatycznie rejestrowany dla dzierżawy subskrypcji. Jeśli chcesz wyświetlić monitorowanie danych dla maszyny rozmieszczony jako dzierżawy, uruchom następujące polecenie z programu PowerShell (po możesz [zainstalować i połączyć](azure-stack-connect-powershell.md) jako dzierżawy): 
       
        Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Insights 

 - Zostanie wyświetlona Eksportuj funkcji w portalu dla grup zasobów, jednak tekst nie jest wyświetlony Eksportuj.      
 - Możesz rozpocząć wdrożeniu większych niż dostępne przydziału zasobów magazynowania.  Kończy się niepowodzeniem, wdrażania i zasoby konto zostanie zawieszone.  Dostępne są dwie opcje naprawy:
     - Administrator usługi można zwiększyć przydziału, ale zmiany nie będą uwzględniane natychmiast i często trwać do godziny propagowanie.
     - Administrator usługi można utworzyć plan dodatek zawierający dodatkowe przydziału dzierżawy następnie można dodać do subskrypcji.
 - Tworzenie maszyny wirtualne w środowiskach stosem Azure przy użyciu tożsamości w "Azure — Chiny" za pomocą portalu, nie zobaczą rozmiarów maszyn wirtualnych dostępnych do wybrania w **rozmiar** kroku wdrażania maszyn wirtualnych i nie będzie można kontynuować wdrożenia.
 - Podczas maszyn wirtualnych rzeczywiście został pomyślnie wdrożony, może zostać wyświetlony błąd wdrażania w portalu.
 - Po usunięciu planu, oferty lub innej subskrypcji maszyny wirtualne nie można usunąć.
 - Zostanie wyświetlona rozszerzenia maszyn wirtualnych na rynku.
 - Nie można wdrożyć maszyn wirtualnych z zapisanego obrazu maszyn wirtualnych.
 - Dzierżaw może zostać wyświetlony usług, które nie są uwzględniane w swoją subskrypcję.  Gdy dzierżaw próbują wdrażanie tych zasobów, ta osoba otrzyma komunikat o błędzie.  Przykład: Dzierżawy Subskrypcja obejmuje tylko zasobów magazynowania.  Dzierżawy zostanie wyświetlona opcja tworzenia inne zasoby, takie jak maszyny wirtualne.  W tym scenariuszu podczas próby wdrażanie Głosowa, dzierżawy otrzymają komunikat z informacją, że nie można utworzyć maszyn wirtualnych. 
 - Podczas instalowania TP2, nie należy aktywować hosta wkrótce wygaśnie OS w miejsce, w którym możesz uruchomić skrypt konfiguracji stos Azure lub może zostać wyświetlony komunikat o błędzie wiadomości określeń Windows wirtualnego dysku twardego.


## <a name="deployment"></a>Wdrożenie

### <a name="deployment-failure"></a>Niepowodzenie wdrażania
Jeśli wystąpi błąd podczas instalacji, Instalator stosu Azure umożliwia kontynuowanie niepowodzenia instalacji, wykonując, [Uruchom ponownie kroków wdrażania](azure-stack-rerun-deploy.md).

### <a name="at-the-end-of-the-deployment-the-powershell-session-is-still-open-and-doesnt-show-any-output"></a>Na końcu wdrażania sesji programu PowerShell jest nadal otwarty, a nie pokazuje dowolne dane wyjściowe

To zachowanie jest prawdopodobnie tylko wyniku domyślne zachowanie oknie polecenia programu PowerShell, jeśli został wybrany. Faktycznie powiodła się wdrażania Zapewnić, ale skrypt została wstrzymana, wybierając okna. Można sprawdzić, czy jest to możliwe, szukając wyraz "wybierz pozycję" w pasek tytułu okna.  Naciśnij klawisz ESC, aby usunąć zaznaczenie go, a powinien być wyświetlany komunikat o zakończeniu po nim.

## <a name="templates"></a>Szablony

### <a name="azure-template-wont-deploy-to-azure-stack"></a>Szablon Azure nie wdrażanie stos Azure

Upewnij się, że:

- Szablon musi korzystać z usługi Microsoft Azure, który jest już dostępna lub w podglądzie Azure stosu.
- Interfejsów API używanych dla określonego zasobu są obsługiwane przez lokalne wystąpienie Azure stos oraz że są kierowanie prawidłową lokalizację ("lokalne" w Azure stos Technical Preview (TP) 2, a "Wschodniego USA" lub "Indie Płd." platformy Azure).
- Przejrzyj [Ten artykuł](https://github.com/Azure/AzureStack-QuickStart-Templates/blob/master/README.md) na temat polecenia cmdlet Test AzureRmResourceGroupDeployment, który efektywnej niewielkie różnice w Menedżera zasobów Azure składni.

Za pomocą szablonów stos Azure już podany w [repozytorium GitHub](http://aka.ms/AzureStackGitHub/) ułatwiające rozpoczęcie pracy.

## <a name="virtual-machines"></a>Maszyn wirtualnych

### <a name="after-starting-my-microsoft-azure-stack-poc-host-all-my-tenants-vms-are-gone-from-hyper-v-manager-and-come-back-automatically-after-waiting-a-bit"></a>Po uruchomieniu host mojej firmy Microsoft Azure stos Zapewnić wszystkie moje dzierżaw maszyny wirtualne zniknęła z Menedżera funkcji Hyper-V i wróć automatycznie po nieco oczekiwanie?

Jak system wróci podsystemu magazynowania i potrzeba RPs określenia spójności. Czas potrzebny zależy od urządzenia i dane techniczne używane, ale może być trochę czasu, po ponownym uruchomieniu hosta dzierżawy maszyny wirtualne wróć do zostanie rozpoznany.

### <a name="i-have-deleted-some-virtual-machines-but-still-see-the-vhd-files-on-disk-is-this-behavior-expected"></a>Usunięto niektórych maszyn wirtualnych, ale nadal widzisz wirtualnego dysku twardego pliki z dysku. To zachowanie jest planowane?

Tak, jest to zachowanie poprawnie. Ponieważ została zaprojektowana w ten sposób:

- Po usunięciu maszyny wirtualnych dysków twardych nie zostaną usunięte. Dyski są osobne zasoby w grupie zasobów.
- Po usunięciu konta miejsca do magazynowania otrzymuje usunięcie jest widoczny bezpośrednio za pośrednictwem Azure Menedżera zasobów (portal programu PowerShell), ale dyski, które mogą zawierać nadal są przechowywane w magazynie aż śmieci jest uruchomiony.

Jeśli zobaczysz wirtualnych dysków twardych "wdów", należy wiedzieć, jeśli są one częścią folderów dla konta miejsca do magazynowania, który został usunięty. Jeśli konto miejsca do magazynowania nie została usunięta, jest normalny, że są nadal dostępne.

Więcej o konfigurowaniu progu przechowywania w [Zarządzanie kontami miejsca do magazynowania](azure-stack-manage-storage-accounts.md).

## <a name="installation-steps"></a>Kroki instalacji
Poniższe informacje dotyczące kroki instalacji stosem Azure mogą być przydatne podczas rozwiązywania problemów:  

| Indeks | Nazwa | Opis|
| ----- | ----- | -----|
|0,11 | (DEP) Sprawdź poprawność fizycznych komputerów | Sprawdzanie poprawności sprzętu i konfiguracji systemu operacyjnego w węzłach fizycznym. |
| 0.12 | (DEP) Konfigurowanie sieci fizycznych komputerów do zatwierdzenia Koncepcji | Konfigurowanie wirtualnej sieci przełączników i interfejsów. |
| 0.14 | (DEP) Wdrażanie domeny | Wdrażanie usługi Active Directory w maszyn wirtualnych. |
| 0,15 | (DEP) Konfigurowanie serwera domeny | Konfigurowanie serwera domeny z grupami zabezpieczeń itp. |
| wartość 0,16 | (DEP) Konfigurowanie fizyczny komputer | Konfigurowanie sieci, dołączanie do domeny i Administratorzy lokalni konfiguracji. |
| 0,18 | (ZATRZYMAJ) Skonfiguruj klaster miejsca do magazynowania | Utwórz klaster miejsca do magazynowania, Utwórz serwer puli i plik magazynu. |
| 0.19 | (WWK) Infrastruktura tkaninie konfiguracji | Skonfiguruj wymagania wstępne dotyczące rozmieszczania tkaninie. |
| 0.21 | (NETTO) Konfigurowanie BGP oraz translatora adresów Sieciowych | Instaluje BGP i translatora adresów Sieciowych - wymagane tylko do jednego węzła. |
| 0.22 | (NETTO) Konfigurowanie translatora adresów Sieciowych i serwer czasu | Synchronizuje serwer czasu i konfiguruje translatora adresów Sieciowych wpisów. |
| 40.41 | (WWK) Tworzenie maszyny wirtualne gościa | Tworzenie zarządzania maszyny wirtualne. |
| 40.42 | (FBI) Konfigurowanie programu PowerShell JEA | Konfigurowanie programu PowerShell JEA dla wszystkich ról. |
| 40.43 | (FBI) Konfigurowanie urzędu certyfikacji stosem Azure | Instaluje Azure stosem urząd certyfikacji. |
| 40.44 | (FBI) Konfigurowanie urzędu certyfikacji stosu Azure | Konfiguruje Urząd certyfikacji stosem Azure. |
| 40.45 | (NETTO) Konfigurowanie NC na maszyny wirtualne | Instaluje NC na maszyny wirtualne gościa |
| 40.46 | (NETTO) Konfigurowanie NC na maszyny wirtualne | Konfigurowanie NC na maszyny wirtualne gościa |
| 40.47 | (NETTO) Konfigurowanie maszyny wirtualne gościa | Konfigurowanie zarządzania maszyny wirtualne z ACL NC. |
| 60.61.81 | (FBI) Wdrażanie usługi dzwonienia tkaninie Azure stosem — FabricRing wymagania wstępne | Tworzy służącą do FabricRing |
| 60.61.82 | (FBI) Wdrażanie usługi dzwonienia tkaninie Azure stosu — wdrażanie tkaninie dzwonienia klaster | Instaluje i konfiguruje klaster dzwonienia tkaninie stosem Azure. |
| 60.61.83 | (FBI) Wdrażanie rozszerzenia administratora do dostawców zasobów | Instalowanie rozszerzeń administratora dla zasobu dostawców |
| 60.61.84 | (ACS) Konfigurowanie spójne Azure miejsca do magazynowania w węźle poziomu. | Instaluje i konfiguruje spójne Azure miejsca do magazynowania w węźle poziomu. |
| 60.61.85 | (ACS) Konfigurowanie spójne Azure miejsca do magazynowania w poziomie klaster. | Instaluje i konfiguruje spójne Azure miejsca do magazynowania w poziomie klaster. |
| 60.61.86 | (FBI) Wdrażanie usługi kontroler dzwonienia tkaninie Azure stos — wymagania wstępne | Wymagania wstępne dotyczące InfraServiceController |
| 60.61.87 | (FBI) Wdrażanie usługi kontroler dzwonienia tkaninie Azure stos - wymagania wstępne | Wymagania wstępne dotyczące WWK |
| 60.61.88 | (FBI) Wdrażanie usługi kontroler dzwonienia tkaninie Azure stosem — wymagania wstępne | Wymagania wstępne dotyczące ASAppGateway |
| 60.61.89 | (FBI) Wdrażanie usługi kontroler dzwonienia tkaninie Azure stos - wymagania wstępne | Wymagania wstępne dotyczące kontrolera magazynu |
| 60.61.90 | (FBI) Wdrażanie usługi kontroler dzwonienia tkaninie Azure stos - wymagania wstępne | Wymagania wstępne dotyczące HealthMonitoring |
| 60.61.91 | (FBI) Wdrażanie usługi kontroler dzwonienia tkaninie Azure stosu — wymagania wstępne | Wymagania wstępne dotyczące NZ |
| 60.61.92 | (FBI) Wdrażanie usługi kontroler dzwonienia tkaninie Azure stosu — wymagania wstępne | Wymagania wstępne dotyczące PMM |
| 60.61.93 | (Katal) Tworzyć główne usługi AzureStack | Tworzenie wykresu Azure aplikacji oraz główne usługi w AAD. |
| 60.61.94 | (NETTO) Ustawienia GW maszyny wirtualne | Instaluje GW na maszyny wirtualne gościa. |
| 60.61.95 | (NETTO) Konfigurowanie GW maszyny wirtualne | Konfiguruje GW na maszyny wirtualne gościa. |
| 60.61.96 | (NETTO) Wdrażanie IDN na hosts | Wdrażanie IDN na hosts infrastruktury |
| 60.61.97 | (NETTO) Konfigurowanie IDN | Konfigurowanie roli IDN |
| 60.61.98 | (FBI) Konfigurowanie pośrednictwem WSUS SMS | Instaluje serwer WSUS na maszyny wirtualne gościa. |
| 60.61.99 | (FBI) Konfigurowanie pośrednictwem WSUS SMS | Konfiguruje serwer WSUS na maszyny wirtualne gościa. |
| 60.61.100 | (FBI) Konfigurowanie maszyny wirtualne Azure SQL | Instalacje serwera Azure SQL w maszyny wirtualne gościa |
| 60.61.101 | (Katal) Konfiguracja wymagania wstępne dotyczące została maszyny wirtualne. | Konfiguruje wymagania wstępne dotyczące Microsoft Azure stos na maszyny wirtualne gościa. |
| 60.61.102 | (Katal) BYŁO maszyny wirtualne | Instaluje program Microsoft Azure stos na maszyny wirtualne gościa. |
| 60.120.121 | (FBI) Wdrażanie dostawców zasobów i kontrolery | Instalacje dostawców zasobów i kontrolerów |
| 60.120.121 | (FBI) Wdrażanie dostawców zasobów i kontrolery | Instalacje dostawców zasobów i kontrolerów |
| 60.120.121 | (FBI) Wdrażanie dostawców zasobów i kontrolery | Instalacje dostawców zasobów i kontrolerów |
| 60.120.121 | (FBI) Wdrażanie dostawców zasobów i kontrolery | Instalacje dostawców zasobów i kontrolerów |
| 60.120.121 | (FBI) Wdrażanie dostawców zasobów i kontrolery | Instalacje dostawców zasobów i kontrolerów |
| 60.120.121 | (FBI) Wdrażanie dostawców zasobów i kontrolery | Instalacje dostawców zasobów i kontrolerów |
| 60.120.121 | (FBI) Wdrażanie dostawców zasobów i kontrolery | Instalacje dostawców zasobów i kontrolerów |
| 60.120.121 | (FBI) Wdrażanie dostawców zasobów i kontrolery | Instalacje dostawców zasobów i kontrolerów |
| 60.120.122 | (FBI) Kontroler konfiguracji | Konfiguruje kontrolery |
| 60.120.123 | (Katal) Konfigurowanie WAS maszyny wirtualne | Konfiguruje Microsoft Azure stos na maszyny wirtualne gościa. |
| 60.120.124 | (Katal) Konfiguracja AAD Azure stosu. | Konfiguruje Azure stos z usługą Azure Active Directory. |
| 60.120.125 | (Katal) Instalowanie usług ADFS organizacji | Instalacje usługi federacyjne Active Directory (ADFS) |
| 60.120.126 | (Katal) Instalowanie usług ADFS i wykres | Instalacje Azure stos wykresu |
| 60.120.127 | (Katal) Konfigurowanie usług ADFS organizacji | Konfiguruje usługi federacyjne Active Directory (ADFS) |
| 60.140.141 | (FBI) Konfigurowanie SRP | Konfiguruje dostawcy zasobów miejsca do magazynowania |
| 60.140.142 | (ACS) Konfigurowanie spójne Azure miejsca do magazynowania. | Konfiguruje spójne Azure miejsca do magazynowania. |
| 60.140.143 | (FBI) Tworzenie kont miejsca do magazynowania | Utwórz wszystkie konta magazynu może być używany przez inny dostawców. |
| 60.140.144 | (FBI) Zarejestrować zastosowania SRP | Zarejestruj zastosowania dla dostawcy miejsca do magazynowania. |
| 60.140.145 | (WWK) Migrowanie utworzonego maszyny wirtualne, Hosts i klaster do WWK | Migruje obiektów utworzonych maszyny wirtualne, Hosts i klaster do WWK |
| 60.140.146 | (FBI) Konfigurowanie programu Windows Defender | Konfiguruje programu Windows Defender |
| 60.160.161 | (MIESIĘCZNY) Konfigurowanie monitorowania agenta | Konfiguruje monitorowania agenta |
| 60.160.162 | (FBI) Wymagania wstępne NRP | Wymagania wstępne dotyczące instalacji NRP |
| 60.160.163 | (FBI) Wdrożenie NRP | Instalacje NRP  |
| 60.160.164 | (FBI) Konfiguracja NRP | Konfiguruje NRP |
| 60.160.165 | (FBI) Wymagania wstępne CRP | Wymagania wstępne dotyczące instalacji CRP |
| 60.160.166 | (FBI) Wdrożenie CRP | Instalacje CRP |
| 60.160.167 | (FBI) Konfiguracja CRP | Konfiguruje CRP |
| 60.160.168 | (FBI) Wymagania wstępne FRP | Wymagania wstępne dotyczące instalacji FRP |
| 60.160.169 | (FBI) Wdrożenie FRP | Instalacje FRP  |
| 60.160.170 | (FBI) Konfiguracja FRP | Konfiguruje FRP |
| 60.160.174 | (FBI) URP wstępne | Wymagania wstępne dotyczące instalacji URP |
| 60.160.175 | (FBI) Wdrożenie URP | Instalacje URP  |
| 60.160.176 | (FBI) Konfiguracja URP | Konfiguruje URP |
| 60.160.171 | (FBI) Wymagania wstępne HRP | Wymagania wstępne dotyczące instalacji HRP |
| 60.160.172 | (FBI) HRP wdrażania | Instalacje HRP  |
| 60.160.173 | (FBI) Konfiguracja HRP | Konfiguruje HRP |
| 60.160.177 | (KV) Wymagania wstępne KeyVault | Wymagania wstępne dotyczące instalacji KeyVault |
| 60.160.178 | (KV) Wdrożenie KeyVault | KeyVault instalacji  |
| 60.160.179 | (KV) Konfiguracja KeyVault | Konfiguruje KeyVault | 
| 60.190.191 | (FBI) Konfigurowanie galerii | Konfigurowanie galerii |
| 60.190.192 | (FBI) Konfigurowanie usługi dzwonienia tkaninie | Konfigurowanie usługi dzwonienia tkaninie |
| 60.221 | (FBI) Konfigurowanie konsoli maszyny wirtualne | Instaluje serwer konsoli na maszyny wirtualne gościa. |
| 60.222 | (FBI) Konfigurowanie konsoli maszyny wirtualne | Przenoszenie zawartości DVM do konsoli maszyn wirtualnych. |
| 251 | Przygotowywanie do ponownego uruchomienia przyszłych hosta | Ustawianie zasad Uruchom ponownie komputer |


## <a name="next-steps"></a>Następne kroki

[Często zadawane pytania](azure-stack-FAQ.md)
