Jeśli korzystasz z subskrypcji usługi Azure umiejscowienia nie administratora i właściciela, takich jak firmy właścicielem subskrypcji, należy sprawdzić przed w tym dokumencie, wykonując kroki:

* Logowanie Azure musi mieć co najmniej __współautora__ dostęp do grup zasobów Azure używaną podczas tworzenia HDInsight (i inne zasoby Azure)

* Osoba posiadająca w co najmniej __współautora__ dostęp do Azure subskrypcji musisz ją wcześniej zarejestrować dostawcy dla zasobu w przypadku korzystania. Rejestracja dostawcy się dzieje, gdy użytkownik z trybu współautora dostęp do subskrypcji tworzy zasób po raz pierwszy dla tej subskrypcji. Go można także wykonywać bez tworzenia zasobu przez [dostawcę za pomocą usługi REST rejestrowanie](https://msdn.microsoft.com/library/azure/dn790548.aspx).

Aby uzyskać więcej informacji na temat pracy z zarządzania dostępu zobacz następujące dokumenty:

* [Wprowadzenie do zarządzania dostępu w portalu Azure](../articles/active-directory/role-based-access-control-what-is.md)
* [Zarządzanie dostępem do zasobów subskrypcji Azure za pomocą przypisania ról](../articles/active-directory/role-based-access-control-configure.md)
