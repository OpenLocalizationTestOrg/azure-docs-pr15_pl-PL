<properties
    pageTitle="Zarządzanie uprawnieniami do zasobów dla poszczególnych użytkowników w stos Azure (administrator usługi i dzierżawy) | Microsoft Azure"
    description="Jako administrator usługi lub dzierżawy Dowiedz się, jak zarządzać uprawnieniami do zasobów dla poszczególnych użytkowników."
    services="azure-stack"
    documentationCenter=""
    authors="ErikjeMS"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="erikje"/>

# <a name="manage-user-permissions"></a>Zarządzanie uprawnieniami użytkowników

Użytkownika w stos Azure może być czytnika, właściciel lub współautora dla każdego wystąpienia subskrypcji, grupa zasobów lub usługi. Na przykład użytkownik A może mieć uprawnienia czytnika 1 subskrypcji, ale do 7 maszyn wirtualnych uprawnienia właściciela.

-   Czytnik: Użytkownik może wyświetlać wszystkie elementy, ale nie można wprowadzać zmiany.

-   Współautor: Użytkownik może zarządzać wszystko oprócz dostęp do zasobów.

-   Właściciel: Użytkownik może zarządzać wszystko, łącznie z dostępem do zasobów.


## <a name="set-access-permissions-for-a-user"></a>Ustawianie uprawnień dostępu dla użytkownika

1.  Zaloguj się przy użyciu konta, które ma uprawnienia właściciela do zasobu, którą chcesz zarządzać.

2.  W karta dla zasobu, kliknij ikonę programu **Access** ![](media/azure-stack-manage-permissions/image1.png).

3.  W karta **użytkowników** kliknij pozycję **role**.

4.  W karta **role** kliknij przycisk **Dodaj** , aby dodać uprawnienia dla użytkownika.

## <a name="next-steps"></a>Następne kroki

[Dodawanie dzierżawę stos Azure](azure-stack-add-new-user-aad.md)
