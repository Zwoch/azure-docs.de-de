---
title: 'Azure AD Connect: Nahtloses einmaliges Anmelden | Microsoft-Dokumentation'
description: In diesem Thema wird die nahtlose einmalige Anmeldung von Azure Active Directory (Azure AD) beschrieben und wie Sie damit eine echte einmalige Anmeldung für Desktopbenutzer im Unternehmen innerhalb Ihres Unternehmensnetzwerks bereitstellen können.
services: active-directory
keywords: Was ist Azure AD Connect, Active Directory installieren, erforderliche Komponenten für Azure AD, SSO, Single Sign-On, einmaliges Anmelden
documentationcenter: ''
author: swkrish
manager: mtillman
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/22/2018
ms.author: billmath
ms.openlocfilehash: 6a106ba59f09ffc20f8461c5267ae06695df4cd0
ms.sourcegitcommit: c3d53d8901622f93efcd13a31863161019325216
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2018
---
# <a name="azure-active-directory-seamless-single-sign-on"></a>Nahtlose einmalige Anmeldung mit Azure Active Directory

## <a name="what-is-azure-active-directory-seamless-single-sign-on"></a>Was ist die nahtlose einmalige Anmeldung mit Azure Active Directory?

Die nahtlose einmalige Anmeldung mit Azure Active Directory (nahtlose SSO mit Azure AD) meldet Benutzer automatisch auf ihren Unternehmensgeräten an, die mit dem Unternehmensnetzwerk verbunden sind. Wenn diese Funktion aktiviert ist, müssen Benutzer zur Anmeldung bei Azure AD nicht ihr Kennwort und in der Regel nicht einmal ihren Benutzernamen eingeben. Diese Funktion ermöglicht Ihren Benutzern einen einfachen Zugriff auf Ihre cloudbasierten Anwendungen, ohne dass zusätzliche lokale Komponenten erforderlich sind.

>[!VIDEO https://www.youtube.com/embed/PyeAC85Gm7w]

Die nahtlose SSO kann mit den Anmeldemethoden [Kennworthashsynchronisierung](active-directory-aadconnectsync-implement-password-hash-synchronization.md) oder [Passthrough-Authentifizierung](active-directory-aadconnect-pass-through-authentication.md) kombiniert werden.

![Nahtloses einmaliges Anmelden](./media/active-directory-aadconnect-sso/sso1.png)

>[!IMPORTANT]
>Die nahtlose einmalige Anmeldung ist _nicht_ auf Active Directory-Verbunddienste (AD FS) anwendbar.

## <a name="key-benefits"></a>Hauptvorteile

- *Die Benutzeroberfläche*
  - Benutzer werden automatisch sowohl bei lokalen als auch bei cloudbasierten Anwendungen angemeldet.
  - Benutzer müssen nicht ständig ihr Kennwort eingeben.
- *Einfache Bereitstellung und Verwaltung*
  - Keine zusätzlichen lokalen Komponenten erforderlich.
  - Funktioniert mit jedem Verfahren für die Cloudauthentifizierung: [Kennworthashsynchronisierung](active-directory-aadconnectsync-implement-password-hash-synchronization.md) oder [Passthrough-Authentifizierung](active-directory-aadconnect-pass-through-authentication.md).
  - Kann mithilfe einer Gruppenrichtlinie für einige oder alle Benutzer eingeführt werden.
  - Registrieren Sie Geräte ohne Windows 10 bei Azure AD, ohne dass eine AD FS-Infrastruktur erforderlich ist. Für diese Funktion benötigen Sie Version 2.1 oder höher des [Clients für die Arbeitsplatzeinbindung](https://www.microsoft.com/download/details.aspx?id=53554).

## <a name="feature-highlights"></a>Wichtige Features

- Beim Anmeldebenutzernamen kann es sich entweder um den lokalen Standard-Benutzernamen (`userPrincipalName`) handeln oder um ein anderes Attribut (`Alternate ID`), das in Azure AD Connect konfiguriert ist. Beide Anwendungsfälle funktionieren, weil die nahtloses SSO den `securityIdentifier`-Anspruch im Kerberos-Ticket verwendet, um die entsprechenden Benutzerobjekte in Azure AD zu suchen.
- Die nahtlose SSO ist eine opportunistische Funktion. Tritt aus irgendeinem Grund ein Fehler auf, wird die reguläre Benutzeranmeldung durchgeführt, d.h. Benutzer müssen ihr Kennwort auf der Anmeldeseite eingeben.
- Leitet eine Anwendung (z.B. https://myapps.microsoft.com/contoso.com) die Parameter `domain_hint` (OpenID Connect) oder `whr` (SAML) – die Ihren Mandanten identifizieren, oder den Parameter `login_hint`, der den Benutzer identifiziert, in der Azure AD-Anmeldeanforderung weiter, werden Benutzer automatisch ohne Eingabe von Benutzername oder Kennwort angemeldet.
- Benutzer genießen auch dann eine Anmeldung ohne Benutzereingriff, wenn eine Anwendung (z.B. https://contoso.sharepoint.com) Anmeldeanforderungen an Mandantenendpunkte von Azure AD – d.h. https://login.microsoftonline.com/contoso.com/<..> oder https://login.microsoftonline.com/<tenant_ID>/<..> – anstelle des allgemeinem Azure AD-Endpunkts (https://login.microsoftonline.com/common/<...>) sendet.
- Die Abmeldung wird unterstützt. Dadurch können Benutzer ein anderes Azure AD-Konto für die Anmeldung auswählen, anstatt mit der nahtlosen einmaligen Anmeldung automatisch angemeldet zu werden.
- Office 365-Clients (ab 16.0.8730.xxxx) werden mit einem nicht interaktiven Ablauf unterstützt.
- Kann über Azure AD Connect aktiviert werden
- Es handelt sich um ein kostenloses Feature, sodass Sie für dessen Verwendung keine kostenpflichtigen Editionen von Azure AD benötigen.
- Ist auf webbrowserbasierten Clients und Office-Clients möglich, die eine [moderne Authentifizierung](https://aka.ms/modernauthga) auf Plattformen und Browsern unterstützen, die eine Kerberos-Authentifizierung ausführen können:

| Betriebssystem/Browser |Internet Explorer|Edge|Google Chrome|Mozilla Firefox|Safari|
| --- | --- |--- | --- | --- | -- 
|Windows 10|Ja|Nein|Ja|Ja\*|N/V
|Windows 8.1|Ja|N/V|Ja|Ja\*|N/V
|Windows 8|Ja|N/V|Ja|Ja\*|N/V
|Windows 7|Ja|N/V|Ja|Ja\*|N/V
|Mac OS X|N/V|N/V|Ja\*|Ja\*|Ja\*

\*Erfordert [zusätzliche Konfigurationsschritte](active-directory-aadconnect-sso-quick-start.md#browser-considerations)

>[!NOTE]
>Bei Windows 10 wird empfohlen, [Azure AD Join](../active-directory-azureadjoin-overview.md) zu verwenden, um eine optimale Funktionsweise der nahtlosen SSO mit Azure AD sicherzustellen.

## <a name="next-steps"></a>Nächste Schritte

- [**Schnellstart**](active-directory-aadconnect-sso-quick-start.md): Einrichten und Ausführen der nahtlosen SSO mit Azure AD
- [**Ausführliche technische Informationen**](active-directory-aadconnect-sso-how-it-works.md): Informationen zur Funktionsweise dieser Funktion
- [**Häufig gestellte Fragen**](active-directory-aadconnect-sso-faq.md) – Antworten auf häufig gestellte Fragen
- [**Problembehandlung**](active-directory-aadconnect-troubleshoot-sso.md) – Beheben von häufig auftretenden Problemen mit diesem Feature
- [**UserVoice:**](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) Verfassen neuer Feature-Anforderungen
