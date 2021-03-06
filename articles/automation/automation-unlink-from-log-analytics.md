---
title: Aufheben der Verknüpfung eines Azure Automation-Kontos mit Log Analytics | Microsoft-Dokumentation
description: Dieser Artikel bietet eine Übersicht über das Aufheben der Verknüpfung eines Azure Automation-Kontos mit einem Log Analytics-Arbeitsbereich.
services: automation
ms.service: automation
author: georgewallace
ms.author: gwallace
ms.date: 03/19/2018
ms.topic: article
manager: carmonm
ms.openlocfilehash: b8b00f8a82dd63df13ccd0bc7e10429323c15ab3
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2018
---
# <a name="how-to-unlink-your-automation-account-from-a-log-analytics-workspace"></a>Aufheben der Verknüpfung eines Automation-Kontos mit einem Log Analytics-Arbeitsbereich

Azure Automation ist in Log Analytics nicht nur zum Unterstützen der Überwachung Ihrer Runbookaufträge in allen Ihren Automation-Konten integriert, sondern auch erforderlich, wenn Sie die folgenden von Log Analytics abhängigen Lösungen importieren:

* [Updateverwaltung](../operations-management-suite/oms-solution-update-management.md)
* [Änderungsnachverfolgung](../log-analytics/log-analytics-change-tracking.md)
* [Starten und Beenden von VMs außerhalb der Kernzeit](automation-solution-vm-management.md)
 
Wenn Sie Ihr Automation-Konto nicht länger in Log Analytics integriert sein soll, können Sie die Verknüpfung direkt im Azure-Portal aufheben.  Bevor Sie fortfahren, müssen Sie zuerst die zuvor erwähnten Lösungen entfernen, da dieser Prozess andernfalls nicht fortgesetzt werden kann.  Lesen Sie das Thema für die jeweilige Lösung, die Sie importiert haben, um die Schritte zu deren Entfernung zu verstehen.  

Nach dem Entfernen dieser Lösungen können Sie die folgenden Schritte ausführen, um die Verknüpfung Ihres Automation-Kontos aufzuheben.

## <a name="unlink-workspace"></a>Aufheben der Verknüpfung mit dem Arbeitsbereich

1. Öffnen Sie im Azure-Portal Ihr Automation-Konto, und klicken Sie auf der Seite „Automation-Konto“ im Abschnitt **Zugehörige Ressourcen** auf **Verknüpfung des Arbeitsbereichs aufheben**.<br><br> ![Option „Unlink workspace“ (Verknüpfung mit Arbeitsbereich aufheben)](media/automation-unlink-from-log-analytics/automation-unlink-workspace-option.png)<br><br>  
2. Klicken Sie auf der Seite „Verknüpfung des Arbeitsbereichs aufheben“ auf **Verknüpfung des Arbeitsbereichs aufheben**.<br><br> ![Seite „Verknüpfung des Arbeitsbereichs aufheben“](media/automation-unlink-from-log-analytics/automation-unlink-workspace-blade.png)<br><br>  Sie werden gefragt, ob Sie fortfahren möchten.<br><br>
3. Während Azure Automation versucht, die Verknüpfung des Kontos mit Ihrem Log Analytics-Arbeitsbereich aufzuheben, können Sie den Fortschritt unter **Benachrichtigungen** im Menü nachverfolgen.

Wenn Sie die Lösung „Updateverwaltung“ verwendet haben, können Sie optional die folgenden Elemente entfernen, die nach dem Entfernen der Lösung nicht mehr benötigt werden.

* Aktualisieren Sie Zeitpläne.  Diese verfügen über Namen, die den Updatebereitstellungen entsprechen, die Sie erstellt haben.

* Für die Lösung erstellte Hybrid Worker-Gruppen.  Jede wird ähnlich wie „machine1.contoso.com_9ceb8108 - 26 c 9-4051-b6b3-227600d715c8“ benannt.

Wenn Sie die Lösung „Starten und Beenden von VMs außerhalb der Kernzeit“ verwendet haben, können Sie optional die folgenden Elemente entfernen, die nach dem Entfernen der Lösung nicht mehr benötigt werden.

* Starten und beenden Sie Zeitpläne für VM-Runbooks. 
* Starten und beenden Sie VM-Runbooks.
* Variables   

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Neukonfigurieren Ihres Automation-Kontos für die Integration in Log Analytics finden Sie unter [Weiterleiten von Auftragsstatus und Auftragsdatenströmen von Automation an Log Analytics](automation-manage-send-joblogs-log-analytics.md). 