---
title: Herstellen einer Verbindung mit Power BI Professional
description: In diesem Tutorial werden die Schritte zum Herstellen einer Verbindung zwischen Power BI Desktop und SQL On-Demand (Vorschauversion) erläutert.
services: synapse-analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: tutorial
ms.subservice: sql
ms.date: 04/15/2020
ms.author: v-stazar
ms.reviewer: jrasnick, carlrab
ms.openlocfilehash: 497c6f669699f1e1ce879c0c7da2593256b97448
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/31/2020
ms.locfileid: "87501098"
---
# <a name="connect-to-synapse-sql-with-power-bi-professional"></a>Herstellen einer Verbindung mit Synapse SQL mithilfe von Power BI Professional

> [!div class="op_single_selector"]
>
> - [Azure Data Studio](get-started-azure-data-studio.md)
> - [Power BI](get-started-power-bi-professional.md)
> - [Visual Studio](../sql-data-warehouse/sql-data-warehouse-query-visual-studio.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)
> - [sqlcmd](../sql/get-started-connect-sqlcmd.md)
> - [SSMS](get-started-ssms.md)

In diesem Tutorial werden die Schritte zum Herstellen einer Verbindung zwischen Power BI Desktop und SQL On-Demand (Vorschauversion) erläutert.

## <a name="prerequisites"></a>Voraussetzungen

Tool zum Ausgeben von Abfragen:

- SQL-Client Ihrer Wahl:

  - Azure Data Studio
  - SQL Server Management Studio

- Power BI Desktop-Installation

Parameter:

| Parameter                                 | BESCHREIBUNG                                                   |
| ----------------------------------------- | ------------------------------------------------------------- |
| Dienstendpunktadresse von SQL On-Demand    | Wird als Servername verwendet.                                   |
| Dienstendpunktregion von SQL On-Demand     | Wird verwendet, um zu bestimmen, welcher Speicher in Beispielen genutzt wird. |
| Benutzername und Kennwort für den Endpunktzugriff | Wird für den Zugriff auf den Endpunkt verwendet.                               |
| Datenbank zum Erstellen von Ansichten     | Diese Datenbank wird als Ausgangspunkt in Beispielen verwendet.       |

## <a name="first-time-setup"></a>Erstmalige Einrichtung

Vor der Verwendung von Beispielen müssen zwei Schritte ausgeführt werden:

1. Erstellen einer Datenbank für Ihre Ansichten
2. Erstellen von Anmeldeinformationen, die von SQL On-Demand für den Zugriff auf Dateien im Speicher verwendet werden

### <a name="create-database"></a>Erstellen einer Datenbank

In diesem Artikel zu den ersten Schritten sollten Sie eine eigene Datenbank zu Demozwecken erstellen. Zum Erstellen von Ansichten ist eine Datenbank erforderlich. Diese Datenbank wird in einigen Beispielabfragen in dieser Dokumentation verwendet.

> [!NOTE]
> Datenbanken werden nur zum Anzeigen von Metadaten (nicht für tatsächliche Daten) verwendet.
>
> Notieren Sie sich den von Ihnen verwendeten Datenbanknamen. Sie benötigen ihn später.

```sql
DROP DATABASE IF EXISTS demo;
```

### <a name="create-credentials"></a>Erstellen von Anmeldeinformationen

Sie müssen Anmeldeinformationen erstellen, damit Sie Abfragen ausführen können. Diese Anmeldeinformationen werden von SQL On-Demand für den Zugriff auf Dateien im Speicher verwendet.

> [!NOTE]
> Sie müssen Anmeldeinformationen für den Speicherkontozugriff erstellen. SQL On-Demand kann zwar auf Speicher aus unterschiedlichen Regionen zugreifen, die Leistung ist jedoch besser, wenn sich der Speicher und der Azure Synapse-Arbeitsbereich in derselben Region befinden.

Führen Sie den folgenden **Codeausschnitt zum Erstellen von Anmeldeinformationen für Containern mit Volkszählungsdaten** aus:

```sql
IF EXISTS (SELECT * FROM sys.credentials WHERE name = 'https://azureopendatastorage.blob.core.windows.net/censusdatacontainer')
DROP CREDENTIAL [https://azureopendatastorage.blob.core.windows.net/censusdatacontainer];
GO

-- Create credentials for Census Data container which resides in a azure open data storage account
-- There is no secret. We are using public storage account which doesn't need secret
CREATE CREDENTIAL [https://azureopendatastorage.blob.core.windows.net/censusdatacontainer]  
WITH IDENTITY='SHARED ACCESS SIGNATURE',  
SECRET = '';
GO
```

## <a name="create-a-power-bi-desktop-report"></a>Erstellen eines Power BI Desktop-Berichts

Öffnen Sie die Power BI Desktop-Anwendung, und wählen Sie die Option **Daten abrufen** aus.

![Öffnen der Power BI Desktop-Anwendung und Auswählen von „Daten abrufen“](./media/get-started-power-bi-professional/step-0-open-powerbi.png)

### <a name="step-1---select-data-source"></a>Schritt 1: Auswählen einer Datenquelle

Wählen Sie im Menü die Option **Azure** und dann **Azure SQL-Datenbank** aus.
![Auswählen der Datenquelle](./media/get-started-power-bi-professional/step-1-select-data-source.png)

### <a name="step-2---select-database"></a>Schritt 2: Auswählen einer Datenbank

Geben Sie die URL für die Datenbank und den Namen der Datenbank an, in der sich die Ansicht befindet.
![Auswählen der Datenbank auf dem Endpunkt](./media/get-started-power-bi-professional/step-2-db.png)

## <a name="next-steps"></a>Nächste Schritte

Fahren Sie mit dem Artikel zum [Abfragen von Speicherdateien](get-started-azure-data-studio.md) fort, um zu erfahren, wie Sie mithilfe von Azure Data Studio eine Verbindung mit SQL On-Demand herstellen.
 