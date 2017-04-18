<properties
    pageTitle="Publikowanie aplikacji w klastrze zdalnego z programem Visual Studio | Microsoft Azure"
    description="Dowiedz się, jak opublikować aplikację do klastrów tkaninie zdalny przy użyciu programu Visual Studio."
    services="service-fabric"
    documentationCenter="na"
    authors="cawams"
    manager="timlt"
    editor="" />

<tags
    ms.service="multiple"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="multiple"
    ms.date="07/29/2016"
    ms.author="cawa" />

# <a name="publish-an-application-to-a-remote-cluster-by-using-visual-studio"></a>Publikowanie aplikacji do klastrów zdalnych przy użyciu programu Visual Studio

> [AZURE.SELECTOR]
- [Programu PowerShell](service-fabric-deploy-remove-applications.md)
- [Programu Visual Studio](service-fabric-publish-app-remote-cluster.md)

<br/>

Rozszerzenie tkaninie usługi Azure programu Visual Studio umożliwia łatwe powtarzalnych i skryptowych opublikować aplikację do klastrów tkaninie usługi.

## <a name="the-artifacts-required-for-publishing"></a>Artefakty wymagane do opublikowania

### <a name="deploy-fabricapplicationps1"></a>Wdrażanie FabricApplication.ps1

To jest skrypt programu PowerShell, który używa ścieżkę profilu Publikuj jako parametru publikowania aplikacji usługi tkaninie. Ponieważ ten skrypt jest częścią aplikacji, jesteś Zapraszamy zmodyfikuj ją stosownie do potrzeb danej aplikacji.

### <a name="publish-profiles"></a>Publikowanie profilów

Folder w projekcie aplikacji usługi tkaninie o nazwie **PublishProfiles** zawiera pliki XML, które zawierać podstawowe informacje o publikowania aplikacji, takich jak:

- Parametry połączenia klaster tkaninie usługi
- Ścieżka do pliku parametrów aplikacji
- Uaktualnianie ustawień

Domyślnie aplikacja będzie zawierać dwie profili publikowania: Local.xml i Cloud.xml. Możesz dodać więcej profilów, kopiując i wklejając jedną z domyślnych plików.

### <a name="application-parameter-files"></a>Pliki aplikacji parametru

Folder w projekcie aplikacji usługi tkaninie o nazwie **ApplicationParameters** zawiera pliki XML dla wartości parametru manifestu zdefiniowane przez użytkownika aplikacji. Pliki manifestu aplikacji można sparametryzowane, dzięki czemu można używać różnych wartości ustawień rozmieszczania. Aby dowiedzieć się więcej na temat parametryzacja aplikacji, zobacz [Zarządzanie wielu środowiska usługi tkaninie](service-fabric-manage-multiple-environment-app-configuration.md).

>[AZURE.NOTE] Dla usług aktora należy utworzyć project najpierw przed podjęciem próby edytowania pliku w edytorze lub za pomocą okna dialogowego Publikuj. Jest to, ponieważ część pliki manifest zostaną wygenerowane podczas kompilacji.

## <a name="to-publish-an-application-by-using-the-publish-service-fabric-application-dialog-box"></a>Aby opublikować aplikację za pomocą okna dialogowego publikowanie aplikacji tkaninie usługi

Poniższe kroki przedstawiają sposoby publikowania aplikacji przy użyciu okna dialogowego **Publikowanie aplikacji tkaninie usługi** dostarczony przez program Visual Studio Tools tkaninie usługi.

1. W menu skrótów w projekcie aplikacji tkaninie usługi wybierz pozycję **Publikuj...** Aby wyświetlić okno dialogowe **Publikowanie aplikacji tkaninie usługi** .

    ![** Okno dialogowe Publikowanie usługi tkaninie aplikacji **][0]

    Plik wybrany w polu listy rozwijanej **profil docelowy** jest wszystkich ustawień, z wyjątkiem **pojawiają wersji**, zapisywania. Możesz użyć ponownie istniejącego profilu lub utworzyć nową, wybierając pozycję **<... zarządzania profilami >** lista rozwijana **profil docelowy** . Po wybraniu profilu publikowania jego zawartość jest wyświetlana w odpowiednich polach okna dialogowego. Aby zapisać zmiany w dowolnym momencie, wybierz link **Zapisz profil** .    

2. W sekcji **końcowy połączenia** określić lokalną lub zdalną tkaninie usługi Klaster w publikacji punktu końcowego. Aby dodać lub zmienić punkt końcowy połączenia, kliknij listę rozwijaną **Końcowy połączenia** . Na liście przedstawiono dostępne klaster tkaninie usługi punkty końcowe połączeń do których można publikować według subskrypcjach Azure. Należy zauważyć, że jeśli użytkownik nie jest już zalogowany do programu Visual Studio, możesz zostanie wyświetlony monit o to zrobić.

    Okno dialogowe wyboru klaster umożliwia wybieranie z zestawu niedostępna subskrypcji i klastrów.

    ![** Okno dialogowe Wybieranie usługi tkaninie klaster **][1]

    >[AZURE.NOTE] Jeśli chcesz opublikować punktu końcowego dowolnego (na przykład klaster firmy), zobacz poniższą sekcję **publikowania do punktu końcowego dowolnego klaster** .

    Po wybraniu punktu końcowego Visual Studio sprawdza połączenie zaznaczonego klaster tkaninie usługi. Jeśli klaster nie jest bezpieczny, Visual Studio można nawiązać ją natychmiast. Jednak jeśli klaster jest bezpieczny, musisz zainstalować certyfikat na komputerze lokalnym, przed kontynuowaniem. Aby uzyskać więcej informacji, zobacz [jak skonfigurować bezpiecznego połączenia](service-fabric-visualstudio-configure-secure-connections.md) . Gdy skończysz, kliknij przycisk **OK** . Klaster zaznaczonego pojawi się w oknie dialogowym **Publikowanie aplikacji tkaninie usługi** .

3. W polu listy rozwijanej **Pliku parametru aplikacji** przejdź do pliku parametru aplikacji. Plik parametru aplikacji zawiera określone przez użytkownika wartości parametrów w pliku manifestu aplikacji. Aby dodać lub zmienić parametr, kliknij przycisk **Edytuj** . Wprowadź lub zmień wartości parametrów w siatce **Parametry** . Po zakończeniu wybierz przycisk **Zapisz** .

    ![** Okno dialogowe Edytowanie parametrów **][2]

4. Użyj pola wyboru **uaktualnienie aplikacji** , określ, czy to publikować akcji jest uaktualnienie. Uaktualnianie publikowanie akcje różnią się od normalnego publikowanie akcje. Aby uzyskać listę różnic, zobacz [Uaktualnianie aplikacji tkaninie usługi](service-fabric-application-upgrade.md) . Aby skonfigurować ustawienia uaktualnienia, wybierz link **Konfigurowanie ustawień uaktualniania** . Zostanie wyświetlony Edytor uaktualnienia parametru. Zobacz [Konfigurowanie uaktualnienia aplikacji tkaninie usługi](service-fabric-visualstudio-configure-upgrade.md) , aby dowiedzieć się więcej na temat uaktualniania parametrów.

5. Wybierz pozycję **pojawiają wersje...** przycisk, aby wyświetlić okno dialogowe **Edytowanie wersji** . Musisz zaktualizować aplikację i wersjach usługi do uaktualnienia do miejsce. Zobacz [aplikacji usługi tkaninie uaktualnienie samouczka](service-fabric-application-upgrade-tutorial.md) , aby dowiedzieć się, jak aplikacja i usługa pojawiają wpływ wersji proces uaktualniania.

    ![** Okno dialogowe Edytowanie wersji **][3]

    Użycie aplikacji oraz usług wersje semantyczny przechowywania wersji, takich jak 1.0.0 lub wartości liczbowe w formacie 1.0.0.0, wybierz opcję **automatycznie zaktualizować aplikację i wersjach usługi** . Po wybraniu tej opcji usługa i numery wersji aplikacji są automatycznie aktualizowane po każdym kodu, konfiguracji lub dane wersji pakietu jest aktualizowana. Jeśli wolisz ręcznie edytować wersje, wyczyść pole wyboru, aby wyłączyć tę funkcję.

    >[AZURE.NOTE] Aby wszystkie pakiet wpisy są wyświetlane w projekcie aktora należy najpierw utworzyć projektu do wygenerowania wpisów w plikach pojawiają usługi.

6. Po zakończeniu określającą wszystkie niezbędne ustawienia, kliknij przycisk **Publikuj** publikowanie aplikacji do wybranego klastrów tkaninie usługi. Ustawienia zostaną zastosowane do procesu publikowania.

## <a name="publish-to-an-arbitrary-cluster-endpoint-including-party-clusters"></a>Publikowanie punktu końcowego dowolnego klaster (w tym klastrów firmy)

Visual Studio publikowania środowisko jest zoptymalizowana pod kątem publikowania do klastrów zdalnego skojarzonego z jedną Azure subskrypcji. Jednak jest możliwe do opublikowania dowolnego punkty końcowe (na przykład tkaninie usług innych firm klastrów) bezpośrednio edytując profil publikowania XML. Zgodnie z powyższym opisem, publikowanie dwa profile są dostarczane przez domyślne —**Local.xml** i **Cloud.xml**— ale można utworzyć dodatkowe profile dla różnych środowiskach — Zapraszamy. Na przykład można utworzyć profil publikowania do klastrów firmy, być może o nazwie **Party.xml**.

Jeśli łączysz się z klastrem niezabezpieczoną, wszystkie opcje, które są wymagane, takich jak jest punkt końcowy połączenia klaster `partycluster1.eastus.cloudapp.azure.com:19000`. W takim przypadku punkt końcowy połączenia w profilu publikowania przypominać następującą tabelę:

```XML
<ClusterConnectionParameters ConnectionEndpoint="partycluster1.eastus.cloudapp.azure.com:19000" />
```

  Jeśli łączysz się z klastrem zabezpieczonego, konieczne będzie dostarczenie szczegółowych informacji dotyczących certyfikat klienta z magazynu lokalnego może być używany do uwierzytelniania. Aby uzyskać więcej informacji zobacz [Konfigurowanie bezpiecznego połączenia z klastrem tkaninie usługi](service-fabric-visualstudio-configure-secure-connections.md).

  Po skonfigurowaniu profilu publikowania można odwoływać się do niej w oknie dialogowym Publikowanie tak jak pokazano poniżej.

  ![Publikowanie nowego profilu w okno dialogowe publikowanie][4]

  Zauważ, że w tym przypadku punktów nowego profilu publikowania do jednego z plików z parametrami aplikacji domyślnej. Jest to właściwe, jeśli chcesz opublikować tej samej konfiguracji aplikacji w wielu środowiskach. Z drugiej strony w przypadku której mają różne konfiguracje dla każdego środowiska, który chcesz opublikować, czy zrozumiałe Aby utworzyć plik parametru aplikacji.

## <a name="next-steps"></a>Następne kroki

Aby dowiedzieć się, jak zautomatyzować proces publikowania w środowisku ciągły integracji, zobacz [Konfigurowanie tkaninie usługi integracji ciągły](service-fabric-set-up-continuous-integration.md).


[0]: ./media/service-fabric-publish-app-remote-cluster/PublishDialog.png
[1]: ./media/service-fabric-publish-app-remote-cluster/SelectCluster.png
[2]: ./media/service-fabric-publish-app-remote-cluster/EditParams.png
[3]: ./media/service-fabric-publish-app-remote-cluster/EditVersions.png
[4]: ./media/service-fabric-publish-app-remote-cluster/publish-to-party-cluster.png
