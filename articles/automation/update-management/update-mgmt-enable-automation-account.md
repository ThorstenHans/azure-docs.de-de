---
title: Aktivieren der Azure Automation-Updateverwaltung über ein Automation-Konto
description: In diesem Artikel erfahren Sie, wie Sie die Updateverwaltung über ein Automation-Konto aktivieren.
services: automation
ms.date: 07/28/2020
ms.topic: conceptual
ms.custom: mvc
ms.openlocfilehash: 930861c61843c5963c83d8fa6dc1efdce20853f4
ms.sourcegitcommit: cee72954f4467096b01ba287d30074751bcb7ff4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2020
ms.locfileid: "87449691"
---
# <a name="enable-update-management-from-an-automation-account"></a>Aktivieren der Updateverwaltung über ein Automation-Konto

In diesem Artikel wird beschrieben, wie Sie mit Ihrem Automation-Konto das Feature [Updateverwaltung](update-mgmt-overview.md) für VMs in Ihrer Umgebung aktivieren können. Sie müssen eine vorhandene VM über die Updateverwaltung aktivieren, um Azure-VMs im großen Stil zu aktivieren.

> [!NOTE]
> Wenn Sie die Updateverwaltung aktivieren, werden nur bestimmte Regionen zum Verknüpfen mit einem Log Analytics-Arbeitsbereich und einem Automation-Konto unterstützt. Eine Liste der unterstützten Zuordnungspaare finden Sie unter [Regionszuordnung für Automation-Konto und Log Analytics-Arbeitsbereich](../how-to/region-mappings.md).

## <a name="prerequisites"></a>Voraussetzungen

* Azure-Abonnement. Wenn Sie noch kein Abonnement haben, können Sie Ihre [MSDN-Abonnentenvorteile aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) oder sich für ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) registrieren.
* [Automation-Konto](../index.yml) zum Verwalten von Computern.
* Ein [virtueller Computer](../../virtual-machines/windows/quick-create-portal.md).

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

## <a name="enable-update-management"></a>Aktivieren der Updateverwaltung

1. Wählen Sie in Ihrem Automation-Konto unter **Updateverwaltung** die Option **Updateverwaltung** aus.

2. Wählen Sie den Log Analytics-Arbeitsbereich und das Automation-Konto aus, und wählen Sie dann **Aktivieren** aus, um die Updateverwaltung zu aktivieren. Die Einrichtung dauert bis zu 15 Minuten.

    ![Aktivieren der Updateverwaltung](media/update-mgmt-enable-automation-account/onboardsolutions2.png)

## <a name="enable-azure-vms"></a>Aktivieren von Azure-VMs

1. Wählen Sie in Ihrem Automation-Konto unter **Updateverwaltung** die Option **Updateverwaltung** aus.

2. Wählen Sie **+ Azure-VMs hinzufügen** aus, und wählen Sie mindestens einen virtuellen Computer aus der Liste aus. Virtuelle Computer, die nicht aktiviert werden können, sind abgeblendet und können nicht ausgewählt werden. Virtuelle Azure-Computer können sich unabhängig vom Standort Ihres Automation-Kontos in einer beliebigen Region befinden.

3. Wählen Sie **Aktivieren** aus, um die ausgewählten virtuellen Computer der gespeicherten Computergruppensuche für das Feature hinzuzufügen.

    ![Aktivieren von Azure-VMs](media/update-mgmt-enable-automation-account/enable-azure-vms.png)

## <a name="enable-non-azure-vms"></a>Aktivieren Azure-fremder virtueller Computer

Computer, die nicht in Azure enthalten sind, müssen manuell hinzugefügt werden.

1. Wählen Sie in Ihrem Automation-Konto unter **Updateverwaltung** die Option **Updateverwaltung** aus.

2. Wählen Sie **Nicht-Azure-Computer hinzufügen** aus. Diese Aktion öffnet ein neues Browserfenster mit [Anweisungen zum Installieren und Konfigurieren des Log Analytics-Agents für Windows](../../azure-monitor/platform/log-analytics-agent.md), sodass der Computer mit dem Erstellen von Berichten für die Updateverwaltung beginnen kann. Wenn Sie einen Computer aktivieren, der aktuell von Operations Manager verwaltet wird, ist kein neuer Agent erforderlich. Die Arbeitsbereichsinformationen werden der Konfiguration des Agents hinzugefügt.

## <a name="enable-machines-in-the-workspace"></a>Aktivieren von Computern im Arbeitsbereich

Manuell installierte Computer oder Computer, die bereits Berichte für Ihren Arbeitsbereich erstellen, müssen Azure Automation hinzugefügt werden, damit die Updateverwaltung aktiviert wird.

1. Wählen Sie in Ihrem Automation-Konto unter **Updateverwaltung** die Option **Updateverwaltung** aus.

2. Wählen Sie **Computer verwalten** aus. Die Schaltfläche **Computer verwalten** ist möglicherweise abgeblendet, wenn Sie zuvor die Option **Auf allen verfügbaren und zukünftigen Computern aktivieren** ausgewählt haben.

    ![Gespeicherte Suchvorgänge](media/update-mgmt-enable-automation-account/managemachines.png)

3. Wenn Sie die Updateverwaltung für alle verfügbaren Computer aktivieren möchten, wählen Sie auf der Seite „Computer verwalten“ die Option **Auf allen verfügbaren Computern aktivieren** aus. Diese Aktion deaktiviert das Steuerelement zum einzelnen Hinzufügen von Computern. Mit dieser Aufgabe werden alle Namen der Computer hinzugefügt, die Berichte für den Arbeitsbereich und die Computergruppe der gespeicherten Suchabfrage erstellen. Diese Aktion deaktiviert die Schaltfläche **Computer verwalten**.

4. Wenn Sie das Feature für alle verfügbaren und zukünftigen Computer aktivieren möchten, wählen Sie **Auf allen verfügbaren und zukünftigen Computern aktivieren** aus. Mit dieser Option werden die gespeicherten Suchen und die Bereichskonfigurationen aus dem Arbeitsbereich gelöscht, und das Feature wird für alle Azure-Computer und Azure-fremden Computer geöffnet, von denen Berichte an den Arbeitsbereich übermittelt werden. Bei Verwendung dieser Aktion wird die Schaltfläche **Computer verwalten** dauerhaft deaktiviert, da keine Bereichskonfiguration mehr vorhanden ist.

5. Die Bereichskonfigurationen können bei Bedarf durch Hinzufügen der anfänglichen gespeicherten Suchen wieder hinzugefügt werden. Weitere Informationen finden Sie unter [Einschränken des Bereitstellungsbereichs für die Updateverwaltung](update-mgmt-scope-configuration.md).

6. Wenn Sie die Funktion für einen oder mehrere Computer aktivieren möchten, wählen Sie **Auf ausgewählten Computern aktivieren** aus, und wählen Sie neben jedem Computer **Hinzufügen** aus. Dadurch werden die Namen der ausgewählten Computer der gespeicherten Computergruppensuchabfrage für das Feature hinzugefügt.

## <a name="next-steps"></a>Nächste Schritte

* Wie Sie die Updateverwaltung für VMs verwenden, erfahren Sie unter [Verwalten von Updates und Patches für Ihre VMs](update-mgmt-manage-updates-for-vm.md).

* Informationen zum Behandeln von Fehlern bei der Updateverwaltung finden Sie unter [Behandeln von Problemen mit der Updateverwaltung](../troubleshoot/update-management.md).