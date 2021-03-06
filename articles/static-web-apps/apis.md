---
title: API-Unterstützung in Azure Static Web Apps mit Azure Functions
description: Hier erfahren Sie, welche API-Features von Azure Static Web Apps unterstützt werden.
services: static-web-apps
author: craigshoemaker
ms.service: static-web-apps
ms.topic: conceptual
ms.date: 05/08/2020
ms.author: cshoe
ms.openlocfilehash: f5f40a615bc5faab6265f42d0728403e2735aa0f
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84791621"
---
# <a name="api-support-in-azure-static-web-apps-preview-with-azure-functions"></a>API-Unterstützung in Azure Static Web Apps (Vorschauversion) mit Azure Functions

Von Azure Static Web Apps werden serverlose API-Endpunkte über [Azure Functions](../azure-functions/functions-overview.md) bereitgestellt. Durch die Verwendung von Azure Functions werden APIs bedarfsgerecht dynamisch skaliert und bieten folgende Features:

- **Integrierte Sicherheit** mit direktem Zugriff auf Benutzerdaten für die [Authentifizierung und rollenbasierte Autorisierung](user-information.md)
- **Nahtloses Routing**, durch das die _API-Route_ ganz ohne benutzerdefinierte CORS-Regeln auf sichere Weise für die Web-App verfügbar gemacht wird
- **Azure Functions v3** (kompatibel mit Node.js 12)
- **HTTP-Trigger** und Ausgabebindungen

## <a name="configuration"></a>Konfiguration

API-Endpunkte sind für die Web-App über die _API-Route_ verfügbar. Diese Route ist zwar vorgegeben, Sie können jedoch entscheiden, in welchem Ordner Sie die zugeordnete Azure Functions-App platzieren möchten. Dieser Ort kann durch [Bearbeiten der YAML-Datei des Workflows](github-actions-workflow.md#build-and-deploy) geändert werden, die sich im Ordner _.github/workflows_ Ihres Repositorys befindet.

## <a name="constraints"></a>Einschränkungen

Von Azure Static Web Apps wird eine API über Azure Functions bereitgestellt. Die Funktionen von Azure Functions sind auf eine bestimmte Gruppe von Features konzentriert, mit denen Sie eine API für eine Web-App erstellen können und die es der Web-App ermöglichen, eine sichere Verbindung mit der API herzustellen. Für diese Features gelten gewisse Einschränkungen:

- Das API-Routenpräfix muss _api_ sein.
- Die API-Funktions-App muss in JavaScript vorhanden sein.
- Routenregeln für API-Funktionen unterstützen nur [Umleitungen](routes.md#redirects) und das [Sichern von Routen mit Rollen](routes.md#securing-routes-with-roles).
- Trigger und Bindungen sind auf [HTTP](../azure-functions/functions-bindings-http-webhook.md) beschränkt.
  - Alle anderen [Trigger und Bindungen von Azure Functions](../azure-functions/functions-triggers-bindings.md#supported-bindings) sind eingeschränkt (mit Ausnahme von Ausgabebindungen).
- Protokolle sind nur verfügbar, wenn Sie Ihrer Functions-App [Application Insights](../azure-functions/functions-monitoring.md) hinzufügen.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Hinzufügen einer API zu Azure Static Web Apps (Vorschauversion) mit Azure Functions](add-api.md)
