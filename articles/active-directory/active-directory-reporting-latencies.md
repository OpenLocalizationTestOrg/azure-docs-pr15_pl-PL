<properties
   pageTitle="Azure Active Directory raportowania opóźnienia | Microsoft Azure"
   description="Czas potrzebny do raportowania zdarzeń się pojawić w usłudze Active Directory platformy Azure"
   services="active-directory"
   documentationCenter=""
   authors="dhanyahk"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="03/07/2016"
   ms.author="dhanyahk"/>

# <a name="azure-active-directory-report-latencies"></a>Opóźnienia raportu Azure Active Directory

*Niniejsza dokumentacja jest częścią [Azure Active Directory raportowania przewodnika](active-directory-reporting-guide.md).*

Raport                                                  | Minimalna  | Średnia    | Maksymalna
------------------------------------------------------- | -------- | ---------- | ----------
**Raporty dotyczące zabezpieczeń**                                    |          |            |
Nieprawidłowe działanie logowania                              | 2 godziny  | 4 godziny    | 8 godzin
Dodatki logowania z nieznanych źródeł                           | 2 godziny  | 4 godziny    | 8 godzin
Dodatki logowania po wielu błędów                        | 2 godziny  | 4 godziny    | 8 godzin
Dodatki logowania z wielu obszarów geograficznych                      | 2 godziny  | 4 godziny    | 8 godzin
Dodatki logowania z adresów IP przy użyciu podejrzane działania     | 2 godziny  | 4 godziny    | 8 godzin
Dodatki znak z prawdopodobnie zakażonych urządzeń                 | 2 godziny  | 4 godziny    | 8 godzin
Użytkownicy z działaniem anomalous logowania                   | 2 godziny  | 4 godziny    | 8 godzin
Użytkownicy z poświadczeniami przecieku                           | 2 godziny  | 4 godziny    | 8 godzin
**Raporty aplikacji**                                 |          |            |
Konto inicjowania obsługi administracyjnej aktywności                           | 2 godziny  | 4 godziny    | 8 godzin
Konto błędy inicjowania obsługi administracyjnej                             | 2 godziny  | 4 godziny    | 8 godzin
Użycie aplikacji                                       | 2 godziny  | 4 godziny    | 8 godzin
Stan najazd hasła                                | 2 godziny  | 4 godziny    | 8 godzin
**Inspekcji i raporty aktywności**                            |          |            |
Raport inspekcji                                            | 1 minuta | 15 minut | 30 minut
Działania (Azure AD) do resetowania hasła                      | 2 godziny  | 4 godziny    | 8 godzin
Działania (Menedżer tożsamości) do resetowania hasła              | 2 godziny  | 4 godziny    | 8 godzin
Działanie rejestracji (Azure AD) do resetowania hasła         | 2 godziny  | 4 godziny    | 8 godzin
Działanie rejestracji (Menedżer tożsamości) do resetowania hasła | 2 godziny  | 4 godziny    | 8 godzin
Wykonanie grupy (Azure AD) tego elementu usługi                 | 2 godziny  | 4 godziny    | 8 godzin
Wykonanie grupy (Menedżer tożsamości) tego elementu usługi         | 2 godziny  | 4 godziny    | 8 godzin
**Raporty usługi RMS**                                         |          |            |
Najbardziej aktywni użytkownicy usługi RMS                                   | 2 godziny  | 4 godziny    | 8 godzin
Użycie usługi RMS                                               | 2 godziny  | 4 godziny    | 8 godzin
Użycie urządzenia RMS                                        | 2 godziny  | 4 godziny    | 8 godzin
Użycie aplikacji usługi RMS włączone                           | 2 godziny  | 4 godziny    | 8 godzin
**Prywatne Podgląd raportów**                             |          |            |
Wszystkie operacje logowania użytkownika                               | 2 godziny  | 4 godziny    | 8 godzin
