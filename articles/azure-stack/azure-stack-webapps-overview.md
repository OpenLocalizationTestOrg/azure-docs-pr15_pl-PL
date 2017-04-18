<properties
    pageTitle="Omówienie aplikacji sieci Web Azure stos | Microsoft Azure"
    description="Omówienie aplikacji Web Apps w stos Azure"
    services="azure-stack"
    documentationCenter=""
    authors="apwestgarth"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="app-service"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="anwestg"/>
    
# <a name="azure-stack-web-apps-overview"></a>Omówienie aplikacji sieci Web stos Azure
    
> [AZURE.NOTE] Poniższe informacje dotyczą tylko wdrożeniach TP1 stos Azure.

Aplikacje sieci Web w usłudze Azure stos jest pierwszym elementem oferująca usługi aplikacji Azure pozyskano stos Azure. Instalator aplikacji sieci Web stos Azure utworzy wystąpienie każdego typu pięć ról wymagane i spowoduje utworzenie na serwerze plików. Mimo że można dodać kolejne wystąpienia dla każdego typu roli, należy pamiętać, że nie jest ilości miejsca do magazynowania dla maszyny wirtualne w Technical Preview 1. Bieżący funkcje dla aplikacji sieci Web stos Azure są przede wszystkim możliwości foundation, które są potrzebne do zarządzania aplikacji sieci web systemu i hosta.

![Aplikacje sieci Web aplikacji usługi Azure stos platformy Azure stosowo portalu][1]

## <a name="limitations-of-the-technical-preview"></a>Ograniczenia dotyczące Technical Preview

Istnieje nie jest obsługiwane dla wersji preview Azure stos aplikacji usługi. Nie umieszczaj obciążenia produkcji w tej wersji preview. Istnieje także uaktualnienie nie między Podgląd Azure stos aplikacji poprawki. Podstawowy celów tych wersji preview są pokazanie co firma Microsoft dostarczanie i uzyskać opinii. 

## <a name="what-is-an-app-service-plan"></a>Co to jest Plan usługi aplikacji?

Aplikacje Web stos Azure dostawcy zasobów używa tego samego kodu, która korzysta z funkcji Azure aplikacje sieci Web w usłudze Azure aplikacji. W wyniku niektóre typowe pojęcia są warte opisem. W aplikacjach sieci Web cennik kontenera dla aplikacji sieci web jest nazywany plan usług aplikacji. Reprezentuje zestaw maszyn wirtualnych dedykowane służy do przechowywania aplikacji. W przypadku danej subskrypcji możesz mieć wiele planów aplikacji usług. To samo dotyczy w aplikacjach sieci Web stos Azure. 

W Azure istnieją udostępnionych i dedykowane pracowników. Pracownikiem udostępnionego obsługuje hostingu aplikacji o dużej gęstości multitenant sieci web, a tylko jeden zestaw udostępnionych pracowników. Serwery dedykowane są tylko używane przez jeden dzierżawy i są dostępne w trzech rozmiarów: mały, średni i duży. Na potrzeby klienci lokalni nie zawsze opisany na podstawie tych terminów. W aplikacjach sieci Web stos Azure zasobów dostawcy Administratorzy mogą definiować poziomów pracownika, które mają być dostępne w taki sposób, że mają one wiele zestawów udostępnionego pracowników lub różnymi zestawami dedykowane pracowników według ich unikatowych potrzeb hostingu w sieci web. Za pomocą tych definicji warstwa pracownik, następnie definiować własne ceny wersji produktu.

## <a name="portal-features"></a>Funkcje portalu

Podobnie jak również z wewnętrzna, aplikacje sieci Web stos Azure używa samo interfejsu użytkownika korzystającego z aplikacji sieci Web Azure. Niektóre funkcje, które zostały wyłączone i nie są jeszcze w stos Azure. Jest to spowodowane oczekiwań specyficzne dla Azure lub usługi, które wymagają te funkcje nie są jeszcze dostępne w stos Azure. 

## <a name="next-steps"></a>Następne kroki

- [Przed rozpoczęciem pracy z aplikacjami Web stos Azure](azure-stack-webapps-before-you-get-started.md)
- [Instalowanie dostawcy zasobów aplikacji sieci Web](azure-stack-webapps-deploy.md)

Można także wypróbować inne [platformy jako usługa (PaaS) usługi](azure-stack-tools-paas-services.md), takie jak [dostawcy zasobów programu SQL Server](azure-stack-sql-rp-deploy-short.md) i [dostawcy zasobów MySQL](azure-stack-mysql-rp-deploy-short.md).

<!--Image references-->
[1]: ./media/azure-stack-webapps-overview/AppService_Portal.png
