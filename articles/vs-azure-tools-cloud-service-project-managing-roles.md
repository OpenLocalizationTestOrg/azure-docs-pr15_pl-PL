<properties
   pageTitle="Zarządzanie rolami w chmurze Azure usług projektami przy użyciu programu Visual Studio | Microsoft Azure"
   description="Dowiedz się, jak dodać nowe role do projektu usługi Azure chmury lub usuwanie istniejących ról z niego przy użyciu programu Visual Studio."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="managing-roles-in-the-azure-cloud-services-projects-with-visual-studio"></a>Zarządzanie rolami w projektach usług Azure chmurze przy użyciu programu Visual Studio

Po utworzeniu projektu usługi Azure chmurze, możesz dodać nowe role do niego lub usunąć istniejących ról z niego. Możesz również zaimportować istniejącego projektu i przekonwertuj ją do roli. Na przykład można importować aplikacji sieci web ASP.NET i wyznaczyć ról w sieci web.

## <a name="adding-or-removing-roles"></a>Dodawanie lub usuwanie ról

**Aby dodać rolę**

W **Eksploratorze rozwiązań**Otwórz menu skrótów dla węzła **role** w projekcie usługi cloud i wybierz przycisk **Dodaj**. Możesz wybierz istniejące rola sieci web lub pracownika z bieżącego rozwiązania lub Utwórz nowy projekt roli sieci web lub pracownika. Lub możesz zaznaczyć odpowiedni projekt, takich jak projekt aplikacji sieci web programu ASP.NET i skojarzyć projektu roli.

**Aby usunąć skojarzenia roli**

W węźle **role** projektu usługi cloud w Eksploratorze rozwiązań Otwórz menu skrótów dla roli, aby usunąć, a następnie wybierz polecenie **Usuń**.

## <a name="removing-and-adding-roles-in-your-cloud-service"></a>Usuwanie i Dodawanie ról w usłudze w chmurze

Jeśli usuwanie roli z Twojego projektu usługi w chmurze, ale później zdecydujesz się dodać roli z powrotem do projektu, zostaną dodane tylko deklaracji roli i podstawowe atrybutów, takich jak informacje punkty końcowe i diagnostyki. Nie dodatkowych zasobów lub odwołaniami zostaną dodane do pliku ServiceDefinition.csdef lub plik ServiceConfiguration.cscfg. Jeśli chcesz dodać tę informację, musisz ręcznie dodać je z powrotem do tych plików.

Na przykład można usunąć rolę usługi sieci web, a później zdecydujesz się na tę rolę z powrotem dodać do rozwiązania. Jeśli to zrobisz, wystąpi błąd. Aby zapobiec ten błąd, należy dodać `<LocalResources>` element wyświetlany w następujących XML do pliku ServiceDefinition.csdef. Użyj nazwy roli usługi sieci web możesz dodać z powrotem do projektu jako część nazwy atrybut **<LocalStorage>** elementu. W tym przykładzie nazwa roli usług sieci web jest **WCFServiceWebRole1**.

    <WebRole name="WCFServiceWebRole1">
        <Sites>
          <Site name="Web">
            <Bindings>
              <Binding name="Endpoint1" endpointName="Endpoint1" />
            </Bindings>
          </Site>
        </Sites>
        <Endpoints>
          <InputEndpoint name="Endpoint1" protocol="http" port="80" />
        </Endpoints>
        <Imports>
          <Import moduleName="Diagnostics" />
        </Imports>
       <LocalResources>
          <LocalStorage name="WCFServiceWebRole1.svclog" sizeInMB="1000" cleanOnRoleRecycle="false" />
       </LocalResources>
    </WebRole>

## <a name="next-steps"></a>Następne kroki

Informacje na temat sposobu konfigurowania ról w programie Visual Studio, czytając [Konfigurowanie ról w usłudze w chmurze Azure z programem Visual Studio](vs-azure-tools-configure-roles-for-cloud-service.md).
