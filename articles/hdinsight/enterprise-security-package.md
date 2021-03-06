---
title: Enterprise-Sicherheitspaket für Azure HDInsight
description: Informieren Sie sich über die Komponenten und Versionen des Enterprise-Sicherheitspakets in Azure HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 05/08/2020
ms.openlocfilehash: 07210350826001bd3e0be6e04be211c9de43695a
ms.sourcegitcommit: 309a9d26f94ab775673fd4c9a0ffc6caa571f598
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/09/2020
ms.locfileid: "82997871"
---
# <a name="enterprise-security-package-for-azure-hdinsight"></a>Enterprise-Sicherheitspaket für Azure HDInsight

Enterprise Security für HDInsight ist ein optionales Paket, das Sie während der Clustererstellung zu Ihrem HDInsight-Cluster hinzufügen können. Das Enterprise Security Package unterstützt Folgendes:

* Integration in Active Directory für die Authentifizierung

    Bisher erstellten Sie HDInsight-Cluster ausschließlich mit einem lokalen Administratorbenutzer oder einem lokalen SSH-Benutzer. Der lokale Administratorbenutzer kann auf alle Dateien, Ordner, Tabellen und Spalten zugreifen.  Mit dem Enterprise-Sicherheitspaket können Sie die rollenbasierte Zugriffssteuerung aktivieren, indem Sie HDInsight mit Ihrem Active Directory-Konto integrieren. Dieses enthält eine lokale Active Directory-Instanz, Azure Active Directory Domain Services oder Active Directory auf einem virtuellen IaaS-Computer. Der Domänenadministrator des Clusters kann Benutzern die Berechtigung erteilen, ihren eigenen Firmen- oder Domänenbenutzernamen und ihr eigenes Kennwort zu verwenden.

    Weitere Informationen finden Sie unter

    * [Einführung in die Apache Hadoop-Sicherheit mit in die Domäne eingebundenen HDInsight-Clustern](./domain-joined/hdinsight-security-overview.md)

    * [Planen von in die Azure-Domäne eingebundenen Apache Hadoop-Clustern in HDInsight](./domain-joined/apache-domain-joined-architecture.md)

    * [Konfigurieren einer in die Domäne eingebundenen Sandboxumgebung](./domain-joined/apache-domain-joined-configure.md)

    * [Konfigurieren von in die Domäne eingebundenen HDInsight-Clustern mit Azure Active Directory Domain Services](./domain-joined/apache-domain-joined-configure-using-azure-adds.md)

* Autorisierung für Daten

  * Integration in Apache Ranger für die Autorisierung für Hive-, Spark SQL- und Yarn-Warteschlangen
  * Sie können die Zugriffssteuerung für Dateien und Ordner festlegen.

    Weitere Informationen finden Sie unter [Konfigurieren von Apache Hive-Richtlinien in HDInsight mit dem Enterprise-Sicherheitspaket](./domain-joined/apache-domain-joined-run-hive.md).

* Anzeigen der Überwachungsprotokolle zum Überwachen der Zugriffe und konfigurierten Richtlinien

## <a name="supported-cluster-types"></a>Unterstützte Clustertypen

Derzeit wird das Enterprise Security Package nur durch die folgenden Clustertypen unterstützt:

* Hadoop (nur HDInsight 3.6)
* Spark
* Kafka
* hbase
* Interactive Query

## <a name="support-for-azure-data-lake-storage"></a>Unterstützung für Azure Data Lake Storage

Das Enterprise-Sicherheitspaket unterstützt die Verwendung von Azure Data Lake Storage sowohl als Primärspeicher als auch als Zusatzspeicher.

## <a name="pricing-and-service-level-agreement-sla"></a>Preise und Vereinbarung zum Servicelevel (SLA)

Informationen zu Preisen und SLA für das Enterprise Security Package finden Sie unter [HDInsight-Preise](https://azure.microsoft.com/pricing/details/hdinsight/).

## <a name="next-steps"></a>Nächste Schritte

* [Einrichten von Clustern in HDInsight mit Apache Hadoop, Spark, Kafka usw.](hdinsight-hadoop-provision-linux-clusters.md)
* [Verwenden eines Windows-Computers mit Apache Hadoop in HDInsight](hdinsight-hadoop-windows-tools.md)
* [Hortonworks-Versionshinweise im Zusammenhang mit Azure HDInsight-Versionen](./hortonworks-release-notes.md)
* [Apache-Komponenten in HDInsight](./hdinsight-component-versioning.md)