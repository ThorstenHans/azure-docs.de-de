---
title: Unterstützung von Remotearbeit in Azure Firewall
description: In diesem Artikel erfahren Sie, wie Azure Firewall die Anforderungen Ihrer Remotemitarbeiter unterstützen kann.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: conceptual
ms.date: 05/04/2020
ms.author: victorh
ms.openlocfilehash: da5100fafc98ae38809c93e9b3db5ef41c58766a
ms.sourcegitcommit: 602e6db62069d568a91981a1117244ffd757f1c2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2020
ms.locfileid: "82863415"
---
# <a name="azure-firewall-remote-work-support"></a>Unterstützung von Remotearbeit in Azure Firewall

Azure Firewall ist ein verwalteter, cloudbasierter Netzwerksicherheitsdienst zum Schutz Ihrer Azure Virtual Network-Ressourcen. Es ist eine vollständig zustandsbehaftete Firewall-as-a-Service mit integrierter Hochverfügbarkeit und uneingeschränkter Cloudskalierbarkeit.

## <a name="virtual-desktop-infrastructure-vdi-deployment-support"></a>Unterstützung von VDI-Bereitstellungen (Virtual Desktop Infrastructure)

Die Anordnung des Arbeitens im Homeoffice verlangt von vielen IT-Organisationen, sich mit grundlegenden Veränderungen in den Bereichen Kapazität, Netzwerk, Sicherheit und Governance auseinanderzusetzen. Beschäftigte sind nicht durch die mehrstufigen Sicherheitsmaßnahmen in Verbindung mit lokalen Diensten geschützt, wenn sie von zu Hause aus arbeiten. VDI-Bereitstellungen (Virtual Desktop Infrastructure) in Azure können Organisationen dabei unterstützen, schnell auf dieses sich ändernde Umfeld zu reagieren. Sie benötigen jedoch eine Möglichkeit, den ein- und ausgehenden Internetdatenverkehr dieser VDI-Bereitstellungen zu schützen. Sie können[DNAT-Regeln](rule-processing.md) für Azure Firewall zusammen mit den auf [Threat Intelligence](threat-intel.md) basierenden Filterfunktionen verwenden, um Ihre VDI-Bereitstellungen zu schützen.

## <a name="azure-windows-virtual-desktop-support"></a>Unterstützung für Windows Virtual Desktop in Azure

Bei Windows Virtual Desktop handelt es sich um einen umfassenden in Azure ausgeführten Dienst zur Desktop- und App-Virtualisierung. Es ist die einzige virtuelle Desktopinfrastruktur (VDI), die eine vereinfachte Verwaltung, mehrere Windows 10-Sitzungen, Optimierungen für Office 365 ProPlus und Unterstützung für Remotedesktopdienste-Umgebungen bietet. Sie können Ihre Windows-Desktops und -Apps in wenigen Minuten in Azure bereitstellen und skalieren und profitieren von integrierten Funktionen für Sicherheit und Compliance. Für Windows Virtual Desktop ist es nicht erforderlich, dass Sie den eingehenden Zugriff auf Ihr virtuelles Netzwerk öffnen. Sie müssen jedoch einen Satz ausgehender Netzwerkverbindungen für die virtuellen Windows Virtual Desktop-Computer zulassen, die in Ihrem virtuellen Netzwerk ausgeführt werden. Weitere Informationen finden Sie unter [Verwenden von Azure Firewall zum Schutz von Windows Virtual Desktop-Bereitstellungen](protect-windows-virtual-desktop.md).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu [Windows Virtual Desktop](https://docs.microsoft.com/azure/virtual-desktop/).