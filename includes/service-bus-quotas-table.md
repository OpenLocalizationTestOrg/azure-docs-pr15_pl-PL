W poniższej tabeli przedstawiono informacje o przydziałach specyficzne dla usługi Bus wiadomości. Limity koncentratory wydarzenia są uwzględniane w tej tabeli, ale bardziej szczegółowe informacje dotyczące koncentratorów zdarzenia, zobacz [Ceny koncentratory zdarzenia](https://azure.microsoft.com/pricing/details/event-hubs/). Aby uzyskać informacje o cenach i innych przydziałów dla Bus usługi Zobacz Omówienie [Ceny Bus serwisu](https://azure.microsoft.com/pricing/details/service-bus/) .

|Nazwy zasobów|Zakres|Typ|Działanie w przypadku przekroczenia|Wartość|
|---|---|---|---|---|
| Maksymalna liczba nazw na subskrypcję Azure|Namespace|Statyczne|Kolejne żądania o dodatkowe przestrzenie nazw będą odrzucane przez portal.|100|
|Rozmiar kolejki tematu|Jednostki|Zdefiniowane po utworzeniu kolejki-tematu.|Wiadomości przychodzące będą odrzucane i wyjątków będzie odbierana przez kod wywołujący.|1, 2, 3, 4 i 5 GB.<br /><br />Po włączeniu [podziału](service-bus-partitioning.md) rozmiar maksymalny kolejki tematu jest 80 GB.|
|Liczba połączeń równoczesne na przestrzeń nazw|Namespace|Statyczne|Kolejne żądania związane z dodatkowych połączeń będą odrzucane i wyjątków będzie odbierana przez kod wywołujący. Operacje pozostałych nie uwzględniane równoczesne połączenia TCP.|NetMessaging: 1000<br /><br />AMQP: 5000|
|Liczba połączeń równoczesne w jednostce kolejki tematu i subskrypcji|Jednostki|Statyczne|Kolejne żądania związane z dodatkowych połączeń będą odrzucane i wyjątków będzie odbierana przez kod wywołujący. Operacje pozostałych nie uwzględniane równoczesne połączenia TCP.|Ograniczona przez limit połączenia równoczesne według nazw.|
|Liczba równoczesne odbiera żądania w jednostce kolejce tematu i subskrypcji|Jednostki|Statyczne|Odbieranie kolejne żądania będą odrzucane i wyjątków będzie odbierana przez kod wywołujący. Przydział dotyczy połączonych liczba równoczesne odbioru operacje w subskrypcjach wszystko na temat.|5000|
|Liczba tematów i kolejek na obszar nazw usługi|Systemowe|Statyczne|Kolejne żądania do utworzenia nowego tematu lub kolejki na obszar nazw usługi będą odrzucane. W wyniku Jeśli skonfigurowany przez [Azure portal][], zostanie wygenerowany komunikat o błędzie. Jeśli w kierownictwa interfejsu API, wyjątek będzie odebrana przez kod wywołujący.|10 000<br /><br />Całkowita liczba tematy oraz kolejek w obszarze nazw usługi musi być mniejsza niż lub równa 10 000.<br/>To nie jest stosowane do Premium oddzielone od siebie wszystkich obiektów.|
|Liczba podzielone na partycje tematy i kolejek na obszar nazw usługi|Systemowe|Statyczne|Kolejne żądania do utworzenia nowego tematu podzielone na partycje lub kolejki na obszar nazw usługi będą odrzucane. W wyniku Jeśli skonfigurowany przez [Azure portal][], zostanie wygenerowany komunikat o błędzie. Jeśli w kierownictwa interfejsu API, wyjątku **QuotaExceededException** będzie odebrana przez kod wywołujący.|Podstawowe i standardowy poziomów - 100<br />Premium - 1000<br/><br />Każdy kolejki podzielone na partycje lub temat zlicza kierunku przydział 10 000 jednostek na przestrzeń nazw.|
|Maksymalny rozmiar każdej wiadomości ścieżki jednostki: kolejki lub tematu|Jednostki|Statyczne|-|260 znaków|
|Maksymalny rozmiar wiadomości nazwę dowolnej jednostki: nazw, subskrypcji, reguła subskrypcji lub Centrum zdarzeń|Jednostki|Statyczne|-|50 znaków|
|Maksymalny rozmiar zdarzeń koncentratory wydarzenia|Systemowe|Statyczne|-|256 KB|
|Rozmiar wiadomości dla obiektu kolejki tematu i subskrypcji|Systemowe|Statyczne|Wiadomości przychodzących, które wykraczają poza tych przydziałów będą odrzucane i wyjątków będzie odbierana przez kod wywołujący.|Maksymalny rozmiar wiadomości: 256KB ([standardowa warstwa](../articles/service-bus/service-bus-premium-messaging.md))-1 MB ([Warstwa Premium](../articles/service-bus/service-bus-premium-messaging.md)). <br /><br />**Uwaga** Ze względu na zapas systemu ten limit jest zazwyczaj nieco.<br /><br />Nagłówek maksymalny rozmiar: 64KB<br /><br />Maksymalna liczba właściwości nagłówka w zbiór właściwości: **bajt/wewnętrznej Wartość MaxValue**<br /><br />Maksymalny rozmiar właściwości zbioru właściwości: bez limitu jawne. Ograniczone przez nagłówek maksymalny rozmiar.|
|Rozmiar wiadomości właściwości dla obiektu kolejki tematu i subskrypcji|Systemowe|Statyczne|Powoduje wygenerowanie wyjątku **SerializationException** .|Rozmiar właściwości maksymalna liczba wiadomości dla każdej właściwości jest 32K. Całkowity rozmiar wszystkich właściwości nie może przekraczać 64 KB. Dotyczy to cały nagłówek [BrokeredMessage](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.brokeredmessage.aspx), który ma zarówno właściwości użytkownika, a także właściwości systemu (na przykład [SequenceNumber](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.brokeredmessage.sequencenumber.aspx), [etykiety](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.brokeredmessage.label.aspx), [komunikatu](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.brokeredmessage.messageid.aspx)i tak dalej).|
|Liczba subskrypcji na temat|Systemowe|Statyczne|Kolejne żądania związane z tworzeniem dodatkowe subskrypcje tematu zostanie odrzucone. W wyniku Jeśli skonfigurowane za pośrednictwem portalu, będą wyświetlane komunikat o błędzie. Jeśli z interfejsu API zarządzania wyjątków będzie odebrana przez kod wywołujący.|2000|
|Liczbę filtrów SQL na temat|Systemowe|Statyczne|Kolejne żądania tworzenia dodatkowe filtry na temat będą odrzucane i wyjątków będzie odbierana przez kod wywołujący.|2000|
|Liczba korelacji filtrów w jednym tematu|Systemowe|Statyczne|Kolejne żądania tworzenia dodatkowe filtry na temat będą odrzucane i wyjątków będzie odbierana przez kod wywołujący.|100 000|
|Rozmiar SQL filtrów i akcje|Systemowe|Statyczne|Kolejne żądania tworzenia dodatkowe filtry zostaną odrzucone i wyjątków będzie odbierana przez kod wywołujący.|Maksymalna długość ciągu warunek filtru: 1024 (1K).<br /><br />Maksymalna długość ciągu działania reguły: 1024 (1K).<br /><br />Maksymalna liczba wyrażeń na Akcja reguły: 32.|
|Liczba reguł [SharedAccessAuthorizationRule](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sharedaccessauthorizationrule.aspx) na przestrzeń nazw, kolejki lub tematu|Jednostką, przestrzeń nazw|Statyczne|Kolejne żądania tworzenia dodatkowych reguł będą odrzucane i wyjątków będzie odbierana przez kod wywołujący.|Maksymalna liczba reguł: 12. <br /><br /> Reguły, które są skonfigurowane w nazw Bus usługi dotyczą wszystkie kolejki i tematy w tym nazw.

[Azure portal]: https://portal.azure.com