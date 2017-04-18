<properties
   pageTitle="Rozwiązywanie problemów z role, które mogły zostać uruchomione | Microsoft Azure"
   description="Poniżej przedstawiono kilka typowych przyczyn, dlaczego rolę usługi w chmurze może się nie uruchamiać. Znajdują się też rozwiązania tych problemów."
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
   ms.date="09/02/2016"
   ms.author="v-six" />

# <a name="troubleshoot-cloud-service-roles-that-fail-to-start"></a>Rozwiązywanie problemów z ról usługi w chmurze, które mogły zostać uruchomione

Poniżej przedstawiono kilka typowych problemów i rozwiązań związane z usługami w chmurze Azure role, które mogły zostać uruchomione.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="missing-dlls-or-dependencies"></a>Brak dll lub zależności

Nie odpowiada ról i które są cykliczne między **Inicjowanie**, **zajęty**i **Zatrzymywanie** stany może być spowodowany dll brakujące lub zespołów.

Objawy Brak dll lub zestawy może być:

- Wystąpienia roli jest przewijanie **Inicjowanie**, **zajęty**i **Zatrzymywanie** Państwa.
- Wystąpienia roli został przeniesiony na **Gotowe** , ale jeśli przejdziesz do aplikacji sieci web, strony nie jest wyświetlane.

Istnieje kilka zalecane metody badania tych problemów.

## <a name="diagnose-missing-dll-issues-in-a-web-role"></a>Sprawdzanie pod kątem brakujących błędów DLL w roli sieci web

Po przejściu do witryny sieci Web, który jest używany w sieci web roli i przeglądarka wyświetla błąd serwera podobny do następującego, może to oznaczać, że brakuje DLL.

![Błąd serwera "/" aplikacji.](./media/cloud-services-troubleshoot-roles-that-fail-start/ic503388.png)

## <a name="diagnose-issues-by-turning-off-custom-errors"></a>Diagnozowanie problemów przez wyłączenie błędy niestandardowe

Konfigurując web.config dla roli web zestaw tryb błędu niestandardowego w celu wyłączenia i ponowne rozmieszczanie usługi można wyświetlić bardziej szczegółowe informacje o błędzie.

Aby wyświetlić błędy dokładniejszy bez korzystania z pulpitu zdalnego:

1. Otwórz rozwiązanie w programie Microsoft Visual Studio.

2. W **Eksploratorze rozwiązań**zlokalizuj plik web.config, a następnie otwórz go.

3. W pliku web.config Odszukaj sekcję system.web i Dodaj poniższy wiersz:

    ```xml
    <customErrors mode="Off" />
    ```

4. Zapisz plik.

5. Ponownie utworzyć pakiet i ponownie wdróż usługę.

Po usługi jest ponownego rozmieszczania serwera, zostanie wyświetlony komunikat o błędzie z nazwą zestawu brakujące lub DLL.

## <a name="diagnose-issues-by-viewing-the-error-remotely"></a>Sprawdzanie pod kątem błędów, wyświetlając komunikat o błędzie zdalnie

Za pomocą pulpitu zdalnego dostępu do roli i zdalne wyświetlanie bardziej szczegółowe informacje o błędzie. Aby wyświetlić błędy przy użyciu pulpitu zdalnego, wykonaj następujące czynności:

1. Upewnij się, że Azure SDK 1.3 lub nowszym jest zainstalowany.

2. Podczas wdrażania rozwiązania przy użyciu programu Visual Studio wybierz "Konfiguruj połączenia pulpitu zdalnego...". Aby uzyskać więcej informacji na temat konfigurowania Podłączanie pulpitu zdalnego Zobacz [Za pomocą pulpitu zdalnego z rolami Azure](../vs-azure-tools-remote-desktop-roles.md).

3. W portalu Microsoft Azure klasyczny po wystąpienie jest wyświetlany stan **Gotowe**, kliknij jedno z wystąpień roli.

4. Kliknij ikonę **Połącz** w obszarze **Dostęp zdalny do** wstążki.

5. Zaloguj się do maszyny wirtualnej przy użyciu poświadczeń, które zostały określone podczas konfiguracji pulpitu zdalnego.

6. Otwórz okno polecenia.

7. Typ `IPconfig`.

8. Uwaga wartość adresu IP protokołu IPV4.

9. Otwórz program Internet Explorer.

10. Wpisz adres i nazwę aplikacji sieci web. Na przykład `http://<IPV4 Address>/default.aspx`.

Przechodzenie do witryny sieci Web zwróci teraz dokładniejsze komunikaty o błędach:

* Błąd serwera "/" aplikacji.

* Opis: Wystąpił nieobsługiwany wyjątek podczas wykonywania bieżącego żądania sieci web. Przejrzyj śledzenia stosu, aby uzyskać więcej informacji o błędzie i pochodzenie w kodzie.

* Szczegóły wyjątku: System.IO.FIleNotFoundException: nie można załadować pliku lub zestawu "Microsoft.WindowsAzure.StorageClient, wersja = 1.1.0.0, kultury = neutralność, PublicKeyToken = 31bf856ad364e35' lub jednej z jego zależności. System nie może znaleźć określonego pliku.

Na przykład:

![Błąd serwera jawne "/" aplikacji](./media/cloud-services-troubleshoot-roles-that-fail-start/ic503389.png)

## <a name="diagnose-issues-by-using-the-compute-emulator"></a>Diagnozowanie problemów przy użyciu emulatora obliczeń

Emulator obliczeń Microsoft Azure umożliwia diagnozowanie i rozwiązywanie problemów brakujących zależności i web.config błędy.

Aby uzyskać najlepsze wyniki przy użyciu tej metody diagnostyki należy używać komputera lub maszyn wirtualnych, która ma czystej instalacji systemu Windows. Aby najlepiej symulować Azure środowisko, za pomocą systemu Windows Server 2008 R2 x64.

1. Zainstalować wersję autonomicznego [Azure SDK](https://azure.microsoft.com/downloads/).

2. Na komputerze rozwoju budowania projektu usługi cloud.

3. W Eksploratorze Windows przejdź do folderu bin\debug projektu usługi cloud.

4. Skopiuj plik folderu i .cscfg .csx do komputera, którego używasz do debugowania problemów.

5. Na komputerze Oczyść, Otwórz okno wiersza polecenia SDK Azure i wpisz `csrun.exe /devstore:start`.

6. W wierszu polecenia wpisz `run csrun <path to .csx folder> <path to .cscfg file> /launchBrowser`.

7. Po uruchomieniu roli pojawi się szczegółowe informacje o błędzie w programie Internet Explorer. Za pomocą standardowej systemu Windows, narzędzia do rozwiązywania problemów w celu dalszego przeanalizowania problemu.

## <a name="diagnose-issues-by-using-intellitrace"></a>Diagnozowanie problemów za pomocą IntelliTrace

Dla pracownika i role w sieci web, które używają programu .NET Framework 4 można użyć [IntelliTrace](https://msdn.microsoft.com/library/dd264915.aspx), która jest dostępna w [Programie Microsoft Visual Studio Ultimate](https://www.visualstudio.com/products/visual-studio-ultimate-with-MSDN-vs).

Wykonaj poniższe czynności, aby wdrożyć usługę IntelliTrace włączone:

1. Upewnij się, czy Azure SDK 1.3 lub nowszym jest zainstalowany.

2. Wdrażanie rozwiązania przy użyciu programu Visual Studio. Podczas wdrażania zaznacz pole wyboru **Włącz IntelliTrace dla ról .NET 4** .

3. Po uruchomieniu wystąpienia Otwórz **Eksploratora serwera**.

4. Rozwijanie **Azure\\usług w chmurze** węzeł i Znajdź rozmieszczenia.

5. Rozwijanie rozmieszczenia, aż zobaczysz wystąpienia roli. Kliknij prawym przyciskiem myszy na jednym z wystąpienia.

6. Wybierz pozycję **Dzienniki IntelliTrace widok**. **Podsumowanie IntelliTrace** zostanie otwarty.

7. Odszukaj sekcję wyjątki w podsumowaniu. W przypadku wyjątków sekcji będzie oznaczone **Wyjątku**.

8. Rozwiń **Wyjątku** i sprawdź błędy **System.IO.FileNotFoundException** podobny do następującego:

![Wyjątku, Brak pliku lub zestawu](./media/cloud-services-troubleshoot-roles-that-fail-start/ic503390.png)

## <a name="address-missing-dlls-and-assemblies"></a>Brak dll i zestawy

Aby rozwiązać zestawu błędów i brak DLL, wykonaj następujące czynności:

1. Otwórz rozwiązanie w programie Visual Studio.

2. W **Eksploratorze rozwiązań**Otwórz folder **odwołania** .

3. Kliknij zestaw identyfikowany w błąd.

4. W okienku **Właściwości** Znajdź **właściwość lokalną kopię** i ustaw wartość **PRAWDA**.

5. Ponownie wdróż usług w chmurze.

Po upewnieniu się, że wszystkie błędy zostały poprawione, należy wdrożyć usługę bez zaznaczając pole wyboru **Włącz IntelliTrace dla ról .NET 4** .

## <a name="next-steps"></a>Następne kroki

Wyświetl więcej [artykułów Rozwiązywanie problemów z](https://azure.microsoft.com/documentation/articles/?tag=top-support-issue&product=cloud-services) usługami w chmurze.

Aby dowiedzieć się, jak rozwiązywać problemy roli usługi cloud przy użyciu danych diagnostyki komputera Azure PaaS, zobacz [Piotr Williamson serii](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).
