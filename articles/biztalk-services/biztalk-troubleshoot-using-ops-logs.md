<properties 
    pageTitle="Rozwiązywanie problemów z usługami BizTalk korzystanie z dzienników operacji | Microsoft Azure" 
    description="Rozwiązywanie problemów z usługami BizTalk za pomocą dzienników operacji. MABS, WABS" 
    services="biztalk-services" 
    documentationCenter="" 
    authors="MandiOhlinger" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="biztalk-services" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="mandia"/>


# <a name="biztalk-services-troubleshoot-using-operation-logs"></a>Usługami BizTalk: Rozwiązywanie problemów, korzystanie z dzienników operacji

## <a name="what-are-the-operation-logs"></a>Co to są dzienniki operacji
Dzienniki operacji to funkcja usługi zarządzania dostępnych w portalu klasyczny Azure, która umożliwia wyświetlanie historycznych dzienników operacji wykonywanych na usługi Azure, łącznie z usługami BizTalk. Umożliwia wyświetlanie danych historycznych związanych z operacjami zarządzania od Twojej subskrypcji usługi BizTalk od 180 dni.

> [AZURE.NOTE]Ta funkcja przechwytuje tylko dzienników operacji zarządzania BizTalk usług, takich jak uruchomienia usługi, kopii nawet itd. Operacje te są śledzone niezależnie od tego, czy są wykonywane za pomocą [BizTalk usługi REST API](http://msdn.microsoft.com/library/azure/dn232347.aspx)lub portalu klasyczny Azure. Aby uzyskać pełną listę operacji, które są śledzone przy użyciu usług zarządzania zobacz [Operacji prześledzonych przy użyciu Azure usług zarządzania](#bizops).<br/><br/>
To nie przechwytywać dzienniki działań związanych z środowisko uruchomieniowe usługi BizTalk (na przykład wiadomość jest przetwarzana przez mosty itd.). Aby wyświetlić te dzienniki, za pomocą widoku śledzenia z portalu usługi BizTalk. Aby uzyskać więcej informacji zobacz [Śledzenie wiadomości](http://msdn.microsoft.com/library/azure/hh949805.aspx).

## <a name="view-biztalk-services-operation-logs"></a>Wyświetlanie dzienników operacji usług BizTalk
1. W portalu klasyczny Azure wybierz **Usługi zarządzania**, a następnie wybierz kartę **Dzienniki operacji** .
2. Można filtrować dziennikach na podstawie innego parametrów takich jak subskrypcji, zakres dat, typ usługi (np. usługi BizTalk), nazwa usługi lub stanu operacji (powiodło się, niepowodzenie).
3. Zaznacz znacznik wyboru, aby wyświetlić na filtrowanej liście. Na poniższej ilustracji przedstawiono czynności związanych z testbiztalkservice:  ![wyświetlanie dzienników operacji][ViewLogs] 
4. Aby wyświetlić więcej informacji o określonej operacji, zaznacz wiersz, a następnie kliknij pozycję **Szczegóły** na pasku zadań u dołu.


## <a name="bizops"></a>Operacje śledzone przy użyciu usług zarządzania Azure
W poniższej tabeli przedstawiono czynności, które są śledzone przy użyciu usług zarządzania Azure:

Nazwa operacji | Zadanie
--- | ---
CreateBizTalkService | Operację, aby utworzyć nową usługę BizTalk
DeleteBizTalkService | Operacja, aby usunąć usługę BizTalk
RestartBizTalkService | Aby ponownie uruchomić usługę BizTalk operacja
StartBizTalkService | Operacja, aby uruchomić usługę BizTalk
StopBizTalkService | Operacja, aby zatrzymać usługę BizTalk
DisableBizTalkService | Aby wyłączyć usługę BizTalk operacja
EnableBizTalkService | Operacja, aby włączyć usługę BizTalk
BackupBizTalkService | Operacja do tworzenia kopii zapasowych usługi BizTalk
RestoreBizTalkService | Operacja, aby przywrócić usługę BizTalk z określonej kopii zapasowej
SuspendBizTalkService | Operacja zawieszenia usługi BizTalk
ResumeBizTalkService | Aby wznowić usługę BizTalk operacja
ScaleBizTalkService | Operacja przeskalować usługi BizTalk w górę lub w dół
ConfigUpdateBizTalkService | Operacja aktualizowania konfiguracji usługi BizTalk
ServiceUpdateBizTalkService | Operacja uaktualnić lub starszą wersję usługi BizTalk do innej wersji
PurgeBackupBizTalkService | Operacja, aby wyczyścić kopie zapasowe usługi BizTalk poza okres przechowywania


## <a name="see-also"></a>Zobacz też
- [Aby utworzyć kopię zapasową usługi BizTalk](http://go.microsoft.com/fwlink/p/?LinkID=325584)
- [Przywracanie z kopii zapasowej usługi BizTalk](http://go.microsoft.com/fwlink/p/?LinkID=325582)
- [Usługi BizTalk: Deweloper, podstawowej, standardowe i wykres wersji Premium](http://go.microsoft.com/fwlink/p/?LinkID=302279)
- [Usługi BizTalk: Klasyczny portal Azure za pomocą inicjowania obsługi administracyjnej](http://go.microsoft.com/fwlink/p/?LinkID=302280)
- [Usługi BizTalk: Wykres stan inicjowania obsługi administracyjnej](http://go.microsoft.com/fwlink/p/?LinkID=329870)
- [BizTalk usługi: Karty pulpitu nawigacyjnego, monitorowanie i skali](http://go.microsoft.com/fwlink/p/?LinkID=302281)
- [Usługi BizTalk: ograniczanie](http://go.microsoft.com/fwlink/p/?LinkID=302282)
- [Usługi BizTalk: Nazwa wystawcy i klucza wystawcy](http://go.microsoft.com/fwlink/p/?LinkID=303941)
- [Jak mogę rozpocząć przy użyciu SDK usług BizTalk Azure](http://go.microsoft.com/fwlink/p/?LinkID=302335)

[ViewLogs]: ./media/biztalk-troubleshoot-using-ops-logs/Operation-Logs.png
 
