<properties
   pageTitle="Klaster tkaninie usługi bezpiecznego | Microsoft Azure"
   description="W tym artykule opisano scenariusze zabezpieczeń klastrze tkaninie usługi i różnych technologii używanych do wykonania tych scenariuszy."
   services="service-fabric"
   documentationCenter=".net"
   authors="ChackDan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/19/2016"
   ms.author="chackdan"/>

# <a name="service-fabric-cluster-security-scenarios"></a>Scenariusze dotyczące zabezpieczeń klaster tkaninie usługi

Klaster tkaninie usługi jest zasób, którego jesteś właścicielem. Klastrów powinny być zawsze zabezpieczone aby zapobiec połączeni z klastrem, zwłaszcza w przypadku, gdy został uruchomiony obciążenia produkcji przez nieupoważnionych użytkowników. Chociaż można utworzyć niezabezpieczoną klaster, zrobić tak umożliwi każdy użytkownik anonimowy się z nim połączyć, jeśli go udostępnia zarządzania punktów końcowych do publicznego Internetu. 

Ten artykuł zawiera omówienie scenariuszy zabezpieczeń dla klastrów uruchomionych Azure lub autonomicznego i różnych technologii, używane do wykonania tych scenariuszy. Scenariusze zabezpieczeń klaster są:

- Węzeł węzeł zabezpieczeń
- Węzeł Klient zabezpieczeń
- Kontrola dostępu oparta na rolach (RBAC)

## <a name="node-to-node-security"></a>Węzeł węzeł zabezpieczeń
Zabezpiecza komunikacji między maszyny wirtualne lub komputerach w klastrze. Dzięki temu, że komputery, które mogą dołączyć do klaster może uczestniczyć w hostingu aplikacji i usług w klastrze.

![Diagram komunikacji węzeł węzeł][Node-to-Node]

Klastrów uruchomionych dla klastrów Azure lub Wersja autonomiczna z systemem Windows można użyć [Certyfikatu zabezpieczeń](https://msdn.microsoft.com/library/ff649801.aspx) lub [Zabezpieczeń systemu Windows](https://msdn.microsoft.com/library/ff649396.aspx) na komputerach z systemem Windows Server.
### <a name="node-to-node-certificate-security"></a>Węzeł węzeł certyfikat zabezpieczeń
Usługa tkaninie używa certyfikatów serwera X.509, określanych jako części konfiguracji typ węzła podczas tworzenia klastrze. Krótkie omówienie co to są te certyfikaty i jak uzyskiwanie lub tworzenie ich znajduje się na końcu tego artykułu.

Certyfikat zabezpieczeń jest skonfigurowany podczas tworzenia klaster przy użyciu Azure portal, szablony Menedżera zasobów Azure lub autonomicznego szablonu JSON. Możesz określić certyfikatu podstawowego i opcjonalnie certyfikat pomocniczej używany na potrzeby najazdy certyfikatu. Certyfikaty głównego i pomocniczego zadanej powinny być inne niż administrator klienta i certyfikatów klienta tylko do odczytu, wybrane [zabezpieczeń węzeł klient](#client-to-node-security).

Azure w artykule [Konfigurowanie klaster za pomocą szablonu Menedżera zasobów Azure](service-fabric-cluster-creation-via-arm.md) Aby dowiedzieć się, jak skonfigurować certyfikat zabezpieczeń w klastrze.

Autonomiczna Windows Server odczytywanie [bezpiecznego klastrze autonomicznego w systemie Windows przy użyciu certyfikaty X.509](service-fabric-windows-cluster-x509-security.md)

### <a name="node-to-node-windows-security"></a>Zabezpieczenia węzła do węzła systemu windows
Autonomiczna Windows Server odczytywanie [bezpiecznego klastrze autonomicznego w systemie Windows przy użyciu zabezpieczeń systemu Windows](service-fabric-windows-cluster-windows-security.md)

## <a name="client-to-node-security"></a>Węzeł Klient zabezpieczeń
Uwierzytelnianie klientów i zabezpiecza komunikacji między klientem a poszczególnych węzłów w klastrze. Ten rodzaj zabezpieczeń uwierzytelnia i zabezpiecza komunikację klienta, która zapewnia, że tylko autoryzowani użytkownicy mogą uzyskiwać dostęp klaster i aplikacje wdrożony w klastrze. Klienci są jednoznacznie zidentyfikować za pomocą poświadczeń zabezpieczeń systemu Windows lub poświadczeń zabezpieczeń certyfikatu.

![Diagram komunikacji węzeł klient][Client-to-Node]

[Certyfikat zabezpieczeń](https://msdn.microsoft.com/library/ff649801.aspx) lub [Zabezpieczeń systemu Windows](https://msdn.microsoft.com/library/ff649396.aspx), można użyć klastrów uruchomionych dla klastrów Azure lub Wersja autonomiczna z systemem Windows.

### <a name="client-to-node-certificate-security"></a>Węzeł klient certyfikat zabezpieczeń
 Węzeł Klient certyfikatu zabezpieczeń jest skonfigurowany podczas tworzenia klaster, albo przez portal Azure, szablony Menedżera zasobów lub od szablonu JSON autonomicznej, określając certyfikat klienta administracyjne i/lub certyfikat klienta użytkownika.  Administrator klientów i użytkowników certyfikaty klientów zadanej powinny być inne niż certyfikaty głównego i pomocniczego, wybrane [zabezpieczeń węzła do węzła](#node-to-node-security).

Klientów łączących się z klastrem przy użyciu certyfikatu administrator mieć pełny dostęp do funkcji zarządzania.  Klientów łączących się z klastrem przy użyciu certyfikatu klienta tylko do odczytu użytkownik ma dostęp tylko do odczytu do zarządzania. Innymi słowy te certyfikaty są używane do kontroli dostępu podstawach ról (RBAC) opisano w dalszej części tego artykułu.

Azure w artykule [Konfigurowanie klaster za pomocą szablonu Menedżera zasobów Azure](service-fabric-cluster-creation-via-arm.md) Aby dowiedzieć się, jak skonfigurować certyfikat zabezpieczeń w klastrze.

Autonomiczna Windows Server odczytywanie [bezpiecznego klastrze autonomicznego w systemie Windows przy użyciu certyfikaty X.509](service-fabric-windows-cluster-x509-security.md)

### <a name="client-to-node-azure-active-directory-aad-security-on-azure"></a>Węzeł Klient Azure Active Directory (AAD) zabezpieczeń Azure
Klastrów uruchomionych Azure można także bezpieczny dostęp do punktów końcowych zarządzania przy użyciu Azure Active Directory (AAD). Zobacz [Konfigurowanie klaster, przy użyciu szablonu Menedżera zasobów Azure](service-fabric-cluster-creation-via-arm.md) informacji na temat sposobu tworzenia niezbędne artefakty AAD, jak wypełnić je podczas tworzenia klaster i sposobu łączenia się z tymi klastrów później.

## <a name="security-recommendations"></a>Zalecenia dotyczące zabezpieczeń
Dla klastrów Azure zaleca się używanie zabezpieczeń AAD do uwierzytelniania klientów i certyfikaty dla zabezpieczeń węzła do węzła.

W przypadku autonomicznego Windows Server klastrów zaleca się używanie zabezpieczeń systemu Windows z grupą zarządzane konta (GMA), jeśli masz system Windows Server 2012 R2 i usługi Active Directory. W przeciwnym razie nadal używać zabezpieczeń systemu Windows z kontami systemu Windows.

## <a name="role-based-access-control-rbac"></a>Kontrola dostępu (RBAC) oparta na rolach
Kontrola dostępu umożliwia administratora klastrów, aby ograniczyć dostęp do niektórych operacji klaster dla różnych grup użytkowników, zwiększanie bezpieczeństwa klaster. Dwa typy kontroli dostępu innym są obsługiwane przez klientów łączenia się z klastrem: rolę administratora i roli użytkownika.

Administratorzy mają pełny dostęp do funkcji zarządzania (w tym możliwości odczytu/zapisu). Jeśli jesteś użytkownikiem, domyślnie mają tylko do odczytu w możliwościach zarządzania (na przykład możliwość wykonywania kwerend) i możliwość usuwania aplikacji i usług.

Można określić klienta role administratorów i użytkowników w momencie tworzenia klaster, dostarczając osobnych tożsamości (certyfikaty, AAD itp.) dla każdej z nich. Aby uzyskać więcej informacji na domyślne ustawienia kontroli dostępu i jak zmienić ustawienia domyślne zobacz [oparta na rolach kontrola dostępu dla klientów usługi tkaninie](service-fabric-cluster-security-roles.md).


## <a name="x509-certificates-and-service-fabric"></a>Certyfikaty X.509 i tkaninie usługi
Certyfikaty cyfrowe X.509 często umożliwiają do uwierzytelniania klientów i serwerów i szyfrowanie i cyfrowe podpisywanie wiadomości. Aby uzyskać więcej informacji o tych certyfikatów przejdź do [pracy z certyfikatów](http://msdn.microsoft.com/library/ms731899.aspx).

Kilka ważnych uwag brać pod uwagę:

- Certyfikaty używane w klastrów uruchomiony obciążenia produkcji powinny zostać utworzone za pomocą usługi poprawnie skonfigurowany certyfikat systemu Windows Server lub uzyskanego z zatwierdzony [Urząd certyfikacji (CA)](https://en.wikipedia.org/wiki/Certificate_authority).
- Nigdy nie używaj wszelkie tymczasowe lub sprawdzić certyfikaty w produkcji, które są tworzone za pomocą narzędzi, takich jak MakeCert.exe.
- Można użyć certyfikatu z podpisem własnym, ale należy tylko to zrobić dla klastrów test, a nie w produkcji.

### <a name="server-x509-certificates"></a>Certyfikat X.509 serwera

Certyfikaty serwera mają głównym zadaniem uwierzytelniania serwera (węzeł) do klientów lub uwierzytelniania serwera (węzeł) na serwerze (węzeł). Jedna z początkowej kontroli podczas klienta lub węzeł uwierzytelnia węzła jest wartość typowe nazwy w polu temat. Ta nazwa typowych lub jednej nazwy alternatywne temat certyfikaty musi istnieć na liście dozwolonych typowych nazw.

W poniższym artykule jak wygenerować certyfikaty z nazwy alternatywne tematu (SAN): [jak dodawać alternatywna nazwa podmiotu do certyfikatu bezpiecznego LDAP](http://support.microsoft.com/kb/931351).

Pole Subject mogą zawierać kilka wartości, każdy prefiksem inicjowania, aby wskazać typ wartości. Najczęściej inicjowania jest "CN" Nazwa typowych; na przykład "CN = www.contoso.com". Istnieje także możliwość pola tematu być puste. Jeśli pole opcjonalne alternatywna nazwa podmiotu została wpisana, musi zawierać nazwisko typowych certyfikatu i pojedyncze wpisy w poszczególnych alternatywna nazwa podmiotu. Te są wprowadzane jako wartości nazw DNS.

Wartość pola zamierzone cele certyfikatu powinien zawierać odpowiednią wartość, takie jak "Uwierzytelniania serwera" lub "Uwierzytelnianie klienta na podstawie".

### <a name="client-x509-certificates"></a>Certyfikaty X.509 klienta

Certyfikaty klienta nie są zazwyczaj wydawane przez urząd certyfikacji innych firm. Zamiast tego osobistych bieżącej lokalizacji użytkownika zazwyczaj zawiera certyfikaty klienta umieszczone w nim przez główny urząd z przeznaczeniem "Uwierzytelniania klienta". Klient może używać takiego certyfikatu, gdy jest wymagane uwierzytelnianie wzajemnego.

>[AZURE.NOTE] Wszystkie operacje zarządzania w klastrze tkaninie usługi wymagają certyfikatów serwera. Certyfikaty klienta nie można używać do zarządzania.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->


## <a name="next-steps"></a>Następne kroki

Ten artykuł zawiera informacje dotyczące zabezpieczeń klaster pojęć związanych. Następny, [utworzyć klaster w Azure za pomocą szablonu Menedżera zasobów](service-fabric-cluster-creation-via-arm.md) lub za pośrednictwem [Azure portal](service-fabric-cluster-creation-via-portal.md).

<!--Image references-->
[Node-to-Node]: ./media/service-fabric-cluster-security/node-to-node.png
[Client-to-Node]: ./media/service-fabric-cluster-security/client-to-node.png
