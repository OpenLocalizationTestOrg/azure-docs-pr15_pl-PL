<properties
    pageTitle="Usługi Azure AD domeny: Włączanie usług domenowych AD Azure | Microsoft Azure"
    description="Wprowadzenie do usług domenowych Active Directory platformy Azure"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/19/2016"
    ms.author="maheshu"/>

# <a name="enable-azure-ad-domain-services"></a>Włączanie usług domenowych AD Azure

## <a name="task-3-enable-azure-ad-domain-services"></a>Zadanie 3: Włączanie usług domenowych AD Azure
W tym zadaniu można włączyć usługi Azure AD domeny w katalogu. Wykonaj poniższe kroki konfiguracji, aby włączyć usługi Azure AD domeny w katalogu.

1. Przejdź do **portalu klasyczny Azure** ([https://manage.windowsazure.com](https://manage.windowsazure.com)).

2. Wybierz węzeł **Usługi Active Directory** w okienku po lewej stronie.

3. Wybierz pozycję dzierżawy Azure AD (katalog), dla którego chcesz włączyć usługi Azure AD domeny.

    ![Wybierz katalog Azure AD](./media/active-directory-domain-services-getting-started/select-aad-directory.png)

4. Kliknij kartę **Konfiguruj** .

    ![Konfigurowanie karty katalogu](./media/active-directory-domain-services-getting-started/configure-tab.png)

5. Przewiń w dół do sekcji **usług domenowych**.

    ![Sekcja konfiguracji usług domeny](./media/active-directory-domain-services-getting-started/domain-services-configuration.png)

6. Przełącz opcję tytuł **włączyć usługi domeny dla tego katalogu** **Tak**. Można zauważyć kilka więcej opcji konfiguracji usługi Azure AD domeny są wyświetlane na stronie.

    ![Włączanie usług domenowych](./media/active-directory-domain-services-getting-started/enable-domain-services.png)

    > [AZURE.NOTE] Po włączeniu usługi Azure AD domen dla dzierżawy usługi Azure AD generuje i przechowuje mieszania poświadczeń Kerberos i NTLM, które są wymagane do uwierzytelniania użytkowników.

7. Określ **nazwę domeny DNS domeny usługi**.

   - Domyślną nazwę domeny w katalogu (czyli kończące się literami **. onmicrosoft.com** sufiks domeny) jest zaznaczona domyślnie.

   - Lista zawiera wszystkie domeny, które zostały skonfigurowane dla katalogu Azure AD — w tym zweryfikowane, a także niezweryfikowany domen, skonfigurowane na karcie "Domeny".

   - Ponadto można też wpisać niestandardowej nazwy domeny. W tym przykładzie możemy wpisana w niestandardowej nazwy domeny "contoso100.com".

     > [AZURE.WARNING] Upewnij się, że prefiks domeny nazwy domeny, której określ (na przykład "contoso100" w polu Nazwa domeny "contoso100.com") jest mniej niż 15 znaków. Nie można utworzyć Azure AD domenie usług domenowych z prefiksem domeny więcej niż 15 znaków.

8. Upewnij się, że nazwa domeny DNS wybrana dla domeny zarządzanych jeszcze nie istnieje w wirtualnej sieci. W szczególności, czy:

   - masz już domenę z tą samą nazwą domeny DNS w sieci wirtualnej.

   - wirtualnej sieci, wybrane połączenie VPN z siecią lokalnego i masz domeny do współpracy z tą samą nazwą domeny DNS w sieci lokalnej.

   - masz istniejące usługi w chmurze o tej nazwie w wirtualnej sieci.

9. Następnym krokiem jest zaznacz wirtualnej sieci, w którym chcesz usług domenowych AD Azure, aby był dostępny. Wybierz pozycję wirtualnej sieci i dedykowane utworzoną z listy rozwijanej, tytuł **usług domenowych łączenie się z tą siecią wirtualną**podsieć.

   - Upewnij się, że wirtualnej sieci, które określono należy do regionu Azure obsługiwanych przez usługi Azure AD domeny. Odwołują się do strony [usługi Azure według regionów](https://azure.microsoft.com/regions/#services/) wiedzieć Azure regiony, w których Azure AD Domain Services jest dostępny.

   - Wirtualnych sieci należących do regionu, w którym usług domenowych AD Azure nie jest obsługiwane nie są wyświetlane na liście rozwijanej.
   
   - Dedykowane podsieci wirtualną sieć za pomocą usługi Azure AD domeny. Upewnij się, że nie należy zaznaczać podsieci bramy. Zobacz [Uwagi dotyczące sieci](active-directory-ds-networking.md). 

   - Podobnie wirtualnych sieci, które zostały utworzone za pomocą Menedżera zasobów Azure nie jest wyświetlana na liście rozwijanej. Menedżer zasobów — wirtualnej sieci nie są obecnie obsługiwane w usługach domenowych AD Azure.

10. Aby włączyć usługi Azure AD domeny, w okienku zadań u dołu strony kliknij przycisk **Zapisz** .

11. Na stronie są wyświetlane "Oczekiwanie na..." Stan, gdy dla katalogu włączone jest usług domenowych Azure AD.

    ![Włączanie usług domenowych — w stanie oczekiwania](./media/active-directory-domain-services-getting-started/enable-domain-services-pendingstate.png)

    > [AZURE.NOTE] Azure usług domenowych AD zapewnia wysoką dostępność dla zarządzanych domeny. Po włączeniu usług domenowych AD Azure, zwróć uwagę, adresów IP, w których usługi domeny są dostępne w wirtualnej sieci są wyświetlane po kolei. Drugi adres IP zostanie wyświetlony wkrótce, jak szybko usługi umożliwia wysoką dostępność dla swojej domeny. Gdy szybkiej jest skonfigurowane i aktywne dla swojej domeny, powinny być widoczne dwie adresów IP w **usługach domenowych** części na karcie **Konfigurowanie** .

12. Po około 20 – 30 minut zostanie wyświetlony pierwszy adres IP, w którym jest dostępna w sieci wirtualnej w polu **adres IP** , na stronie **Konfigurowanie** usług domenowych.

    ![Usług domenowych enabled - IP pierwszego obsługi administracyjnej](./media/active-directory-domain-services-getting-started/domain-services-enabled-firstdc-available.png)

13. Gdy szybkiej działa dla swojej domeny, zobaczysz dwa adresy IP wyświetlane na stronie. Zarządzane domeny jest dostępna w wybranych wirtualnej sieci w obu tych adresów IP. Zanotuj adresów IP, możesz zaktualizować ustawienia DNS dla wirtualnych sieci. Ten krok umożliwia maszyn wirtualnych w wirtualnej sieci, aby połączyć się z domeną dla operacji, takich jak dołączyć do domeny.

    ![Domeny usługi włączone — zarówno adresy IP obsługi administracyjnej](./media/active-directory-domain-services-getting-started/domain-services-enabled-bothdcs-available.png)

> [AZURE.NOTE] W zależności od rozmiaru dzierżawcy usługi Azure AD (liczba użytkowników, grupy itp.), synchronizacji z domeną zarządzanych zajmie trochę czasu. Ten proces synchronizacja ma miejsce w tle. Dla dużych dzierżaw z dziesiątki tysięcy obiektów może upłynąć dzień lub dwa dla wszystkich użytkowników, członkostwa w grupach i poświadczeń w celu zsynchronizowania.

<br>

## <a name="task-4---update-dns-settings-for-the-azure-virtual-network"></a>Zadanie 4 — aktualizacja ustawień DNS pod kątem Azure wirtualnej sieci
[Aktualizowanie ustawień DNS dla Azure wirtualnej sieci](active-directory-ds-getting-started-dns.md)jest następnego zadania konfiguracji.
