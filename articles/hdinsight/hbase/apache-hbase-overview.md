---
title: Was ist HBase in Azure HDInsight? | Microsoft-Dokumentation
description: "Eine Einführung in Apache HBase in HDInsight, ein NoSQL-Datenbankbuild auf Hadoop. Erfahren Sie mehr über Anwendungsfälle, und vergleichen Sie HBase mit anderen Hadoop-Clustern."
keywords: "BigTable, NoSQL, was ist HBase, Apache HBase, HBase, HBase-Übersicht"
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: d2a76d53-133a-4849-a30c-88d9c794391c
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/22/2018
ms.author: jgao
ms.openlocfilehash: 9b1453828c61d9944938b695b329aebec5ab06e4
ms.sourcegitcommit: 12fa5f8018d4f34077d5bab323ce7c919e51ce47
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/23/2018
---
# <a name="what-is-hbase-in-hdinsight-a-nosql-database-that-provides-bigtable-like-capabilities-for-hadoop"></a>Überblick über HBase in HDInsight: Eine NoSQL-Datenbank, die BigTable-ähnliche Funktionen für Hadoop bereitstellt
Apache HBase ist eine Open-Source-NoSQL-Datenbank, die auf Hadoop basiert und nach dem Vorbild von Google BigTable erstellt wurde. HBase bietet wahlfreien Zugriff und starke Konsistenz für große Mengen unstrukturierter und teilstrukturierter Daten in einer schemalosen Datenbank, die nach Spaltenfamilien gegliedert ist.

Daten werden in den Zeilen einer Tabelle gespeichert und die Daten in einer Zeile zu einer Spaltenfamilie zusammengefasst. HBase ist eine schemalose Datenbank in dem Sinne, dass weder die Spalten noch der Typ der darin gespeicherten Daten vor der Verwendung definiert werden müssen. Der Open-Source-Code lässt sich linear skalieren, sodass Petabytes von Daten auf Tausenden von Knoten verarbeitet werden können. HBase nutzt Datenredundanz, Stapelverarbeitung und andere Funktionen, die von verteilten Anwendungen im Hadoop-Ökosystem zur Verfügung gestellt werden.

[!INCLUDE [hdinsight-price-change](../../../includes/hdinsight-enhancements.md)]

## <a name="how-is-hbase-implemented-in-azure-hdinsight"></a>Wie wird HBase in Azure HDInsight implementiert?
HDInsight HBase wird als verwalteter Cluster angeboten, der in die Azure-Umgebung integriert ist. Die Cluster sind so konfiguriert, dass Daten direkt in [Azure Storage](./../hdinsight-hadoop-use-blob-storage.md) oder [Azure Data Lake Store](./../hdinsight-hadoop-use-data-lake-store.md) gespeichert werden. Dies sorgt für geringe Wartezeit und mehr Flexibilität bei Entscheidungen bezüglich der Leistung und Kosten. So können Kunden interaktive Websites erstellen, die mit großen Datensätzen arbeiten, und Dienste entwickeln, die Sensor- und Telemetriedaten aus Millionen von Endpunkten speichern. Diese Daten können dann mit Hadoop-Jobs analysiert werden. HBase und Hadoop sind gute Ausgangspunkte für Big-Data-Projekte in Azure – sie ermöglichen besonders in Echtzeitanwendungen die Verarbeitung großer Datensätze.

Die HDInsight-Implementierung nutzt die skalierbare Architektur von HBase für automatische Partitionierung von Tabellen, starke Konsistenz für Lese- und Schreibvorgänge sowie automatisches Failover. Die Leistung wird durch speicherinterne Zwischenspeicherung für Lesevorgänge und Schreibvorgänge mit hohem Durchsatz optimiert. Ein HBase-Cluster kann in einem virtuellen Netzwerk erstellt werden. Details hierzu finden Sie unter [Erstellen von HDInsight-Clustern im virtuellen Azure-Netzwerk](./apache-hbase-provision-vnet.md).

## <a name="how-is-data-managed-in-hdinsight-hbase"></a>Wie werden Daten in HDInsight HBase verwaltet?
Daten können Sie in HBase mit den Befehlen `create`, `get`, `put` und `scan` über die HBase-Shell verwalten. Daten werden mit dem Befehl `put` in die Datenbank geschrieben und mit `get` gelesen. Der `scan`-Befehl wird verwendet, um Daten aus mehreren Zeilen in einer Tabelle abzurufen. Sie können Daten auch über die HBase-C#-API verwalten. Diese bietet eine Clientbibliothek auf der HBase-REST-API. Eine HBase-Datenbank können Sie auch mit Hive abfragen. Eine Einleitung in diese Programmiermodelle finden Sie unter [Erste Schritte mit HBase mit Hadoop in HDInsight](./apache-hbase-tutorial-get-started-linux.md). Coprozessoren sind ebenfalls verfügbar und ermöglichen die Verarbeitung von Daten in den Knoten, die die Datenbank hosten.

> [!NOTE]
> Thrift wird von HBase in HDInsight nicht unterstützt.
>

## <a name="scenarios-use-cases-for-hbase"></a>Szenarios: Anwendungsfälle für HBase
Der kanonische Anwendungsfall, für den BigTable (und daher auch HBase) erstellt wurde, ist die Websuche. Suchmaschinen erstellen Indizes, die Begriffe den Webseiten zuordnen, die diese Begriffe enthalten. Es gibt jedoch noch viele weitere Anwendungsfälle für HBase. Einige von ihnen werden in diesem Abschnitt beschrieben.

* Schlüssel-Wert-Speicherung
  
    HBase kann für die Schlüssel-Wert-Speicherung verwendet werden und ist für die Verwaltung von Nachrichtensystemen geeignet. Facebook nutzt HBase für sein Nachrichtensystem. HBase ist ideal für die Speicherung und Verwaltung von Internetkommunikation. WebTable nutzt HBase, um Tabellen, die aus Webseiten extrahiert wurden, zu suchen und zu verwalten.
* Sensordaten
  
    HBase kann für das Erfassen von Daten verwendet werden, die schrittweise aus verschiedenen Quellen gesammelt werden. Dies umfasst Analysen sozialer Netzwerke, Zeitreihen, Gewährleistung der Aktualität interaktiver Dashboards mit Trends und Indikatoren sowie die Verwaltung von Prüfungsprotokollierungssystemen. Zu den Beispielen zählen das Bloomberg Trader Terminal und die Open Time Series Database (OpenTSDB), die Metriken speichert, die zum Status von Serversystemen gesammelt wurden, und den Zugriff darauf ermöglicht.
* Echtzeitabfrage
  
    
            [Phoenix](http://phoenix.apache.org/) ist eine SQL-Abfrage-Engine für Apache HBase. Der Zugriff erfolgt als JDBC-Treiber. So können Sie HBase-Tabellen mit SQL abfragen und verwalten.
* HBase als Plattform
  
    Anwendungen können auf HBase ausgeführt werden, indem sie HBase als Datenspeicher nutzen. Beispiele hierfür sind Phoenix, OpenTSDB, Kiji und Titan. Es ist auch ein Integration von Anwendungen mit HBase möglich. Beispiele hierfür sind Hive, Pig, Solr, Storm, Flume, Impala, Spark, Ganglia und Drill.

## <a name="next-steps"></a>Nächste Schritte
* [Erste Schritte mit HBase mit Hadoop in HDInsight](./apache-hbase-tutorial-get-started-linux.md)
* [Erstellen von HDInsight-Clustern in Azure Virtual Network](./apache-hbase-provision-vnet.md)
* [Konfigurieren der HBase-Replikation in HDInsight](apache-hbase-replication.md)
* [Verwenden von Maven zur Entwicklung von Java-Anwendungen, die HBase mit HDInsight (Hadoop) nutzen](./apache-hbase-build-java-maven-linux.md)

## <a name="see-also"></a>Weitere Informationen
* [Apache HBase](https://hbase.apache.org/)
* [Bigtable: A Distributed Storage System for Structured Data (in englischer Sprache)](http://research.google.com/archive/bigtable.html)




