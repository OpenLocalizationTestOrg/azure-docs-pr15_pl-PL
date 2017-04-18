<properties
   pageTitle="Typowe przyczyny role usługi w chmurze odtwarzanie | Microsoft Azure"
   description="Roli usługi w chmurze nieoczekiwanie odtwarza może spowodować istotne przestoje. Poniżej przedstawiono kilka typowych problemów powodujących role do odtwarzania, które mogą pomóc w zmniejszeniu przestoje."
   services="cloud-services"
   documentationCenter=""
   authors="simonxjx"
   manager="felixwu"
   editor=""
   tags="top-support-issue"/>
<tags
   ms.service="cloud-services"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="tbd"
   ms.date="09/02/2016"
   ms.author="v-six" />

# <a name="common-issues-that-cause-roles-to-recycle"></a>Typowe problemy powodujące ról do Kosza

W tym artykule omówiono niektóre typowe przyczyny problemów wdrożenia i porady dotyczące rozwiązywania problemów, ułatwiające rozwiązywania tych problemów. Wskazuje, że istnieje problem z aplikacją jest wystąpienie roli nie można uruchomić lub jego cykli Państw inicjowania, zajęty i zatrzymywanie.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="missing-runtime-dependencies"></a>Brak obsługi współzależności

Jeśli rola w aplikacji zależy od dowolnego zestawu, który nie jest częścią .NET Framework lub Azure zarządzane biblioteki, należy jawnie przypisać optycznego w pakiet aplikacji. Należy pamiętać, że innych systemów Microsoft nie są dostępne na Azure domyślnie. Jeśli Twoja rola zależy od takie ramy, należy dodać te zestawy do pakiet aplikacji.

Przed budowanie i pakowanie aplikacji, sprawdź następujące elementy:

- Jeśli przy użyciu programu Visual studio, upewnij się, że właściwości **Kopii lokalnej** jest ustawiona na **PRAWDA** dla każdego zestawu docelowego w projekcie, który nie jest częścią Azure SDK lub .NET Framework.

- Upewnij się, że plik web.config nie odwołuje się do dowolnych nieużywane zestawów w elemencie kompilacji.

- **Tworzenie akcji** każdego pliku .cshtml jest ustawiona na **zawartości**. Dzięki temu, będą poprawnie wyświetlane w pakiecie pliki, a umożliwia innych plików do są wyświetlane w pakiecie.

## <a name="assembly-targets-wrong-platform"></a>Platformy problem zestawu elementów docelowych

Azure jest środowiskiem 64-bitowej. W związku z tym zestawy .NET dla celu 32-bitowej nie będzie działać w Azure.

## <a name="role-throws-unhandled-exceptions-while-initializing-or-stopping"></a>Rola zgłasza nieobsługiwanego wyjątki podczas inicjowania lub zatrzymywanie

Wyjątki od wygenerowanych za pomocą metod klasy [RoleEntryPoint] , która zawiera [OnStart], [OnStop]i [uruchomić] metody, są nieobsługiwanego wyjątki. Jeśli powodu nieobsługiwanego wyjątku występuje w jednej z następujących metod, roli będzie Kosza. Jeśli wielokrotnie odtwarzanie roli go może Zgłaszanie wyjątku nieobsługiwanego zawsze, gdy próbuje uruchomić.

## <a name="role-returns-from-run-method"></a>Rola zwraca z metody Run

Metoda [Uruchamianie] jest przeznaczona zakończenia. Jeśli kod zastępuje metodę [uruchomić] , należy wstrzymania czas nieokreślony. Jeśli zwraca metody [Run] , odtwarza roli.

## <a name="incorrect-diagnosticsconnectionstring-setting"></a>Nieprawidłowe ustawienie DiagnosticsConnectionString

Jeśli aplikacja używa diagnostyki Azure, musisz określić pliku konfiguracji usługi `DiagnosticsConnectionString` ustawienia konfiguracji. To ustawienie należy określić połączenie HTTPS do swojego konta magazynu platformy Azure.

Aby upewnić się, że usługi `DiagnosticsConnectionString` ustawienie jest poprawny, przed wdrożeniem pakietu aplikacji Azure, sprawdź poniższe czynności:  

- `DiagnosticsConnectionString` Ustawienie punktów z kontem magazynującym w Azure.  
  Domyślnie to ustawienie wskazuje konta emulowanej przestrzeni dyskowej, aby jawnie należy zmienić to ustawienie przed wdrożeniem pakietu aplikacji. Jeśli nie możesz zmienić to ustawienie, powoduje wyjątek wystąpienie roli próbuje uruchomić monitorowania diagnostycznego. Może to powodować wystąpienie roli do Kosza na czas nieokreślony.

- W następującym [formacie](../storage/storage-configure-connection-string.md)określono parametry połączenia. (Protokół musi być określony jako HTTPS). Zamień *MyAccountName* nazwę swojego konta miejsca do magazynowania i *MyAccountKey* przy użyciu klucza dostępu:    

        DefaultEndpointsProtocol=https;AccountName=MyAccountName;AccountKey=MyAccountKey

  Jeśli tworzysz aplikację za pomocą narzędzi Azure dla programu Microsoft Visual Studio, można użyć [strony właściwości](https://msdn.microsoft.com/library/ee405486) wpis.

## <a name="exported-certificate-does-not-include-private-key"></a>Eksportowany certyfikat nie zawiera klucz prywatny

Aby uruchomić ról w sieci web w obszarze SSL, należy się upewnić, że certyfikatu wyeksportowane zarządzania zawiera klucz prywatny. Wyeksportuj certyfikat za pomocą *Menedżera certyfikatów systemu Windows* , należy **Tak,** zaznacz opcję **Eksportuj klucz prywatny** . Certyfikat musi wyeksportowany w formacie PFX, który jest jedynym formatem obecnie obsługiwane.

## <a name="next-steps"></a>Następne kroki

Wyświetl więcej [artykułów Rozwiązywanie problemów z](https://azure.microsoft.com/documentation/articles/?tag=top-support-issue&product=cloud-services) usługami w chmurze.

Wyświetl więcej roli odtwarzanie scenariuszy w [serii Tomasz Williamson](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).

[RoleEntryPoint]: https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.aspx
[OnStart]: https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx
[OnStop]: https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.onstop.aspx
[Uruchom]: https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx
