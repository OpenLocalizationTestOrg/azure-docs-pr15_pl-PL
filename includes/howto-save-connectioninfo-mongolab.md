Gdy jest możliwe wklejanie MongoLab URI do kodu, zaleca się konfigurowanie go w środowisku w celu ułatwienia zarządzania. Dzięki temu zmiana identyfikator URI można aktualizować ją za pośrednictwem portalu Azure bez przechodzenia do kodu.


1. W Portal Azure wybierz **Aplikacje sieci Web**.
1. Kliknij nazwę aplikacji sieci web na liście aplikacji sieci Web.  
![WebAppEntry][entry-website]  
Wyświetla na pulpicie nawigacyjnym aplikacji sieci Web.

1. Na pasku menu, kliknij przycisk **Konfiguruj** .  
![WebAppDashboardConfig][focus-mongolab-websitedashboard-config]

1. Przewiń w dół do sekcji Parametry połączenia.  
![WebAppConnectionStrings][focus-mongolab-websiteconnectionstring]

1. W polu **Nazwa**wprowadź MONGOLAB_URI.
1. **Wartość**Wklej parametry połączenia, możemy uzyskane w poprzedniej sekcji.
1. **Niestandardowe** wybierz z listy rozwijanej **Typ** (zamiast **SQLAzure**ustawienie domyślne).
1. Kliknij przycisk **Zapisz** na pasku narzędzi.  
![SaveWebApp][button-website-save]

**Uwaga:** Dodaje Azure **CUSTOMCONNSTR\_ ** prefiks tej zmiennej, która jest dlaczego kod powyżej odwołania **CUSTOMCONNSTR\_MONGOLAB_URI.**

[entry-website]: ./media/howto-save-connectioninfo-mongolab/entry-website.png
[focus-mongolab-websitedashboard-config]: ./media/howto-save-connectioninfo-mongolab/focus-mongolab-websitedashboard-config.png
[focus-mongolab-websiteconnectionstring]: ./media/howto-save-connectioninfo-mongolab/focus-mongolab-websiteconnectionstring.png
[button-website-save]: ./media/howto-save-connectioninfo-mongolab/button-website-save.png
