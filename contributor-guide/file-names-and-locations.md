<properties title="" pageTitle="Nazwy plików i lokalizacje Azure artykuły techniczne" description="W tym artykule wyjaśniono struktury plików artykuły i konwencje nazewnictwa, które należy wykonać podczas tworzenia nowego artykułu." metaKeywords="" services="" solutions="" documentationCenter="" authors="tysonn" videoId="" scriptId="" manager="required" />

<tags ms.service="contributor-guide" ms.devlang="" ms.topic="article" ms.tgt_pltfrm="" ms.workload="" ms.date="03/14/2016" ms.author="tysonn" />

#<a name="file-names-and-locations-for-azure-technical-articles"></a>Nazwy plików i lokalizacje Azure artykuły techniczne

W naszym techniczne repozytorium zawartości firma Microsoft korzysta z jednego folderu ( **artykułów** ) dla wszystkich artykułów. Istnieje nie hierarchii folderów — wszystkie artykuły są przechowywane w strukturze pliku prostego. Jeśli tworzysz folderów z artykułami w nich, nie można opublikować artykuły.

Zamiast używać struktura pliku jako organizowanie zasady, firma Microsoft korzysta z konwencji nazewnictwa plików ściśle identyfikujący wyraźnie tematy i które w sumie kierunku wyszukiwanie w sieci web.

Oto, co należy wiedzieć:

+ [Reguły]
+ [Wzór]
+ [Przykłady standardowy]
+ [Zawartość witryny Marketplace]
+ [Zatwierdzanie nazwy pliku]

##<a name="rules"></a>Reguły

- Nazwy plików mogą zawierać tylko małe litery, numery i łączniki. 
- Bez spacji i znaków interpunkcyjnych. Za pomocą łączników do oddzielania wyrazów i liczb w polu Nazwa pliku.
- Nie więcej niż 80 znaków — jest to ograniczenie systemu publikowania
- Użyj zlecenia akcji, charakterystyczne, takich jak opracowywanie, kupić, tworzenie, rozwiązywanie problemów z. Nie — toku wyrazy.
- Nie zawiera żadnych wyrazów małych - i,, w albo itp.
- Wszystkie pliki musi znajdować się w promocji cenowych i używanie .md rozszerzenie pliku.
- Pisownię wyrazów. unikanie niezatwierdzone lub niepotrzebnych skrótów w nazwach plików

Skrótów i initialisms w nazwach plików - wytycznymi:

- Nie używaj skrótów nazw usługi Azure - pierwszych wyrazów nazwy pliku powinna być standardowe, etykietkami Azure nazwę usług lub technologii. 
-   Nie zezwalaj na Menedżera zasobów lub arm jako skrótów w dowolne miejsce w polu Nazwa pliku
- Inne skróty standardowych mogą być stosowane w razie potrzeby w nazwach plików. 

##<a name="pattern"></a>Wzór

Poniżej przedstawiono ogólne deseniu:

 **Service-platform-Language-Content-Product-Version.MD**

Za pomocą części deseniu, które Zastosuj i przejrzyj listę artykułów w repozytorium, aby uzyskać ogólny obraz tego, nazwy istniejących. Nazwy, które nie zaczynają się od platformy deweloperów lub nazwę usługi to prawdopodobnie podejrzanych i poślizgiem przez.

##<a name="standard-examples"></a>Przykłady standardowy

Oto kilka przykładów prawidłowych nazw, które postępuj zgodnie ze wzorcem. :

- cloud usług — dotnet ciągły delivery.md
- Telefon komórkowy usług — ios — get-started.md
- documentdb — Zarządzanie — account.md
- Mobile-Services-DotNet-backend-Get-Started-Settings-Sync.MD
- Active-Directory-Java-AUTHENTICATE-Users-Access-Control-Eclipse.MD
- Virtual-Machines-Install-Windows-Server-2008R2.MD
- pamięć podręczna aspnet sesji stan dostawcy
- Azure-SDK-DotNet-Release-Notes-2-8
- storsimple-Disaster-Recovery-Using-Azure-Site-Recovery

##<a name="marketplace-content"></a>Zawartość witryny Marketplace

Aby odróżnić zawartości, która omówiono partnera udział Azure marketplace, uruchom nazw plików z "marketplace". Tej zawartości nie może być zbyt typowych podczas tworzenia większość zawartości partnera na własny partnerów witryn sieci web.

- Marketplace-mongodb-Virtual-Machines-Install-Windows-Server-2008R2.MD

##<a name="file-name-approval"></a>Zatwierdzanie nazwy pliku

To zadanie naszych grupie recenzentów żądanie pobieraj przejrzeć nazwy plików, gdy nowy plik jest przesyłany do repozytorium po raz pierwszy. Pobieraj żądanie recenzenci należy Sprawdź nazwę pliku i przekazywanie opinii za pośrednictwem strumienia pobieraj żądania komentarza, jeśli potrzebne są zmiany. Nazwa pliku musi zostać poprawione przed zaakceptowaniu żądania pobieraj. Współautorzy mogą łatwo push aktualizacji na żądanie pobieraj oczekiwanie.

##<a name="folder-names-in-the-repo"></a>Nazwy folderów w repo

Foldery powinny być tworzone tylko dla usług i nazwę pliku powinny być takie same usługi informacji o pracy. Tylko litery i łączniki, a wszystkie zapisywane małymi literami. Uzyskaj akceptację od administratora repozytorium przed utworzeniem nowy folder, który nie jest wydane usługi.

##<a name="changing-case-in-file-names"></a>Zmiana wielkości liter w nazwach plików

Systemy operacyjne Windows są liter, więc jeśli chcesz zmienić nazwę pliku, aby rozwiązać problem obudowy, warto zmiany istotnych, jeśli nie możesz wprowadzić zmiany na Linux lub Mac. Na przykład:

  biztalk-administration-and-Development-Task-List-in-BizTalk-Services--> biztalk-services-administration-and-development-task-list

Aby zmienić nazwę pliku, użyj następującego polecenia:
```
  git mv <articles/service-folder/current-file-name.md> <articles/service-folder/new-file-name>
```

###<a name="contributors-guide-links"></a>Współautorzy przewodnik łącza

- [Artykuł Omówienie](./../README.md)
- [Indeks artykułów ze wskazówkami dotyczącymi](./contributor-guide-index.md)


<!--Anchors-->
[Reguły]: #rules
[Wzór]: #pattern
[Przykłady standardowy]: #standard-examples
[Zawartość witryny Marketplace]: #marketplace-content
[Zatwierdzanie nazwy pliku]: #file-name-approval
