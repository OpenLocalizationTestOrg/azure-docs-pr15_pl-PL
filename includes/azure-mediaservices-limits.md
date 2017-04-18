Zasób|Domyślny Limit|Limit maksymalny
---|---|---
Azure kont usługi multimediów (AMS) w jednym subskrypcji||25
Zasoby dla każdego konta AMS||1 000 000 zł<sup>1</sup>
Łańcuchowej zadania na zadanie||30
Zasoby na zadanie||50
Zasoby na zadanie||100
Zadania dla każdego konta AMS ||50 000<sup>2</sup>
Unikatowe Locator skojarzone z środka trwałego za jednym razem||5<sup>4</sup>
Live kanały na konto AMS </p></td>|5</p></td>|N/d<sup>1</sup>
Programy w stanie zatrzymania na kanał </p></td>|50</p></td>|N/d<sup>1</sup>
Programów podczas uruchamiania stan na kanał </p></td>|3</p></td>|3
Przesyłanie strumieniowe punkty końcowe w bieżącej Województwo według konta AMS</p></td>|2</p></td>|N/d<sup>1</sup>
Przesyłanie strumieniowe jednostki na streaming punktu końcowego </p></td>|10 </p></td>|N/d<sup>1</sup>
Jednostki kodowania na AMS konta </p></td>|25</p></td>|N/d<sup>1</sup>
Konta miejsca do magazynowania | |1000<sup>5</sup>
Zasady || 1 000 000<sup>6</sup>

<sup>1</sup> możesz zażądać zaktualizować limity dotyczące tego przydziału, otwierając bilet pomocy technicznej. Nie należy tworzyć więcej kont AMS, aby zwiększyć limity, zamiast tego przesyłanie bilet pomocy technicznej.

<sup>2</sup> numer zawiera kolejce, gotowych aktywna i anulowane zadania. Nie ma usuniętych zadań. Możesz usunąć stare zadań za pomocą **IJob.Delete** lub **Usuwanie** żądania HTTP.

<sup>3</sup> dokonując żądanie do listy zadania jednostek maksymalnie 1000 zostaną zwrócone na żądanie. Jeśli potrzebujesz umożliwia śledzenie wszystkich zadań przesłane, umożliwia góry i Pomiń zgodnie z opisem w [Opcje kwerendy systemu OData](http://msdn.microsoft.com/library/gg309461.aspx).

<sup>4</sup> Locator nie są przeznaczone do zarządzania kontrola dostępu użytkownika. Aby nadać różne uprawnienia dla poszczególnych użytkowników, należy używać rozwiązań zarządzania prawami cyfrowymi (DRM).

<sup>5</sup> konta miejsca do magazynowania muszą być z tej samej subskrypcji Azure.

<sup>6</sup> jest ograniczona do 1 000 000 zł zasad dla różnych zasad AMS (na przykład dla zasad Locator lub ContentKeyAuthorizationPolicy). Należy używać tej samej zasady, jeśli są zawsze przy użyciu tych samych dniach / uprawnienia dostępu / itd.
