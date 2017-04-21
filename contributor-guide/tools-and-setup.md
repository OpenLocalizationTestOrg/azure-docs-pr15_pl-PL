<properties
pageTitle="Instalowanie i Konfigurowanie narzędzia do tworzenia w GitHub"
description="Narzędzia i czynności, aby skonfigurowany do tworzenia Azure zawartości w GitHub."
services="contributor-guide"
documentationCenter=""
authors="tysonn"  
manager="carolz" />

<tags
ms.service="contributor-guide"
 ms.devlang=""
 ms.topic="article"
  ms.tgt_pltfrm=""
  ms.workload=""
  ms.date="01/19/2015"
  ms.author="tysonn" />

#<a name="install-and-set-up-tools-for-authoring-in-github"></a>Instalowanie i Konfigurowanie narzędzia do tworzenia w GitHub

Wykonaj czynności podane w tym artykule, aby skonfigurować narzędzia za Azure dokumentacji technicznej. Współautorzy nieformalnych i rzadkie prawdopodobnie za pomocą GitHub interfejsu użytkownika opisanych w kroku 2.

Jeśli znasz cyfra, warto przejrzeć kilka terminologii cyfra: [https://help.github.com/articles/github-glossary](https://help.github.com/articles/github-glossary). Ponadto tego wątku zdarzeń StackOverflow zawiera Glosariusz terminów cyfra będzie wystąpienia tego zestawu kroków: [http://stackoverflow.com/questions/7076164/terminology-used-by-git](http://stackoverflow.com/questions/7076164/terminology-used-by-git)

## <a name="contents"></a>Zawartość

- [Utworzenie konta GitHub i konfiguracja profilu](#create-a-github-account-and-set-up-your-profile)
- [Utwórz konto w usłudze Disqus](#sign-up-for-disqus)
- [Określanie, czy naprawdę potrzebujesz wykonaj pozostałe kroki](#determine-whether-you-really-need-to-follow-the-rest-of-these-steps)
- [Uprawnienia w GitHub](#permissions-in-github)
- [Instalowanie cyfra dla systemu Windows](#install-git-for-windows)
- [Włącz uwierzytelnianie dwuskładnikowe](#enable-two-factor-authentication)
- [Instalowanie edytora promocji cenowych](#install-a-markdown-editor)
- [Konfigurowanie Atom](#configure-atom)
- [Rozwidlenia repozytorium i skopiuj go na komputerze](#fork-the-repository-and-copy-it-to-your-computer)
- [Skonfiguruj swoją nazwę użytkownika i lokalnie w wiadomości e-mail](#configure-your-user-name-and-email-locally)
- [Następne kroki](#next-steps)

## <a name="create-a-github-account-and-set-up-your-profile"></a>Utworzenie konta GitHub i konfiguracja profilu

Do współtworzenia Azure techniczne zawartość, musisz mieć konto [GitHub](http://www.github.com) .

Jeśli jesteś współautora firmy Microsoft, musisz skonfigurować konto GitHub, więc możesz już zidentyfikowane jako pracownik Microsoft. Konfigurowanie profilu w następujący sposób:

- **Obraz profilu**: obraz o Tobie (wymagany)
- **Nazwa**: usługi imię i nazwisko (wymagany)
- **E-mail**: Twój adres e-mail firmy Microsoft (opcjonalnie)
- **Firma**: Microsoft Corporation (wymagany)
- **Lokalizacja**: listy lokalizacji (opcjonalnie)

Twój profil powinien wyglądać tego profilu:

<p align="center">
 ![Przykład profilu GitHub](./media/tools-and-setup/githubprofile.png)

## <a name="sign-up-for-disqus"></a>Utwórz konto w usłudze Disqus

Co opublikowanych Azure artykuł techniczny ma strumień komentarz udostępniane przez usługę Disqus.

 ![Discus logo](./media/tools-and-setup/discus.png)

W przypadku pracownika firmy Microsoft i autora lub współautora do artykułu, musisz Załóż Disqus, aby wziąć udział w strumieniu komentarza do tego artykułu.

1. Załóż konto u [http://www.disqus.com/](http://www.disqus.com/)
2. Wypełnij formularz profilu w następujący sposób:

 - **Imię i nazwisko**: imię i nazwisko tak jak z listy książki adres firmy Microsoft oraz informacje w nawiasach kwadratowych, czyli aliasu oraz @MSFT. Format: *imię nazwisko [alias@MSFT] *
 - **Lokalizacja**: swojej lokalizacji
 - **Krótki mnie**: tytuł

## <a name="determine-whether-you-really-need-to-follow-the-rest-of-these-steps"></a>Określanie, czy naprawdę potrzebujesz wykonaj pozostałe kroki

Nie może być konieczne do wykonania tych kroków w tym artykule. To zależy od rodzaju zawartości udziału chcesz lub do podjęcia.

###<a name="submit-a-text-only-change-to-an-existing-article"></a>Przesyłanie tylko tekst zmiany do istniejącego artykułu

Jeśli tylko potrzebne lub chcesz wprowadzić aktualizacje tekstowych do istniejącego artykułu, prawdopodobnie nie musisz wykonaj pozostałe kroki. Za pomocą edytora promocji cenowych oparte na sieci web firmy GitHub Aby przesłać zmiany. Kliknij łącze GitHub w artykule, który chcesz zmodyfikować:

 ![Przykład profilu GitHub](./media/tools-and-setup/contributetogit.png)

 Następnie kliknij ikonę edycji w wersji GitHub artykułu

 ![Przykład profilu GitHub](./media/tools-and-setup/editicon.PNG)

 Która otwiera edytor prostych w użyciu sieci web, który ułatwia przesyłanie zmian. Nie musisz wykonaj czynności opisane w tym artykule.

###<a name="all-other-changes"></a>Wszystkie inne zmiany
Interfejs użytkownika GitHub obsługuje tworzenia nowych plików oraz przeciąganie i upuszczanie obrazów. Jednak podczas pracy w interfejsie użytkownika, zarządzanie gałęziami może być skomplikowana, zaleca się zwykle zainstalować narzędzia i polecenia służące do tworzenia i zarządzania nimi artykuły informacje. Jeśli chcesz użyć interfejsu użytkownika, zobacz:

- [Tworzenie plików na Github](https://github.com/blog/1327-creating-files-on-github)
- [Przekazywanie plików do usługi repozytoria](https://github.com/blog/2105-upload-files-to-your-repositories)

Dla następujących rodzajów pracy zalecamy instalowanie i Dowiedz się, jak za pomocą narzędzi:

 - Wprowadzenie istotnych zmian do artykułu
 - Tworzenie i publikowanie nowego artykułu
 - Dodawanie nowych obrazów lub aktualizowanie obrazów
 - Aktualizowanie artykuł dni bez publikowania zmian z tych dni w okresie
 - Tworzenie zawartości dla wersji, zawierającą usługi w określonym dniu w określonym czasie

##<a name="permissions-in-github"></a>Uprawnienia w GitHub

Każda z kontem GitHub współtworzyć Azure techniczne zawartości za pomocą naszych publicznej repozytorium na [https://github.com/Azure/azure-content](https://github.com/Azure/azure-content). Nie specjalne uprawnienia są wymagane.

Jeśli jesteś PM firmy Microsoft lub zapis, który pracuje nad Azure zawartości, należy pracować w naszym prywatne repozytorium zawartości azure zawartości — pr. Odwiedź stronę [https://repos.opensource.microsoft.com](https://repos.opensource.microsoft.com ) , aby zażądać uprawnienia do odczytu, umożliwiające podejmowanie służąca do prywatnych repo - Zaloguj się przy użyciu przycisku GitHub > kliknij Azure > kliknij pozycję **Dołącz do zespołu** lub **Dołączanie do innego zespołu**, a następnie wyszukaj i dołączenia do grupy **azure zawartości odczytu** .

## <a name="install-git-for-windows"></a>Instalowanie cyfra dla systemu Windows

Zainstaluj cyfra dla systemu Windows z [http://git-scm.com/download/win](http://git-scm.com/download/win). Pobieranie instaluje systemu kontroli wersji cyfra i instaluje imprezie cyfra, wiersza polecenia aplikacji, którą będzie umożliwia interakcję z lokalnego repozytorium cyfra.

Możesz zaakceptować domyślne ustawienia; Jeśli chcesz, aby polecenia były dostępne w wierszu polecenia systemu Windows, wybierz odpowiednią opcję włączy tę opcję.

<p align="center">
 ![Przykład profilu GitHub](./media/tools-and-setup/gitbashinstall.png)

(Uwaga: nie jest taki sam, jak "Github dla systemu Windows". "Github dla systemu Windows" jest różne narzędzia graficznego interfejsu użytkownika, które będą również działać, jeśli chcesz poczytaj na siebie. [https://windows.github.com/](https://windows.github.com/))

## <a name="enable-two-factor-authentication"></a>Włącz uwierzytelnianie dwuskładnikowe

Musisz włączyć uwierzytelnianie dwa czynniki (2FA) na Twoim koncie GitHub Jeśli pracujesz w prywatnej repozytorium zawartości. Wymagane jest w repozytorium prywatne.

Aby włączyć tę opcję, postępuj zgodnie z instrukcjami oba następujące GitHub tematy pomocy:

- [Około uwierzytelnianie dwuskładnikowe](https://help.github.com/articles/about-two-factor-authentication/)
- [Tworzenie token dostępu do użycia wiersza polecenia](https://help.github.com/articles/creating-an-access-token-for-command-line-use/)

Po utworzeniu tokenu, zaznacz wszystkie zakresy dostępne w Interfejsie użytkownika tworzenia ([szczegółowych informacji na temat każdego zakresu](https://developer.github.com/v3/oauth/#scopes))

Po włączeniu 2FA, należy wprowadzić token dostępu zamiast hasła GitHub w wierszu polecenia podczas próby dostępu do repozytorium GitHub z poziomu wiersza polecenia. Token dostępu nie jest kod uwierzytelniania, który zostanie wyświetlony w wiadomości SMS, podczas konfigurowania 2FA. Jest długi ciąg, który wygląda mniej więcej tak: fdd3b7d3d4f0d2bb2cd3d58dba54bd6bafcd8dee. Kilka uwagi na ten temat:

- Po utworzeniu token dostępu, można go zapisać w pliku tekstowym, aby był łatwo dostępne w razie konieczności.

- Później gdy trzeba Wklej tokenu ustalić, czy istnieją dwa sposoby, aby wkleić w wierszu polecenia:

 - Kliknij ikonę w lewym górnym rogu okna wiersza polecenia > Edytuj > Wklej.
 - Kliknij prawym przyciskiem myszy ikonę w lewym górnym rogu okna, a następnie kliknij polecenie Właściwości > Opcje > Tryb szybkiej edycji. Konfiguruje wiersza polecenia, którą można wkleić, klikając prawym przyciskiem myszy w oknie wiersza polecenia.

## <a name="install-a-markdown-editor"></a>Instalowanie edytora promocji cenowych

Firma Microsoft autor zawartości przy użyciu notacji prosty "promocji cenowych" w plikach raczej niż zespolonej "markup" (HTML, XML itp.). Tak musisz zainstalować Edytor promocji cenowych.

- **Atom**: większość użytkowników za pomocą edytora promocji cenowych Atom w GitHub: [http://atom.io](http://atom.io). Nie wymaga licencję do użytku służbowego. Ma sprawdzanie pisowni.

- **Notatnik**: za pomocą programu Notatnik dla opcji bardzo proste.

- **Prose**: jest Edytor promocji cenowych lightweight, eleganckie online i Otwórz źródło, który oferuje Podgląd. Odwiedź stronę [http://prose.io](http://prose.io) i Autoryzuj Prose w repozytorium.

- **[Kod visual Studio](https://www.visualstudio.com/products/code-vs.aspx)** — wpis firmy Microsoft, w tym miejscu.

## <a name="configure-atom"></a>Konfigurowanie Atom

Jeśli używasz Atom, musisz skonfigurować kilka rzeczy.

- Atom domyślnie przy użyciu 2 spacji, tabulatorów, ale promocji cenowych oczekuje 4 spacje. Jeśli pozostanie w domyślnej dwóch artykułu w podglądzie lokalne atrakcyjnego wyglądu, ale nie, gdy jest importowana do Azure. Skonfigurować w taki sposób, Atom używanie spacji 4 — to ustawienie można znaleźć w obszarze Plik > Ustawienia > Ustawienia edytora > karta długości.
- Prawdopodobnie będziesz mieć możliwość włączyć zawijanie wygładzone w tej sekcji, której ma taki sam, jak "zawijanie" w Notatniku.
- Aby włączyć Podgląd promocji cenowych, kliknij pozycję pakiety > Podgląd promocji cenowych > Podgląd przełącznika. Za pomocą klawiszy Ctrl-Shift-M Aby przełączyć podgląd widoku HTML.

## <a name="fork-the-repository-and-copy-it-to-your-computer"></a>Rozwidlenia repozytorium i skopiuj go na komputerze

1. Tworzenie rozwidlenia repozytorium w GitHub — przejdź do góry i do prawej strony, a następnie kliknij przycisk rozwidlenie. Jeśli zostanie wyświetlony monit, wybierz swoje konto jako miejsce, w którym ma zostać utworzona rozwidlenie lokalizacji. Spowoduje to utworzenie kopii repozytorium w ramach konta Centrum cyfra. Ogólnie mówiąc autorzy technicznych i menedżerowie program konieczne rozwidlenie azure zawartości — pr repo prywatne. Społeczności konieczne rozwidlenie azure zawartości, repo publicznej. Musisz rozwidlenia jeden raz; Po pierwszym konfiguracji Jeśli chcesz skopiować do rozwidlenie na inny komputer, wystarczy, że uruchamiania poleceń, które należy wykonać w tej sekcji, aby skopiować repo na komputerze.  Jeśli chcesz utworzyć rozwidlenia zarówno repozytoria, należy utworzyć rozwidlenia dla każdego repozytorium.

2. Kopiowanie osobistych programu Access Token uzyskanego od [https://github.com/settings/tokens](https://github.com/settings/tokens). Możesz zaakceptować domyślne uprawnienia dla tokenu.  Zapisz osobistych programu Access Token w pliku tekstowego do późniejszego użycia.

3. Następnie skopiuj repozytorium do komputera przy użyciu poświadczeń osadzony w ciągu polecenia.  W tym celu należy otworzyć imprezie cyfra, a następnie uruchom go jako administrator. W wierszu polecenia wpisz następujące polecenie.  To polecenie tworzy katalog azure-content(-pr) na Twoim komputerze.  Jeśli używasz domyślnej lokalizacji będzie c:\users<your Windows user name>\azure-content(-pr).

Publiczne repo:

        git clone https://[your GitHub user name]:[token]@github.com/<your GitHub user name>/azure-content.git

Prywatne repo:

        git clone https://[your GitHub user name]:[token]@github.com/<your GitHub user name>/azure-content-pr.git

Na przykład tego polecenia klonowanie może wyglądać następująco:

        git clone https://smithj:b428654321d613773d423ef2f173ddf4a312345@github.com/smithj/azure-content-pr.git  

## <a name="set-remote-repository-connection-and-configure-credentials"></a>Połączenia zdalnego repozytorium i Konfigurowanie poświadczeń

Tworzenie odwołania do repozytorium głównego przez wprowadzenie tych poleceń. Aby uzyskać najnowsze zmiany na komputerze lokalnym i ponownie przekazać zmiany do GitHub spowoduje to ustawienie połączenia z repozytorium w GitHub. To polecenie konfiguruje również token lokalnie, aby nie trzeba wprowadź swoją nazwę i hasło, za każdym razem próby uzyskania dostępu nadrzędny repo i usługi rozwidlenia na GitHub.

Publiczne repo:

        cd azure-content
        git remote add upstream https://[your GitHub user name]:[token]@github.com/Azure/azure-content.git
        git fetch upstream

Prywatne repo:

        cd azure-content-pr
        git remote add upstream https://[your GitHub user name]:[token]@github.com/Azure/azure-content-pr.git
        git fetch upstream

Trwa to zazwyczaj trochę czasu. Po wykonaniu tej czynności nie musisz ponownie rozwidlenia lub wprowadź ponownie poświadczenia. Czy wystarczy ponownie skopiować rozwidlenia komputerze lokalnym, jeśli skonfigurowano narzędzia na innym komputerze.


## <a name="configure-your-user-name-and-email-locally"></a>Skonfiguruj swoją nazwę użytkownika i lokalnie w wiadomości e-mail

Aby upewnić się, że są wyświetlane w poprawnie jako współautora, musisz skonfigurować swoją nazwę użytkownika i poczty e-mail lokalnie w Cyfra.

1. Rozpocznij imprezie cyfra i przełącz się do zawartości azure lub azure zawartości — pr:

   ````
   cd azure-content
   ````

 lub

   ````
   cd azure-content-pr
   ````

2. Skonfiguruj swoją nazwę użytkownika, dopasowując je do nazwy po skonfigurowaniu go w swoim profilu GitHub:

    ````
    git config --global user.name "John Doe"
    ````
3. Konfigurowanie poczty e-mail, dopasowując je główny adres e-mail, wyznaczona w swoim profilu GitHub; Jeśli jesteś pracownika MSFT, należy swój adres e-mail MSFT:

    ````
    git config --global user.email "alias@example.com"
    ````
4. Typ `git config -l` i przejrzyj ustawienia lokalne, aby upewnić się, nazwę użytkownika i poczty e-mail w konfiguracji są poprawne.

##<a name="next-steps"></a>Następne kroki

- Opis typu zawartości, co należy do technicznych repo zawartości i wiesz, co nie należy. Zapoznaj się z [orientacji zawartości kanału](./content-channel-guidance.md)!
- Wykonaj [poniższe czynności, aby utworzyć lub zmodyfikować artykułu i prześlij go do opublikowania](./git-commands-for-master.md).
- Skopiuj [Szablon promocji cenowych](../markdown templates/markdown-template-for-new-articles.md) jako podstawy dla nowego artykułu.
- Za pomocą [następującej listy kontrolnej, aby zweryfikować żądania pobieraj będą zgodne z kryteriami jakości](./contributor-guide-pr-criteria.md) scalania.


###<a name="contributors-guide-navigation"></a>Współautorzy przewodnik nawigacji

- [Artykuł Omówienie](./../README.md)
- [Indeks artykułów ze wskazówkami dotyczącymi](./contributor-guide-index.md)



<!--Anchors-->
[Use a customer-friendly voice]: #use-a-customer-friendly-voice
[Consider localization and machine translation]: #consider-localization-and-machine-translation
[other style and voice issues to watch for]: #other-style-and-voice-issues-to-watch-for


[Create a GitHub account and set up your profile]: #create-a-github-account-and-set-up-your-profile
[Determine whether you really need to follow the rest of these steps]: #determine-whether-you-really-need-to-follow-the-rest-of-these-steps
[Permissions in GitHub]: #permissions-in-github
[Install Git for Windows]: #install-git-for-windows
[Enable two-factor authentication]: #enable-two-factor-authentication
[Install a markdown editor]: #install-a-markdown-editor
[Fork the repository and copy it to your computer]: #fork-the-repository-and-copy-it-to-your-computer
[Install git-credential-winstore]: #install-git-credential-winstore
[Sign up for Disqus]: #sign-up-for-disqus
[Configure your user name and email locally]: #configure-your-user-name-and-email-locally
[Next steps]: #next-steps
