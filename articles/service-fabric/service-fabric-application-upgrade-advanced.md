<properties
   pageTitle="Uaktualnianie aplikacji: tematy dotyczące zaawansowanego | Microsoft Azure"
   description="W tym artykule opisano niektóre zaawansowane tematy dotyczące uaktualniania aplikacji usługi tkaninie."
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/14/2016"
   ms.author="subramar"/>

# <a name="service-fabric-application-upgrade-advanced-topics"></a>Uaktualnianie aplikacji usługi tkaninie: tematy dotyczące zaawansowanego

## <a name="adding-or-removing-services-during-an-application-upgrade"></a>Dodawanie lub usuwanie usług podczas uaktualniania aplikacji

Jeśli nowa usługa zostanie dodany do aplikacji, która jest już wdrożony i opublikowany jako uaktualnienie, Nowa usługa jest dodawana do wdrożonej aplikacji.  Takie uaktualnienie nie wpływa na usług, które zostały już część aplikacji. Jednak wystąpienie usługi, który został dodany musi być uruchomiona nową usługę, aby była aktywna (za pomocą `New-ServiceFabricService` polecenia cmdlet).

Usługi również może być usuwany ze aplikację jako część uaktualnienia. Jednak wszystkie bieżącego wystąpienia usługi usunięte być do muszą być zatrzymane przed wykonaniem uaktualnienia (za pomocą `Remove-ServiceFabricService` polecenia cmdlet). 

## <a name="manual-upgrade-mode"></a>Ręczne trybu uaktualniania

> [AZURE.NOTE]  Niemonitorowanych trybu ręcznego należy rozważyć tylko w przypadku uaktualniania nie powiodło się lub zawieszone. Monitorowane tryb jest zalecany tryb uaktualniania dla aplikacji usługi tkaninie.

Azure tkaninie usługa zawiera wiele tryby uaktualnienia do obsługi klastrów rozwoju i produkcji. Wybrane opcje wdrażania mogą się różnić w różnych środowiskach.

Monitorowane stopniowe uaktualnianie aplikacji jest najbardziej typowe uaktualnienie do użycia w produkcji. Po określeniu zasady uaktualniania tkaninie usługi sprawia, że aplikacja jest prawidłowy, przed przechodzi uaktualnienia.

 Administrator aplikacji umożliwia ręczne stopniowe tryb uaktualnienia aplikacji mają całkowitą kontrolę nad postęp uaktualnienia za pomocą różnych domenach uaktualnienia. W tym trybie jest przydatny, gdy wymagane są zasady oceny kondycji niestandardowych lub złożonych lub nietypowe uaktualnienie się dzieje (na przykład, aplikacja jest już utraty danych).

Na koniec automatyczne uaktualnienie aplikacji stopniowe przydaje się do rozwoju lub testowania środowiskach o podanie cyklu iteracji szybkie podczas opracowywania usługi.

## <a name="change-to-manual-upgrade-mode"></a>Zmienianie trybu ręcznego uaktualnienia
**Ręczne**— Zatrzymaj uaktualnienia aplikacji w bieżącym UD i zmienić tryb uaktualniania niemonitorowanych ręczne. Administrator musi ręcznie połączenie **MoveNextApplicationUpgradeDomainAsync** Kontynuuj uaktualnianie lub wyzwalanie wycofywania przez inicjowanie nowe uaktualnienie. Po uaktualnieniu wejścia trybu ręcznego, pozostaje w trybie ręcznym aż rozpoczęciu nowych aktualizacji. Polecenie **GetApplicationUpgradeProgressAsync** zwraca TKANINIE\_aplikacji\_uaktualnienie\_stan\_STOPNIOWYCH\_do przodu\_OCZEKUJE.

## <a name="upgrade-with-a-diff-package"></a>Uaktualnianie z pakietem różnic

Aplikacja tkaninie usługi mogą być uaktualnione przez inicjowania obsługi administracyjnej przy użyciu pakietu aplikacji pełny, niezależny. Aplikacja można też uaktualnić przy użyciu pakietu różnic zawierający tylko te pliki aplikacji zaktualizowane, manifest aplikacji zaktualizowane i pliki manifestu usługi.

Pakiet aplikacji pełny zawiera wszystkie pliki potrzebne do uruchomienia aplikacji usługi tkaninie. Pakiet różnic zawiera tylko te pliki, które zostały zmienione między ostatniego przepisu i bieżące uaktualnienie oraz manifest aplikacji pełnego i usługa pojawiają plików. Wszelkie odniesienia w manifest aplikacji lub manifestu usługi, którego nie można odnaleźć w układzie kompilacji są wyszukiwane w magazynie obrazu.

Pakiety pełny aplikacji są wymagane dla pierwszej instalacji aplikacji z klastrem. Kolejne aktualizacje mogą być pakiet aplikacji pełny lub pakiet różnic.

Okazji przy użyciu pakietu różnic jest dobrym rozwiązaniem:

* Pakiet różnic jest preferowane, gdy masz pakietu dużej aplikacji, który odwołuje się do kilku plików manifestu usługi i/lub kilku pakietów kodu, pakietów konfiguracji lub danych.

* Pakiet różnic jest preferowane, gdy masz system wdrożenia generowany przez układ kompilacji bezpośrednio z programu procesu tworzenia aplikacji. W tym przypadku mimo że nie zmienił się kodu, zestawy nowo zbudowane uzyskać różne sumy kontrolnej. Za pomocą pakietu aplikacji pełny należy zaktualizować wersję przesyłek kodu. Za pomocą pakietu różnic, możesz zapewniają tylko pliki, które zostały zmienione i pliki manifest miejsce, w którym został zmieniony tej wersji.

Gdy aplikacja jest uaktualniany przy użyciu programu Visual Studio, pakietu różnic jest opublikowany automatycznie. Aby ręcznie utworzyć pakiet różnic, należy zaktualizować manifest aplikacji i usług manifesty, ale tylko zmienione pakietów powinien zostać uwzględniony w pakiecie gotowych aplikacji. 

Na przykład Zacznijmy następującej aplikacji (opisane w celu łatwiejszego zrozumienia numery wersji):

```text
app1        1.0.0
  service1  1.0.0
    code    1.0.0
    config  1.0.0
  service2  1.0.0
    code    1.0.0
    config  1.0.0
```

Teraz załóżmy chcesz zaktualizować tylko kod pakietu service1 za pomocą pakietu różnic przy użyciu programu PowerShell. Teraz aplikacja zaktualizowane ma następującą strukturę folderów:

```text
app1        2.0.0      <-- new version
  service1  2.0.0      <-- new version
    code    2.0.0      <-- new version
    config  1.0.0
  service2  1.0.0
    code    1.0.0
    config  1.0.0
```

W takim przypadku możesz zaktualizować manifest aplikacji 2.0.0 i manifestu usługi dla service1 tak odzwierciedlała aktualizacja pakietu kodu. Folder pakietu aplikacji czy ma następującą strukturę:

```text
app1/
  service1/
    code/
```

## <a name="next-steps"></a>Następne kroki

[Uaktualnianie do aplikacji przy użyciu programu Visual Studio](service-fabric-application-upgrade-tutorial.md) opisano uaktualnienia aplikacji przy użyciu programu Visual Studio.

[Uaktualnianie programu Powershell przy użyciu aplikacji](service-fabric-application-upgrade-tutorial-powershell.md) opisano uaktualnienia aplikacji przy użyciu programu PowerShell.

Kontrolowanie, jak uaktualnienie aplikacji przy użyciu [Parametrów uaktualnienia](service-fabric-application-upgrade-parameters.md).

Wprowadzić do uaktualnienia aplikacji zgodnych, Dowiedz się, jak używać [Szeregowania danych](service-fabric-application-upgrade-data-serialization.md).

Rozwiązywanie typowych problemów w uaktualnienia aplikacji, odwołując się do kroki [Rozwiązywania problemów uaktualnienia aplikacji](service-fabric-application-upgrade-troubleshooting.md).
 
