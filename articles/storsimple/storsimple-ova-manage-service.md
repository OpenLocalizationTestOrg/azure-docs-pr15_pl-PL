<properties 
   pageTitle="Wdrożyć usługę Menedżer StorSimple tablicy virtual StorSimple | Microsoft Azure"
   description="Wyjaśniono, jak tworzyć i usuwać usługę Menedżer StorSimple w portalu klasyczny Azure i opisano, jak zarządzać klucza rejestru usługi."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="05/19/2016"
   ms.author="alkohli" />

# <a name="deploy-the-storsimple-manager-service-for-storsimple-virtual-array"></a>Wdrożyć usługę Menedżer StorSimple StorSimple wirtualnych tablicy

## <a name="overview"></a>Omówienie

Usługa Menedżera StorSimple działa w programie Microsoft Azure i łączy do wielu urządzeń StorSimple. Po utworzeniu usługę służy go do zarządzania urządzeniami w portalu Microsoft Azure klasyczny działa w przeglądarce. Pozwala na monitorowanie wszystkich urządzeń, które są połączone z usługą Menedżera StorSimple z jednym centralnym miejscu, w związku z tym zminimalizowanie obciążenia administracyjne.

Strona początkowa Menedżera StorSimple wymieniono wszystkie usługi Menedżera StorSimple, które umożliwia zarządzanie urządzeniami StorSimple miejsca do magazynowania. Dla każdej usługi Menedżera StorSimple następujące informacje są prezentowane na stronie Menedżera StorSimple:

- **Nazwa** — nazwę, która została przypisana w usłudze Menedżer StorSimple podczas jej tworzenia. Nie można zmienić nazwy usługi, po utworzeniu usługę.

- **Stan** — stan usługi, która może być **aktywna**, **Tworzenie**lub **Online**.

- **Lokalizacja** — lokalizacji geograficznej, w której zostanie wdrożony urządzenia StorSimple.

- **Subskrypcja** — rozliczeń subskrypcji, która jest skojarzona z tej usługi.

Typowe zadania, które mogą być wykonywane za pośrednictwem strony Menedżera StorSimple są następujące:

- Tworzenie usługi
- Usuwanie usługi
- Uzyskiwanie klucza rejestru usługi
- Ponownie wygenerować klucza rejestru usługi

Ten samouczek opisano, jak wykonać poszczególne z tych zadań. Informacje zawarte w tym artykule ma zastosowanie tylko do tablic wirtualnych StorSimple. Aby uzyskać więcej informacji na serii StorSimple 8000 przejdź do [Wdrażanie usługi Menedżera StorSimple](storsimple-manage-service.md).

## <a name="create-a-service"></a>Tworzenie usługi

Opcja **Szybkiego tworzenia** umożliwia tworzenie usługi Menedżera StorSimple, jeśli chcesz wdrożyć urządzenia StorSimple. Aby utworzyć usługę, musisz mieć:

- Subskrypcję z Umowa dla przedsiębiorstw
- Aktywne konto miejsca do magazynowania Microsoft Azure
- Rozliczeniowym, która służy do zarządzania dostępu

Możesz również wybrać do generowania domyślne konto miejsca do magazynowania, podczas tworzenia usługi.

Jednej usługi można zarządzać wielu urządzeń. Jednak urządzenie nie może obejmować wielu usług. Duże przedsiębiorstwo mogą mieć wiele wystąpień usług do pracy z różnych subskrypcjach, organizacji lub nawet wdrożenia lokalizacji.  

> [AZURE.NOTE] Potrzebujesz osobne wystąpienia usługi StorSimple Menedżer urządzeń serii StorSimple 8000 i tablic wirtualnych StorSimple do zarządzania.

Wykonaj poniższe czynności, aby utworzyć usługę.

[AZURE.INCLUDE [storsimple-ova-create-new-service](../../includes/storsimple-ova-create-new-service.md)]

## <a name="delete-a-service"></a>Usuwanie usługi

Przed usunięciem usługi, upewnij się, że żadne urządzenia połączonego z go. Jeśli usługa jest używany, Dezaktywuj podłączone urządzenia. Operacja Dezaktywuj serwera połączenia między urządzeniem i usługę, ale zachować dane urządzenia w chmurze. 

> [AZURE.IMPORTANT] Po usunięciu usługi nie można cofnąć tej operacji. 

Wykonaj poniższe czynności, aby usunąć usługę.

### <a name="to-delete-a-service"></a>Aby usunąć usługi

1. Na stronie **Menedżera StorSimple usługi** wybierz usługę, którą chcesz usunąć.

1. Kliknij przycisk **Usuń** w dolnej części strony.

1. W oknie potwierdzenia kliknij przycisk **Tak** . Może potrwać kilka minut w usłudze zostanie usunięta.

## <a name="get-the-service-registration-key"></a>Uzyskiwanie klucza rejestru usługi

Po pomyślnym utworzeniu usługi musisz zarejestrować urządzenie StorSimple z usługą. Aby zarejestrować pierwszego urządzenia StorSimple, konieczne będzie klucza rejestru usługi. Aby zarejestrować dodatkowe urządzenia z istniejącej usługi StorSimple, konieczne będzie zarówno klucz rejestru, jak i usługi klucza szyfrowania danych (który jest generowany na pierwszym urządzeniu podczas rejestrowania). Aby uzyskać więcej informacji na temat klucza szyfrowania danych usługi zobacz [Uzyskiwanie klucza szyfrowania danych usługi z sieci web lokalnego interfejsu użytkownika](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key). 

Wykonaj poniższe czynności, aby uzyskać klucz rejestru usługi.

[AZURE.INCLUDE [storsimple-ova-get-service-registration-key](../../includes/storsimple-ova-get-service-registration-key.md)]

Zachowaj klucz rejestru usługi w bezpiecznym miejscu. Konieczne będzie ten klucz, a także klucza szyfrowania danych usługi, aby zarejestrować dodatkowe urządzenia z tej usługi. Po uzyskaniu klucza rejestru usługi, należy skonfigurować urządzenia przy użyciu programu Windows PowerShell w celu interfejsu StorSimple.

## <a name="regenerate-the-service-registration-key"></a>Ponownie wygenerować klucza rejestru usługi

Będzie musiał ponownie wygenerować klucza rejestru usługi, jeśli jest wymagane do wykonywania najważniejszych obrotu lub lista administratorów usługi uległa zmianie. Po wygenerować klucz, nowy klucz jest używany tylko do rejestrowania kolejnych urządzeń. Nie ma wpływu tego procesu są urządzenia, które zostały już zarejestrowane.

Wykonaj poniższe czynności, aby wygenerować klucza rejestru usługi.

### <a name="to-regenerate-the-service-registration-key"></a>Aby ponownie wygenerować klucza rejestru usługi

1. Na stronie **usługi Menedżera StorSimple** kliknij **Klucz rejestru**.

1. W oknie dialogowym **Klucza rejestru usługi** kliknij pozycję **Generuj**.

1. Zostanie wyświetlony komunikat potwierdzenia. Kliknij **przycisk OK** , aby kontynuować ponownego wygenerowania.

1. Pojawi się nowy klucz rejestru usługi.

1. Skopiuj ten klucz i zapisać w celu rejestrowania wszystkich nowych urządzeń z tej usługi.

1. Kliknij ikonę wyboru ![Ikona modułu sprawdzania](./media/storsimple-ova-manage-service/image7.png) Aby zamknąć to okno dialogowe.


## <a name="next-steps"></a>Następne kroki

- Dowiedz się, jak [rozpocząć pracę](storsimple-ova-deploy1-portal-prep.md) z tablicę virtual StorSimple.
    
- Dowiedz się, jak [administrować urządzenia StorSimple](storsimple-ova-web-ui-admin.md).

 
