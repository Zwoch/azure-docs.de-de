---
title: "Ändern, Löschen oder Verwalten Ihrer Verwaltungsgruppen: Azure | Microsoft-Dokumentation"
description: Hier erfahren Sie, wie Sie die Verwaltungsgruppenhierarchie verwalten und aktualisieren.
author: rthorn17
manager: rithorn
editor: 
ms.assetid: 
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/1/2018
ms.author: rithorn
ms.openlocfilehash: 33797ddcd2a6ff083c5fb4b2fa7ddb8f9d6bd76c
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2018
---
# <a name="manage-your-resources-with-management-groups"></a>Verwalten von Ressourcen mit Verwaltungsgruppen 
Bei Verwaltungsgruppen handelt es sich um Container, mit denen Sie Zugriff, Richtlinien und Konformität abonnementübergreifend verwalten können. Sie können diese Container ändern, löschen und verwalten, um Hierarchien zu erhalten, die mit [Azure Policy](../azure-policy/azure-policy-introduction.md) und mit der [rollenbasierten Zugriffssteuerung (Role-Based Access Control, RBAC) von Azure](../active-directory/role-based-access-control-what-is.md) verwendet werden können. Weitere Informationen zu Verwaltungsgruppen finden Sie unter [Organize your resources with Azure Management Groups](management-groups-overview.md) (Organisieren von Ressourcen mit Azure-Verwaltungsgruppen).

Das Feature für Verwaltungsgruppen ist in einer öffentlichen Vorschauversion verfügbar. Melden Sie sich zur Verwendung von Verwaltungsgruppen beim [Azure-Portal](https://portal.azure.com) an. Alternativ können Sie zur Verwaltung Ihrer Verwaltungsgruppen [Azure PowerShell](https://www.powershellgallery.com/packages/AzureRM.ManagementGroups/0.0.1-preview), die [Azure CLI](https://docs.microsoft.com/en-us/cli/azure/extension?view=azure-cli-latest#az_extension_list_available) oder die [REST-API](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/managementgroups/resource-manager/Microsoft.Management/preview/2018-01-01-preview) verwenden.

Wenn Sie Änderungen an einer Verwaltungsgruppe vornehmen möchten, müssen Sie Mitglied der Rolle „Besitzer“ oder „Mitwirkender“ für die Verwaltungsgruppe sein. Wählen Sie zum Anzeigen der Ihnen zugewiesenen Berechtigungen die Verwaltungsgruppe aus, und klicken Sie dann auf **IAM**. Weitere Informationen zu RBAC-Rollen finden Sie unter [Erste Schritte mit der rollenbasierten Zugriffssteuerung im Azure-Portal](../active-directory/role-based-access-control-what-is.md).

## <a name="change-the-name-of-a-management-group"></a>Ändern des Namens einer Verwaltungsgruppe 
Der Name einer Verwaltungsgruppe kann über das Portal, mithilfe von PowerShell oder mithilfe der Azure CLI geändert werden.

### <a name="change-the-name-in-the-portal"></a>Ändern des Namens im Portal

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Klicken Sie auf **Alle Dienste** > **Verwaltungsgruppen**.  
3. Wählen Sie die Verwaltungsgruppe aus, die Sie umbenennen möchten. 
4. Klicken Sie oben auf der Seite auf die Option **Gruppe umbenennen**. 

    ![Gruppe umbenennen](media/management-groups/detail_action_small.png)
5. Wenn das Menü geöffnet wird, geben Sie den neuen Namen ein, der angezeigt werden soll.

    ![Gruppe umbenennen](media/management-groups/rename_context.png) 
4. Wählen Sie **Speichern**aus. 

### <a name="change-the-name-in-powershell"></a>Ändern des Namens in PowerShell

Verwenden Sie zum Aktualisieren des Anzeigenamens den Befehl **Update-AzureRmManagementGroup**. Wenn Sie beispielsweise den Namen einer Verwaltungsgruppe von „Contoso IT“ in „Contoso Group“ ändern möchten, führen Sie den folgenden Befehl aus: 

```azurepowershell-inetractive
C:\> Update-AzureRmManagementGroup -GroupName ContosoIt -DisplayName "Contoso Group"  
``` 

### <a name="change-the-name-in-azure-cli"></a>Ändern des Namens über die Azure CLI

Verwenden Sie für die Azure CLI den Aktualisierungsbefehl. 

```azure-cli
C:\> az account management-group update --group-name Contoso --display-name "Contoso Group" 
```

---
 
## <a name="delete-a-management-group"></a>Löschen einer Verwaltungsgruppe
Um eine Verwaltungsgruppe zu löschen, müssen die folgenden Anforderungen erfüllt sein:
1. Unter der Verwaltungsgruppe gibt es keine untergeordneten Verwaltungsgruppen oder Abonnements. 
    - Informationen zum Verschieben eines Abonnements aus einer Verwaltungsgruppe finden Sie unter [Verschieben eines Abonnements in eine andere Verwaltungsgruppe](#Move-subscriptions-in-the-hierarchy). 
    - Informationen zum Verschieben einer Verwaltungsgruppe in eine andere Verwaltungsgruppe finden Sie unter [Verschieben von Verwaltungsgruppen in der Hierarchie](#Move-management-groups-in-the-hierarchy). 
2. Sie haben Schreibzugriff auf die Verwaltungsgruppe (Rolle „Besitzer“ oder „Mitwirkender“ für die Verwaltungsgruppe). Wählen Sie zum Anzeigen der Ihnen zugewiesenen Berechtigungen die Verwaltungsgruppe aus, und klicken Sie dann auf **IAM**. Weitere Informationen zu RBAC-Rollen finden Sie unter [Erste Schritte mit der rollenbasierten Zugriffssteuerung im Azure-Portal](../active-directory/role-based-access-control-what-is.md).  

### <a name="delete-in-the-portal"></a>Löschen im Portal

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Klicken Sie auf **Alle Dienste** > **Verwaltungsgruppen**.  
3. Wählen Sie die zu löschende Verwaltungsgruppe aus. 
    
    ![Löschen einer Gruppe](media/management-groups/delete.png)
4. Klicken Sie auf **Löschen**. 
    - Wenn das Symbol abgeblendet ist, zeigen Sie mit der Maus darauf, um den Grund dafür anzuzeigen. 
5. Ein Fenster wird geöffnet, in dem Sie das Löschen der Verwaltungsgruppe bestätigen müssen. 

    ![Löschen einer Gruppe](media/management-groups/delete_confirm.png) 
6. Klicken Sie auf **Ja**. 


### <a name="delete-in-powershell"></a>Löschen in PowerShell

Verwenden Sie zum Löschen von Verwaltungsgruppen den Befehl **Remove-AzureRmManagementGroup** in PowerShell. 

```azurepowershell-interactive
Remove-AzureRmManagementGroup -GroupName Contoso
```

### <a name="delete-in-azure-cli"></a>Löschen über die Azure-Befehlszeilenschnittstelle
Verwenden Sie bei der Azure CLI den Befehl „az account management-group delete“. 

```azure-cli
C:\> az account management-group delete --group-name Contoso
```
---

## <a name="view-management-groups"></a>Anzeigen von Verwaltungsgruppen
Sie können jede Verwaltungsgruppe anzeigen, für die Sie eine direkte oder geerbte RBAC-Rolle besitzen.  

### <a name="view-in-the-portal"></a>Anzeigen im Portal
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Klicken Sie auf **Alle Dienste** > **Verwaltungsgruppen**. 
3. Die Seite mit der Verwaltungsgruppenhierarchie wird geladen. Auf dieser Seite werden alle Gruppen angezeigt, auf die Sie Zugriff haben. 
    ![Hauptseite](media/management-groups/main.png)
4. Wählen Sie eine einzelne Verwaltungsgruppe aus, um Details anzuzeigen.  

### <a name="view-in-powershell"></a>Anzeigen in PowerShell
Mit dem Befehl „Get-AzureRmManagementGroup“ rufen Sie alle Gruppen ab.  

```azurepowershell-interactive
Get-AzureRmManagementGroup
```
Verwenden Sie den Parameter „-GroupName“, um die Informationen einer einzelnen Verwaltungsgruppe anzuzeigen.

```azurepowershell-interactive
Get-AzureRmManagementGroup -GroupName Contoso
```

### <a name="view-in-azure-cli"></a>Anzeigen in der Azure CLI
Verwenden Sie den Befehl „list“, um alle Gruppen abzurufen.  

```azure-cli
az account management-group list
```
Verwenden Sie den Befehl „show“, um die Informationen einer einzelnen Verwaltungsgruppe anzuzeigen.

```azurepowershell-interactive
az account management-group show --group-name Contoso
```
---

## <a name="move-subscriptions-in-the-hierarchy"></a>Verschieben von Abonnements in der Hierarchie
Ein Grund zum Erstellen einer Verwaltungsgruppe ist das Bündeln von Abonnements. Nur Verwaltungsgruppen und Abonnements können als untergeordnete Elemente einer anderen Verwaltungsgruppe festgelegt werden. Ein Abonnement, das in eine Verwaltungsgruppe verschoben wird, erbt den gesamten Benutzerzugriff und alle Richtlinien von der übergeordneten Verwaltungsgruppe. 

Für das Verschieben des Abonnements benötigen Sie einige Berechtigungen: 
- Die Rolle „Besitzer“ für das untergeordnete Abonnement
- Die Rolle „Besitzer“ oder „Mitwirkender“ für die neue übergeordnete Verwaltungsgruppe 
- Die Rolle „Besitzer“ oder „Mitwirkender“ für die alte übergeordnete Verwaltungsgruppe
Wählen Sie zum Anzeigen der Ihnen zugewiesenen Berechtigungen die Verwaltungsgruppe aus, und klicken Sie dann auf **IAM**. Weitere Informationen zu RBAC-Rollen finden Sie unter [Erste Schritte mit der rollenbasierten Zugriffssteuerung im Azure-Portal](../active-directory/role-based-access-control-what-is.md). 

### <a name="move-subscriptions-in-the-portal"></a>Verschieben von Abonnements im Portal

**Hinzufügen eines vorhandenen Abonnements zu einer Verwaltungsgruppe**
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Klicken Sie auf **Alle Dienste** > **Verwaltungsgruppen**. 
3. Wählen Sie die Verwaltungsgruppe aus, die als übergeordnete Gruppe festgelegt werden soll.      
5. Klicken Sie am oberen Rand der Seite auf **Vorhandene hinzufügen**. 
6. Wählen Sie im angezeigten Menü den **Ressourcentyp** des Elements aus, das Sie verschieben möchten (d.h. **Abonnement**).
7. Wählen Sie in der Liste das Abonnement mit der richtigen ID aus. 

    ![Untergeordnete Elemente](media/management-groups/add_context_2.png)
8. Klicken Sie auf „Speichern“.

**Entfernen eines Abonnements aus einer Verwaltungsgruppe**
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Klicken Sie auf **Alle Dienste** > **Verwaltungsgruppen**. 
3. Wählen Sie die geplante Verwaltungsgruppe aus. (Hierbei handelt es sich um die aktuelle übergeordnete Gruppe.)  
4. Klicken Sie in der Liste am Ende der Zeile des zu verschiebenden Abonnements auf die Ellipse.

    ![Move](media/management-groups/move_small.png)
5. Klicken Sie auf **Verschieben**.
6. Klicken Sie im angezeigten Menü auf **Übergeordnete Verwaltungsgruppe**.

    ![Move](media/management-groups/move_small_context.png)
7. Wählen Sie **Speichern** aus.

### <a name="move-subscriptions-in-powershell"></a>Verschieben von Abonnements in PowerShell
Wenn Sie zum Verschieben eines Abonnements in PowerShell den Befehl „Add-AzureRmManagementGroupSubscription“.  

```azurepowershell-interactive
New-AzureRmManagementGroupSubscription -GroupName Contoso -SubscriptionId 12345678-1234-1234-1234-123456789012
```

Verwenden Sie zum Entfernen der Verknüpfung zwischen dem Abonnement und der Verwaltungsgruppe den Befehl „Remove-AzureRmManagementGroupSubscription“.

```azurepowershell-interactive
Remove-AzureRmManagementGroupSubscription -GroupName Contoso -SubscriptionId 12345678-1234-1234-1234-123456789012
```

### <a name="move-subscriptions-in-azure-cli"></a>Verschieben von Abonnements in der Azure CLI
Verwenden Sie zum Verschieben eines Abonnements in der CLI den Befehl „add“. 

```azure-cli
C:\> az account management-group add --group-name Contoso --subscription 12345678-1234-1234-1234-123456789012
```

Verwenden Sie zum Entfernen des Abonnements aus der Verwaltungsgruppe den Befehl „remove“.  

```azure-cli
C:\> az account management-group remove --group-name Contoso --subscription 12345678-1234-1234-1234-123456789012
```

---

## <a name="move-management-groups-in-the-hierarchy"></a>Verschieben von Verwaltungsgruppen in der Hierarchie  
Wenn Sie eine übergeordnete Verwaltungsgruppe verschieben, werden auch alle untergeordneten Ressourcen, die Verwaltungsgruppen, Abonnements, Ressourcengruppen und Ressourcen enthalten, mit der übergeordneten Gruppe verschoben.   

### <a name="move-management-groups-in-the-portal"></a>Verschieben von Verwaltungsgruppen im Portal

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Klicken Sie auf **Alle Dienste** > **Verwaltungsgruppen**. 
3. Wählen Sie die Verwaltungsgruppe aus, die als übergeordnete Gruppe festgelegt werden soll.      
5. Klicken Sie am oberen Rand der Seite auf **Vorhandene hinzufügen**.
6. Wählen Sie im angezeigten Menü den **Ressourcentyp** des Elements aus, das Sie verschieben möchten (d.h. **Verwaltungsgruppe**).
7. Wählen Sie die Verwaltungsgruppe mit der richtigen ID und dem entsprechenden Namen aus.

    ![Verschieben](media/management-groups/add_context.png)
8. Wählen Sie **Speichern** aus.

### <a name="move-management-groups-in-powershell"></a>Verschieben von Verwaltungsgruppen in PowerShell
Verwenden Sie den Befehl „Update-AzureRmManagementGroup“ in PowerShell, um eine Verwaltungsgruppe in eine andere Gruppe zu verschieben.  

```powershell
C:\> Update-AzureRmManagementGroup -GroupName Contoso  -ParentName ContosoIT
```  
### <a name="move-management-groups-in-azure-cli"></a>Verschieben von Verwaltungsgruppen in der Azure CLI
Verwenden Sie den Befehl „update“, um eine Verwaltungsgruppe mit der Azure CLI zu verschieben. 

```azure-cli
C:/> az account management-group udpate --group-name Contoso --parent-id "Contoso Tenant" 
``` 

---

## <a name="next-steps"></a>Nächste Schritte 
Weitere Informationen zu Verwaltungsgruppen finden Sie unter folgenden Links: 
- [Organize your resources with Azure management groups](management-groups-overview.md) (Organisieren von Ressourcen mit Azure-Verwaltungsgruppen)
- [Erstellen von Verwaltungsgruppen zum Organisieren von Azure-Ressourcen](management-groups-create.md)
- [Installieren des Azure Powershell-Moduls](https://www.powershellgallery.com/packages/AzureRM.ManagementGroups/0.0.1-preview)
- [Spezifikationen zur REST-API](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/managementgroups/resource-manager/Microsoft.Management/preview/2018-01-01-preview)
- [Installieren der Erweiterung für die Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/en-us/cli/azure/extension?view=azure-cli-latest#az_extension_list_available)