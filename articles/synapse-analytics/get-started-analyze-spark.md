---
title: 'Tutorial: Erste Schritte zum Analysieren mit Spark'
description: In diesem Tutorial werden die grundlegenden Schritte zum Einrichten und Verwenden von Azure Synapse Analytics beschrieben.
services: synapse-analytics
author: saveenr
ms.author: saveenr
manager: julieMSFT
ms.reviewer: jrasnick
ms.service: synapse-analytics
ms.topic: tutorial
ms.date: 07/20/2020
ms.openlocfilehash: 5c6b35c1d9f00cae8fc688569e3a491679900995
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "87093481"
---
# <a name="analyze-with-apache-spark"></a>Analysieren mit Apache Spark

In diesem Tutorial werden die grundlegenden Schritte zum Laden und Analysieren von Daten mit Apache Spark für Azure Synapse beschrieben.

## <a name="load-the-nyc-taxi-data-into-the-spark-nyctaxi-database"></a>Laden der NYC Taxi-Daten in die Spark-Datenbank „nyctaxi“

In einer Tabelle in **SQLDB1** sind Daten verfügbar. Laden Sie sie in eine Spark-Datenbank mit dem Namen **nyctaxi**.

1. Navigieren Sie in Synapse Studio zum Hub **Entwickeln**.
1. Wählen Sie **+**  > **Notebook** aus.
1. Legen Sie am oberen Rand des Notebooks den Wert von **Anfügen an** auf **Spark1** fest.
1. Wählen Sie **Code hinzufügen** aus, um eine Notebook-Codezelle hinzuzufügen, und fügen Sie dann den folgenden Text ein:

    ```scala
    %%spark
    spark.sql("CREATE DATABASE IF NOT EXISTS nyctaxi")
    val df = spark.read.sqlanalytics("SQLDB1.dbo.Trip") 
    df.write.mode("overwrite").saveAsTable("nyctaxi.trip")
    ```

1. Navigieren Sie zum Hub **Daten**, klicken Sie mit der rechten Maustaste auf **Datenbanken**, und wählen Sie dann **Aktualisieren** aus. Die folgenden Datenbanken sollten angezeigt werden:

    - **SQLDB1** (SQL-Pool)
    - **nyctaxi** (Spark)

## <a name="analyze-the-nyc-taxi-data-using-spark-and-notebooks"></a>Analysieren der NYC Taxi-Daten mithilfe von Spark und Notebooks

1. Wechseln Sie zurück zu Ihrem Notebook.
1. Erstellen Sie eine neue Codezelle, und geben Sie den folgenden Text ein. Führen Sie die Zelle anschließend aus, um die NYC Taxi-Daten anzuzeigen, die wir in die Spark-Datenbank **nyctaxi** geladen haben.

   ```py
   %%pyspark
   df = spark.sql("SELECT * FROM nyctaxi.trip") 
   display(df)
   ```

1. Führen Sie den folgenden Code aus, um dieselbe Analyse durchzuführen, die wir zuvor mit dem SQL-Pool **SQLDB1** vorgenommen haben. Mit diesem Code werden die Ergebnisse der Analyse in einer Tabelle mit dem Namen **nyctaxi.passengercountstats** gespeichert und die Ergebnisse visualisiert.

   ```py
   %%pyspark
   df = spark.sql("""
      SELECT PassengerCount,
          SUM(TripDistanceMiles) as SumTripDistance,
          AVG(TripDistanceMiles) as AvgTripDistance
      FROM nyctaxi.trip
      WHERE TripDistanceMiles > 0 AND PassengerCount > 0
      GROUP BY PassengerCount
      ORDER BY PassengerCount
   """) 
   display(df)
   df.write.saveAsTable("nyctaxi.passengercountstats")
   ```

1. Wählen Sie in den Zellenergebnissen **Diagramm** aus, um die visualisierten Daten anzuzeigen.

## <a name="customize-data-visualization-with-spark-and-notebooks"></a>Anpassen der Datenvisualisierung mit Spark und Notebooks

Sie können mit Notebooks steuern, wie Diagramme gerendert werden. Mit dem folgenden Code wird ein einfaches Beispiel angezeigt. Hierbei werden die gängigen Bibliotheken **matplotlib** und **seaborn** verwendet. Mit dem Code wird die gleiche Art von Liniendiagramm wie bei den zuvor ausgeführten SQL-Abfragen gerendert.

```py
%%pyspark
import matplotlib.pyplot
import seaborn

seaborn.set(style = "whitegrid")
df = spark.sql("SELECT * FROM nyctaxi.passengercountstats")
df = df.toPandas()
seaborn.lineplot(x="PassengerCount", y="SumTripDistance" , data = df)
seaborn.lineplot(x="PassengerCount", y="AvgTripDistance" , data = df)
matplotlib.pyplot.show()
```



## <a name="load-data-from-a-spark-table-into-a-sql-pool-table"></a>Laden von Daten aus einer Spark-Tabelle in eine SQL-Pooltabelle

Weiter oben haben wir Daten aus der SQL-Pooltabelle **SQLDB1.dbo.Trip** in die Spark-Tabelle **nyctaxi.trip** kopiert. Anschließend haben wir mit Spark die Daten in der Spark-Tabelle **nyctaxi.passengercountstats** aggregiert. Nun kopieren wir die Daten aus **nyctaxi.passengercountstats** in eine SQL-Pooltabelle mit dem Namen **SQLDB1.dbo.PassengerCountStats**.

Führen Sie die folgende Zelle in Ihrem Notebook aus. Hiermit wird die aggregierte Spark-Tabelle zurück in die SQL-Pooltabelle kopiert.

```scala
%%spark
val df = spark.sql("SELECT * FROM nyctaxi.passengercountstats")
df.write.sqlanalytics("SQLDB1.dbo.PassengerCountStats", Constants.INTERNAL )
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Analysieren von Daten im Speicher](get-started-analyze-storage.md)


