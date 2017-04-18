**1. jak klienci będą otrzymywać powiadomienia Ustępujący zestawu SDK?**

Firma Microsoft udostępni 12 miesięcy powiadomienie w celu obsługi Ustępujący zestawu SDK w celu ułatwienia płynnego przejścia do obsługiwanych SDK. Ponadto klienci zostaną powiadomieni za pomocą różnych komunikacji kanałów — Portal Azure zarządzania, Centrum deweloperów, wpis w blogu bezpośredniej komunikacji do przydzielania i administratorów usługi.

**2. klientów aplikacje można pisać za pomocą "to-be" wycofanych SDK DocumentDB w okresie 12 miesięcy?** 

Tak, klienci będą mieć pełny dostęp do tworzenia, wdrażania i modyfikowania aplikacji przy użyciu "to-be" wycofanych SDK DocumentDB w okresie prolongaty 12 miesięcy. W okresie prolongaty 12 miesięcy klienci są zaleca się migrowanie do nowszego obsługiwanej wersji programu DocumentDB SDK zależnie od potrzeb.

**3. można klientów tworzyć i modyfikować aplikacji przy użyciu wycofanych SDK DocumentDB po 12 miesięcy powiadomień?**

Po okresie powiadomień 12 miesięcy będzie można usunąć zestawu SDK. Platformy DocumentDB będzie nie zezwalają na dostęp do innym DocumentDB przez aplikacje przy użyciu wycofanych zestawu SDK. Ponadto firma Microsoft nie udostępni obsługi klienta na wycofanych SDK.

**4. co się dzieje klienta uruchomione aplikacje korzystające z nieobsługiwanej wersji DocumentDB SDK?**

Wszelkich prób do łączenia się z usługą DocumentDB wersją wycofanych SDK będą odrzucane. 

**5. zostanie nowe funkcje zastosowany do wszystkich innych niż wycofany SDK**

Nowe funkcje tylko zostaną dodane do nowej wersji. Jeśli używasz wersji stare, nie wycofany zestawu SDK wezwaniach do DocumentDB będą nadal działać w poprzedniej, ale nie będą mieć dostępu do dowolnego nowe funkcje.  

**6. co zrobić, jeśli nie można zaktualizować Moja aplikacja przed daty**

Zaleca się, jak najwcześniej uaktualnienie do najnowszej SDK. Po SDK oznakowanego emerytury będą miały 12 miesięcy, aby zaktualizować aplikację. Jeśli bez względu na powód, nie możesz ukończyć aktualizację aplikacji w tym czasie następnie skontaktuj się [Zespołu DocumentDB](mailto:askdocdb@microsoft.com) i zażądać ich pomocy przed datą progowa.
