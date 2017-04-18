<properties
    pageTitle="Samouczek: Aplikacji sieci Web z bazą danych dzierżawy wielu elementów przy użyciu struktury obiektu i zabezpieczeń na poziomie wiersza"
    description="Dowiedz się, jak opracowywaniu aplikacji sieci web programu ASP.NET MVC 5 z wielu dzierżawą bazy danych SQL backent, przy użyciu struktury obiektu i zabezpieczeń na poziomie wiersza."
  metaKeywords="azure asp.net mvc entity framework multi tenant row level security rls sql database"
    services="app-service\web"
    documentationCenter=".net"
    manager="jeffreyg"
  authors="tmullaney"/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="04/25/2016"
    ms.author="thmullan"/>

# <a name="tutorial-web-app-with-a-multi-tenant-database-using-entity-framework-and-row-level-security"></a>Samouczek: Aplikacji sieci Web z bazą danych dzierżawy wielu elementów przy użyciu struktury obiektu i zabezpieczeń na poziomie wiersza

Ten samouczek przedstawiono sposób tworzenie aplikacji sieci web wielu dzierżawy z modelem dzierżawy "[udostępnioną bazą danych, schemat udostępnionej](https://msdn.microsoft.com/library/aa479086.aspx)", przy użyciu struktury obiektu i [Zabezpieczeń na poziomie wiersza](https://msdn.microsoft.com/library/dn765131.aspx). W tym modelu pojedynczy baza danych zawiera dane dla wielu dzierżaw i każdego wiersza w każdej tabeli jest skojarzony z "Dzierżawy identyfikatora" Wiersz poziom zabezpieczeń kontrola dostępu, nową funkcję bazy danych SQL Azure służy do dzierżaw uniemożliwić dostęp do danych innych osób. W tym celu tylko jedną, niewielką zmianę do aplikacji. Gromadząc logika dostępu dzierżawy w samej bazy danych, kontroli upraszcza kod aplikacji i zmniejsza ryzyko upływu przypadkowe danych między dzierżawami.

Zacznijmy od prostej aplikacji Contact Manager z [Tworzenie aplikacji specjalista MVP w dziedzinie programu ASP.NET z auth i bazy danych SQL i wdrażanie usługi aplikacji Azure](web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md). Prawo teraz aplikacja pozwala wszystkim użytkownikom (dzierżaw), aby wyświetlić wszystkie kontakty:

![Skontaktuj się z aplikacji Menedżera przed włączeniem kontroli](./media/web-sites-dotnet-entity-framework-row-level-security/ContactManagerApp-Before.png)

Z kilkoma niewielkie zmiany zostaną Dodajemy obsługę wielu dzierżawy, tak, aby użytkownicy będą mogli zobaczyć tylko kontakty, które należą do nich.

## <a name="step-1-add-an-interceptor-class-in-the-application-to-set-the-sessioncontext"></a>Krok 1: Dodawanie klasy Interceptor w aplikacji, aby ustawić SESSION_CONTEXT

Ma jedną zmianę aplikacji, którą należy wprowadzić. Ponieważ wszyscy użytkownicy aplikacji połączenia z bazą danych przy użyciu samej parametry połączenia (to znaczy samej logowanie SQL), nie jest obecnie nie ma sposobu zasady kontroli wiedzieć użytkownika, który należy go filtrować. Ta metoda jest często w aplikacjach sieci web, ponieważ umożliwia wydajną buforowanie połączeń, ale oznacza to, że jest potrzebna w inny sposób identyfikowania bieżącego użytkownika aplikacji w bazie danych. Rozwiązanie ma aplikacji Ustawianie pary klucz wartość dla bieżącego użytkownika w [SESSION_CONTEXT](https://msdn.microsoft.com/library/mt590806) natychmiast po otwarciu połączenie, przed rozpoczęciem wykonywania kwerendy. SESSION_CONTEXT jest magazynem wartość klucza o zakresie sesji i nasze zasady kontroli użyje użytkownika znajdujące się w niej identyfikowania bieżącego użytkownika.

[Interceptor](https://msdn.microsoft.com/data/dn469464.aspx) (w szczególności [DbConnectionInterceptor](https://msdn.microsoft.com/library/system.data.entity.infrastructure.interception.idbconnectioninterceptor)), zostanie dodana nowa funkcja Framework jednostki (EF) 6, aby automatycznie ustawić bieżącego użytkownika w SESSION_CONTEXT przez wykonywanie instrukcji T SQL przy każdym otwarciu EF połączenia.

1.  Otwórz projekt ContactManager w programie Visual Studio.
2.  Kliknij prawym przyciskiem myszy folder modeli w Eksploratorze rozwiązań, a następnie wybierz pozycję Dodaj > zajęć.
3.  Określ nazwę nowej klasy "SessionContextInterceptor.cs" i kliknij przycisk Dodaj.
4.  Zamień zawartość SessionContextInterceptor.cs poniższy kod.

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Web;
using System.Data.Common;
using System.Data.Entity;
using System.Data.Entity.Infrastructure.Interception;
using Microsoft.AspNet.Identity;

namespace ContactManager.Models
{
    public class SessionContextInterceptor : IDbConnectionInterceptor
    {
        public void Opened(DbConnection connection, DbConnectionInterceptionContext interceptionContext)
        {
            // Set SESSION_CONTEXT to current UserId whenever EF opens a connection
            try
            {
                var userId = System.Web.HttpContext.Current.User.Identity.GetUserId();
                if (userId != null)
                {
                    DbCommand cmd = connection.CreateCommand();
                    cmd.CommandText = "EXEC sp_set_session_context @key=N'UserId', @value=@UserId";
                    DbParameter param = cmd.CreateParameter();
                    param.ParameterName = "@UserId";
                    param.Value = userId;
                    cmd.Parameters.Add(param);
                    cmd.ExecuteNonQuery();
                }
            }
            catch (System.NullReferenceException)
            {
                // If no user is logged in, leave SESSION_CONTEXT null (all rows will be filtered)
            }
        }
        
        public void Opening(DbConnection connection, DbConnectionInterceptionContext interceptionContext)
        {
        }

        public void BeganTransaction(DbConnection connection, BeginTransactionInterceptionContext interceptionContext)
        {
        }

        public void BeginningTransaction(DbConnection connection, BeginTransactionInterceptionContext interceptionContext)
        {
        }

        public void Closed(DbConnection connection, DbConnectionInterceptionContext interceptionContext)
        {
        }

        public void Closing(DbConnection connection, DbConnectionInterceptionContext interceptionContext)
        {
        }

        public void ConnectionStringGetting(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void ConnectionStringGot(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void ConnectionStringSet(DbConnection connection, DbConnectionPropertyInterceptionContext<string> interceptionContext)
        {
        }

        public void ConnectionStringSetting(DbConnection connection, DbConnectionPropertyInterceptionContext<string> interceptionContext)
        {
        }

        public void ConnectionTimeoutGetting(DbConnection connection, DbConnectionInterceptionContext<int> interceptionContext)
        {
        }

        public void ConnectionTimeoutGot(DbConnection connection, DbConnectionInterceptionContext<int> interceptionContext)
        {
        }

        public void DataSourceGetting(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void DataSourceGot(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void DatabaseGetting(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void DatabaseGot(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void Disposed(DbConnection connection, DbConnectionInterceptionContext interceptionContext)
        {
        }

        public void Disposing(DbConnection connection, DbConnectionInterceptionContext interceptionContext)
        {
        }

        public void EnlistedTransaction(DbConnection connection, EnlistTransactionInterceptionContext interceptionContext)
        {
        }

        public void EnlistingTransaction(DbConnection connection, EnlistTransactionInterceptionContext interceptionContext)
        {
        }

        public void ServerVersionGetting(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void ServerVersionGot(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void StateGetting(DbConnection connection, DbConnectionInterceptionContext<System.Data.ConnectionState> interceptionContext)
        {
        }

        public void StateGot(DbConnection connection, DbConnectionInterceptionContext<System.Data.ConnectionState> interceptionContext)
        {
        }
    }

    public class SessionContextConfiguration : DbConfiguration
    {
        public SessionContextConfiguration()
        {
            AddInterceptor(new SessionContextInterceptor());
        }
    }
}
```

To jest wymagana zmiana tylko aplikacji. Zrealizuj i tworzenie i publikowanie aplikacji.

## <a name="step-2-add-a-userid-column-to-the-database-schema"></a>Krok 2: Dodawanie kolumny Identyfikator użytkownika ze schematem bazy danych

Następnie należy dodać kolumnę identyfikator użytkownika do tabeli Kontakty, które chcesz skojarzyć każdy wiersz użytkownika (dzierżawy). Firma Microsoft zmieni schemat bezpośrednio w bazie danych, dzięki czemu firma Microsoft nie trzeba do uwzględnienia w naszym EF modelu danych w tym polu.

Połączenia z bazą danych bezpośrednio za pomocą programu SQL Server Management Studio lub Visual Studio, a następnie wykonaj następujące czynności T-SQL:

```
ALTER TABLE Contacts ADD UserId nvarchar(128)
    DEFAULT CAST(SESSION_CONTEXT(N'UserId') AS nvarchar(128))
```

Spowoduje to dodanie kolumny Identyfikator użytkownika do tabeli Kontakty. Firma Microsoft korzysta z typem danych nvarchar(128) zgodnie z nazw użytkownika, przechowywane w tabeli AspNetUsers i tworzymy ograniczenie domyślne, które zostanie automatycznie ustawiona nazwa użytkownika nowo wstawionych wierszy za użytkownika obecnie przechowywanych w SESSION_CONTEXT.

Teraz tabela wygląda następująco:

![Tabela SSMS kontaktów](./media/web-sites-dotnet-entity-framework-row-level-security/SSMS-Contacts.png)

Podczas tworzenia nowych kontaktów, automatycznie zostaną przydzieleni poprawny identyfikator użytkownika. Do celów pokaz jednak Przejdźmy przypisać kilka następujących istniejące kontakty do istniejącego użytkownika.

Jeśli w aplikacji już została utworzona kilku użytkowników (np., za pomocą lokalnych, Google lub Facebook kont), zostaną one wyświetlone w tabeli AspNetUsers. Poniżej ekranu jest tylko jeden użytkownik pory.

![Tabela SSMS AspNetUsers](./media/web-sites-dotnet-entity-framework-row-level-security/SSMS-AspNetUsers.png)

Skopiuj identyfikator user1@contoso.com, i wklej je w poniższych instrukcji T-SQL. Wykonanie tej instrukcji chcesz skojarzyć z kontakty trzy ta nazwa użytkownika.

```
UPDATE Contacts SET UserId = '19bc9b0d-28dd-4510-bd5e-d6b6d445f511'
WHERE ContactId IN (1, 2, 5)
```

## <a name="step-3-create-a-row-level-security-policy-in-the-database"></a>Krok 3: Tworzenie zasad zabezpieczeń na poziomie wiersza w bazie danych

Ostatnim krokiem jest utworzenie zasad zabezpieczeń, które używa nazwie SESSION_CONTEXT automatycznie filtrować wyniki zwrócone przez kwerendy.

Gdy połączenie z bazą danych, wykonaj następujące T-SQL:

```
CREATE SCHEMA Security
go

CREATE FUNCTION Security.userAccessPredicate(@UserId nvarchar(128))
    RETURNS TABLE
    WITH SCHEMABINDING
AS
    RETURN SELECT 1 AS accessResult
    WHERE @UserId = CAST(SESSION_CONTEXT(N'UserId') AS nvarchar(128))
go

CREATE SECURITY POLICY Security.userSecurityPolicy
    ADD FILTER PREDICATE Security.userAccessPredicate(UserId) ON dbo.Contacts,
    ADD BLOCK PREDICATE Security.userAccessPredicate(UserId) ON dbo.Contacts
go

```

Ten kod wykonuje trzy elementy. Najpierw tworzy nowy schemat, zgodnie z zaleceniami dotyczącymi centralizowanie i ograniczanie dostępu do obiektów kontroli. Następnie tworzy predykatu funkcję, która może zwrócić "1", gdy nazwa użytkownika rzędu pasuje do nazwy użytkownika w SESSION_CONTEXT. Na koniec tworzy zasady zabezpieczeń, które dodaje tej funkcji jako predykat filtru i bloku na tabeli Kontakty. Predykat filtru powoduje zapytania przywrócić tylko te wiersze, które należą do bieżącego użytkownika, a predykacie blok działa ze względów bezpieczeństwa, aby zapobiec aplikacji kiedykolwiek przypadkowo wstawiania wiersza dla użytkownika problem.

Uruchom aplikację i zaloguj się jako user1@contoso.com. Ten użytkownik widzi teraz tylko kontakty możemy przypisane do ta nazwa użytkownika wcześniej:

![Skontaktuj się z aplikacji Menedżera przed włączeniem kontroli](./media/web-sites-dotnet-entity-framework-row-level-security/ContactManagerApp-After.png)

Aby zatwierdzić to dodatkowo, spróbuj zarejestrować nowego użytkownika. Ponieważ nie zostały przypisane do nich zobaczą żadnych kontaktów. Jeśli ta osoba Utwórz nowy kontakt, zostanie przypisany do nich, a tylko będą one mogły je zobaczyć.

## <a name="next-steps"></a>Następne kroki

To wszystko! Prosta aplikacji sieci web Contact Manager przekonwertowany do wielu dzierżawy jedno miejsce, w którym każdy użytkownik ma własną listę kontaktów. Przy użyciu zabezpieczeń na poziomie wiersza, możemy zostały unikać złożoność Wymuszanie reguł dostępu dzierżawy w naszym kod aplikacji. Przejrzystość umożliwia skoncentrowanie się na problem rzeczywistą firm wykonywanego aplikacji i zmniejsza również ryzyko przypadkowo przeciek danych, jak aplikacja jest ścieżki bazowej kodu rozwoju.

Ten samouczek zawiera tylko wewnętrzna powierzchni możliwości programu kontroli. Na przykład jest możliwe jest bardziej zaawansowane lub logika szczegółowego dostępu który jest można zachować więcej niż tylko bieżący identyfikator użytkownika w SESSION_CONTEXT. Użytkownik może również do [integracji kontrola dostępu z bibliotekami klienta narzędzia elastyczne bazy danych](../sql-database/sql-database-elastic-tools-multi-tenant-row-level-security.md) do obsługi wielu dzierżawy odłamki w warstwie danych.

Oprócz tych możliwości również pracujemy nad nawet udoskonalić kontroli. Jeśli masz pytania pomysłów i rzeczy, które chcesz wyświetlić, napisz nam w komentarzach. Dziękujemy za swoją opinię!
