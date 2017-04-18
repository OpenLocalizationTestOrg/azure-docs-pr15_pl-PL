<properties
    pageTitle="Koncentratory Azure powiadomień"
    description="Dowiedz się, jak używać powiadomień wypychanych w Azure. Przykłady kodu napisanego w C# za pomocą interfejsu API .NET."
    authors="ysxu"
    manager="erikre"
    editor=""
    services="notification-hubs"
    documentationCenter=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="multiple"
    ms.devlang="multiple"
    ms.topic="hero-article"
    ms.date="08/25/2016"
    ms.author="yuaxu"/>


#<a name="azure-notification-hubs"></a>Koncentratory Azure powiadomień

##<a name="overview"></a>Omówienie

Azure koncentratory powiadomienie o podanie infrastrukturę wypychanych prostych w użyciu, wiele platform, skalowanej, która umożliwia wysyłanie powiadomienia push urządzeń przenośnych z dowolnego wewnętrznej bazy danych (w chmurze lub lokalnego) do dowolnej platformy urządzeń przenośnych.

Z biegami powiadomienie można łatwo wysłać powiadomienia wypychane i platform, spersonalizowany dostępnego szczegóły systemów powiadomienie o różnych platform (obwodowym układzie Nerwowym). Przy użyciu jednego interfejsu API rozmowy można kierować poszczególnym użytkownikom lub segmentów wszystkich odbiorców zawierające miliony użytkowników na wszystkich swoich urządzeniach.

Za pomocą powiadomień koncentratory zarówno przedsiębiorstwa, jak i dla klientów indywidualnych scenariuszach. Na przykład:

- Wyślij powiadomienia wiadomości podziału miliony z krótki czas oczekiwania (koncentratory powiadomienie uprawnień aplikacji Bing preinstalowane na wszystkich urządzeniach systemu Windows i Windows Phone).
- Wyślij opartych na lokalizacji kupony odcinków użytkownika.
- Wysyłanie powiadomienia o zdarzeniach użytkownikom lub grupom sports/Finanse i gry aplikacji.
- Informować użytkowników o zdarzeń przedsiębiorstwa, takich jak nowe wiadomości i wiadomości e-mail i informacji o potencjalnych klientach.
- Wyślij jeden-jednorazowej-opcję wymagania haseł uwierzytelniania wieloskładnikowego.



##<a name="what-are-push-notifications"></a>Co to są powiadomienia Push?

Smartfony i tablety "powiadamiać" użytkowników po wystąpieniu zdarzenia. Te powiadomienia może mieć wiele form.

W aplikacji ze Sklepu Windows i Windows Phone, można powiadomienia w postaci _wyskakującego_: zostanie wyświetlone okno niemodalny dźwiękowe sygnału nowe powiadomienie. Inne typy powiadomień, które są obsługiwane zawierać powiadomienia _kafelka_, _nieprzetworzonych_i _znaczek_ . Aby uzyskać więcej informacji na temat typów powiadomień obsługiwane na urządzenia z systemem Windows zobacz [Kafelki, znaczki i powiadomień](http://msdn.microsoft.com/library/windows/apps/hh779725.aspx).

W przypadku urządzeń firmy Apple iOS naciśnięcie podobnie powiadamia użytkownika przy użyciu okna dialogowego zgłaszającego użytkownikowi wyświetlanie i zamykanie powiadomienie. Klikając pozycję **Widok** zostanie otwarta aplikacja odbiera wiadomość. Aby uzyskać więcej informacji na iOS powiadomień zobacz [iOS powiadomienia](http://go.microsoft.com/fwlink/?LinkId=615245).

Powiadomienia wypychane pomoc wyświetlania informacji świeży pozostając sprawności energetycznej urządzeń przenośnych. Powiadomienia mogą być wysyłane przez systemy wewnętrznej bazy danych na urządzenia przenośne, nawet wtedy, gdy nie działają odpowiednich aplikacji na urządzeniu. Powiadomienia wypychane są kluczową składnik dla aplikacji dla klientów indywidualnych, miejsce, w którym są używane w celu zwiększenia zaangażowania aplikacji i zastosowania. Powiadomienia są również przydatne dla przedsiębiorstw, gdy aktualne informacje zwiększa czas reakcji pracownika do zdarzeń biznesowych.

Przykłady określonych scenariuszach zaangażowania urządzeń przenośnych są:

1.  Aktualizowanie kafelka w systemie Windows 8 lub Windows Phone przy użyciu bieżących informacji finansowych.
2.  Alerty użytkownika z wyskakującego, niektóre elementu pracy przypisany temu użytkownikowi w aplikacji dla przedsiębiorstw przepływu pracy.
3.  Wyświetlanie znaczek z numerem bieżącej sprzedaży prowadzi w aplikacji dla CRM (na przykład Microsoft Dynamics CRM).

##<a name="how-push-notifications-work"></a>Jak wypychanych powiadomień pracy

Powiadomienia wypychane są dostarczane za pośrednictwem infrastruktury specyficzne dla platformy o nazwie _Systemy powiadamiania platformy_ (obwodowym układzie Nerwowym). Obwodowym układzie Nerwowym udostępnia funkcje barebones (oznacza to, że nie jest obsługiwane w przypadku emisji, personalizacji) i że masz nie interfejs. Na przykład aby wysłać powiadomienie do aplikacji ze Sklepu Windows, deweloper należy skontaktować się WNS (Usługa powiadomień systemu Windows). Wysyłanie powiadomienia do urządzenia z systemem iOS, tym samym Deweloper ma kontaktu APN (Usługa powiadomień Push firmy Apple), a następnie wyślij wiadomość po raz drugi. Azure koncentratory powiadomienie o pomoc, dostarczając interfejs oraz inne funkcje do obsługi powiadomień wypychanych w każdej z tych platform.

Na wysokim poziomie jednak we wszystkich systemach powiadomienie platformy zgodne ze wzorcem same:

1.  Obwodowym układzie Nerwowym pobrać _obsługiwać_kontaktuje się z aplikacji klienckiej. Typ uchwyt zależy od systemu. W przypadku WNS, jest identyfikator URI i "kanał powiadomień." W przypadku APN jest tokenu.
2.  Aplikacja klienta przechowuje ten uchwyt w aplikacji _wewnętrznej_ do późniejszego wykorzystania. WNS wewnętrznej jest zazwyczaj usługi w chmurze. W przypadku firmy Apple system jest nazywany _dostawcy_.
3.  Aby wysłać powiadomienia wypychane, wewnętrznej aplikacji kontakty obwodowym układzie Nerwowym wystąpienie klienta określonej aplikacji docelowej za pomocą uchwytu.
4.  Obwodowym układzie Nerwowym przekazuje powiadomienia na urządzeniu określony przez uchwytu.

![][0]

##<a name="the-challenges-of-push-notifications"></a>Wyzwania powiadomienia wypychane

Systemy te są bardzo zaawansowanym, nadal opuszczają ilości pracy przez dewelopera aplikacji w celu zaimplementowania nawet typowe scenariusze powiadomienia wypychane, takich jak emisja lub wysyłania powiadomień wypychanych użytkownikom segmentowany.

Powiadomienia wypychane to jeden z najbardziej wymagane funkcje usług w chmurze dla aplikacji dla urządzeń przenośnych. Przyczyną jest infrastruktury wymagane do ich praca dość złożone i głównie wpływu na głównym warunków logicznych aplikacji. Wyzwania w budowanie infrastruktury wypychanych na żądanie, należą:

- **Zależność platformy.** Aby móc wysyłać powiadomienia do urządzeń na innych platformach, wiele interfejsów musi być kodowane w wewnętrznej. Nie tylko różnią się niższego poziomu szczegółów, ale prezentacji powiadomienia (kafelków, wyskakującego lub znaczek) jest również zależne od platformy. Różnice te mogą prowadzić do kodu wewnętrznej złożone i słabo Obsługa.

- **Skala.** Skalowanie infrastruktura ta obejmuje dwa aspekty:
    + Zgodnie z wytycznymi obwodowym układzie Nerwowym tokeny urządzenia można odświeżać każdorazowo po uruchomieniu aplikacji. Prowadzi to do dużej ilości ruchu (i uzyskuje dostęp do bazy danych wynikające z niego) tylko na bieżąco tokeny urządzenia. Gdy liczba urządzeń zawiera (prawdopodobnie miliony), koszt tworzenia i przechowywania infrastruktura ta jest nonnegligible.

    + Większość PNSs nie obsługują emisji na wielu urządzeniach. Wynika, że emisji miliony urządzeń powoduje miliony połączenia do PNSs. Możliwość skalowania te żądania nie jest proste, ponieważ zazwyczaj deweloperów aplikacji chcesz zachować całkowity czas oczekiwania w dół. Na przykład ostatnie urządzenie otrzymać wiadomość nie powinny być powiadomienie 30 minut po wysłaniu powiadomienia o jego, jak w przypadku większości przypadków go środki w celu ma powiadomienia wypychane.
- **Routing.** PNSs umożliwia wysyłanie wiadomości do urządzenia. Jednak w większości aplikacji Powiadomienia są przeznaczone dla użytkowników lub grup odsetek (na przykład pod kątem pracowników przypisane do konta klienta). Sposób w celu kierowania powiadomienia o jego poprawne urządzeń, wewnętrznej aplikacji muszą utrzymać rejestru, która przypisuje grupy odsetek tokeny urządzenia. Ten ogólnych dodaje do całkowity czas na rynku i konserwacja kosztów aplikacji.

##<a name="why-use-notification-hubs"></a>Dlaczego warto używać koncentratory powiadomienia?

Powiadomienie o koncentratory wyeliminowania złożoności: nie masz do zarządzania wyzwania powiadomienia wypychane. Zamiast tego możesz użyć koncentratora powiadomienie. Koncentratory powiadomienie korzystania z infrastruktury powiadomień wypychanych pełnego wiele platform, skalowanej i znacznie zmniejszyć kod specyficzne dla wypychanych uruchamiany w wewnętrznej bazy danych aplikacji. Koncentratory powiadomienie wykonania wszystkich funkcji infrastruktury push. Urządzenia tylko są odpowiedzialne za przeprowadzenie rejestracji obwodowym układzie Nerwowym uchwyty i wewnętrznej bazy danych jest odpowiedzialny za wysłanie wiadomości niezależny od platformy użytkownikom lub grupom odsetek, jak pokazano na poniższym rysunku:

![][1]


Powiadomienie o koncentratory zawierają infrastrukturze powiadomień wypychanych gotowych do użycia z następujące zalety:

- **Wielu platformach.**
    +  Obsługa wszystkich głównych platformy urządzeń przenośnych. Powiadomienie o koncentratory można wysłać powiadomień wypychanych do aplikacji ze Sklepu Windows, iOS, Android i Windows Phone.

    +  Powiadomienie o koncentratory zapewnia wspólny interfejs do wysyłania powiadomień do wszystkich obsługiwanych platformach. Protokoły specyficzne dla platformy nie są wymagane. Aplikacja wewnętrznej można wysłać powiadomień w formatach specyficzny dla platformy lub niezależne od platformy. Aplikacja komunikuje się tylko z koncentratorów powiadomienie.

    +  Zarządzanie urządzeniami uchwyt. Powiadomienie o koncentratory przechowuje rejestru uchwyt i opinii PNSs.

- **Współpraca z dowolnym wewnętrznej**: chmurki lub lokalny program .NET, PHP, Java, węzła itp.

- **Skala.** Powiadomienie o koncentratory Skala miliony urządzeń bez konieczności ponownego zaprojektować i shard.


- **Sformatowany zbiór wzorców dostarczania**:

    - *Emisja*: umożliwia u jednoczesnego emisji miliony urządzeń przy użyciu jednego interfejsu API rozmowy.

    - *Emisji pojedynczej i multiemisji*: przekazać do znaczników reprezentujących poszczególnych użytkowników, w tym wszystkich urządzeń; lub większą grupę; na przykład czynniki oddzielnym formularzu (tabletu a telefonu).

    - *Segmentacji*: Push do części złożone zdefiniowanych przez wyrażeń znacznik (na przykład urządzenia Nowy Jork po Yankees).

    Każde urządzenie, wysyłając uchwytu do koncentratora powiadomień można określić jeden lub więcej _znaczników_. Aby uzyskać więcej informacji na temat [znaczników]. Znaczniki nie ma wstępnie obsługi administracyjnej lub usunięte. Znaczniki zapewniają prosty sposób wysyłanie powiadomienia do użytkowników lub grup odsetek. Ponieważ znaczniki mogą zawierać dowolne identyfikator specyficzne dla aplikacji (na przykład identyfikatorów użytkownika lub grupy), ich zastosowania zwolnienia aplikacji wewnętrznej z muszą do przechowywania i zarządzania uchwyty urządzenia.

- **Personalizacji**: każde urządzenie można mieć jeden lub więcej szablonów, aby osiągnąć lokalizacji na urządzenie i personalizacja bez wpływu na kod wewnętrznej.

- **Zabezpieczenia**: udostępnionych hasło dostępu (SA) lub federacyjnych uwierzytelniania.

- **Sformatowany telemetrycznego**: dostępne w portalu i programowy.


##<a name="integration-with-app-service-mobile-apps"></a>Integracja z aplikacji usługi aplikacji dla urządzeń przenośnych

W celu ułatwienia bezproblemowe i ujednolicających obsługi wielu usług Azure, [Mobile usługi aplikacji] ma Obsługa powiadomień wypychanych za pomocą koncentratorów powiadomienie. [Telefon komórkowy usługi aplikacji] oferuje platformy opracowywania aplikacji mobilnej wysoce skalowalna, ogólnie dostępna dla deweloperów przedsiębiorstwa i integratorów, aby przesunąć bogatego zestawu funkcji deweloperów urządzeń przenośnych.

Deweloperów aplikacji Mobile mogą korzystać koncentratory powiadomienie z następujących przepływu pracy:

1. Pobieranie uchwyt obwodowym układzie Nerwowym urządzenia
2. Zarejestrować urządzenie i [Szablony] w koncentratory powiadomienie za pośrednictwem wygodny interfejsu API rejestru SDK klienta aplikacji Mobile
    + Należy zauważyć, że aplikacje Mobile paski z dala od komputera wszystkie znaczniki rejestracji ze względów bezpieczeństwa. Praca z koncentratorów powiadomienie z sieci wewnętrznej bazy danych bezpośrednio do znaczniki można skojarzyć z urządzenia.
3. Wysyłanie powiadomienia z sieci wewnętrznej bazy danych aplikacji z koncentratorów powiadomień

Poniżej przedstawiono niektóre udogodnienia pozyskano deweloperów w tym integracji:

- **SDK klienta przenośnego aplikacji.** Te wielu platform SDK udostępnia proste interfejsów API rejestracji i zwróć się do Centrum powiadomień połączone za pomocą aplikacji dla urządzeń przenośnych automatycznie. Nie trzeba deweloperów Drąż za pośrednictwem koncentratory powiadomienie o poświadczenia i pracować z usługą dodatkowe.
    + SDK automatycznie oznakować danego urządzenia o uwierzytelniony identyfikator użytkownika aplikacji Mobile umożliwiające naciśnij, aby scenariusz użytkownika.
    + SDK automatycznie za pomocą Identyfikatora instalacji aplikacji Mobile jako identyfikator GUID zarejestrować za pomocą koncentratorów powiadomienie, zapisując deweloperów problemy utrzymania wielu GUID usługi.
    
- **Model instalacji.** Aplikacje Mobile współpracuje z koncentratorów powiadomienie najnowszą modelem wypychanych reprezentować wszystkie właściwości wypychanych skojarzony z urządzeniem w instalacji JSON wyrównane usługi powiadomień wypychanych i łatwy w użyciu.

- **Elastyczność.** Zawsze można deweloperów do pracy z koncentratorów powiadomienie bezpośrednio nawet w przypadku integracji w miejscu.

- **Zintegrowane środowisko w [Azure portal].** Powiadomienia push możliwości jest reprezentowane graficznie w aplikacji Mobile i deweloperów ułatwia pracę z Centrum skojarzone powiadomienie za pośrednictwem aplikacji Mobile.



##<a name="next-steps"></a>Następne kroki

Więcej informacji na temat koncentratory powiadomienie można znaleźć w następujących tematach:

+ **[Jak klienci używają koncentratory powiadomień]**

+ **[Powiadomienie o koncentratory samouczki i prowadnice]**

+ **Powiadomienie o wprowadzenie koncentratory samouczki** ([iOS], [Android], [uniwersalnego systemu Windows], [Windows Phone], [Kindle], [Xamarin.iOS], [Xamarin.Android])

Odpowiednie .NET zarządzanego interfejsu API odwołania dla powiadomienia wypychane znajdują się w folderze:

+ [Microsoft.WindowsAzure.Messaging.NotificationHub]
+ [Microsoft.ServiceBus.Notifications]


  [0]: ./media/notification-hubs-overview/registration-diagram.png
  [1]: ./media/notification-hubs-overview/notification-hub-diagram.png
  [Jak klienci używają koncentratory powiadomień]: http://azure.microsoft.com/services/notification-hubs
  [Powiadomienie o koncentratory samouczki i prowadnice]: http://azure.microsoft.com/documentation/services/notification-hubs
  [iOS]: http://azure.microsoft.com/documentation/articles/notification-hubs-ios-get-started
  [Android]: http://azure.microsoft.com/documentation/articles/notification-hubs-android-get-started
  [Uniwersalny systemu Windows]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-get-started
  [Windows Phone]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-phone-get-started
  [Kindle]: http://azure.microsoft.com/documentation/articles/notification-hubs-kindle-get-started
  [Xamarin.iOS]: http://azure.microsoft.com/documentation/articles/partner-xamarin-notification-hubs-ios-get-started
  [Xamarin.Android]: http://azure.microsoft.com/documentation/articles/partner-xamarin-notification-hubs-android-get-started
  [Microsoft.WindowsAzure.Messaging.NotificationHub]: http://msdn.microsoft.com/library/microsoft.windowsazure.messaging.notificationhub.aspx
  [Microsoft.ServiceBus.Notifications]: http://msdn.microsoft.com/library/microsoft.servicebus.notifications.aspx
  [Aplikacje dla urządzeń przenośnych aplikacji usługi]: https://azure.microsoft.com/en-us/documentation/articles/app-service-mobile-value-prop/
  [Szablony]: notification-hubs-templates-cross-platform-push-messages.md
  [Azure portal]: https://portal.azure.com
  [znaczniki]: (http://msdn.microsoft.com/library/azure/dn530749.aspx)
