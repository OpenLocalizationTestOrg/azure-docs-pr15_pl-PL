<properties
    pageTitle="Dodawanie dostawcy zasobów aplikacji sieci Web Azure stos | Microsoft Azure"
    description="Szczegółowe wskazówki dotyczące wdrażania aplikacji Web Apps w stos Azure"
    services="azure-stack"
    documentationCenter=""
    authors="ccompy, apwestgarth"
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

# <a name="add-a-web-apps-resource-provider-to-azure-stack"></a>Dodawanie aplikacji sieci Web dostawcy zasobów stos Azure

> [AZURE.NOTE] Poniższe informacje dotyczą tylko wdrożeniach TP1 stos Azure.

Dodawanie dostawcy zasobów aplikacji sieci Web do stosu Azure obejmuje siedem kroków:

1.  Pobierz wymagane składniki.
2.  Tworzenie certyfikatów może być używany przez aplikacje sieci Web stosem Azure.
3.  Pobierz etapu i instalowania aplikacji sieci Web stosem Azure za pomocą Instalatora. 
4.  Sprawdź poprawność instalacji aplikacji sieci Web.
5.  Tworzenie rekordów DNS dla frontonu i urządzenia do równoważenia obciążenia Management Server.
6.  Zarejestruj nowo wdrożonym dostawcy zasobów aplikacji sieci Web z ARM.
7.  Testowanie dostawcy zasobów aplikacji sieci Web.

## <a name="download-required-components"></a>Pobrać wymaganych składników

1.  Pobieranie pakietu [Instalatora Podgląd Azure stosem aplikacji usługi](http://aka.ms/azasinstaller). 
2.  Pobierz [Azure stos aplikacji usługi wdrażania Pomocnik skryptów](http://aka.ms/azashelper). 
3.  Wyodrębnianie plików z pliku zip skryptów Pomocnik, powinny być trzy skryptów:
    - Tworzenie AppServiceCerts.ps1
    - Tworzenie AppServiceDnsRecords.ps1
    - Rejestr AppServiceResourceProvider.ps1 

## <a name="create-certificates-to-be-used-by-azure-stack-web-apps"></a>Tworzenie certyfikatów może być używany przez aplikacje sieci Web stos Azure

Pierwszy skrypt działa z urząd certyfikacji stos Azure, aby utworzyć 3 certyfikatów, które są wymagane przez aplikacje sieci Web. Uruchom skrypt na ClientVM zyskujemy pewność, że korzystasz z programu PowerShell jako azurestack\administrator:
1.  W sesji programu PowerShell działającego jako **azurestack\administrator**Wykonaj skrypt **AppServiceCerts.ps1 Utwórz** .  Spowoduje to utworzenie trzy certyfikatów, w tym samym folderze co skrypt, które są wymagane przez aplikacje sieci Web.
2.  Wprowadź hasło, aby zabezpieczyć pliki pfx i zanotuj go jako należy wprowadzić ją w Instalatora aplikacji sieci Web stos Azure.

## <a name="use-the-installer-to-download-and-install-azure-stack-web-apps"></a>Aby pobrać i zainstalować aplikacje sieci Web stosu Azure za pomocą Instalatora pakietu

Instalator appservice.exe będzie:
1.  Monit o zaakceptować firmy Microsoft i innych firm umowy licencyjne.
2.  Zbierz informacje na temat wdrażania stos Azure.
3.  Tworzenie kontenera obiektów blob w stos Azure konta miejsca do magazynowania określonego.
4.  Pobierz pliki potrzebne do zainstalowania aplikacji sieci Web stosem Azure dostawcy zasobów.
5.  Przygotowywanie Zainstaluj, aby wdrażanie aplikacji sieci Web dostawcy zasobów w środowisku stos Azure.
6.  Przekaż pliki do konta usługi aplikacji miejsca do magazynowania.
7.  Prezentowanie informacji potrzebnych do Rozpoczynanie szablonu Azure Menedżera zasobów.

Poniższe kroki poprowadzą etapów instalacji:

>[AZURE.NOTE] Aby uruchomić Instalatora, należy użyć podwyższonym poziomem uprawnień konta (administrator lokalnego lub domeny). Jeśli możesz zalogować się jako azurestack\azurestackuser, zostanie wyświetlony monit o poświadczenia podwyższonym poziomem uprawnień. 

1.  Uruchamianie appservice.exe jako **azurestack\administrator**. 
2.  Kliknij pozycję **Rozmieść za pomocą Menedżera zasobów Azure**.

![Instalator Technical Preview 1 aplikacji usługi Azure stosem][1]

3.  Przejrzyj i zaakceptuj postanowienia licencyjne dotyczące wersji wstępnej oprogramowania firmy Microsoft, a następnie kliknij przycisk **Dalej**.
4.  Przejrzyj i zaakceptuj warunki trzecia partylicense, a następnie kliknij przycisk **Dalej**.
5.  Przejrzyj informacje o konfiguracji aplikacji usługi w chmurze, a następnie kliknij przycisk **Dalej**.

![Konfiguracja chmury aplikacji usługi Azure stos aplikacji usługi Technical Preview 1][2]

6. Kliknij przycisk **Połącz** (obok pola subskrypcje stos Azure).

![Stos Azure aplikacji usługi Technical Preview 1 aplikacji usługi Cloud konfiguracji ekranu dwóch][3]

7.  W oknie uwierzytelniania stosem Azure podać **konta Azure Active Directory usługi administratora** i **hasło**, a następnie kliknij przycisk **Zaloguj**.
**Uwaga:** To konto usługi Azure Active Directory podana po wdrożeniu Azure stosu.
    - Kliknij **Strzałkę w dół** po prawej stronie pola obok **Azure stosem subskrypcji** , a następnie wybierz subskrypcję.

![Stos Azure aplikacji usługi Technical Preview 1 subskrypcji zaznaczenia][5]

8.  Kliknij **Strzałkę w dół** po prawej stronie pola obok **Lokalizacji stos Azure**.
    - Wybierz pozycję **lokalny**.
9. Wprowadź **nazwę** dla administratora.
10. Wprowadź **hasło** administratora.
11. Przejrzyj **Szczegóły programu SQL Server** i wprowadź zmiany, w razie potrzeby.
12. Przejrzyj **Konta logowania administratora systemu** i wprowadzić zmiany w razie potrzeby.
13. Wprowadź **Hasło administratora systemu**.
14. Kliknij przycisk **Dalej**.  W tym momencie Instalator teraz sprawdzi szczegóły połączenia dla programu SQL Server opisane.

![Stos Azure aplikacji usługi Technical Preview 1 subskrypcji zaznaczenia][4]    

15. Kliknij przycisk **Przeglądaj** obok **Pliku certyfikatu SSL domyślne aplikacje sieci Web** i przejdź do używanie **. AzureStack.Local** certyfikat [utworzony wcześniej](#Create-Certificates-To-Be-Used-By-Azure-Stack-Web-Apps).
16. Wprowadź **Hasło certyfikatu** ustawiany po utworzeniu certyfikatów.
17. Kliknij przycisk **Przeglądaj** obok **Pliku certyfikatu SSL dostawcy zasobów** i przejdź do zarządzania **. AzureStack.Local** certyfikat [utworzony wcześniej](#Create-Certificates-To-Be-Used-By-Azure-Stack-Web-Apps).
18. Wprowadź **Hasło certyfikatu** ustawiany po utworzeniu certyfikatów.
19. Kliknij przycisk **Przeglądaj** obok **Pliku certyfikatów głównych dostawcy zasobów** i przejdź do zarządzania **. AzureStack.Local** certyfikat [utworzony wcześniej](#Create-Certificates-To-Be-Used-By-Azure-Stack-Web-Apps).
20. Kliknij przycisk **Następny** Instalator sprawdzi podane hasło certyfikatu.

![Informacje dotyczące certyfikatu Technical Preview 1 aplikacji usługi Azure stos][6]

Wdrażanie potrwa około 45 60 minut do wykonania.

![Postęp instalacji Technical Preview 1 aplikacji usługi Azure stos][7]

21. Po pomyślnym zakończeniu działania Instalatora pakietu kliknij przycisk **Zakończ**.

## <a name="validate-web-apps-installation"></a>Sprawdź poprawność instalacji aplikacji sieci Web

1.  Na tym **Komputerze obsługującym stosem Azure** Otwórz **Menedżera funkcji Hyper-V**.
2.  Znajdź **Maszyn wirtualnych CN0** i **Łączenie** maszyn wirtualnych.
![Menedżer funkcji Hyper-V Technical Preview 1 aplikacji usługi Azure stos][8]
3.  Na pulpicie ten maszyn wirtualnych **Sieci Web chmury Management Console**kliknij dwukrotnie.
![Konsola zarządzania Technical Preview 1 aplikacji usługi Azure stos][9]
4.  Przejdź do **zarządzania serwerów**.
5.  W przypadku wszystkich komputerach **Gotowe** przejdź do następnego kroku. 
![Stan 1 serwery zarządzane Technical Preview aplikacji usługi Azure stos][10]

## <a name="create-dns-records-for-the-management-server-and-front-end-load-balancers"></a>Tworzenie rekordów DNS dla serwera zarządzania i urządzenia do równoważenia obciążenia fronton
1.  Otwórz wystąpienie programu PowerShell jako **azurestack\administrator**.
2.  Przejdź do lokalizacji skryptów pobranego i wyodrębnionych w [kroku wstępne](#Download-Required-Components).
3.  Uruchamianie skryptu **Tworzenie AppServiceDnsRecords.ps1** , spowoduje to utworzenie wpisy DNS, aby włączyć żądania aplikacji sieci web i portalu do serwerów frontonu.  Podczas rozmieszczania ARM aplikacji sieci Web, dwa oprogramowanie urządzenia do równoważenia obciążenia (SLBs), zostały utworzone przez dostawcę zasobu w sieci. Wskazywały na serwerach frontonu oraz serwerów zarządzania. Portal i żądania oparty na architekturze ARM Azure stos aplikacji zasobów usługodawcy przejdź do Management Server.

## <a name="register-the-newly-deployed-azure-stack-web-apps-provider-with-arm"></a>Zarejestrowania nowo wdrożonym dostawcy aplikacji sieci Web stos Azure z ARM
1.  Otwórz wystąpienie programu PowerShell jako **azurestack\administrator**.
2.  Przejdź do lokalizacji skryptów pobranego i wyodrębnionych w [kroku wstępne](#Download-Required-Components).
3.  Uruchom skrypt **AppServiceResourceProvider.ps1 rejestru** . 

>[AZURE.NOTE] Wpisz nazwę użytkownika i hasło **dokładnie (w tym małe i wielkie litery)** , która została wprowadzona dla pola **Maszyny wirtualne administratora** i **hasło** w instalacji lub rejestracji dostawcy zasobu zakończy się niepowodzeniem.

## <a name="test-drive-azure-stack-web-apps"></a>Test dysk Azure stosem aplikacji Web Apps

Teraz, gdy masz wdrożony i zarejestrowana dostawcy zasobów aplikacji sieci Web, możesz przetestować, aby upewnić się, że dzierżaw można wdrażać aplikacje sieci web.

1.  W portalu stosu Azure kliknij pozycję Nowy, kliknij Web + Mobile i kliknij aplikacji sieci Web.
2.  W karta aplikacji sieci Web wpisz nazwę w polu aplikacji sieci Web.
3.  W obszarze Grupa zasobów kliknij pozycję Nowy, a następnie wpisz nazwę w polu Grupa zasobów. 
4.  Kliknij plan Lokalizacja usługi aplikacji, a następnie kliknij przycisk Utwórz nowy.
5.  W aplikacji usługi karta planu wpisz nazwę w polu plan aplikacji usługi.
6.  Kliknij przycisk warstwy ceny, kliknij pozycję Free udostępnione lub udostępnione udostępnione, kliknij przycisk Wybierz, kliknij przycisk OK, a następnie kliknij Utwórz.
7.  W obszarze minuty, kafelka dla nowej aplikacji sieci web zostanie wyświetlony na pulpicie nawigacyjnym. Kliknij w polu.
8.  W karta aplikacji sieci web kliknij przycisk Przeglądaj, aby wyświetlić domyślną witryną sieci Web dla tej aplikacji.


**Wdrażanie WordPress, DNN lub Django witryny sieci Web (opcjonalnie)**

1. W portalu stos Azure kliknij przycisk "+", przejdź do usługi Azure Marketplace wdrażanie Django witrynę sieci Web, a następnie poczekaj, aż pomyślnego zakończenia. Platforma sieci web Django używa bazy danych w oparciu o system plików i nie wymaga żadnych dostawców dodatkowych zasobów, takich jak SQL lub MySQL.  

2. Jeśli wdrożono także dostawcą zasobów MySQL, należy wdrożyć WordPress witryny sieci Web z witryny Marketplace. Gdy zostanie wyświetlony monit o parametry bazy danych, wprowadzić nazwę użytkownika jako *User1@Server1* (przy użyciu nazwy użytkownika i nazwa serwera wybranych przez użytkownika).

3. Jeśli wdrożono także dostawcą zasobów programu SQL Server, należy wdrożyć DNN witryny sieci Web z witryny Marketplace. Gdy zostanie wyświetlony monit o parametry bazy danych, wybierz bazę danych na komputerze z programem SQL Server, podłączonego do dostawcy zasobów.

## <a name="next-steps"></a>Następne kroki

Można także wypróbować inne [platformy jako usługa (PaaS) usługi](azure-stack-tools-paas-services.md), takie jak [dostawcy zasobów programu SQL Server](azure-stack-sql-rp-deploy-short.md) i [dostawcy zasobów MySQL](azure-stack-mysql-rp-deploy-short.md).

<!--Image references-->
[1]: ./media/azure-stack-webapps-deploy/AppService_exe_Start.png
[2]: ./media/azure-stack-webapps-deploy/AppService_exe_DefaultEntriesStep1.png
[3]: ./media/azure-stack-webapps-deploy/AppService_exe_DefaultEntriesStep2.png
[4]: ./media/azure-stack-webapps-deploy/AppService_exe_DefaultEntriesStep2_populated.png
[5]: ./media/azure-stack-webapps-deploy/AppService_exe_DefaultEntriesStep2_SubscriptionSelection.png
[6]: ./media/azure-stack-webapps-deploy/AppService_exe_DefaultEntriesStep3_Certificates.png
[7]: ./media/azure-stack-webapps-deploy/AppService_exe_InstallationProgress.png
[8]: ./media/azure-stack-webapps-deploy/HyperV.png
[9]: ./media/azure-stack-webapps-deploy/MMC.png
[10]: ./media/azure-stack-webapps-deploy/ManagedServers.png


<!--Links-->
[Azure_Stack_App_Service_preview_installer]: http://go.microsoft.com/fwlink/?LinkID=717531
[WebAppsDeployment]: http://go.microsoft.com/fwlink/?LinkId=723982
[AppServiceHelperScripts]: http://go.microsoft.com/fwlink/?LinkId=733525
