<properties 
pageTitle="Arkusz podpowiedzi zawierający XPath konfiguracji roli usługi cloud | Microsoft Azure" 
description="Różne ustawienia XPath umożliwia w konfiguracji roli usługi cloud uwidaczniają ustawienia jako zmienna środowiska." 
services="cloud-services" 
documentationCenter="" 
authors="Thraka" 
manager="timlt" 
editor=""/>
<tags 
ms.service="cloud-services" 
ms.workload="tbd" 
ms.tgt_pltfrm="na" 
ms.devlang="na" 
ms.topic="article" 
ms.date="08/10/2016" 
ms.author="adegeo"/>

# <a name="expose-role-configuration-settings-as-an-environment-variable-with-xpath"></a>Udostępniania ustawienia konfiguracji roli jako zmienna środowiska o wyrażenia XPath.

Pracownik usługi w chmurze lub w pliku definicji usługi sieci web roli pozwala udostępnić środowisko uruchomieniowe konfiguracji wartości jako zmienne środowiska. Następujące wyrażenie XPath wartości są obsługiwane (odpowiadające wartości interfejsu API).

Te wartości wyrażenia XPath są również dostępne za pośrednictwem biblioteki [Microsoft.WindowsAzure.ServiceRuntime](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleenvironment.aspx) . 

## <a name="app-running-in-emulator"></a>Aplikacja działa w emulatora

Wskazuje, że aplikacja działa w emulatorze.

| Typ  | Przykład |
| ----- | ------- |
| Wyrażenie XPath | xpath="/RoleEnvironment/Deployment/@emulated" |
| Kod  | Wariancja x = RoleEnvironment.IsEmulated; |


## <a name="deployment-id"></a>Identyfikator rozmieszczania

Pobiera identyfikator rozmieszczania dla danego wystąpienia.

| Typ  | Przykład |
| ----- | ------- |
| Wyrażenie XPath | xpath="/RoleEnvironment/Deployment/@id" |
| Kod  | var deploymentId = RoleEnvironment.DeploymentId; |


## <a name="role-id"></a>Identyfikator roli 

Pobiera bieżący identyfikator roli dla danego wystąpienia.

| Typ  | Przykład |
| ----- | ------- |
| Wyrażenie XPath | xpath="/RoleEnvironment/CurrentInstance/@id" |
| Kod  | Identyfikator var = RoleEnvironment.CurrentRoleInstance.Id; |


## <a name="update-domain"></a>Aktualizowanie domeny

Pobiera domeny aktualizacji wystąpienia.

| Typ  | Przykład |
| ----- | ------- |
| Wyrażenie XPath | xpath="/RoleEnvironment/CurrentInstance/@updateDomain" |
| Kod  | Wariancja ud = RoleEnvironment.CurrentRoleInstance.UpdateDomain; |


## <a name="fault-domain"></a>Błąd domeny

Pobiera domeny błędów wystąpienia.

| Typ  | Przykład |
| ----- | ------- |
| Wyrażenie XPath | xpath="/RoleEnvironment/CurrentInstance/@faultDomain" |
| Kod  | Wariancja fd = RoleEnvironment.CurrentRoleInstance.FaultDomain; |


## <a name="role-name"></a>Nazwa roli

Pobiera nazwę roli wystąpień.

| Typ  | Przykład |
| ----- | ------- |
| Wyrażenie XPath | xpath="/RoleEnvironment/CurrentInstance/@roleName" |
| Kod  | var rname = RoleEnvironment.CurrentRoleInstance.Role.Name;  |


## <a name="config-setting"></a>Ustawienia konfiguracji

Pobiera wartość ustawienia określonej konfiguracji.

| Typ  | Przykład |
| ----- | ------- |
| Wyrażenie XPath | xpath="/RoleEnvironment/CurrentInstance/ConfigurationSettings/ConfigurationSetting[@name='Setting1']/@value" |
| Kod  | Ustawienie wariancja = RoleEnvironment.GetConfigurationSettingValue("Setting1"); |
 
## <a name="local-storage-path"></a>Ścieżka lokalnego magazynu

Pobiera ścieżkę magazynu lokalnego dla danego wystąpienia.

| Typ  | Przykład |
| ----- | ------- |
| Wyrażenie XPath | xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='LocalStore1']/@path" |
| Kod  | var localResourcePath = RoleEnvironment.GetLocalResource("LocalStore1"). RootPath; |


## <a name="local-storage-size"></a>Rozmiar magazynu lokalnego

Pobiera rozmiar magazynu lokalnego dla danego wystąpienia.

| Typ  | Przykład |
| ----- | ------- |
| Wyrażenie XPath | xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='LocalStore1']/@sizeInMB" |
| Kod  | var localResourceSizeInMB = RoleEnvironment.GetLocalResource("LocalStore1"). MaximumSizeInMegabytes; |

## <a name="endpoint-protocol"></a>Protokół końcowy 

Pobiera protokołu punkt końcowy dla danego wystąpienia.

| Typ  | Przykład |
| ----- | ------- |
| Wyrażenie XPath | xpath="/RoleEnvironment/CurrentInstance/Endpoints/Endpoint[@name='Endpoint1']/@protocol" |
| Kod  | Wariancja prot = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["Endpoint1"]. Protocol (protokół); |

## <a name="endpoint-ip"></a>Punkt końcowy IP

Otrzymuje adres IP określony punkt końcowy.

| Typ | Przykład |
| ----- | ---- |
| Wyrażenie XPath | xpath="/RoleEnvironment/CurrentInstance/Endpoints/Endpoint[@name='Endpoint1']/@address" |
| Kod  | Wariancja adres = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["Endpoint1"]. IPEndpoint.Address |

## <a name="endpoint-port"></a>Port punktu końcowego 

Pobiera port punktu końcowego dla danego wystąpienia.

| Typ  | Przykład |
| ----- | ------- |
| Wyrażenie XPath | xpath="/RoleEnvironment/CurrentInstance/Endpoints/Endpoint[@name='Endpoint1']/@port" |
| Kod  | var port = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["Endpoint1"]. IPEndpoint.Port; |





## <a name="example"></a>Przykład

Oto przykład roli Pracownik, który tworzy zadanie uruchamiania przy użyciu zmiennej środowiska o nazwie `TestIsEmulated` ustaw [ @emulated wartość wyrażenia xpath](#app-running-in-emulator). 

```xml
<WorkerRole name="Role1">
    <ConfigurationSettings>
      <Setting name="Setting1" />
    </ConfigurationSettings>
    <LocalResources>
      <LocalStorage name="LocalStore1" sizeInMB="1024"/>
    </LocalResources>
    <Endpoints>
      <InternalEndpoint name="Endpoint1" protocol="tcp" />
    </Endpoints>
    <Startup>
      <Task commandLine="example.cmd inputParm">
        <Environment>
          <Variable name="TestConstant" value="Constant"/>
          <Variable name="TestEmptyValue" value=""/>
          <Variable name="TestIsEmulated">
            <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated"/>
          </Variable>
          ...
        </Environment>
      </Task>
    </Startup>
    <Runtime>
      <Environment>
        <Variable name="TestConstant" value="Constant"/>
        <Variable name="TestEmptyValue" value=""/>
        <Variable name="TestIsEmulated">
          <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated"/>
        </Variable>
        ...
      </Environment>
    </Runtime>
    ...
</WorkerRole>
```

## <a name="next-steps"></a>Następne kroki

Dowiedz się więcej o pliku [ServiceConfiguration.cscfg](cloud-services-model-and-package.md#serviceconfigurationcscfg) .

Utwórz pakiet [ServicePackage.cspkg](cloud-services-model-and-package.md#servicepackagecspkg) .

Włączanie [pulpitu zdalnego](cloud-services-role-enable-remote-desktop.md) dla roli.
