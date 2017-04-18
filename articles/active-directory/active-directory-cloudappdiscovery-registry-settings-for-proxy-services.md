<properties 
    pageTitle="Ustawienia rejestru odnajdowania aplikacji dla serwera Proxy usług w chmurze | Microsoft Azure" 
    description="Celem w tym temacie jest dostarczanie czynności, które należy wykonać, aby ustawić wymagany port na komputerach z systemem agenta odnajdowania aplikacji chmury." 
    services="active-directory" 
    documentationCenter="" 
    authors="markusvi" 
    manager="femila"/>

<tags 
    ms.service="active-directory" 
    ms.workload="identity" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/10/2016" 
    ms.author="markusvi"/>

# <a name="cloud-app-discovery-registry-settings-for-proxy-services"></a>Ustawienia rejestru odnajdowania aplikacji chmury usługi serwera Proxy

Domyślnie agentem odnajdowania aplikacji chmurze jest skonfigurowany do używania tylko porty 80 i 443. Jeśli planujesz dotyczących instalowania chmury odnajdowania aplikacji w środowisku z serwera proxy, który korzysta z portu niestandardowego (80 ani 443), musisz skonfigurować usługi agentów do użycia tego portu. Konfiguracja jest oparty na klucza rejestru.


Celem w tym temacie jest dostarczanie czynności, które należy wykonać, aby ustawić port wymagane na komputerach z systemem agenta odnajdowania aplikacji chmury.



**Aby zmodyfikować port używany przez komputer z zainstalowanym agenta odnajdowania aplikacji w chmurze, należy wykonać następujące czynności:**


1. Uruchom Edytor rejestru. <br> ![Uruchom](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy01.png)

2. Przejdź do lub utwórz następujący klucz rejestru: <br> **HKLM_LOCAL_MACHINE\Software\Microsoft\Cloud Discovery\Endpoint aplikacji** 

3. Utwórz nową wartość **ciągu wielokrotnego** o nazwie **porty**. ![Nowy](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy02.png)

4. Aby otworzyć okno dialogowe **Edytowanie ciągu wielokrotnego** , kliknij dwukrotnie wartość porty.


5. W polu tekstowym dane wartości wpisz następujące wartości i dodać wszystkie niestandardowe porty, które są używane przez organizację: <br><br>
**80** <br>
**8080** <br>
**8118** <br>
**8888** <br>
**81** <br>
**12080** <br>
**6999** <br>
**30606** <br>
**31595** <br>
**4080** <br>
**443** <br>
**1110** <br><br>
![Edytowanie ciągu wielokrotnego](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy03.png)

6. Kliknij **przycisk OK** , aby zamknąć okno dialogowe **Edytowanie ciągu wielokrotnego** .



**Dodatkowe zasoby**


* [Jak odkryć chmurze unsanctioned aplikacje, które są używane w organizacji](active-directory-cloudappdiscovery-whatis.md) 


