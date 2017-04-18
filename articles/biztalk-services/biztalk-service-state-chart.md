<properties 
    pageTitle="Zadania dozwolone w różne stany lub stany w usługach BizTalk | Microsoft Azure" 
    description="Akcje operacje dozwolone w inny stan MABS: zatrzymywanie, Rozpoczęcie, uruchom ponownie, zawieszania, życiorys, usuwanie, skalowanie, aktualizowanie konfiguracji i kopii w górę" 
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



# <a name="biztalk-services-service-state-chart"></a>BizTalk usługi: Wykres stanu usługi
W zależności od bieżącego stanu usługi BizTalk istnieje operacji, które można lub nie można wykonać w usłudze BizTalk.

Na przykład możesz dodawać nową usługę BizTalk w portalu klasyczny Azure. Po pomyślnym zakończeniu, usługa BizTalk jest w stanie aktywnym. W stanie aktywne możesz wyłączyć usługę BizTalk. Jeśli Zatrzymaj zakończyło się pomyślnie, usługa BizTalk trafia do stanu zatrzymania. Jeśli Zatrzymaj kończy się niepowodzeniem, usługę BizTalk przechodzi do stanu StopFailed. W stanie StopFailed należy ponownie uruchomić usługę BizTalk. Jeśli spróbujesz operację, która nie jest dozwolona, takie jak życiorys usługę BizTalk następujący błąd:

**Operacja nie jest dozwolona**

Aby obsługi administracyjnej usługi BizTalk, przejdź do [usług BizTalk: portal klasyczny inicjowania obsługi administracyjnej Azure za pomocą](http://go.microsoft.com/fwlink/p/?LinkID=302280).

W poniższej tabeli wymieniono operacji lub akcje, które mogą być wykonywane, gdy usługa BizTalk znajduje się w określonym stanie. ✔ oznacza, że operacja jest dozwolona w tym stanie. Pusty wpis oznacza, że nie można wykonać operacji w tym stanie.

## <a name="start-stop-restart-suspend-resume-and-delete-operations"></a>Rozpocznij, Zatrzymaj, uruchom ponownie, zawieszania życiorysu i usuwanie operacji
<table border="1">
<tr>
        <th colspan="15">Operacja lub akcji</th>
</tr>

<tr>
        <th rowspan="18">Stan usługi BizTalk</th>
</tr>
<tr bgcolor="FAF9F9">
        <th> </th>
        <th>Rozpoczynanie</th>
        <th>Zatrzymywanie</th>
        <th>Uruchom ponownie</th>
        <th>Zawieszenia</th>
        <th>Życiorys</th>
        <th>Usuwanie</th>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Aktywne</b></td>
<td> </td>
<td><center>✔</center></td>
<td><center>✔</center></td>
<td><center>✔</center></td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Wyłączone</b></td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Zawieszone</b></td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Zatrzymano</b></td>
<td><center>✔</center></td>
<td> </td>
<td><center>✔</center></td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Usługa aktualizacji nie powiodła się</b></td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>DisableFailed</b></td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>EnableFailed</b></td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>StartFailed<br/>
StopFailed<br/>
RestartFailed</b></td>
<td><center>✔</center></td>
<td><center>✔</center></td>
<td><center>✔</center></td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>SuspendedFailed<br/>
ResumeFailed</b></td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
<td><center>✔</center></td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>CreatedFailed<br/>
RestoreFailed<br/></b></td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>ConfigUpdateFailed</b></td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>ScaleFailed</b></td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
</table>
<br/>

## <a name="scale-update-configuration-and-backup-operations"></a>Skala, konfigurację aktualizacji i kopii zapasowych
<table border="1">
<tr>
        <th colspan="15">Operacja lub akcji</th>
</tr>

<tr>
        <th rowspan="18">Stan usługi BizTalk</th>
</tr>
<tr bgcolor="FAF9F9">
        <th> </th>
        <th>Skala</th>
        <th>Aktualizowanie konfiguracji</th>
        <th>Wykonywanie kopii zapasowych</th>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Aktywne</b></td>
<td><center>✔</center></td>
<td><center>✔</center></td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Wyłączone</b></td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Zawieszone</b></td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Zatrzymano</b></td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Usługa aktualizacji nie powiodła się</b></td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>DisableFailed</b></td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>EnableFailed</b></td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>StartFailed<br/>
StopFailed<br/>
RestartFailed</b></td>
<td> </td>
<td><center>✔</center></td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>SuspendedFailed<br/>
ResumeFailed</b></td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>CreatedFailed<br/>
RestoreFailed<br/></b></td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>ConfigUpdateFailed</b></td>
<td> </td>
<td><center>✔</center></td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>ScaleFailed</b></td>
<td><center>✔</center></td>
<td> </td>
<td> </td>
</tr>
</table>

## <a name="see-also"></a>Zobacz też
- [Usługi BizTalk: Klasyczny portal Azure za pomocą inicjowania obsługi administracyjnej](http://go.microsoft.com/fwlink/p/?LinkID=302280)<br/>
- [BizTalk usługi: Karty pulpitu nawigacyjnego, monitorowanie i skali](http://go.microsoft.com/fwlink/p/?LinkID=302281)<br/>
- [Usługi BizTalk: Deweloper, podstawowej, standardowe i wykres wersji Premium](http://go.microsoft.com/fwlink/p/?LinkID=302279)<br/>
- [BizTalk usługi: Kopia zapasowa i przywracanie](http://go.microsoft.com/fwlink/p/?LinkID=329873)<br/>
- [Usługi BizTalk: ograniczanie](http://go.microsoft.com/fwlink/p/?LinkID=302282)<br/>
- [Usługi BizTalk: Nazwa wystawcy i klucza wystawcy](http://go.microsoft.com/fwlink/p/?LinkID=303941)<br/>
- [Jak mogę rozpocząć przy użyciu SDK usług BizTalk Azure](http://go.microsoft.com/fwlink/p/?LinkID=302335)


 
