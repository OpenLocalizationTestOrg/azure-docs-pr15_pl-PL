<properties
   pageTitle="Domyślny rozmiar folderu TEMP jest za mały dla roli | Microsoft Azure"
   description="Rola usługi cloud ma ograniczoną ilość miejsca do magazynowania dla folderu TEMP. Ten artykuł zawiera wskazówki na temat zaczyna brakować miejsca."
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
   ms.date="10/12/2016"
   ms.author="v-six" />

# <a name="default-temp-folder-size-is-too-small-on-a-cloud-service-webworker-role"></a>Domyślny rozmiar folderu TEMP jest za mały na rolę chmury usługi sieci web i pracownika

Domyślny katalog tymczasowy roli Pracownik lub sieci web usługi cloud zawiera maksymalny rozmiar 100 MB, co może stać się pełny w pewnym momencie. W tym artykule opisano, jak unikać brakiem miejsca dla katalogu tymczasowego.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="why-do-i-run-out-of-space"></a>Dlaczego brakować miejsca

Standardowe zmienne środowiska Windows TEMP i TMP są dostępne dla kodu uruchamianego w aplikacji. Zarówno TEMP i TMP wskaż jeden katalog, który zawiera maksymalny rozmiar 100 MB. Wszystkie dane, który jest przechowywany w katalogu nie jest zachowywane przez do zarządzania cyklem życia usługi w chmurze; w przypadku usunięciu wystąpienia rola w usłudze w chmurze katalogu jest wyczyszczone.

## <a name="suggestion-to-fix-the-problem"></a>Aby rozwiązać ten problem

Implementowanie jednego z następujących opcji:

- Konfigurowanie zasobów magazynu lokalnego i uzyskać do niego dostęp bezpośrednio zamiast TEMP lub TMP. Aby uzyskać dostęp do zasobu lokalnego magazynu z kodu uruchamianego w aplikacji, zadzwoń do metody [RoleEnvironment.GetLocalResource](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleenvironment.getlocalresource.aspx) . 

- Konfigurowanie zasobów magazynu lokalnego, a punktu TEMP i TMP katalogów, aby wskazywały ścieżka zasobu magazynu lokalnego. Ta modyfikacja powinny zostać wykonane w metody [RoleEntryPoint.OnStart](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx) .

W poniższym przykładzie pokazano, jak zmodyfikować katalogów docelowej dla TEMP i TMP z poziomu metody OnStart:


```csharp
using System;
using Microsoft.WindowsAzure.ServiceRuntime;

namespace WorkerRole1
{
    public class WorkerRole : RoleEntryPoint
    {
        public override bool OnStart()
        {
            // The local resource declaration must have been added to the
            // service definition file for the role named WorkerRole1:
            //
            // <LocalResources>
            //    <LocalStorage name="CustomTempLocalStore"
            //                  cleanOnRoleRecycle="false"
            //                  sizeInMB="1024" />
            // </LocalResources>

            string customTempLocalResourcePath =
            RoleEnvironment.GetLocalResource("CustomTempLocalStore").RootPath;
            Environment.SetEnvironmentVariable("TMP", customTempLocalResourcePath);
            Environment.SetEnvironmentVariable("TEMP", customTempLocalResourcePath);

            // The rest of your startup code goes here…

            return base.OnStart();
        }
    }
}
```

## <a name="next-steps"></a>Następne kroki

Przeczytaj blog, w którym opisano, [jak zwiększyć rozmiar folderu Azure Web roli ASP.NET tymczasowego](http://blogs.msdn.com/b/kwill/archive/2011/07/18/how-to-increase-the-size-of-the-windows-azure-web-role-asp-net-temporary-folder.aspx).

Wyświetl więcej [artykułów Rozwiązywanie problemów z](/?tag=top-support-issue&product=cloud-services) usługami w chmurze.

Aby dowiedzieć się, jak rozwiązywać problemy roli usługi cloud przy użyciu danych diagnostyki komputera Azure PaaS, wyświetlanie [Tomasz Williamson serii](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).
