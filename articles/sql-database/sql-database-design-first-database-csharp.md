---
title: Entwerfen Ihrer ersten Azure SQL-Datenbank – C# | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Ihre erste Azure SQL-Datenbank entwerfen und für diese mithilfe von ADO.NET eine Verbindung mit einem C#-Programm herstellen.
services: sql-database
author: MightyPen
manager: craigg-msft
ms.reviewer: CarlRabeler
ms.service: sql-database
ms.custom: develop databases, mvc, devcenter
ms.topic: tutorial
ms.date: 03/15/2018
ms.openlocfilehash: 3b6f260983e3c826bf558f0fe6d1a0fa6ae6b3af
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/17/2018
---
# <a name="design-an-azure-sql-database-and-connect-with-cx23-and-adonet"></a>Entwerfen einer Azure SQL-Datenbank und Herstellen einer Verbindung für diese mit C&#x23; und ADO.NET

Azure SQL-Datenbank ist eine relationale DBaaS-Lösung (Database-as-a-Service) in der Microsoft-Cloud (Azure). In diesem Tutorial erfahren Sie, wie Sie das Azure-Portal und ADO.NET mit Visual Studio für folgende Aufgaben verwenden: 

> [!div class="checklist"]
> * Erstellen einer Datenbank im Azure-Portal
> * Einrichten einer Firewallregel auf Serverebene im Azure-Portal
> * Herstellen einer Verbindung für die Datenbank mit ADO.NET und Visual Studio
> * Erstellen von Tabellen mit ADO.NET
> * Einfügen, Aktualisieren und Löschen von Daten mit ADO.NET 
> * Abfragen von Daten mit ADO.NET

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Eine Installation von [Visual Studio Community 2017, Visual Studio Professional 2017 oder Visual Studio Enterprise 2017](https://www.visualstudio.com/downloads/).

<!-- The following included .md, sql-database-tutorial-portal-create-firewall-connection-1.md, is long.
And it starts with a ## H2.
-->

[!INCLUDE [sql-database-tutorial-portal-create-firewall-connection-1](../../includes/sql-database-tutorial-portal-create-firewall-connection-1.md)]


<!-- The following included .md, sql-database-csharp-adonet-create-query-2.md, is long.
And it starts with a ## H2.
-->

[!INCLUDE [sql-database-csharp-adonet-create-query-2](../../includes/sql-database-csharp-adonet-create-query-2.md)]


## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial wurden grundlegende Datenbankaufgaben behandelt, z.B. das Erstellen einer Datenbank und von Tabellen, das Laden und Abfragen von Daten und das Wiederherstellen der Datenbank zu einem früheren Zeitpunkt. Es wurde Folgendes vermittelt:
> [!div class="checklist"]
> * Erstellen einer Datenbank
> * Einrichten einer Firewallregel
> * Herstellen einer Verbindung mit der Datenbank mit [Visual Studio und C#](sql-database-connect-query-dotnet-visual-studio.md)
> * Erstellen von Tabellen.
> * Einfügen, Aktualisieren und Löschen von Daten
> * Abfragen von Daten

Im nächsten Tutorial erfahren Sie, wie Sie Ihre Daten migrieren.

> [!div class="nextstepaction"]
>[Migrieren einer SQL Server-Datenbank zu Azure SQL-Datenbank](sql-database-migrate-your-sql-server-database.md)

