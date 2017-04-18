<properties
   pageTitle="Aby uruchomić i debugowania usługi w chmurze na komputerze lokalnym przy użyciu emulatora Express | Microsoft Azure"
   description="Aby uruchomić i debugowania usługi w chmurze na komputerze lokalnym przy użyciu emulatora Express"
   services="visual-studio-online"
   documentationCenter="n/a"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="visual-studio-online"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="multiple"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />


# <a name="using-emulator-express-to-run-and-debug-a-cloud-service-on-a-local-machine"></a>Aby uruchomić i debugowania usługi w chmurze na komputerze lokalnym przy użyciu emulatora Express

Przy użyciu emulatora Express, można przetestować i debugowania usługi w chmurze bez uruchamiania programu Visual Studio jako administrator. Możesz skonfigurować ustawienia projektu, aby użyć Express emulatora lub emulatorze pełny, w zależności od wymagań usługi w chmurze. Aby uzyskać więcej informacji na temat pełnego emulatora zobacz [uruchomić aplikację Azure w emulatorze obliczenia](./storage/storage-use-emulator.md). Express emulatora została uwzględniona w Azure SDK 2.1 i od Azure SDK 2.3, jest to emulator domyślne.

## <a name="using-emulator-express-in-the-visual-studio-ide"></a>W programie Visual Studio IDE przy użyciu emulatora Express

Po utworzeniu nowego projektu w Azure SDK 2.3 lub nowszym Express emulatora jest już zaznaczona. Dla istniejących projektów, które zostały utworzone za pomocą wcześniejszej wersji pakietu wykonaj poniższe czynności, aby zaznaczyć Express emulatora.

### <a name="to-configure-a-project-to-use-emulator-express"></a>Aby skonfigurować projektu, aby użyć emulatora Express

1. W menu skrótów dla Azure projektu wybierz polecenie **Właściwości**, a następnie wybierz kartę **sieć Web** .

1. W obszarze **Lokalnego serwera rozwoju**wybierz przycisk **opcji Użyj programu IIS Express** . Express emulatora nie jest zgodna z serwerem sieci Web usług IIS.

1. W obszarze **emulatora**wybierz przycisk opcji **Użyj emulatora Express** .

    ![Emulator Express](./media/vs-azure-tools-emulator-express-debug-run/IC673363.gif)

## <a name="launching-emulator-express-at-a-command-prompt"></a>Uruchamianie Express emulatora w wierszu polecenia

W wierszu polecenia można uruchomić wersji express emulatora obliczyć Azure, csrun.exe, używając opcji /useemulatorexpress.

## <a name="limitations"></a>Ograniczenia

Przed użyciem emulatora wyrazić, należy pamiętać o pewne ograniczenia:

- Express emulatora nie jest zgodna z serwerem sieci Web usług IIS.

- Usługa w chmurze może zawierać wiele ról, ale każdej roli jest ograniczona do jednego wystąpienia.

- Nie masz dostępu do numery portów poniżej 1000. Na przykład jeśli korzystasz z dostawcą uwierzytelniania, zwykle korzystającego z portem poniżej 1000, konieczne może być Zmień tę wartość na numer portu, który jest powyżej 1000.

- Wszelkie ograniczenia, które dotyczą emulatora obliczyć Azure dotyczą również Express emulatora. Na przykład nie może mieć więcej niż 50 wystąpień roli na wdrożenia. Zobacz [uruchomić aplikację Azure w emulatorze obliczeń](http://go.microsoft.com/fwlink/p/?LinkId=623050)

## <a name="next-steps"></a>Następne kroki

[Debugowanie usług w chmurze](https://msdn.microsoft.com/library/azure/ee405479.aspx)
