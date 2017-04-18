Skrypt wdrożenia pominie tworzenia wirtualnego środowiska Azure, jeśli wykryje zgodne środowisko wirtualne już istnieje.  To może przyspieszyć wdrożenie znacznie.  Pakiety, które są już zainstalowane zostaną pominięte, pip.

W niektórych sytuacjach warto wymusić usunięcie tego środowiska wirtualnego.  Można to zrobić, jeśli jednak chcesz uwzględnić środowisko wirtualne jako część repozytorium.  Można też to zrobić, jeśli musisz usunąć niektóre pakietów lub sprawdzić zmiany do requirements.txt.

Istnieje kilka opcji zarządzania istniejące środowisko wirtualne Azure:

### <a name="option-1-use-ftp"></a>Opcja 1: Użyj FTP

Klient FTP połączyć się z serwerem i będzie można usunąć folder koperta.  Należy zauważyć, że niektórzy klienci FTP (na przykład przeglądarki sieci web) może być tylko do odczytu i nie można usunąć folderów, więc warto upewnij się, że klient FTP za pomocą tej funkcji.  Nazwa hosta FTP i użytkownika są wyświetlane w aplikacji sieci web karta w [Azure Portal](https://portal.azure.com).

### <a name="option-2-toggle-runtime"></a>Opcja 2: Przełącznik runtime

Poniżej przedstawiono alternatywę wykorzystującego faktów skrypt wdrożenia spowoduje usunięcie folderu Koperta gdy nie pasuje odpowiedniej wersji Python.  To będzie skuteczne Usuń istniejące środowisko i Utwórz nową.

1. Przełącz się do innej wersji programu Python (za pośrednictwem runtime.txt lub karta **Ustawienia aplikacji** w Azure Portal)
1. cyfra push pewne zmiany (Ignoruj pip błędów instalacji, jeśli istnieją)
1. Powróć do pierwszej wersji Python
1. cyfra przekazać niektóre zmiany, ponownie

### <a name="option-3-customize-deployment-script"></a>Opcja 3: Dostosowywanie skrypt wdrożenia

Jeśli został dostosowany skrypt wdrożenia, możesz zmienić kod w deploy.cmd wymuszone Usuń folder koperta.
