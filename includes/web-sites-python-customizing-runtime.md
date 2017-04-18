Azure będzie określanie wersji programu Python używać dla jego środowiska wirtualne z następującym priorytetem:

1. Wersja określona w runtime.txt w folderze głównym
1. Wersja określona przez ustawienie Python w konfiguracji aplikacji sieci web ( **Ustawienia** > karta**Ustawienia aplikacji** dla aplikacji sieci web w Azure Portal)
1. Jeśli żadna z powyższych podano, Python 2.7 to ustawienie domyślne

Prawidłowe wartości dla zawartości 

    \runtime.txt

są:

- Python 2.7
- Python 3.4

Jeśli wersja micro określono (trzecia cyfra), jest ignorowana.
