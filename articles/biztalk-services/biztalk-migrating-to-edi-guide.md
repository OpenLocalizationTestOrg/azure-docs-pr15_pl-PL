<properties   
    pageTitle="Migrowanie serwera BizTalk Edytuj rozwiązania do przewodnika techniczne usługi BizTalk | Microsoft Azure"
    description="Migrowanie grupy użytkowników do MABS; Usługi BizTalk Microsoft Azure"
    services="biztalk-services"
    documentationCenter="na"
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


# <a name="migrating-biztalk-server-edi-solutions-to-biztalk-services-technical-guide"></a>Migrowanie serwera BizTalk Edytuj rozwiązania do usług BizTalk: Podręcznik techniczny

Autor: Michał Wieman i Nitin Mehrotra

Recenzentów: Karthik Bharthy

Napisane przy użyciu: Microsoft Azure BizTalk Services — luty 2014 Zwolnij.

## <a name="introduction"></a>Wprowadzenie

(Lub grupy użytkowników) jest jednym z najbardziej rozpowszechnione oznacza przez dane programu exchange, które firmy drogą elektroniczną, również określane jako jako firma — firma lub B2B transakcji. Serwer BizTalk miał Edytuj obsługę przez dekadę, począwszy od początkowej BizTalk Server. Z usługami BizTalk Microsoft nadal obsługę rozwiązań Edytuj platformy Microsoft Azure. Transakcje B2B głównie pochodzi spoza organizacji, a stąd jest łatwiejsze do wdrożenia, jeśli został wdrożony na platformie chmury. Microsoft Azure obsługuje tę funkcję za pośrednictwem usługi BizTalk.

Gdy niektórzy klienci obejrzeć usług BizTalk jako platforma "greenfield" dla nowych rozwiązań grupy użytkowników, wielu klientów mieć bieżącego rozwiązania Edytuj serwer BizTalk, które chcą przeprowadzić migrację do Azure. Ponieważ Edytuj usług BizTalk są zaprojektowane pod oparte na tym samym obiektów klucza, gdy architektura Edytuj serwera BizTalk (obrotu partnerów, jednostki, umów), użytkownik może przeprowadzić migrację artefakty BizTalk Server lub grupy użytkowników do usługi BizTalk.

W tym dokumencie omówiono niektóre różnice związane z Migrowanie artefakty Edytuj serwer BizTalk do usług BizTalk. Ten dokument założono znają Edytuj serwer BizTalk przetwarzania i obrotu umów partnera. Uzyskać więcej informacji dotyczących serwera BizTalk lub grupy użytkowników zobacz [Trading partnera zarządzania przy użyciu BizTalk Server](https://msdn.microsoft.com/library/bb259970.aspx).

## <a name="which-version-of-biztalk-server-edi-artifacts-can-be-migrated-to-biztalk-services"></a>Jaka wersja programu BizTalk Server Edytuj artefakty możesz przeprowadzić migrację do usługi BizTalk?

Moduł Edytuj serwer BizTalk została znacznie ulepszona BizTalk Server 2010 po jego została zmienionym wyglądzie do uwzględnienia partnerów, profilów i umów. Usługi BizTalk zastosowano ten sam model do organizowania partnerów handlowych i firm podziały tych partnerów handlowych. W wyniku Migrowanie artefakty Edytuj z BizTalk Server 2010 i nowszych wersjach usług BizTalk, jest procesem bardziej bezpośrednio do przodu. Aby przeprowadzić migrację artefaktów Edytuj związanych z wersjami przed BizTalk Server 2010, możesz najpierw uaktualnienie do systemu BizTalk Server 2010, a następnie przeprowadzić migrację z artefaktów Edytuj do usług BizTalk.

## <a name="scenariosmessage-flow"></a>Przepływ scenariusze i komunikatów

Jako z serwerem BizTalk Edytuj przetwarzania w usługach BizTalk jest wbudowany wokół rozwiązanie zarządzania Trading partnera (TPM). Rozwiązanie TPM zawiera następujące kluczowe składniki:

- Partnerów handlowych, które odpowiadają organizacji w transakcji B2B.
- Profile, które przedstawiają podziały partnera handlowego.
- Trading umów partnera (lub umów), które odpowiadają Umowa biznesowa między dwa partnerów/profile.

Poniższa ilustracja przedstawia podobieństwa, a także różnic rozwiązanie Edytuj serwer BizTalk i Edytuj usług BizTalk rozwiązanie:

![][EDImessageflow]

Kluczowe różnice i podobieństwa przepływu rozwiązanie grupy użytkowników na serwerze BizTalk i BizTalk usług są:

- Tak, jak na podstawie planowana EDIReceive serwer BizTalk odbieranie wiadomości grupy użytkowników i planowana EDISend, aby wysłać wiadomość lub grupy użytkowników, usług BizTalk używa odbieranie Edytuj mostka do odbierania i wysyłanie Edytuj mostka wysyłania wiadomości grupy użytkowników. Na serwerze BizTalk procesy są skojarzone z umową za pomocą wysyłania lub odbierania porty. W usługach BizTalk samej Umowy oznacza wysyłania lub odbierania mostka.
- Na serwerze BizTalk po przetworzeniu proces EDIReceive wiadomości Edytuj wiadomość jest zrzucana do bazy danych programu SQL Server. Proces EdiSend następnie wybiera wiadomości z bazy danych programu SQL Server, przetwarza je, a następnie wysyła go do partnera handlowego.

    W usługach BizTalk po odbieranie Edytuj mostka przetwarza wiadomość grupy użytkowników, jego kieruje wiadomość do proces zewnętrzny. Proces zewnętrzny może działać na Microsoft Azure lub lokalnego. Proces zewnętrzny należy skierować wiadomość do mostka Wyślij Edytuj; Wyślij mostka nie założenia pobierać wiadomości. Po przetworzeniu wiadomości, Edytuj mostka Wyślij kieruje wiadomość do partnera handlowego.

Usługi BizTalk zapewnia łatwy w użyciu konfiguracji szybkie tworzenie i wdrażanie umowę B2B między partnerów handlowych bez konfigurowanie dowolnej Microsoft Azure obliczyć wystąpienia (role sieci Web lub pracownika), żadnej bazy danych Microsoft Azure SQL lub kont Microsoft Azure miejsca do magazynowania. Scenariusze bardziej złożone będzie wymagało wiązanie w przepływach pracy lub innego typu przetwarzania usługi "wokół krawędzi" umowę Trading partnera, przed lub po przetworzeniu mostka Trading Edytuj umowy partnera. Szczegółowo poniższa sekwencja zdarzeń występują w trakcie przetwarzania w usługach BizTalk wiadomości grupy użytkowników.

1. Otrzymaniu wiadomości grupy użytkowników z partnera, Fabrikam handlowego.  Odbieranie wiadomości grupy użytkowników od partnerów handlowych, usług BizTalk obsługuje protokoły transportowe, takich jak FTP, SFTP AS2 i HTTP/S.

2. Przetwarzanie handlowych odbieranie strony umowy partnera rozkłada wiadomości grupy użytkowników do formatu XML.  Zdemontowany wiadomości grupy użytkowników (w formacie XML) jest rozsyłana do punktów końcowych Bus usług, takich jak punktu końcowego usługi Bus przekazywania, usługa Bus tematu, kolejki Bus usługi lub mostka BizTalk usług.

3. Zdemontowany wiadomości XML może odebrany od punktu końcowego w celu dalszego przetwarzania niestandardowe.  Te punkty końcowe może przetworzyć składnika lokalnego lub wystąpienie Microsoft Azure obliczyć przetwarzania dalszych wiadomości w usłudze Windows Workflow (WF) lub Windows Communication Foundation (WCF), na przykład.

4. "Przetwarzanie Wyślij po stronie" partnera handlowego umowy następnie zbierane wiadomości XML do formatu Edytuj i przesyła go do partnera handlowego firmy Contoso.  Do wysyłania wiadomości grupy użytkowników do partnerów handlowych, usług BizTalk obsługuje protokoły używanych do odbierania wiadomości grupy użytkowników.

Dalsze ten dokument zawiera wskazówki pojęć związanych z migracji kilka różnych artefaktów Edytuj serwer BizTalk do usług BizTalk.

## <a name="sendreceive-ports-to-trading-partners"></a>Wyślij/Odbierz porty do partnerów handlowych

Na serwerze BizTalk skonfigurowaniu odbieranie lokalizacji i portów odbioru odbierać wiadomości grupy użytkowników i XML z partnerami handlowymi i skonfigurować porty Wyślij do wysyłania wiadomości grupy użytkowników i XML do partnera handlowego. Następnie wiążą się następujące porty handlowych umową partnera za pomocą konsoli administracyjnej serwera BizTalk. W usługach BizTalk, lokalizacji, w którym chcesz otrzymywać wiadomości od partnerów handlowych i miejsce, w którym wysyłasz wiadomości do partnerów handlowych są skonfigurowane w ramach handlowych partnera samej umowy (jako składnik ustawienia transportu) w portalu usługi BizTalk.  Dlatego nie naprawdę masz pojęcia "Wyślij porty" i "Odbieranie lokalizacje", na se w usługach BizTalk. Aby uzyskać więcej informacji zobacz [Tworzenie umów](https://msdn.microsoft.com/library/windowsazure/hh689908.aspx).

## <a name="pipelines-bridges"></a>Procesy (mosty)

Edytuj serwer BizTalk procesy są jednostki przetwarzania wiadomości, które również mogą zawierać niestandardowe logika dotycząca możliwości przetwarzania określonych, zależnie od potrzeb przez aplikację. Dla usług BizTalk równowartość będzie mostka grupy użytkowników. Jednak w usługach BizTalk, teraz, mosty grupy użytkowników są "zamknięty".  Oznacza to, że nie możesz dodać własne niestandardowe działania do mostka grupy użytkowników. Poza mostka Edytuj w aplikacji należy wszelkie niestandardowe przetwarzanie przed lub po wiadomości wprowadza mostka skonfigurowanego jako część umowy Trading partnera. Mosty EAI mogli wykonać czynności niestandardowe. Jeśli chcesz czynności niestandardowe, służy mosty EAI przed lub po wiadomość jest przetwarzana przez mostka Edytuj. Aby uzyskać więcej informacji zobacz [jak zawiera niestandardowy kod w mosty](https://msdn.microsoft.com/library/azure/dn232389.aspx).

Można wstawiać Publikuj i Subskrybuj przepływu z kodu niestandardowego i/lub za pomocą wiadomości kolejki i tematy przed umowę partnerów handlowych odbiera wiadomości lub po umowę przetwarza wiadomość i przekierowuje go do punktu końcowego usługi Bus Bus usługi.

Zobacz **Przepływu scenariusze-wiadomości** w tym temacie deseń przepływu wiadomości.

## <a name="agreements"></a>Umów

Jeśli znasz Server 2010 handlowych partnera umowy na potrzeby przetwarzania grupy użytkowników, usług BizTalk trading umów partnera Sprawdź dobrze znany. Większość ustawień umowy są takie same i używanie tą samą terminologię. W niektórych przypadkach ustawienia umowy są znacznie upraszcza w porównaniu do tych samych ustawień na serwerze BizTalk. Obsługa usług BizTalk firmy Microsoft Azure X12, EDIFACT i AS2 transportu.

Microsoft Azure BizTalk Services udostępnia narzędzia do **Migracji danych TPM** przeprowadzić migrację handlowych partnerów i umów z modułu partnera Trading serwera BizTalk do portalu usługi BizTalk. Narzędzia do migracji danych TPM jest dostępna w ramach pakietu Narzędzia, które można pobrać z [MABS SDK](http://go.microsoft.com/fwlink/p/?LinkId=235057). Pakiet zawiera również plik readme, który zawiera instrukcje dotyczące używania narzędzia i podstawowe informacje dotyczące rozwiązywania problemów dla wybranego narzędzia.

## <a name="schemas"></a>Schematy

Usługi BizTalk zapewniają schematy grupy użytkowników, które mogą być używane w usługach BizTalk rozwiązań.  Ponadto schematy Edytuj serwer BizTalk można również z usługami BizTalk ponieważ węzeł główny schematu Edytuj jest tym samym na serwerze BizTalk, a także usługi BizTalk. W ten sposób można bezpośrednio wykonać usługi schematy Edytuj serwer BizTalk i używanie ich w tworzonych za pomocą usług BizTalk rozwiązania grupy użytkowników. Możesz również pobrać schematy z [Zestawu SDK MABS](http://go.microsoft.com/fwlink/p/?LinkId=235057).

## <a name="maps-transforms"></a>Mapy (transformacji)

Mapy na serwerze BizTalk są nazywane przekształcenia w usługach BizTalk. Migrowanie map z serwera BizTalk do usług BizTalk może być jedna z bardziej złożone zadań w celu uzyskania (w zależności od złożoności mapy). Narzędzie mapowania używane dla usług BizTalk różni się od mapowania BizTalk. Mimo że mapowania głównie wygląda tak samo, różni się podstawowym formacie mapy. Różnią się również functoids (nazywane **Operacje Map** w usługach BizTalk) dostępne dla użytkowników.  Nie można w praktyce bezpośrednio użyć mapy BizTalk w usługach BizTalk. Ponadto nie wszystkie dostępne na serwerze BizTalk functoids są dostępne jako operacje map w usługach BizTalk.

### <a name="new-transform-operations"></a>Nowe operacje przekształcania

Podczas listy operacji mapy przekształcania może wydawać się bardzo różne mapowania serwera BizTalk, przekształceń usług BizTalk mają nowych sposobów te same zadania. Na przykład przekształceń usług BizTalk mają **Operacji na liście** dostępne. To nie jest dostępna w mapowania BizTalk.  **Operacje na liście** umożliwiają tworzenie i mają dotyczyć "List", gdzie lista jest to zestaw elementów (nazywane także "wiersze") i gdzie każdy element może mieć wiele składników (nazywane także "kolumny").  Możesz posortować listę, zaznacz elementy według stanu, itp.

Inny przykład nowych funkcji przekształceń usług BizTalk są **Operacje pętli**.  Jest trudne do utworzenia pętli zagnieżdżonych w mapowania serwera BizTalk.  W związku z tym operacje mapy pętli są dodawane do przekształceń BizTalk usług.

Inny przykład jest jeszcze **Jeśli-to-inaczej** operacja Mapa wyrażenia.  Wykonywanie operacji Jeśli to inaczej było możliwe w mapowania BizTalk, ale jest wymagane wielu functoids pozornie proste zadania.

### <a name="migrating-biztalk-server-maps"></a>Migrowanie serwera BizTalk mapy

Usługi BizTalk Microsoft Azure udostępnia narzędzia do migracji serwera BizTalk mapy do przekształcenia BizTalk usług. **BTMMigrationTool** jest dostępny jako część pakietu **Narzędzia** do dyspozycji [Pobierz BizTalk Services SDK](http://go.microsoft.com/fwlink/p/?LinkId=235057). Aby uzyskać więcej informacji na temat narzędzia zobacz [Konwertowanie Mapa BizTalk Przekształcanie usług BizTalk](https://msdn.microsoft.com/library/windowsazure/hh949812.aspx).

Może też przeglądać próbki przez Sandro Pereira, specjalista MVP w dziedzinie BizTalk, o tym, jak przeprowadzić [migrację map BizTalk Server do przekształcenia BizTalk usług](http://social.technet.microsoft.com/wiki/contents/articles/23220.migrating-biztalk-server-maps-to-windows-azure-biztalk-services-wabs-maps.aspx).

## <a name="orchestrations"></a>Orchestrations

Jeśli musisz przeprowadzić migrację aranżacji serwera BizTalk przetwarzania Microsoft Azure, orchestrations należy ulegną, ponieważ Microsoft Azure nie obsługuje uruchomionego orchestrations serwera BizTalk.  Można zmodyfikować funkcji aranżacji w usłudze Windows przepływu pracy Foundation 4.0 (WF4).  Będzie zmiana wykonane, że jest obecnie nie przeprowadzania migracji z serwera BizTalk orchestrations do WF4. Oto niektóre zasoby dla przepływu pracy systemu Windows:

- [*Jak zintegrować usługę WCF przepływu pracy z kolejek Bus usług i tematy*](https://msdn.microsoft.com/library/azure/hh709041.aspx) przez Paolo Salvatori. 

- [ *Tworzenie aplikacji przy użyciu programu Windows Workflow Foundation i Azure* sesji](http://go.microsoft.com/fwlink/p/?LinkId=237314) z konferencji 2011 kompilacji.

- [*Centrum deweloperów Foundation przepływu pracy systemu Windows*](http://go.microsoft.com/fwlink/p/?LinkId=237315) w witrynie MSDN.

- [*Dokumentacja 4 Foundation przepływu pracy systemu Windows (WF4)*](https://msdn.microsoft.com/library/dd489441.aspx) w witrynie MSDN.

## <a name="other-considerations"></a>Inne zagadnienia

Poniżej przedstawiono kilka zagadnienia, które należy wykonać podczas korzystania z usług BizTalk.

### <a name="fallback-agreements"></a>Umów alternatywnych

Edytuj serwer BizTalk przetwarzanie ma pojęcia "Powrót umów".  Czy usług BizTalk **nie** korzystają z pojęcia umowy powrót pory.  W tematach BizTalk dokumentacji [Roli umów podczas przetwarzania grupy użytkowników](http://go.microsoft.com/fwlink/p/?LinkId=237317) i [Konfigurowanie globalnego lub powrót umowy właściwości](https://msdn.microsoft.com/library/bb245981.aspx) informacji na używania umów powrót na serwerze BizTalk.

### <a name="routing-to-multiple-destinations"></a>Rozsyłanie do wielu miejsc docelowych

Mosty usług BizTalk w jego bieżącym stanie nie obsługuje przesyłania wiadomości do wielu miejsc docelowych przy użyciu publikowanie-subskrypcji modelu. Zamiast tego można przesyłać wiadomości z mostka usług BizTalk w temacie Bus usługa może być wiele subskrypcji komunikat na więcej niż jeden z punktów końcowych.

## <a name="conclusion"></a>Wnioski

Microsoft Azure BizTalk usług są aktualizowane w regularnych punkty kontrolne, aby dodać więcej funkcji i możliwości. Przy każdej aktualizacji firma Microsoft czekamy na pomocniczych lepszą funkcje ułatwiające tworzenie rozwiązań zakończenia do końca przy użyciu usług BizTalk i inne technologie Azure.

## <a name="see-also"></a>Zobacz też

[Tworzenie aplikacji przedsiębiorstwa z platformy Azure](https://msdn.microsoft.com/library/azure/hh674490.aspx)

[EDImessageflow]: ./media/biztalk-migrating-to-edi-guide/IC719455.png
