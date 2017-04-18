<properties
   pageTitle="Zarządzaj aplikacjami w programie Visual Studio | Microsoft Azure"
   description="Visual Studio umożliwia tworzenie, opracowywanie pakowanie, wdrażania i debugowanie tkaninie usługi aplikacje i usługi."
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/09/2016"
   ms.author="seanmck;mikhegn"/>

# <a name="use-visual-studio-to-simplify-writing-and-managing-your-service-fabric-applications"></a>Pisanie i zarządzanie aplikacjami usług tkaninie za pomocą programu Visual Studio

Możesz zarządzać tkaninie usługi Azure aplikacji i usług za pomocą programu Visual Studio. Po skonfigurowano [środowiska programowania](service-fabric-get-started.md), możesz używać programu Visual Studio, tworzenie aplikacji usług tkaninie, dodawanie usług lub pakowania, Rejestruj i wdrażanie aplikacji w klastrze rozwoju lokalnego.

## <a name="deploy-your-service-fabric-application"></a>Wdrażanie aplikacji tkaninie usługi

Domyślnie wdrażania aplikacji łączy następujące czynności w jednej operacji proste:

1. Tworzenie pakietu aplikacji
2. Przekazywanie pakietu aplikacji do sklepu obrazu
3. Rejestrowanie typ aplikacji
4. Usuwanie dowolnego uruchomione wystąpienia aplikacji
5. Tworzenie nowego wystąpienia aplikacji

W programie Visual Studio naciśnięcie klawisza **F5** także wdrażania aplikacji i dołączyć debugowania do wszystkich wystąpień aplikacji. Za pomocą **Klawiszy Ctrl + F5** Aby wdrożyć aplikację bez debugowania lub można publikować w klastrze lokalną lub zdalną przy użyciu profilu publikowania. Aby uzyskać więcej informacji zobacz [Publikowanie aplikacji klaster zdalny przy użyciu programu Visual Studio](service-fabric-publish-app-remote-cluster.md).

### <a name="application-debug-mode"></a>Tryb debugowania aplikacji

Domyślnie program Visual Studio usuwa istniejące wystąpienia tego typu aplikacji po zatrzymaniu debugowania lub (Jeśli wdrożono aplikacji bez nich debugowania), gdy Rozmieść ponownie aplikację. W takim przypadku danych wszystkich aplikacji zostanie usunięty. Podczas debugowania lokalnie, warto zachować dane, które zostały już utworzone podczas testowania nowej wersji aplikacji, mają zostać zachowane uruchamianie aplikacji lub chcesz sesji debugowania kolejnych uaktualnienia aplikacji. Program Visual Studio Tools tkaninie usługi zapewniają właściwość o nazwie **Tryb debugowania aplikacji**, jakie formanty czy **F5** należy odinstalować aplikację, Zachowaj aplikacji uruchomiony po debugowanie kończy lub umożliwić aplikacji w kolejnych sesji debugowania, zamiast usunięte i ponownego rozmieszczania serwera.

#### <a name="to-set-the-application-debug-mode-property"></a>Aby ustawić właściwość tryb debugowania aplikacji

1. W menu skrótów projekt aplikacji wybierz pozycję **Właściwości** (lub naciśnij klawisz **F4** ).
2. W oknie **Właściwości** należy ustawić właściwość **Tryb debugowania aplikacji** .

    ![Ustawianie właściwości tryb debugowania aplikacji][debugmodeproperty]

Dostępne są dostępne opcje **Tryb debugowania aplikacji** .

1. **Automatyczne uaktualnienie**: aplikacja będzie nadal działać po zakończeniu sesji debugowania. Przy następnym **F5** zostanie potraktowany wdrażanie jako uaktualnienie przy użyciu trybu automatycznego niemonitorowanych szybko uaktualnienia aplikacji do nowszej wersji dodanym ciągiem daty. Proces uaktualniania zachowuje wszelkie dane wprowadzone w poprzedniej sesji debugowania.

2. **Zachowaj aplikacji**: aplikacja przechowuje uruchomiona w klastrze po zakończeniu sesji debugowania. Na następnym **F5** aplikacji zostaną usunięte i nowych aplikacji zostanie wdrożony z klastrem.

3. **Usuwanie aplikacji** powoduje, że aplikacja została usunięta po zakończeniu sesji debugowania.

Dla **Automatyczne uaktualnienie** dane zostaną zachowane przez zastosowanie możliwości uaktualnienia aplikacji usługi tkaninie, ale jest on dostosowanych Aby zoptymalizować pod kątem wydajności, zamiast bezpieczeństwo. Aby uzyskać więcej informacji na temat uaktualniania aplikacji i jak można wykonać uaktualnienie w środowisku rzeczywistą zobacz [Uaktualnianie aplikacji usługi tkaninie](service-fabric-application-upgrade.md).

![Przykład nową wersję aplikacji z datą dołączana][preservedata]

>[AZURE.NOTE] Ta właściwość nie istnieje przed narzędzia tkaninie usługi w wersji 1.1 programu Visual Studio. Przed 1.1 użyj właściwości **Zachowania danych przy uruchamianiu** uzyskanie takie samo zachowanie. Opcja "Zachowaj aplikacji" została wprowadzona w wersji 1.2 narzędzia tkaninie usługi programu Visual Studio.

## <a name="add-a-service-to-your-service-fabric-application"></a>Dodawanie usługi w aplikacji usługi tkaninie

Możesz dodać nowych usług do aplikacji, aby rozszerzyć jego funkcje.  Aby upewnić się, że usługa znajduje się do pakietu aplikacji, Dodaj usługę za pomocą element menu **Nowa usługa tkaninie...** .

![Dodaj nową usługę tkaninie do aplikacji][newservice]

Wybierz typ projektu tkaninie usługi, aby dodać do aplikacji, a następnie określ nazwę dla usługi.  Zobacz [Wybieranie struktury tej usługi](service-fabric-choose-framework.md) pomagające zdecydować, jakiego typu usługi, aby użyć.

![Wybierz typ projektu tkaninie usługi, aby dodać do aplikacji][addserviceproject]

Nowa usługa zostaną dodane do rozwiązania i istniejący pakiet aplikacji. Odwołania do usług i w przypadku wystąpienia domyślnego usługi zostaną dodane do manifest aplikacji. Usługa zostanie utworzona i pracę przy następnym wdrażania aplikacji.

![Nowa usługa, zostaną dodane do Twojej manifest aplikacji][newserviceapplicationmanifest]

## <a name="package-your-service-fabric-application"></a>Pakowanie aplikacji tkaninie usługi

Aby wdrożyć aplikację i jego usług do klastrów, należy utworzyć pakiet aplikacji.  Pakiet porządkuje manifest aplikacji, manifest(s) usługi i inne niezbędne pliki w określonym układzie.  Program Visual Studio konfiguruje i zarządza pakietu w folderze projekt aplikacji w katalogu "pkg".  Kliknięcie przycisku **pakietu** z menu kontekstowego **aplikacji** utworzony lub zaktualizowany pakiet aplikacji.  Można to zrobić, jeśli wdrażania aplikacji za pomocą niestandardowych skryptów programu PowerShell.

## <a name="remove-applications-and-application-types-using-cloud-explorer"></a>Usuwanie aplikacji i typów aplikacji przy użyciu Eksploratora chmury

Można wykonywać operacje zarządzania podstawowe klaster z w programie Visual Studio za pomocą Eksploratora chmury, które można uruchamiać w menu **Widok** . Na przykład możesz usunąć aplikacji i wstrzymywanie obsługi administracyjnej aplikacji typów dla klastrów lokalną lub zdalną.

![Usuwanie aplikacji](./media/service-fabric-manage-application-in-visual-studio/removeapplication.png)

>[AZURE.TIP] Aby uzyskać bardziej rozbudowane funkcje zarządzania klaster zobacz [wizualizacji klaster w Eksploratorze tkaninie usługi](service-fabric-visualizing-your-cluster.md).


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Następne kroki

- [Model aplikacji tkaninie usługi](service-fabric-application-model.md)
- [Wdrażanie aplikacji tkaninie usługi](service-fabric-deploy-remove-applications.md)
- [Zarządzanie parametry aplikacji w wielu środowiskach](service-fabric-manage-multiple-environment-app-configuration.md)
- [Debugowanie aplikacji tkaninie usługi](service-fabric-debugging-your-application.md)
- [Wizualizowanie klaster przy użyciu Eksploratora tkaninie usługi](service-fabric-visualizing-your-cluster.md)

<!--Image references-->
[addserviceproject]:./media/service-fabric-manage-application-in-visual-studio/addserviceproject.png
[manageservicefabric]: ./media/service-fabric-manage-application-in-visual-studio/manageservicefabric.png
[newservice]:./media/service-fabric-manage-application-in-visual-studio/newservice.png
[newserviceapplicationmanifest]:./media/service-fabric-manage-application-in-visual-studio/newserviceapplicationmanifest.png
[preservedata]:./media/service-fabric-manage-application-in-visual-studio/preservedata.png
[debugmodeproperty]:./media/service-fabric-manage-application-in-visual-studio/debugmodeproperty.png
