---
title: Verwenden des Azure-Portals zum Aufrufen direkter Methoden
description: Dieser Artikel bietet eine Übersicht über die Verwendung des Azure-Portals zum Aufrufen direkter Methoden.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.service: media-services
ms.subservice: ''
ms.workload: ''
ms.topic: how-to
ms.custom: ''
ms.date: 07/24/2020
ms.author: inhenkel
ms.openlocfilehash: 763dd82c8263a5e180468f9fbd7f86526295a80d
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/28/2020
ms.locfileid: "87279286"
---
# <a name="how-to-use-azure-portal-to-invoke-direct-methods"></a>Verwenden des Azure-Portals zum Aufrufen direkter Methoden

IoT Hub gibt Ihnen die Möglichkeit, [direkte Methoden](/azure/iot-hub/iot-hub-devguide-direct-methods#method-invocation-for-iot-edge-modules) auf Edgegeräten von der Cloud aus aufzurufen. Das Modul „Live Video Analytics in IoT Edge (LVA)“ stellt mehrere [direkte Methoden](/azure/media-services/live-video-analytics-edge/direct-methods) vor, die zur Definition, Bereitstellung und Instanziierung verschiedener Workflows für die Analyse von Livevideos verwendet werden können.

In diesem Artikel erfahren Sie, wie Sie direkte Methodenaufrufe in Live Video Analytics für ein IoT Edge-Modul über das Azure-Portal aufrufen können.

## <a name="prerequisites"></a>Voraussetzungen

* Das Modul „Live Video Analytics in IoT Edge“ wird auf Ihrem Edgegerät ausgeführt, wobei Sie entweder die unter [Schnellstart: Live Video Analytics in IoT Edge](/azure/media-services/live-video-analytics-edge/get-started-detect-motion-emit-events-quickstart) beschriebenen Methoden oder das [-Portal verwenden.](/azure/media-services/live-video-analytics-edge/deploy-iot-edge-device)

* Sie verstehen [Live Video Analytics](/azure/media-services/live-video-analytics-edge/overview) und das [Mediengraphkonzept](/azure/media-services/live-video-analytics-edge/media-graph-concept).

## <a name="invoking-direct-methods-via-azure-portal"></a>Aufrufen von direkten Methoden über das Azure-Portal

Jede der [direkten Methoden](/azure/media-services/live-video-analytics-edge/direct-methods), die über das LVA-Modul verfügbar gemacht werden, kann über das Azure-Portal aufgerufen werden. Die folgenden Schritte enthalten die Details für eine direkte Methode. Sie können andere direkte Methoden mit ähnlichen Schritten aufrufen. Für jede direkte Methode ist jedoch ein bestimmter JSON-Text erforderlich.

Verwenden Sie den `GraphTopologyList`-Methodenaufruf, um eine Liste aller Graphtopologien abzurufen, die derzeit im Modul „Live Video Analytics in IoT Edge“ bereitgestellt werden. Verwenden Sie die folgenden Schritte, um diese direkte Methode aufzurufen:

1. Anmelden im Azure-Portal
1. Finden Sie die relevante Ressourcengruppe auf der Startseite Ihres Portals, um Ihren IoT Hub zu finden, oder wenn Sie Ihren IoT Hub kennen, wählen Sie ihn aus.
    ![Ressourcengruppe auf der Startseite des Portals](media/use-azure-portal-to-invoke-directs-methods/portal-rg-home.png)
1. Wählen Sie auf der IoT Hub-Seite unter „Automatische Geräteverwaltung“ die Option „IoT Edge“ aus, um die verschiedenen Geräte-IDs aufzulisten. Wählen Sie die entsprechende Geräte-ID aus, um die auf dem Gerät ausgeführten Module aufzulisten.
    ![IoT Hub-Seite](media/use-azure-portal-to-invoke-directs-methods/iot-hub-page.png)
1. Wählen Sie das Modul „Live Video Analytics in IoT Edge“ aus, um dessen Konfigurationsseite aufzurufen.<br><br>
    ![Wählen Sie das Modul „Live Video Analytics in IoT Edge“ aus, um dessen Konfigurationsseite aufzurufen.](media/use-azure-portal-to-invoke-directs-methods/modules.png)
1. Wählen Sie die Menüoption „Direkte Methode“ aus. <br><br>
    ![Klicken Sie auf die Menüoption „Direkte Methode“.](media/use-azure-portal-to-invoke-directs-methods/module-details.png)
    > [!NOTE]
    > Sie müssen in den Abschnitten für die Verbindungszeichenfolge möglicherweise einen Wert hinzufügen, wie Sie auf der aktuellen Seite sehen können. Sie brauchen im Abschnitt „Einstellungsname“ nichts auszublenden oder zu ändern. Es ist in Ordnung, diese Informationen öffentlich zu belassen.

1. Geben Sie anschließend im Feld **Methodenname** den Wert *GraphTopologyList* ein.
1. Kopieren Sie den folgenden JSON-Code in das Feld für die **Nutzlast**.
    ```json
    {
    "@apiVersion":
    }
    ```
1. Wählen Sie die Schaltfläche **Methode aufrufen** oben auf der Seite aus.<br><br>
    ![Schaltfläche „Methode aufrufen“](media/use-azure-portal-to-invoke-directs-methods/direct-method.png)
1. Im **Ergebnisbereich** sollte eine 200-Statusmeldung angezeigt werden.<br><br>
    ![Verbindungstimeout](media/use-azure-portal-to-invoke-directs-methods/connection-timeout.png)

## <a name="responses"></a>Antworten

| Bedingung             | Statuscode | Detaillierter Fehlercode |
|-----------------------|-------------|---------------------|
| Erfolg               | 200         | –                 |
| Allgemeine Benutzerfehler   | 400er Bereich   |                     |
| Allgemeine Serverfehler | 500er Bereich   |                     |

## <a name="next-steps"></a>Nächste Schritte

Weitere direkte Methoden finden Sie auf der Seite [Direkte Methoden](/azure/media-services/live-video-analytics-edge/direct-methods).

> [!NOTE]
> Eine Graphinstanz instanziiert eine bestimmte Topologie, daher müssen Sie sicherstellen, dass Sie die richtige Topologie festgelegt haben, bevor Sie eine Graphinstanz erstellen.

[Schnellstart: Erkennen von Bewegung und Ausgeben von Ereignissen](/azure/media-services/live-video-analytics-edge/get-started-detect-motion-emit-events-quickstart) ist eine geeignete Referenz, um die genaue Abfolge direkter Methodenaufrufe zu verstehen.
