<properties
    pageTitle="Tożsamość hybrydowego usługi Azure Active Directory zagadnienia projektowe — ustalenie hybrydowych zadania związane z zarządzaniem tożsamości | Microsoft Azure"
    description="Za pomocą pola Kontrola dostępu warunkowego usługi Azure Active Directory sprawdza określone warunki wybierz podczas uwierzytelniania użytkownika i przed zezwoleniem na dostęp do aplikacji. Po tych warunki są spełnione, użytkownik uwierzytelniony i dostęp do aplikacji."
    documentationCenter=""
    services="active-directory"
    authors="billmath"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity" 
    ms.date="08/08/2016"
    ms.author="billmath"/>

# <a name="plan-for-hybrid-identity-lifecycle"></a>Planowanie hybrydowych tożsamości cyklu 

Tożsamość jest jednym z podstawowych kwestii dotyczących strategii enterprise mobilności i aplikacji programu access. Czy są logowanie się do swojego urządzenia przenośnego lub aplikacji władz akredytacji bezpieczeństwa, tożsamości jest kluczem do uzyskania dostępu do dowolnych danych. Na najwyższym poziomie Zarządzanie tożsamościami obejmuje widać i synchronizowanie między magazynów Twojej tożsamości, które zawiera Automatyzowanie i centralizowanie proces inicjowania obsługi administracyjnej zasobów. Rozwiązanie tożsamości powinny być scentralizowane tożsamości w obrębie lokalnej i w chmurze i za pomocą niektórych formularza federacji tożsamości można zachować scentralizowane uwierzytelnianie i bezpieczne udostępnianie i współpraca z użytkownikami zewnętrznymi i firmy. Zasoby z zakresu od systemów operacyjnych i aplikacji do osób z lub powiązane z organizacją. Struktura organizacji może się zmienić, aby zezwalały na zasady i procedury obsługi administracyjnej.

Jest również istotne rozwiązanie tożsamości skierowaną, aby umożliwić użytkownikom, podając je Samoobsługowe środowiska do zachowania ich wydajność. Rozwiązanie tożsamości jest bardziej rozbudowany, jeśli umożliwia rejestracji jednokrotnej dla użytkowników przez wszystkie zasoby potrzebują dostępu na wszystkich administratorów poziomy użyć standardowe procedury zarządzania poświadczeń użytkownika. Niektóre poziomy Administracja można ograniczyć lub wyeliminować, w zależności od szerokości obsługi administracyjnej rozwiązania do zarządzania. Ponadto można bezpiecznego dystrybuować funkcje administracji, ręcznie lub automatycznie między różnymi organizacje. Na przykład administrator domeny może służyć tylko zasoby ludzkie i w tej domenie. Ten użytkownik umożliwia wykonywanie zadań administracyjnych i obsługi administracyjnej, ale nie ma uprawnień do wykonywania zadań konfiguracji, takich jak tworzenie przepływów pracy.


## <a name="determine-hybrid-identity-management-tasks"></a>Określanie hybrydowych zadania związane z zarządzaniem tożsamości
Rozpowszechnianie zadań administracyjnych w organizacji zwiększa dokładność i skuteczności administracji i zwiększa saldo obciążenie pracą w organizacji. Poniżej przedstawiono tabele przestawne, które Definiowanie systemu zarządzania niezawodne tożsamości.

 ![](./media/hybrid-id-design-considerations/Identity_management_considerations.png)


Aby zdefiniować hybrydowych zadania związane z zarządzaniem tożsamości, należy zrozumieć niektóre najważniejsze cechy organizacji, która będzie przyjęcie hybrydowych tożsamości. Należy zrozumieć repozytoria bieżącego, używany dla tożsamości źródeł. Informacje o tych podstawowych elementów, uzyskuje foundational wymagania i na podstawie potrzebujesz zadawać pytania bardziej szczegółowego, które prowadzą do lepszych decyzji projekt dotyczących rozwiązania tożsamości.  

Podczas definiowania tych wymagań, upewnij się, że co najmniej są odpowiedzi na poniższe pytania

- Opcje obsługi administracyjnej: 
 - Rozwiązanie tożsamości hybrydowych obsługuje z systemu obsługi administracyjnej i zarządzanie dostępem niezawodne konta?
 - W jaki sposób użytkownicy, grupy i hasła, przechodząc do zarządzania?
 - Zarządzanie tożsamościami cyklu życia jest odpowiada? 
      - Jak długo zawieszenie konta aktualizacji hasła potrwa?
      
- Zarządzanie licencjami: 
 - Czy zarządzanie licencjami hybrydowych tożsamości rozwiązanie uchwyty?
     - Jeśli tak, jakie funkcje są dostępne?
- Rozwiązanie obsługuje zarządzanie licencjami oparte na grupach? 
      — Jeśli tak, czy jest możliwe przypisać do niego grupy zabezpieczeń? 
       — Jeśli tak, zostanie katalogu chmury automatycznie przypisywać licencje do wszystkich członków grupy? 
        — Co się stanie, jeśli użytkownik jest następnie dodane do lub usunięte z grupy, będzie licencji automatyczne przypisanie lub usunięte odpowiednio? 

- Integracja z innych dostawców tożsamości innych firm:
- Tego rozwiązania hybrydowego można zintegrować z dostawcy tożsamości innych firm w celu wdrożenia logowania jednokrotnego?
- Jest to możliwe ujednolicić wszystkich dostawców innej tożsamości w systemie spójnych tożsamości?
- Jeśli tak, jak i które są one i jakie funkcje są dostępne?

## <a name="synchronization-management"></a>Zarządzanie synchronizacją
Jednym z celów menedżera tożsamości, aby można było wyświetlić wszystkich dostawców tożsamości które będą stale synchronizowane. Zachowaj dane synchronizowane zgodnie z dostawcą tożsamości autorytatywne wzorca. W scenariuszu tożsamości hybrydowego z modelu zarządzania zsynchronizowane Zarządzanie wszystkie tożsamości użytkownika i urządzenie lokalnego serwera i synchronizowanie konta i ewentualnie haseł w chmurze. Użytkownik wprowadzi samego hasła lokalnego jako on lub Anna usługa w chmurze, a u logowania, hasło został zweryfikowany przez rozwiązanie tożsamości. Ten model używa narzędzie do synchronizacji katalogów.
 
![](./media/hybrid-id-design-considerations/Directory_synchronization.png)Projekt pisane z wielkiej litery synchronizacji rozwiązania hybrydowego tożsamości upewnij się, że należy odpowiedzieć na następujące pytania: • co to są dostępne dla tożsamości rozwiązania hybrydowego rozwiązania synchronizacji?
• Co to są logowaniu jednokrotnym dostępne możliwości?
• Jakie są opcje do federacji tożsamości między B2B i B2C?

## <a name="next-steps"></a>Następne kroki
[Określanie strategii wdrażania zarządzania tożsamości hybrydowego](active-directory-hybrid-identity-design-considerations-lifecycle-adoption-strategy.md)


## <a name="see-also"></a>Zobacz też
[Omówienie zagadnienia projektu](active-directory-hybrid-identity-design-considerations-overview.md)

