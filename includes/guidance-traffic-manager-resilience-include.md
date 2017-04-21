##<a name="highly-available-solutions-with-azure-traffic-manager"></a>Wysoce dostępnych rozwiązań za pomocą Menedżera ruch Azure

Należy określić, czy może być spełnione wymagania wysokiej dostępności z pracą przy użyciu Menedżera Azure ruch tylko jeśli trzeba połączyć Menedżer ruchu z innych rozwiązań DNS lub procesów. W zależności od potrzeb można używać:

- **Menedżer ruchu samodzielnie**. W przypadku wystarczających do obciążenia pracą 99,99% czas pracy służy Menedżer ruchu samodzielnie. W przypadku awarii w usłudze Menedżer ruchu użytkownicy nie będą mieli dostęp do Twojej obciążenie pracą do momentu ponownego ustanowienia usługę Menedżer ruchu.

- **Innym rozwiązaniem Menedżer ruch wraz z Menedżer ruchu Azure za pomocą**. W przypadku awarii w usłudze Menedżer ruchu możesz zmienić rekordu CNAME wskaż innych ruch usługę menedżera. Dostęp do Twojej obciążenie pracą jest nadal dostępne i rozłożone do wszystkich lokalizacji hostingu z pracą. To jest najbardziej drogich rozwiązaniem, ale może być wymagane na potrzeby obciążenia wymagające wyższą Umowa dotycząca poziomu usług.
