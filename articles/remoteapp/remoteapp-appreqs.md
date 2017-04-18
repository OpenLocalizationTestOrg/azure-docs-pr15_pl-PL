
<properties
    pageTitle="Wymagania dotyczące aplikacji dla Azure RemoteApp | Microsoft Azure"
    description="Więcej informacji na temat wymagań dotyczących aplikacji, które ma być używany w Azure RemoteApp"
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



# <a name="app-requirements"></a>Wymagania dotyczące aplikacji

> [AZURE.IMPORTANT]
> Azure RemoteApp jest są przerywane. W tym artykule [anons](https://go.microsoft.com/fwlink/?linkid=821148) , aby uzyskać szczegółowe informacje.

Azure RemoteApp obsługuje przesyłanie strumieniowe 32-bitowej lub 64-bitowych aplikacji systemu Windows z obrazu systemu Windows Server 2012 R2. Większość istniejących 32-bitowej lub 64-bitowych aplikacji systemu Windows uruchom "jako" w Azure RemoteApp (usługami pulpitu zdalnego lub wcześniej znany jako usługi terminalowe) środowiska. Jednak istnieje różnica między running i uruchamianie również — niektóre aplikacje działać prawidłowo i przeprowadzić, gdy inne osoby nie. Wskazówki dotyczące tworzenia aplikacji w środowisku usługi pulpitu zdalnego i testowania zgodności zawiera następujące informacje.

Porada: Nieustannie pracujemy nad utworzyć kilka przykładów pracy aplikacji. Pojawi się nowe tematy, które omówić przy użyciu programu Microsoft Access, QuickBooks i App-V w RemoteApp.

## <a name="requirements"></a>Wymagania dotyczące
Te trzy warunki, jeśli wykonane, pomocy aplikacji uruchomić również w RemoteApp:

1.  Aplikacje, które spełniają wszystkie [wymagań dotyczących certyfikacji dla aplikacji komputerowych systemu Windows](https://msdn.microsoft.com/library/windows/desktop/hh749939.aspx) i zgodne z [usługami pulpitu zdalnego programowania wskazówki](https://msdn.microsoft.com/library/aa383490.aspx) będzie miał pełną zgodność z RemoteApp.
2.  Aplikacji powinna nigdy nie są przechowywane dane lokalnie w obrazie lub wystąpienia RemoteApp, które mogą zostać utracone.  Po utworzeniu zbioru RemoteApp wystąpienia są sklonowany są bezstanowe i powinien zawierać tylko aplikacji. Przechowywanie danych w zewnętrznym źródle lub w profilu użytkownika.
3.  Niestandardowe obrazy nigdy nie powinny zawierać dane, które mogą zostać utracone.  

## <a name="testing-your-apps"></a>Testowanie aplikacji
Wykonaj poniższe kroki w celu testowania aplikacji:

1.  Instalowanie systemu Windows Server 2012 R2 i aplikacji
2.  Włączanie pulpitu zdalnego
3.  Utwórz dwa konta użytkowników, UserA i UserB, dodając obu kont użytkownika do grupy zabezpieczeń pulpitu zdalnego.
4.  Sprawdzanie zgodności wiele sesji ustalając dwóch jednoczesnego licencji sesji do komputera podczas uruchamiania aplikacji.
5.  Sprawdź poprawność zachowanie aplikacji

## <a name="application-development-guidelines"></a>Wskazówki dotyczące programowania aplikacji
Użyj tych wskazówek dotyczącej opracowywania aplikacji dla RemoteApp.

### <a name="multiple-users"></a>Wielu użytkowników

- Instalowanie [aplikacji dla jednego użytkownika ](https://msdn.microsoft.com/library/aa380661.aspx), można utworzyć problemy z udostępnionej bazy danych.
- Aplikacje należy [przechowywać informacji o użytkowniku](https://msdn.microsoft.com/library/aa383452.aspx) w lokalizacjach specyficzne dla użytkownika, niezależnie od globalnym informacje, które dotyczą wszystkich użytkowników.
- Funkcja RemoteApp korzysta wielu [nazw obiektów jądra](https://msdn.microsoft.com/library/aa382954.aspx); Globalny obszar nazw jest używana przede wszystkim przez usługi w aplikacji klient/serwer.
- Nie jest bezpieczne przyjęto założenie, że nazwa komputera lub [adres IP](https://msdn.microsoft.com/library/aa382942.aspx) przypisany do komputera są skojarzone z jednego użytkownika ponieważ wielu użytkowników może być zalogowany jednocześnie na serwerze hosta sesji pulpitu zdalnego (hosta sesji pulpitu zdalnego).

### <a name="performance"></a>Wydajność
- Wyłącz [efekty graficzne](https://msdn.microsoft.com/library/aa380822.aspx) , przed dodaniem aplikacji do RemoteApp.
- Aby zmaksymalizować dostępność Procesora dla wszystkich użytkowników, wyłącz [zadania w tle](https://msdn.microsoft.com/library/aa380665.aspx) lub tworzenie zadania w tle wydajność, które nie są zasobów.
- Należy dostosować i saldo aplikacji [zastosowania wątku](https://msdn.microsoft.com/library/aa383520.aspx) w środowisku wielu użytkowników, wieloprocesorowych.
- Aby zoptymalizować wydajność, jest zalecane dla aplikacji [Wykrywanie](https://msdn.microsoft.com/library/aa380798.aspx) czy działają w sesji klienta.
