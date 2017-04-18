<properties
  pageTitle="Pakiet Azure IoT i Azure Active Directory | Microsoft Azure"
  description="W tym artykule opisano, jak pakiet IoT Azure korzysta z usługi Azure Active Directory do zarządzania uprawnieniami."
  services=""
  suite="iot-suite"
  documentationCenter=""
  authors="aguilaaj"
  manager="timlt"
  editor=""/>

<tags
  ms.service="iot-suite"
  ms.devlang="na"
  ms.topic="article"
  ms.tgt_pltfrm="na"
  ms.workload="na"
  ms.date="10/24/2016"
  ms.author="araguila"/>
  
# <a name="permissions-on-the-azureiotsuitecom-site"></a>Uprawnienia w witrynie azureiotsuite.com

## <a name="what-happens-when-you-sign-in"></a>Co się dzieje po zalogowaniu się

Po pierwszym zalogowaniu się w [azureiotsuite.com][lnk-azureiotsuite], witryny określa poziomów uprawnień jest oparte na zaznaczonej dzierżawy Azure Active Directory (AAD) i Azure subskrypcji.

1.  Witryna najpierw znajdzie z platformy Azure dzierżaw AAD, które należą do, aby wypełnić listę dzierżaw widoczny obok nazwy użytkownika zalogowany. W tej chwili witryny można uzyskać tylko tokeny użytkownika dla jednej dzierżawy naraz. Dzięki temu podczas przełączania do innej dzierżawy za pomocą menu rozwijanego w prawym górnym rogu witryny ponownie rejestruje w dzierżawy uzyskać tokenów dla dzierżawy.

2.  Następnie witryny znajduje z platformy Azure subskrypcje, które skojarzono z zaznaczonego dzierżawy. Dostępne subskrypcje zostanie wyświetlony po utworzeniu nowego rozwiązania wstępnie skonfigurowane.

3.  Na koniec witryny pobiera wszystkie zasoby w grupach zasobów i subskrypcje oznakowane jako wstępnie rozwiązań i wypełnia kafelków na stronie głównej.

W poniższych sekcjach opisano role, mające kontrolowanie dostępu do wstępnie rozwiązań.

## <a name="aad-roles"></a>Role AAD

Role AAD rozwiązań należy wstępnie możliwości sterowania i zarządzanie użytkownikami w wstępnie rozwiązanie.

Można znaleźć więcej informacji dotyczących ról administratora w AAD w [artykule przypisywanie ról administratora w Azure AD][lnk-aad-admin], ale w tym artykule omówiono przede wszystkim **Administratora globalnego** i ról **Członek użytkowników domeny** używane przez wstępnie rozwiązań.

**Administratora globalnego:** Może być wielu administratorów globalnych na dzierżawcę AAD. Po utworzeniu dzierżawę AAD jest domyślnie administratora globalnego usługi dzierżawy. Administrator globalny umożliwia obsługę wstępnie rozwiązanie i przypisano roli **administratora** dla aplikacji umieszczona w ich dzierżawy AAD. Jeśli jednak innego użytkownika w tej samej dzierżawy AAD tworzy aplikację, domyślnej roli, których udzielono administratora globalnego jest **Tylko NIEJAWNE więcej**. Globalne Administratorzy mogą przypisywać role dla aplikacji za pomocą [portal Azure klasyczny][lnk-classic-portal].

**Członek użytkowników domeny:** Może być wiele domeny i użytkowników przypadająca na dzierżawę AAD. Użytkownik domeny umożliwia obsługę wstępnie rozwiązania w ramach [azureiotsuite.com] [ lnk-azureiotsuite] witryny. Rola domyślne, które otrzymują dla aplikacji, którą ich obsługi administracyjnej jest **administratora**. Mogą utworzyć aplikację przy użyciu skryptu build.cmd w [azure iot-remote monitorowania] [ lnk-rm-github-repo] lub [azure iot przewidywanych obsługi] [ lnk-pm-github-repo] repozytorium, ale domyślna rola otrzymują jest **NIEJAWNE tylko do odczytu**, nie masz uprawnień do przypisywania ról. Jeśli inny użytkownik w dzierżawie AAD tworzy aplikację, ich przypisana rola **NIEJAWNE tylko do odczytu** domyślnie dla tej aplikacji. Nie masz możliwość przypisywania ról dla aplikacji; dlatego nie można dodać użytkowników i ról użytkowników dla aplikacji nawet wtedy, gdy ich obsługi administracyjnej.

**Użytkownika Gość-Gość:** Może być wiele gościa użytkownicy goście na dzierżawcę AAD. Użytkownicy Gość mają ograniczony zestaw praw w dzierżawie AAD. W wyniku użytkowników gości nie obsługi administracyjnej wstępnie rozwiązanie w dzierżawie AAD.

Aby uzyskać więcej informacji zobacz następujące zasoby:

- [Tworzenie lub edytowanie użytkowników w Azure AD][lnk-create-edit-users]
- [Przypisywanie ról aplikacji w AAD][lnk-assign-app-roles]

## <a name="azure-subscription-administrator-roles"></a>Role administratora Azure subskrypcji

Role administratora Azure kontrolować możliwość Mapowanie subskrypcji usługi Azure AD dzierżawy.

Można dowiedzieć się więcej o rolach administratora Współtworzenie Azure, Administrator usługi i konta administratora w artykule [jak dodać lub zmienić Administrator Współtworzenie Azure, Administrator usługi i Administrator konta][lnk-admin-roles].

## <a name="application-roles"></a>Role aplikacji

Role aplikacji kontrolowanie dostępu do urządzenia w rozwiązaniu wstępnie skonfigurowane.

Istnieją dwa zdefiniowane i jedna rola niejawne zdefiniowane w aplikacji, która jest tworzona po świadczenie wstępnie rozwiązanie.

-   **ADMINISTRATOR:** Będzie miał pełną kontrolę, aby dodać, zarządzanie i usuwanie urządzeń

-   **Tylko do odczytu:** Ma możliwość wyświetlania urządzeń

-   **NIEJAWNE tylko do odczytu:** To jest taka sama jak tylko do odczytu, ale jest udzielane wszystkim użytkownikom dzierżawy usługi AAD. Zostało to zrobione dla wygody podczas opracowywania. Możesz usunąć tę rolę, zmieniając [RolePermissions.cs] [ lnk-resource-cs] plik źródłowy.

### <a name="changing-application-roles-for-a-user"></a>Zmienianie ról aplikacji dla użytkownika

Poniższa procedura umożliwia tworzenie użytkownika w usługi Active Directory administrator wstępnie rozwiązania.

Musisz być administratorem globalnym AAD zmieniać dla użytkownika:

1. Przejdź do [portalu klasyczny Azure][lnk-classic-portal].

2. Wybierz pozycję **usługi Active Directory**.

3. Kliknij nazwę Twojej dzierżawy AAD (jest to katalog, który został wybrany na azureiotsuite.com przygotowana rozwiązania).

4. Kliknij pozycję **aplikacje**.

5. Kliknij nazwę aplikacji, która jest zgodna z nazwą rozwiązanie wstępnie skonfigurowane. Jeśli nie widzisz aplikacji na liście, Przełącz listy **Pokaż** w dół do **aplikacji właścicielem mojej firmy** i kliknij znacznik wyboru.

7. Kliknij pozycję **Użytkownicy**.

8. Wybierz użytkownika, aby przełączyć role.

9. Kliknij przycisk **Przypisz** , a następnie wybierz rolę (na przykład **Administrator**) chcesz przypisać do użytkownika, kliknij znacznik wyboru.

## <a name="faq"></a>FAQ

### <a name="im-a-service-administrator-and-id-like-to-change-the-directory-mapping-between-my-subscription-and-a-specific-aad-tenant-how-do-i-do-this"></a>Jestem administratorem usługi i chcę zmienić mapowanie katalogu między subskrypcji i określonych dzierżawy AAD. Jak to zrobić?

1. Przejdź do [portalu klasyczny Azure][lnk-classic-portal], na liście usług po lewej stronie kliknij pozycję **Ustawienia** .

2. Wybierz subskrypcję, o których chcesz zmienić mapowanie katalogu do.

3. Kliknij przycisk **Edytuj katalogu**.

4. Wybierz **katalog** chcesz użyć na liście rozwijanej. Kliknij strzałkę do przodu.

5. Potwierdź mapowania katalogów i dotyczy administratorów współpracujących. Uwaga Jeśli chcesz przenieść z innego katalogu, administratorów współpracujących z oryginalnym katalogu zostaną usunięte.

### <a name="im-a-domain-usermember-on-the-aad-tenant-and-ive-created-a-preconfigured-solution-how-do-i-get-assigned-a-role-for-my-application"></a>Jest się członkiem domeny użytkownika/w dzierżawie AAD i zostały utworzone wstępnie rozwiązanie. Jak można uzyskać przypisaną rolę dla mojej aplikacji?

Poproś administratora globalnego, aby przypisać Ci jako administrator globalny w dzierżawie AAD, aby uzyskać uprawnienia, aby przypisać role użytkowników, samodzielnie lub poproś administratorem globalnym, aby przypisanie roli. Jeśli chcesz zmienić dzierżawy AAD wstępnie rozwiązanie zostało rozmieszczone w, zobacz następne pytanie.

### <a name="how-do-i-switch-the-aad-tenant-my-remote-monitoring-preconfigured-solution-and-application-are-assigned-to"></a>Jak przełączyć dzierżawy AAD Moje zdalnego monitorowania rozwiązanie wstępnie skonfigurowane i aplikacji są przypisywane?

Można uruchomić wdrożenia chmury z <https://github.com/Azure/azure-iot-remote-monitoring> i ponownie wdróż z nowo utworzonej dzierżawy AAD. Ponieważ są domyślnie administrator globalny i po utworzeniu nowej dzierżawy AAD, masz dostęp do dodawania użytkowników i przypisywanie ról do tych użytkowników.

1. Utwórz nowy katalog AAD w [portalu zarządzania Azure][lnk-classic-portal].

2. Przejdź do <https://github.com/Azure/azure-iot-remote-monitoring>.

3. Uruchamianie `build.cmd cloud [debug | release] {name of previously deployed remote monitoring solution}` (na przykład `build.cmd cloud debug myRMSolution`)

4. Gdy zostanie wyświetlony monit, ustaw **tenantid** się do nowo utworzonej dzierżawy zamiast dzierżawy usługi poprzedniego.


### <a name="i-want-to-change-a-service-administrator-or-co-administrator-when-logged-in-with-an-organisational-account"></a>Chcę zmienić przez administratora usługi lub Współtworzenie administratora podczas logowania się przy użyciu konta organizacyjnego

Zobacz artykuł pomocy technicznej [Zmiana administratora usługi i Współtworzenie administratora podczas logowania się przy użyciu konta organizacyjnego][lnk-service-admins].

### <a name="why-am-i-seeing-this-error-your-account-does-not-have-the-proper-permissions-to-create-a-solution-please-check-with-your-account-administrator-or-try-with-a-different-account"></a>Dlaczego jest wyświetlany ten błąd? "Konto nie ma odpowiednich uprawnień do utworzenia rozwiązanie. Sprawdź skontaktuj się z administratorem konta, lub spróbuj za pomocą innego konta."

Zapoznaj się ilustracji:

![][img-flowchart]

> [AZURE.NOTE] Jeśli nadal jest wyświetlany błąd po sprawdzania poprawności możesz administrator globalny w dzierżawie AAD i współpracujących administratora dla tej subskrypcji, poproś administratora konta usunięcia użytkownika i ponowne przypisywanie niezbędnych uprawnień w następującej kolejności: Dodawanie użytkownika jako administratora globalnego, a następnie dodaj użytkownika jako administrator współpracujących Azure subskrypcji. Jeśli problemy będą się powtarzać, skontaktuj się z [Pomoc i obsługa techniczna][lnk-help-support].

**Dlaczego jest wyświetlany ten błąd, gdy mam subskrypcji usługi Azure?** *Azure subskrypcji jest wymagane do tworzenia rozwiązań wstępnie skonfigurowane. Możesz utworzyć bezpłatne konto wersji próbnej na kilka minut.*

Jeśli masz pewność, że masz subskrypcję usługi Azure, sprawdź dzierżawy mapowania dla subskrypcji i upewnij się, że wybrano poprawne dzierżawy na liście rozwijanej. Jeśli zostały zatwierdzone odpowiedniej dzierżawy jest poprawny, należy wykonać na powyższym diagramie i sprawdź poprawność Mapowanie subskrypcji i dzierżawy AAD.

## <a name="next-steps"></a>Następne kroki

Aby kontynuować szkoleniowe na temat pakietu IoT, zobacz, jak można [dostosować wstępnie][lnk-customize].

[img-flowchart]: media/iot-suite-permissions/flowchart.png

[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-rm-github-repo]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-pm-github-repo]: https://github.com/Azure/azure-iot-predictive-maintenance
[lnk-aad-admin]: https://azure.microsoft.com/documentation/articles/active-directory-assign-admin-roles/
[lnk-classic-portal]: https://manage.windowsazure.com/
[lnk-create-edit-users]: https://azure.microsoft.com/documentation/articles/active-directory-create-users/
[lnk-assign-app-roles]: https://azure.microsoft.com/documentation/articles/active-directory-application-manifest/
[lnk-service-admins]: https://azure.microsoft.com/support/changing-service-admin-and-co-admin/
[lnk-admin-roles]: https://azure.microsoft.com/documentation/articles/billing-add-change-azure-subscription-administrator/
[lnk-resource-cs]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/DeviceAdministration/Web/Security/RolePermissions.cs
[lnk-help-support]: https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md
