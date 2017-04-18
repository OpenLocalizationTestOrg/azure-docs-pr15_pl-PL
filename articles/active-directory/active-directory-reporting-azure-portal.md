<properties
   pageTitle="Azure Active Directory raportowanie — Podgląd | Microsoft Azure"
   description="Lista różnych dostępnych raportów dla usługi Azure Active Directory podglądu"
   services="active-directory"
   documentationCenter=""
   authors="markusvi"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/30/2016"
   ms.author="markvi"/>

# <a name="azure-active-directory-reporting---preview"></a>Azure Active Directory raportowanie — preview

> [AZURE.SELECTOR]
- [Azure portal](active-directory-reporting-azure-portal.md)
- [Portal Azure klasyczny](active-directory-reporting-guide.md)

*Niniejsza dokumentacja jest częścią [Azure Active Directory raportowania przewodnika](active-directory-reporting-guide.md).*

W przypadku raportów w podglądzie usługi Azure Active Directory, otrzymasz wszystkie informacje, które należy określić, jak robi środowiska. [Co to jest w podglądzie?](active-directory-preview-explainer.md)

Istnieją dwa główne obszary raportowania:

- **Działania logowania** — informacje o korzystaniu z aplikacji zarządzanych i działania logowania użytkownika

- **Dzienniki inspekcji** — System aktywności informacji na temat użytkowników i zarządzanie grupami, zarządzanych aplikacji i działania katalogu

W zależności od zakresu danych, którego szukasz można korzystać z tych raportów albo, klikając pozycję **Użytkownicy i grupy** lub **aplikacje dla przedsiębiorstw** na liście usługi w [portalu Azure](https://portal.azure.com).

## <a name="sign-in-activities"></a>Działania logowania

### <a name="user-sign-in-activities"></a>Działania logowania użytkownika

Z informacji podanych przez użytkownika logowania raportu możesz odpowiedzi na pytania takie jak:

- Co to jest wzorzec logowania użytkownika?
- Ilu użytkowników mają użytkownicy zalogowani w tygodniu?
- Co to jest stan te dodatki logowania?

Punkt wejścia do tych danych znajduje się użytkownika logowania wykres w sekcji **Przegląd** w obszarze **Użytkownicy i grupy**.

 ![Raportowanie] (./media/active-directory-reporting-azure-portal/05.png "Raportowanie")

Na wykresie logowania użytkownika są wyświetlane tygodniowy agregacji logowania dodatki dla wszystkich użytkowników w danym okresie. Wartość domyślna dla okresu to 30 dni.

![Raportowanie] (./media/active-directory-reporting-azure-portal/02.png "Raportowanie")

Po kliknięciu w dniu na wykresie logowania, zostanie wyświetlony szczegółowy wykaz działań logowania.

![Raportowanie] (./media/active-directory-reporting-azure-portal/03.png "Raportowanie")

Każdy wiersz na liście logowania działania umożliwia szczegółowych informacji na temat wybranego logowania takich jak:

- Kto jest zarejestrowana w?

- Jaki był powiązanych głównej nazwy użytkownika?

- Jakie aplikacja została docelowym logowania?

- Co to jest adres IP logowania?

- Jaki był stanu logowania?

### <a name="usage-of-managed-applications"></a>Użycie zarządzanych aplikacji

Mając stanowi danych logowania można odpowiedzieć na pytania takie jak:

- Kto jest za pomocą aplikacje?

- Co to są najważniejsze 3 aplikacji w organizacji?

- Można mieć ostatnio wdrażania aplikacji. Jak jest wykonanie?


Punktu wejścia do tych danych jest górny aplikacji 3 w organizacji w ostatni raport 30 dni w sekcji **Przegląd** w obszarze **aplikacje dla przedsiębiorstw**.

 ![Raportowanie] (./media/active-directory-reporting-azure-portal/06.png "Raportowanie")


Aplikacja zastosowania wykresu tygodniowy agregacji z Zaloguj dodatki aplikacji 3 pierwszych w danym okresie. Wartość domyślna dla okresu to 30 dni.

![Raportowanie] (./media/active-directory-reporting-azure-portal/78.png "Raportowanie")

Jeśli chcesz, możesz ustawienie fokusu na określonej aplikacji.

![Raportowanie] (./media/active-directory-reporting-azure-portal/single_spp_usage_graph.png "Raportowanie")


Po kliknięciu w dniu na wykresie zastosowania aplikacji, możesz uzyskać szczegółowy wykaz działań logowania.


![Raportowanie] (./media/active-directory-reporting-azure-portal/top_app_sign_ins.png "Raportowanie")



Opcja **rejestrowania** umożliwia pełne zestawienie wszystkie zdarzenia logowania do aplikacji.

![Raportowanie] (./media/active-directory-reporting-azure-portal/85.png "Raportowanie")

Za pomocą okna dialogowego Wybór kolumny, można wybrać pola danych, które mają być wyświetlane.

![Raportowanie] (./media/active-directory-reporting-azure-portal/column_chooser.png "Raportowanie")



### <a name="filtering-sign-ins"></a>Filtrowanie rejestrowaniu

Rejestrowaniu można filtrować według przedział czasu, aby ograniczyć liczbę wyświetlanych danych.

![Raportowanie] (./media/active-directory-reporting-azure-portal/927.png "Raportowanie")


Inną metodą do filtrowania pozycji działania logowania jest aby wyszukać określone wpisy.
Metoda wyszukiwania umożliwia zakresu usługi rejestrowaniu wokół określonych **użytkowników**, **grup** lub **aplikacji**.


![Raportowanie] (./media/active-directory-reporting-azure-portal/84.png "Raportowanie")

## <a name="audit-logs"></a>Dzienniki inspekcji

Dzienniki inspekcji w usłudze Azure Active Directory zapewniają rekordów działań systemu w celu zapewnienia zgodności.

Istnieją trzy główne kategorie inspekcji działania pokrewne w portalu Azure:

- Użytkownicy i grupy   

- Aplikacje

- Katalogu   


Aby uzyskać pełną listę działania raportu inspekcji Zobacz [listę zdarzeń raportu inspekcji](active-directory-reporting-audit-events.md#list-of-audit-report-events).


Punkt wejścia do wszystkich danych inspekcji znajduje się **dzienników inspekcji** w sekcji **działania** **Usługi Azure Active Directory**.


![Inspekcja] (./media/active-directory-reporting-azure-portal/61.png "Inspekcja")


Dziennik inspekcji ma widok listy uczestników (), która działań, (co) i celów.


![Inspekcja] (./media/active-directory-reporting-azure-portal/345.png "Inspekcja")


Kliknięcie pozycji w widoku listy, możesz uzyskać więcej szczegółów.

![Inspekcja] (./media/active-directory-reporting-azure-portal/873.png "Inspekcja")




### <a name="users-and-groups-audit-logs"></a>Użytkownicy i grupy dzienników inspekcji


Z użytkownikiem i raporty inspekcji oparte na grupach można uzyskać odpowiedzi na pytania takie jak:

- Jakie typy aktualizacji były stosowane użytkowników?

- Ilu użytkowników zostały zmienione?

- Ile haseł zostały zmienione?

- Co przeprowadzonych przez administratora w katalogu?

- Co to są grupy, które zostały dodane?

- Czy istnieją grupy ze zmianami członkostwa?

- Zostały zmienione właścicieli grupy?

- Jakie licencje zostały przypisane do grupy lub użytkownika?


Jeśli chcesz zapoznać się z inspekcji danych, która jest powiązana użytkownikom i grupom, widoku filtrowanego w obszarze **dzienników inspekcji** można znaleźć w sekcji **aktywności** **użytkowników**i grup.


![Inspekcja] (./media/active-directory-reporting-azure-portal/93.png "Inspekcja")


### <a name="application-audit-logs"></a>Dzienniki inspekcji aplikacji

Przy użyciu aplikacji inspekcji raportów, można uzyskać odpowiedzi na pytania, takie jak:

- Co to są programy, które zostały dodane lub zaktualizowane?

- Co to są programy, które zostały usunięte?

- Zasady usług dla aplikacji zmiany?

- Zostały zmienione nazwy aplikacji?

- Kto przekazała zgody do aplikacji?


Jeśli chcesz zapoznać się z inspekcji dane powiązane z aplikacjami, widoku filtrowanego w obszarze **dzienników inspekcji** można znaleźć w sekcji **aktywności** z **aplikacji przedsiębiorstwa**.


![Inspekcja] (./media/active-directory-reporting-azure-portal/134.png "Inspekcja")


### <a name="filtering-audit-logs"></a>Filtrowanie dzienników inspekcji

Raport inspekcji można filtrować według przedział czasu, aby ograniczyć liczbę wyświetlanych danych.

![Inspekcja] (./media/active-directory-reporting-azure-portal/324.png "Inspekcja")

Inną metodą do filtrowania pozycji dziennika inspekcji jest aby wyszukać określone wpisy.

![Inspekcja] (./media/active-directory-reporting-azure-portal/237.png "Inspekcja")

## <a name="next-steps"></a>Następne kroki

Zobacz [Azure Active Directory raportowania przewodnika](active-directory-reporting-guide.md).
