---
title: Debugging und Problembehandlung von ParallelRunStep
titleSuffix: Azure Machine Learning
description: Debuggen und Problembehandlung für ParallelRunStep in Machine Learning-Pipelines im Azure Machine Learning SDK für Python. Lernen Sie häufige Fallstricke bei der Entwicklung mit Pipelines kennen, und erhalten Sie Tipps, die Ihnen helfen, Ihre Skripts vor und während der Remoteausführung zu debuggen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.custom: troubleshooting
ms.reviewer: jmartens, larryfr, vaidyas, laobri, tracych
ms.author: trmccorm
author: tmccrmck
ms.date: 07/16/2020
ms.openlocfilehash: 16366d9f3be1144a7588ceb9133fb4e2e60db95c
ms.sourcegitcommit: f353fe5acd9698aa31631f38dd32790d889b4dbb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/29/2020
ms.locfileid: "87373707"
---
# <a name="debug-and-troubleshoot-parallelrunstep"></a>Debugging und Problembehandlung von ParallelRunStep
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

In diesem Artikel erfahren Sie, wie Sie Debugging und Problembehandlung für die [ParallelRunStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.parallel_run_step.parallelrunstep?view=azure-ml-py)-Klasse im [Azure Machine Learning SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) durchführen.

## <a name="testing-scripts-locally"></a>Lokales Testen von Skripts

Weitere Informationen finden Sie im Abschnitt [Lokales Testen von Skripts](how-to-debug-pipelines.md#testing-scripts-locally) für Pipelines des maschinellen Lernens. Ihre ParallelRunStep-Instanz wird als Schritt in ML-Pipelines ausgeführt, sodass die gleiche Antwort für beide gilt.

## <a name="debugging-scripts-from-remote-context"></a>Debuggen von Skripts aus Remotekontext

Der Übergang vom lokalen Debuggen eines Bewertungsskripts zum Debuggen eines Bewertungsskripts in einer echten Pipeline kann einen schwierigen Schritt darstellen. Informationen zum Suchen Ihrer Protokolle im Portal finden Sie im [Abschnitt zu Pipelines des maschinellen Lernens über das Debuggen von Skripts aus einem Remotekontext](how-to-debug-pipelines.md#debugging-scripts-from-remote-context). Die Informationen in diesem Abschnitt gelten auch für ParallelRunStep.

Die Protokolldatei `70_driver_log.txt` enthält z. B. Informationen vom Controller, der den ParallelRunStep-Code startet.

Aufgrund der verteilten Struktur von ParallelRunStep-Aufträgen gibt es Protokolle aus mehreren verschiedenen Quellen. Es werden jedoch zwei konsolidierte Dateien erstellt, die allgemeine Informationen bieten:

- `~/logs/overview.txt`: Diese Datei enthält allgemeine Informationen zur Anzahl der bisher erstellten Minibatches (auch als Aufgaben bezeichnet) und der Anzahl der bisher verarbeiteten Minibatches. Am Ende wird das Ergebnis des Auftrags aufgeführt. Wenn bei dem Auftrag ein Fehler aufgetreten ist, wird die Fehlermeldung angezeigt, und es wird beschrieben, wo mit der Problembehandlung begonnen werden sollte.

- `~/logs/sys/master.txt`: Diese Datei stellt die Prinzipalknotenansicht (auch als Orchestrator bezeichnet) des laufenden Auftrags bereit. Dies umfasst die Aufgabenerstellung, die Fortschrittsüberwachung und das Ergebnis der Ausführung.

Protokolle, die mit dem EntryScript-Hilfsprogramm aus dem Eingabeskript generiert wurden, sowie Druckanweisungen finden Sie in den folgenden Dateien:

- `~/logs/user/<ip_address>/<node_name>.log.txt`: Diese Dateien sind die Protokolle, die von „entry_script“ mithilfe des EntryScript-Hilfsprogramms geschrieben wurden. Enthält auch eine Druckanweisung (stdout) von „entry_script“.

Für ein präzises Verständnis von Fehlern in Ihrem Skript gibt es Folgendes:

- `~/logs/user/error.txt`: Diese Datei wird versuchen, die Fehler in Ihrem Skript zusammenzufassen.

Weitere Informationen zu Fehlern in Ihrem Skript finden Sie hier:

- `~/logs/user/error/`: Enthält alle ausgelösten Fehler und vollständige Stapelüberwachungen, die nach Knoten organisiert sind.

Wenn Sie detailliert erfahren möchten, wie die einzelnen Knoten das Bewertungsskript ausgeführt haben, sehen Sie sich die jeweiligen Prozessprotokolle für jeden Knoten an. Die Prozessprotokolle befinden sich im Ordner `sys/node`, gruppiert nach Workerknoten:

- `~/logs/sys/node/<node_name>.txt`: Diese Datei enthält ausführliche Informationen zu den einzelnen Minibatches, die von einem Worker abgerufen oder abgeschlossen werden. Für jeden Minibatch umfasst diese Datei Folgendes:

    - Die IP-Adresse und die PID des Workerprozesses. 
    - Die Gesamtzahl der Elemente, die Anzahl der erfolgreich verarbeiteten Elemente und die Anzahl der nicht erfolgreichen Elemente.
    - Die Startzeit, die Dauer, die Verarbeitungszeit und die Laufzeit der Methode.

Sie finden auch Informationen zur Ressourcennutzung der Prozesse für jeden Worker. Diese Informationen liegen im CSV-Format vor und befinden sich unter `~/logs/sys/perf/overview.csv`. Informationen zu den einzelnen Prozessen finden Sie unter `~logs/sys/processes.csv`.

### <a name="how-do-i-log-from-my-user-script-from-a-remote-context"></a>Wie protokolliere ich mein Benutzerskript aus einem Remotekontext?
ParallelRunStep kann mehrere Prozesse auf einem Knoten entsprechend dem process_count_per_node ausführen. Wir empfehlen die unten gezeigte Verwendung der ParallelRunStep-Protokollierung, um die Protokolle der einzelnen Prozesse auf dem Knoten zu organisieren und die Druck- und Protokollanweisungen zu kombinieren. Sie können eine Protokollierung von EntryScript abrufen, damit die Protokolle im Ordner **logs/user** im Portal aufgeführt werden.

**Ein Beispiel für Eingabeskript mit der Protokollierung:**
```python
from azureml_user.parallel_run import EntryScript

def init():
    """ Initialize the node."""
    entry_script = EntryScript()
    logger = entry_script.logger
    logger.debug("This will show up in files under logs/user on the Azure portal.")


def run(mini_batch):
    """ Accept and return the list back."""
    # This class is in singleton pattern and will return same instance as the one in init()
    entry_script = EntryScript()
    logger = entry_script.logger
    logger.debug(f"{__file__}: {mini_batch}.")
    ...

    return mini_batch
```

### <a name="how-could-i-pass-a-side-input-such-as-a-file-or-files-containing-a-lookup-table-to-all-my-workers"></a>Wie könnte ich eine seitliche Eingabe, z. B. eine oder mehrere Dateien mit einer Nachschlagetabelle, an alle meine Mitarbeiter weitergeben?

Der Benutzer kann mit dem Parameter side_inputs von ParalleRunStep Verweisdaten an das Skript übergeben. Alle als side_inputs bereitgestellten Datasets werden auf den einzelnen Workerknoten eingebunden. Der Benutzer kann den Speicherort der Eingabe durch Übergeben des Arguments erhalten.

Konstruieren Sie ein [Dataset](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py), das die Verweisdaten enthält, und registrieren Sie es in Ihrem Arbeitsbereich. Übergeben Sie es an den Parameter `side_inputs` für Ihr `ParallelRunStep`. Zusätzlich können Sie den Pfad im Abschnitt `arguments` hinzufügen, um einfach auf den eingebundenen Pfad zuzugreifen:

```python
label_config = label_ds.as_named_input("labels_input")
batch_score_step = ParallelRunStep(
    name=parallel_step_name,
    inputs=[input_images.as_named_input("input_images")],
    output=output_dir,
    arguments=["--labels_dir", label_config],
    side_inputs=[label_config],
    parallel_run_config=parallel_run_config,
)
```

Danach können Sie in Ihrem Rückschlussskript (z. B. in Ihrer init()-Methode) wie folgt darauf zugreifen:

```python
parser = argparse.ArgumentParser()
parser.add_argument('--labels_dir', dest="labels_dir", required=True)
args, _ = parser.parse_known_args()

labels_path = args.labels_dir
```

## <a name="next-steps"></a>Nächste Schritte

* Hilfe zum Paket [azureml-pipeline-steps](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps?view=azure-ml-py) finden Sie in der SDK-Referenz. Zeigen Sie die [Referenzdokumentation](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.parallelrunstep?view=azure-ml-py) für die ParallelRunStep-Klasse an.

* Folgen Sie dem [erweiterten Tutorial](tutorial-pipeline-batch-scoring-classification.md) zur Verwendung von Pipelines mit ParallelRunStep. Das Tutorial zeigt, wie eine andere Datei als seitliche Eingabe übergeben werden kann. 
