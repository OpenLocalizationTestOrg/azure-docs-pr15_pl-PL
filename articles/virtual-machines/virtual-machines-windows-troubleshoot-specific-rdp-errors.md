<properties
    pageTitle="Określone komunikaty o błędach RDP dla maszyny wirtualne Azure | Microsoft Azure"
    description="Opis, że określone komunikaty, które mogą wystąpić podczas próby programu Podłączanie pulpitu zdalnego maszyn wirtualnych systemu Windows Azure"
    keywords="Błąd pulpitu zdalnego, błąd Podłączanie pulpitu zdalnego, nie może nawiązać połączenia Głosowa, rozwiązywanie problemów pulpitu zdalnego"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="top-support-issue,azure-service-management,azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="support-article"
    ms.date="10/14/2016"
    ms.author="iainfou"/>

# <a name="troubleshooting-specific-rdp-error-messages-to-a-windows-vm-in-azure"></a>Rozwiązywanie problemów z określonego komunikatu o błędzie RDP maszyn wirtualnych systemu Windows platformy Azure
Podczas korzystania z platformy Azure Podłączanie pulpitu zdalnego maszyn wirtualnych systemu Windows (maszyn wirtualnych) może zostać wyświetlony komunikat o błędzie. W tym artykule opisano niektóre z najbardziej typowych komunikatów o błędach napotkał oraz rozwiązywanie problemów z czynności, aby je rozwiązać. Jeśli występują problemy, nawiązywanie połączenia z maszyn wirtualnych przy użyciu RDP, ale nie stykają się komunikat o błędzie, zobacz temat [Podręcznik dla pulpitu zdalnego rozwiązywania problemów](virtual-machines-windows-troubleshoot-rdp-connection.md).

Informacje dotyczące komunikatów konkretnego problemu zobacz:

- [Sesja zdalna uległa rozłączeniu, ponieważ do dyspozycji licencji serwery licencji zdalnego pulpitu są](#rdplicense).
- [Pulpit zdalny nie może znaleźć komputer "Nazwa"](#rdpname).
- Wystąpił błąd [uwierzytelniania. Nie można skontaktować się z lokalnego serwera zabezpieczeń](#rdpauth).
- [Błąd zabezpieczeń systemu Windows: poświadczenia nie działa](#wincred).
- [Ten komputer nie może połączyć się z komputerem zdalnym](#rdpconnect).

<a id="rdplicense"></a>
## <a name="the-remote-session-was-disconnected-because-there-are-no-remote-desktop-license-servers-available-to-provide-a-license"></a>Zdalnej sesji został odłączony, ponieważ zapewnia licencji serwery licencji zdalnego pulpitu są.

Przyczyna: 120-dniowego okresu prolongaty licencjonowania dla roli serwera pulpitu zdalnego wygasła i jest potrzebne do zainstalowania licencji.

Obejść ten problem Zapisz lokalną kopię pliku RDP z portalu i uruchomić to polecenie w wierszu polecenia programu PowerShell nawiązania połączenia. W tym kroku wyłącza licencjonowania tylko to połączenie:

        mstsc <File name>.RDP /admin

Jeśli rzeczywistości nie potrzebujesz więcej niż dwa pulpitu zdalnego połączenia maszyn wirtualnych, możesz usunąć rolę serwera pulpitu zdalnego za pomocą Menedżera serwera.

Aby uzyskać więcej informacji zobacz w blogu [maszyn wirtualnych Azure nie powiodło się "Nie licencji usług pulpitu zdalnego serwerów dostępnych"](https://blogs.msdn.microsoft.com/mast/2014/01/21/rdp-to-azure-vm-fails-with-no-remote-desktop-license-servers-available/).

<a id="rdpname"></a>
## <a name="remote-desktop-cant-find-the-computer-name"></a>Pulpit zdalny nie może znaleźć komputer "Nazwa".

Przyczyna: Klienta pulpitu zdalnego na komputerze nie może rozpoznać nazwy komputera, w obszarze Ustawienia pliku RDP.

Rozwiązanie:

- Jeśli jesteś w intranecie organizacji, upewnij się, ma dostęp do serwera proxy i może wysyłać ruch HTTPS do niej na komputerze.

- Jeśli korzystasz z lokalnie przechowywany plik RDP, spróbuj użyć, w którym jest generowany przez portal. Ten krok gwarantuje, że masz prawidłową nazwę DNS maszyny wirtualnej lub usługa w chmurze i punktu końcowego maszyn wirtualnych. Oto przykładowy plik RDP wygenerowane przez portal:

        full address:s:tailspin-azdatatier.cloudapp.net:55919
        prompt for credentials:i:1

Część adresu tego pliku RDP obejmuje:
- W pełni kwalifikowaną nazwę domeny: usługi w chmurze, która zawiera maszyn wirtualnych ("Firma azdatatier.cloudapp.net" w tym przykładzie).

- Port TCP zewnętrznego punktu końcowego ruchu pulpitu zdalnego (55919).

<a id="rdpauth"></a>
## <a name="an-authentication-error-has-occurred-the-local-security-authority-cannot-be-contacted"></a>Wystąpił błąd uwierzytelniania. Nie można skontaktować się z lokalnego serwera zabezpieczeń.

Przyczyna: Docelowej maszyn wirtualnych nie może odnaleźć urząd zabezpieczeń w część nazwy użytkownika poświadczenia.

Gdy nazwa użytkownika znajduje się w formularzu *SecurityAuthority*\\*nazwy użytkownika* (przykład: CORP\User1), część *SecurityAuthority* jest maszyn nazwę komputera (Urząd zabezpieczeń lokalnych) lub nazwę domeny usługi Active Directory.

Rozwiązanie:

- Jeśli konto jest lokalne maszyn wirtualnych, upewnij się, że nazwa maszyn wirtualnych jest napisana z błędem.

- Jeśli konto znajduje się w domenie usługi Active Directory, Sprawdź pisownię nazwy domeny.

- Jeśli jest konta domeny usługi Active Directory, a nazwa domeny jest poprawna, sprawdź kontrolera domeny jest dostępna w tej domenie. Jest typowy problem z Azure wirtualnych sieci, zawierających kontrolery domeny kontrolera domeny jest niedostępny, ponieważ nie został uruchomiony. Aby obejść ten problem zamiast konta domeny można użyć lokalnego konta administratora.

<a id="wincred"></a>
## <a name="windows-security-error-your-credentials-did-not-work"></a>Błąd zabezpieczeń systemu Windows: poświadczenia nie działają.

Przyczyna: Docelowej maszyn wirtualnych nie może sprawdzić swoją nazwę konta i hasło.

Komputera z systemem Windows można sprawdzić poświadczenia konta lokalnego lub konta domeny.

- W przypadku kont lokalnych za pomocą *nazwa_komputera*\\składni*nazwy użytkownika* (przykład: SQL1\Admin4798).
- W przypadku kont domen za pomocą *nazwa_domeny*\\składni*nazwy użytkownika* (przykład: CONTOSO\peterodman).

Jeśli masz promowane usługi maszyn wirtualnych do kontrolera domeny w nowej las usługi Active Directory, lokalnego konta administratora podpisaną przy użyciu jest konwertowana na konto równoważne z tego samego hasła w nowy las i domeny. Lokalne konto jest usuwany.

Na przykład jeśli zalogowani przy użyciu konta lokalnego DC1\DCAdmin, a następnie podwyższonym maszyny wirtualnej kontrolera domeny w nowy las dla domeny corp.contoso.com, lokalnego konta DC1\DCAdmin otrzymuje usunięte i utworzeniu nowego konta domeny (CORP\DCAdmin) z tego samego hasła.

Upewnij się, że nazwa konta jest nazwę maszyny wirtualnej, można sprawdzić jako ważne konto i że hasło jest poprawne.

Jeśli chcesz zmienić hasło lokalnego konta administratora, zobacz [Resetowanie hasła lub usługi pulpitu zdalnego dla maszyn wirtualnych systemu Windows](virtual-machines-windows-reset-rdp.md).

<a id="rdpconnect"></a>
## <a name="this-computer-cant-connect-to-the-remote-computer"></a>Ten komputer nie może połączyć się z komputerem zdalnym.

Przyczyna: Konto, z którego jest używany do łączenia nie ma pulpitu zdalnego logowania prawa.

Na każdym komputerze systemu Windows ma pulpitu zdalnego użytkowników lokalnych grupą, która zawiera konta i grupy, które można zdalnie zalogować się do go. Członkowie grupy administratorów lokalnych mieć również dostęp, nawet jeśli te konta nie są widoczne w lokalnej grupy użytkowników pulpitu zdalnego. W przypadku komputerów z domeny lokalnej grupy administratorów zawiera Administratorzy domeny dla domeny.

Upewnij się, że konto, którego używasz do nawiązywania połączeń z uprawnieniami pulpitu zdalnego logowania. Aby obejść ten problem umożliwia domeny lub lokalnego konta administratora Podłączanie pulpitu zdalnego. Aby dodać odpowiednie konto do grupy lokalnej Użytkownicy pulpitu zdalnego, należy użyć przystawką programu Microsoft Management Console (**Narzędzia systemowe > Użytkownicy i grupy lokalne > grupy > Użytkownicy pulpitu zdalnego**).


## <a name="next-steps"></a>Następne kroki
Jeśli żadna z tych błędów wystąpił i występuje nieznany problem z połączeniem się przy użyciu RDP, zobacz temat [Podręcznik dla pulpitu zdalnego rozwiązywania problemów](virtual-machines-windows-troubleshoot-rdp-connection.md).

- [Azure pakiet Diagnostyka IaaS (Windows)](https://home.diagnostics.support.microsoft.com/SelfHelp?knowledgebaseArticleFilter=2976864)
- Aby uzyskać czynności na dostęp do aplikacji uruchomionych maszyny rozwiązywania problemów, zobacz [Rozwiązywanie problemów z dostępem do uruchamiania aplikacji na maszyn wirtualnych Azure](virtual-machines-linux-troubleshoot-app-connection.md).
- Jeśli występują problemy dotyczące nawiązywania połączenia z maszyny Linux platformy Azure za pomocą Secure Shell (SSH), zobacz [Rozwiązywanie problemów z SSH połączenia maszyny Linux platformy Azure](virtual-machines-linux-troubleshoot-ssh-connection.md).