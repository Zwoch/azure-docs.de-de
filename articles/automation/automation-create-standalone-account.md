---
title: Erstellen eines eigenständigen Azure Automation-Kontos
description: Dieser Artikel führt Sie anhand eines Beispiels durch die Schritte zum Erstellen, Testen und Verwenden der Authentifizierung per Sicherheitsprinzipal in Azure Automation.
services: automation
ms.service: automation
author: georgewallace
ms.author: gwallace
ms.date: 03/15/2018
ms.topic: article
manager: carmonm
ms.openlocfilehash: ca00736c6c42223a0fe6259da5ee2531c287de18
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/23/2018
---
# <a name="create-a-standalone-azure-automation-account"></a>Erstellen eines eigenständigen Azure Automation-Kontos
In diesem Artikel erfahren Sie, wie Sie ein Azure Automation-Konto im Azure-Portal erstellen. Mit dem Automation-Konto im Portal können Sie Automation auswerten und sich damit vertraut machen, ohne zusätzliche Verwaltungslösungen oder eine Azure Log Analytics-Integration zu verwenden. Wenn Sie Runbookaufträge später genauer überwachen möchten, können Sie jederzeit entsprechende Verwaltungslösungen hinzufügen oder eine Log Analytics-Integration durchführen. 

Mit einem Automation-Konto können Sie Runbooks authentifizieren, indem Sie Ressourcen im Azure Resource Manager-Bereitstellungsmodell oder im klassischen Bereitstellungsmodell verwalten.

Wenn Sie im Azure-Portal ein Automation-Konto erstellen, werden automatisch folgende Konten erstellt:

* **Ausführendes Konto:** Das Konto führt folgende Aufgaben aus:
  - Erstellen eines Dienstprinzipals in Azure Active Directory (Azure AD)
  - Erstellen eines Zertifikats
  - Zuweisen der Rolle „Mitwirkender“ der rollenbasierten Zugriffssteuerung (RBAC), die zum Verwalten von Azure Resource Manager-Ressourcen mit Runbooks verwendet wird
* **Klassisches ausführendes Konto:** Dieses Konto lädt ein Verwaltungszertifikat hoch. Das Zertifikat verwaltet klassische Ressourcen mithilfe von Runbooks.

Dank der automatischen Erstellung dieser Konten können Sie schnell Runbooks für Ihre Automatisierungsanforderungen erstellen und bereitstellen.  

## <a name="permissions-required-to-create-an-automation-account"></a>Erforderliche Berechtigungen zum Erstellen eines Automation-Kontos
Zum Erstellen oder Aktualisieren eines Automation-Kontos sowie zum Durchführen der Aufgaben in diesem Artikel müssen Sie über folgende Berechtigungen verfügen: 

* Für die Erstellung eines Automation-Kontos muss Ihr Azure AD-Benutzerkonto einer Rolle mit Berechtigungen hinzugefügt werden, die der Rolle „Besitzer“ für **Microsoft. Automation**-Ressourcen entspricht. Weitere Informationen finden Sie unter [Rollenbasierte Zugriffssteuerung in Azure Automation](automation-role-based-access-control.md).  
* Wenn im Azure-Portal unter **Azure Active Directory** > **VERWALTEN** > **App-Registrierungen** die Option **-App-Registrierungen** auf **Ja** festgelegt ist, können Benutzer ohne Administratorrechte in Ihrem Azure AD-Mandanten [Active Directory-Anwendungen registrieren](../azure-resource-manager/resource-group-create-service-principal-portal.md#check-azure-subscription-permissions). Wenn **App-Registrierungen** auf **Nein** festgelegt ist, muss ein Benutzer, der diese Aktion ausführt, ein globaler Administrator in Azure AD sein. 

Wenn Sie kein Mitglied der Active Directory-Instanz des Abonnements sind, werden Sie, bevor Sie der Rolle „Globaler Administrator/Co-Administrator“ des Abonnements hinzugefügt werden, zunächst in Active Directory als Gast hinzugefügt. In diesem Szenario wird auf der Seite **Automation-Konto hinzufügen** diese Meldung angezeigt: „Sie haben keine Berechtigungen zum Erstellen“. 

Wird ein Benutzer zuerst der Rolle des globalen Administrators/Co-Administrators hinzugefügt, können Sie diesen aus der Active Directory-Instanz des Abonnements entfernen, um ihn dann erneut als Vollbenutzer in Active Directory hinzuzufügen.

So überprüfen Sie Benutzerrollen
1. Navigieren Sie im Azure-Portal zum Bereich **Azure Active Directory**.
2. Wählen Sie **Benutzer und Gruppen**.
3. Wählen Sie **Alle Benutzer**. 
4. Wählen Sie nach der Auswahl eines bestimmten Benutzers **Profil** aus. Als Wert des Attributs **Benutzertyp** im Benutzerprofil darf nicht **Gast** angegeben sein.

## <a name="create-a-new-automation-account-in-the-azure-portal"></a>Erstellen eines neuen Automation-Kontos über das Azure-Portal
Führen Sie die folgenden Schritte aus, um ein Azure Automation-Konto über das Azure-Portal zu erstellen:    

1. Melden Sie sich mit einem Konto, das Mitglied der Rolle „Abonnement-Administratoren“ und Co-Administrator des Abonnements ist, beim Azure-Portal an.
2. Klicken Sie auf **+ Ressource erstellen**.
3. Suchen Sie nach **Automation**. Klicken Sie in den Suchergebnissen auf **Automation**.<br><br> ![Suchen nach „Automation & Control“ im Azure Marketplace und Auswählen der Option](media/automation-create-standalone-account/automation-marketplace-select-create-automationacct.png)<br> 
4. Klicken Sie auf dem nächsten Bildschirm auf **Erstellen**.
  ![Hinzufügen des Automation-Kontos](media/automation-create-standalone-account/automation-create-automationacct-properties.png)
  
  > [!NOTE]
  > Sollte im Bereich **Automation-Konto hinzufügen** die folgende Meldung angezeigt werden, ist Ihr Konto kein Mitglied der Rolle „Abonnement-Administratoren“ und kein Co-Administrator des Abonnements:
  >
  > ![Warnung beim Hinzufügen des Automation-Kontos](media/automation-create-standalone-account/create-account-without-perms.png)
  >
5. Geben Sie im Bereich **Automation-Konto hinzufügen** im Feld **Name** einen Namen für das neue Automation-Konto ein.
6. Falls Sie über mehrere Abonnements verfügen, geben Sie im Feld **Abonnement** das Abonnement an, das Sie für das neue Konto verwenden möchten. 
7. Geben Sie unter **Ressourcengruppe** eine neue oder vorhandene Ressourcengruppe ein, oder wählen Sie eine Ressourcengruppe aus. 
8. Wählen Sie unter **Standort** den Standort eines Azure-Datencenters aus.
9. Vergewissern Sie sich, dass die Option **Ausführendes Azure-Konto erstellen** auf **Ja** festgelegt ist, und klicken Sie anschließend auf **Erstellen**.
    
  > [!NOTE]
  > Wenn Sie die Option **Ausführendes Azure-Konto erstellen** auf **Nein** festlegen, wird im Bereich **Automation-Konto hinzufügen** eine Meldung angezeigt. Das Konto wird zwar im Azure-Portal erstellt, verfügt aber im Verzeichnisdienst für Ihr Abonnement (klassisches Bereitstellungsmodell oder Azure Resource Manager-Bereitstellungsmodell) über keine entsprechende Authentifizierungsidentität. Dadurch hat das Automation-Konto auch keinen Zugriff auf Ressourcen in Ihrem Abonnement. Runbooks, die auf dieses Konto verweisen, können in diesem Fall nicht authentifiziert werden und keine Aufgaben für Ressourcen in diesen Bereitstellungsmodellen durchführen.
  >
  > ![Warnung beim Hinzufügen des Automation-Kontos](media/automation-create-standalone-account/create-account-decline-create-runas-msg.png)
  >
  > Wenn der Dienstprinzipal nicht erstellt wird, wird die Rolle „Mitwirkender“ nicht zugewiesen.
  >
10. Den Status der Automation-Kontoerstellung können Sie über das Menü unter **Benachrichtigungen** verfolgen.

### <a name="resources-included"></a>Enthaltene Ressourcen
Nach der erfolgreichen Erstellung des Automation-Kontos werden automatisch verschiedene Ressourcen erstellt. In der folgenden Tabelle sind die Ressourcen für das ausführende Konto zusammengefasst.

| Ressource | BESCHREIBUNG |
| --- | --- |
| AzureAutomationTutorial-Runbook |Ein grafisches Beispielrunbook, das die Authentifizierung mithilfe des ausführenden Kontos veranschaulicht. Das Runbook ruft alle Resource Manager-Ressourcen ab. |
| AzureAutomationTutorialScript-Runbook |Ein PowerShell-Beispielrunbook, das die Authentifizierung mithilfe des ausführenden Kontos veranschaulicht. Das Runbook ruft alle Resource Manager-Ressourcen ab. |
| AzureAutomationTutorialPython2-Runbook |Ein Python-Beispielrunbook, das die Authentifizierung mithilfe des ausführenden Kontos veranschaulicht. Das Runbook führt alle im Abonnement vorhandenen Ressourcengruppen auf. |
| AzureRunAsCertificate |Eine Zertifikatressource, die automatisch zusammen mit dem Automation-Konto oder mithilfe eines PowerShell-Skripts für ein vorhandenes Konto erstellt wird. Das Zertifikat dient zur Authentifizierung bei Azure, um Azure Resource Manager-Ressourcen über Runbooks verwalten zu können. Dieses Zertifikat ist ein Jahr lang gültig. |
| AzureRunAsConnection |Eine Verbindungsressource, die automatisch zusammen mit dem Automation-Konto oder mithilfe eines PowerShell-Skripts für ein vorhandenes Konto erstellt wird. |

In der folgenden Tabelle sind die Ressourcen für das klassische ausführende Konto zusammengefasst.

| Ressource | BESCHREIBUNG |
| --- | --- |
| AzureClassicAutomationTutorial-Runbook |Ein grafisches Beispielrunbook. Das Runbook ruft alle klassischen virtuellen Computer in einem Abonnement mithilfe des klassischen ausführenden Kontos (Zertifikat) ab. Anschließend zeigt es die VM-Namen und den Status an. |
| AzureClassicAutomationTutorialScript-Runbook |Ein PowerShell-Beispielrunbook. Das Runbook ruft alle klassischen virtuellen Computer in einem Abonnement mithilfe des klassischen ausführenden Kontos (Zertifikat) ab. Anschließend zeigt es die VM-Namen und den Status an. |
| AzureClassicRunAsCertificate |Eine automatisch erstellte Zertifikatressource. Das Zertifikat dient zur Authentifizierung bei Azure, um klassische Azure-Ressourcen über Runbooks verwalten zu können. Dieses Zertifikat ist ein Jahr lang gültig. |
| AzureClassicRunAsConnection |Eine automatisch erstellte Verbindungsressource. Die Ressource dient zur Authentifizierung bei Azure, um klassische Azure-Ressourcen über Runbooks verwalten zu können. |


## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen zur grafischen Inhaltserstellung finden Sie unter [Grafische Erstellung in Azure Automation](automation-graphical-authoring-intro.md).
* Erste Schritte mit PowerShell-Runbooks werden unter [Mein erstes PowerShell-Runbook](automation-first-runbook-textual-powershell.md) beschrieben.
* Die ersten Schritte mit PowerShell-Workflow-Runbooks sind unter [Mein erstes PowerShell-Workflow-Runbook](automation-first-runbook-textual.md)beschrieben.
* Informationen zu den ersten Schritten mit Python2-Runbooks finden Sie unter [My first Python2 runbook](automation-first-runbook-textual-python2.md) (Mein erstes Python2-Runbook).
