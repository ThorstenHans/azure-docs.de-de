---
title: Autorisieren des Zugriffs auf Azure App Configuration mittels Azure Active Directory
description: Aktivieren von RBAC zur Autorisierung des Zugriffs auf Ihre Azure App Configuration-Instanz
author: lisaguthrie
ms.author: lcozzens
ms.date: 02/13/2020
ms.topic: conceptual
ms.service: azure-app-configuration
ms.openlocfilehash: d417fa4d6b444f4932338059e2ad499c12d6273e
ms.sourcegitcommit: f353fe5acd9698aa31631f38dd32790d889b4dbb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/29/2020
ms.locfileid: "87371837"
---
# <a name="authorize-access-to-azure-app-configuration-using-azure-active-directory"></a>Autorisieren des Zugriffs auf Azure App Configuration mittels Azure Active Directory
Neben der Verwendung von Hash-based Message Authentication Code (HMAC) unterstützt Azure App Configuration die Verwendung von Azure Active Directory (Azure AD) zum Autorisieren von Anforderungen an App Configuration-Instanzen.  Azure AD gestattet Ihnen die Verwendung der rollenbasierten Zugriffssteuerung (Role-Based Access Control, RBAC), um einem Sicherheitsprinzipal Berechtigungen zu erteilen.  Ein Sicherheitsprinzipal kann ein Benutzer, eine [verwaltete Identität](../active-directory/managed-identities-azure-resources/overview.md) oder ein [Anwendungsdienstprinzipal](../active-directory/develop/app-objects-and-service-principals.md) sein.  Weitere Informationen zu Rollen und Rollenzuweisungen finden Sie unter [Grundlegendes zu verschiedenen Rollen](../role-based-access-control/overview.md).

## <a name="overview"></a>Übersicht
Von einem Sicherheitsprinzipal gesendete Anforderungen für den Zugriff auf eine App Configuration-Ressource müssen autorisiert werden. Mit Azure AD ist der Zugriff auf eine Ressource ein zweistufiger Prozess:
1. Die Identität des Sicherheitsprinzipals wird authentifiziert, und ein OAuth 2.0-Token wird zurückgegeben.  Der Ressourcenname zum Anfordern eines Tokens lautet `https://login.microsoftonline.com/{tenantID}`, wobei `{tenantID}` der Azure Active Directory-Mandanten-ID entspricht, zu der der Dienstprinzipal gehört.
2. Das Token wird als Teil einer Anforderung an den App Configuration-Dienst übergeben, um den Zugriff auf die angegebene Ressource zu autorisieren.

Für den Authentifizierungsschritt ist es erforderlich, dass eine Anwendungsanforderung zur Laufzeit ein OAuth 2.0-Zugriffstoken enthält.  Wenn eine Anwendung in einer Azure-Entität, z. B. einer Azure Functions-App, einer Azure-Web-App oder einem virtuellen Azure-Computer, ausgeführt wird, kann der Zugriff auf die Ressourcen über eine verwaltete Identität erfolgen.  Informationen zum Authentifizieren von Anforderungen, die von einer verwalteten Identität an Azure App Configuration übermittelt werden, finden Sie unter [Authentifizieren des Zugriffs auf Azure App Configuration-Ressourcen mit Azure Active Directory und verwalteten Identitäten für Azure-Ressourcen](howto-integrate-azure-managed-service-identity.md).

Für den Autorisierungsschritt ist es erforderlich, dem Sicherheitsprinzipal mindestens eine RBAC-Rolle zuzuweisen. Azure App Configuration stellt RBAC-Rollen bereit, die Berechtigungssätze für App Configuration-Ressourcen umfassen. Die einem Sicherheitsprinzipal zugewiesenen Rollen bestimmen die dem Prinzipal bereitgestellten Berechtigungen. Weitere Informationen zu RBAC-Rollen finden Sie unter [Integrierte Azure-Rollen für Azure App Configuration](#azure-built-in-roles-for-azure-app-configuration). 

## <a name="assign-rbac-roles-for-access-rights"></a>Zuweisen von RBAC-Rollen für Zugriffsrechte
Azure Active Directory (Azure AD) autorisiert Rechte für den Zugriff auf abgesicherte Ressourcen über die [rollenbasierte Zugriffssteuerung (RBAC)](../role-based-access-control/overview.md).

Wenn einem Azure AD-Sicherheitsprinzipal eine RBAC-Rolle zugewiesen wird, gewährt Azure diesem Sicherheitsprinzipal Zugriff auf diese Ressourcen. Der Umfang des Zugriffs ist auf die App Configuration-Ressource beschränkt. Ein Azure AD-Sicherheitsprinzipal kann ein Benutzer, ein Anwendungsdienstprinzipal oder eine [verwaltete Identität für Azure-Ressourcen](../active-directory/managed-identities-azure-resources/overview.md) sein.

## <a name="azure-built-in-roles-for-azure-app-configuration"></a>Integrierte Azure-Rollen für Azure App Configuration
Azure stellt die folgenden integrierten Azure Rollen zum Autorisieren des Zugriffs auf App Configuration-Daten mittels Azure AD und OAuth bereit:

- **App Configuration-Datenbesitzer:** Verwenden Sie diese Rolle, um Lese-, Schreib- und Löschzugriff auf App Configuration-Daten zu gewähren. Dadurch wird kein Zugriff auf die App Configuration-Ressource gewährt.
- **App Configuration-Datenleser:** Verwenden Sie diese Rolle, um Lesezugriff auf App Configuration-Daten zu gewähren. Dadurch wird kein Zugriff auf die App Configuration-Ressource gewährt.
- **Mitwirkender**: Verwenden Sie diese Rolle, um die App Configuration-Ressource zu verwalten. Obwohl auf die App Configuration-Daten mithilfe von Zugriffsschlüsseln zugegriffen werden kann, wird mit dieser Rolle kein direkter Zugriff auf die Daten unter Verwendung von Azure AD gewährt.
- **Leser:** Verwenden Sie diese Rolle, um Lesezugriff auf die App Configuration-Ressource zu gewähren. Dadurch wird weder Zugriff auf die Zugriffsschlüssel der Ressource noch auf die in App Configuration gespeicherten Daten gewährt.

> [!NOTE]
> Derzeit unterstützen Azure-Portal und CLI nur die HMAC-Authentifizierung, um auf App Configuration-Daten zuzugreifen. Azure AD-Authentifizierung wird nicht unterstützt. Daher benötigen Benutzer von Azure-Portal und CLI die Rolle *Mitwirkender*, um die Zugriffsschlüssel der App Configuration-Ressource abzurufen. Das Gewähren der Rollen *App Configuration-Datenleser* oder *App Configuration-Datenbesitzer* hat keine Auswirkung auf den Zugriff über Portal und CLI.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zur Verwendung [verwalteter Identitäten](howto-integrate-azure-managed-service-identity.md) zum Verwalten Ihres App Configuration-Diensts.
