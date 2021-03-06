---
title: MFA oder 2FA und Privileged Identity Management – Azure AD | Microsoft-Dokumentation
description: Erfahren Sie, wie Azure AD Privileged Identity Management (PIM) die Multi-Factor Authentication (MFA) überprüft.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: how-to
ms.workload: identity
ms.subservice: pim
ms.date: 11/08/2019
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: bf1113f7b2f396deed849fa46108537f290b53a1
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84742095"
---
# <a name="multi-factor-authentication-and-privileged-identity-management"></a>Multi-Factor Authentication und Privileged Identity Management

Es wird empfohlen, von allen Administratoren Multi-Factor Authentication (MFA) zu verlangen. Auf diese Weise sinkt das Risiko eines Angriffs aufgrund eines offengelegten Kennworts.

Sie können festlegen, dass Benutzer beim Anmelden eine mehrstufige Authentifizierung durchlaufen müssen. Sie können auch festlegen, dass Benutzer eine mehrstufige Authentifizierung durchlaufen müssen, wenn sie eine Rolle in Azure Active Directory Privileged Identity Management (Azure AD PIM) aktivieren. Auf diese Weise wird ein Benutzer, der bei der Anmeldung keine mehrstufige Authentifizierung durchlaufen hat, von Privileged Identity Management aufgefordert, dies zu tun.

> [!IMPORTANT]
> Zurzeit funktioniert Azure Multi-Factor Authentication nur mit Geschäfts-, Schul- oder Unikonten, aber nicht mit persönlichen Microsoft-Konten (normalerweise ein persönliches Konto, das zum Anmelden bei Microsoft-Diensten wie Skype, Xbox oder Outlook.com verwendet wird). Aus diesem Grund kann kein Benutzer mit einem persönlichen Konto ein berechtigter Administrator sein, da zum Aktivieren der Rollen mehrstufige Authentifizierung verwendet werden muss. Wenn Benutzer weiterhin Workloads mit einem Microsoft-Konto verwalten müssen, stufen Sie sie vorerst zu permanenten Administratoren hoch.

## <a name="how-pim-validates-mfa"></a>Überprüfen der MFA durch PIM

Wenn ein Benutzer eine Rolle aktiviert, stehen zwei Optionen zum Überprüfen der mehrstufigen Authentifizierung zur Verfügung.

Die einfachste Methode besteht darin, sich bei Benutzern, die eine privilegierte Rolle aktivieren, auf Azure Multi-Factor Authentication zu verlassen. Überprüfen Sie dazu ggf. zunächst, ob diese Benutzer lizenziert sind und sich für Azure Multi-Factor Authentication registriert haben. Weitere Informationen zum Bereitstellen von Azure Multi-Factor Authentication finden Sie unter [Erste Schritte mit Azure Multi-Factor Authentication in der Cloud](../authentication/howto-mfa-getstarted.md). Es ist zwar nicht erforderlich, wird jedoch empfohlen, Azure AD so zu konfigurieren, dass mehrstufige Authentifizierung für diese Benutzer bei ihrer Anmeldung erzwungen wird. Der Grund hierfür ist, dass Tests der mehrstufigen Authentifizierung von Privileged Identity Management selbst vorgenommen werden.

Wenn sich Benutzer lokal authentifizieren, können Sie alternativ festlegen, dass der Identitätsanbieter für mehrstufige Authentifizierung verantwortlich ist. Wenn Sie beispielsweise AD-Verbunddienste so konfiguriert haben, dass vor dem Zugriff auf Azure AD eine smartcardbasierte Authentifizierung erforderlich ist, lesen Sie die Anweisungen zum Konfigurieren von AD FS für das Senden von Ansprüchen an Azure AD unter [Sichern von Cloudressourcen mit Azure Multi-Factor Authentication und AD FS](../authentication/howto-mfa-adfs.md) . Wenn ein Benutzer versucht, eine Rolle zu aktivieren, akzeptiert Privileged Identity Management, dass mehrstufige Authentifizierung für den Benutzer bereits überprüft wurde, sobald die Anwendung die entsprechenden Ansprüche empfängt.

## <a name="next-steps"></a>Nächste Schritte

- [Konfigurieren von Einstellungen für Azure AD-Rollen in PIM](pim-how-to-change-default-settings.md)
- [Konfigurieren von Einstellungen für Azure-Ressourcenrollen in Privileged Identity Management](pim-resource-roles-configure-role-settings.md)
