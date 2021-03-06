---
title: Grenzwerte von Azure Analysis Services-Ressourcen und -Objekten | Microsoft-Dokumentation
description: Dieser Artikel beschreibt Ressourcen- und Objekteinschränkungen für einen Azure Analysis Services-Server.
author: minewiskan
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 05/19/2020
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: c2caab4e449f4299d00fff14b697887ec00f35e6
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "83697496"
---
# <a name="analysis-services-resource-and-object-limits"></a>Grenzwerte von Azure Analysis Services-Ressourcen und -Objekten

In diesem Artikel beschreibt Ressourcen- und Modellobjektgrenzwerte.

## <a name="tier-limits"></a>Tarifgrenzwerte

Informationen zu QPU- und Arbeitsspeicherlimits für die Tarife „Developer“, „Basic“ und „Standard“ finden Sie auf der [Seite mit der Azure Analysis Services-Preisübersicht](https://azure.microsoft.com/pricing/details/analysis-services/).

## <a name="object-limits"></a>Objektgrenzwerte

Hierbei handelt um theoretische Grenzwerte. Die Leistung wird bei geringeren Werten beeinträchtigt.

|Object|Maximale Größe/Anzahl|  
|------------|----------------------------|  
|Datenbank in einer Instanz|16.000|  
|Kombinierte Anzahl an Tabellen und Spalten in einer Datenbank|16.000|  
|Zeilen in einer Tabelle|Unbegrenzt<br /><br /> **Warnung:** Mit der Einschränkung, dass keine einzelne Spalte in der Tabelle mehr als 1.999.999.999.999.997 verschiedene Werte haben kann.|  
|Hierarchien in einer Tabelle|15.999|  
|Ebenen in einer Hierarchie|15.999|  
|Beziehungen|8\.000|  
|Schlüsselspalten in allen Tabellen|15.999|  
|Messungen in Tabellen|2^31-1 = 2.147483.647|  
|Von einer Abfrage zurückgegebene Zellen|2^31-1 = 2.147483.647|  
|Datensatzgröße in der Quellabfrage|64 K|  
|Länge des Objektnamens|512 Zeichen|  


