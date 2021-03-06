---
title: Suchen und Löschen von nicht angefügten verwalteten und nicht verwalteten Azure-Datenträgern | Microsoft-Dokumentation
description: Informationen zum Suchen und Löschen von nicht angefügten verwalteten und nicht verwalteten Azure-Datenträgern (VHDs/Seitenblobs) über Azure PowerShell
services: virtual-machines-windows
documentationcenter: ''
author: ramankumarlive
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/30/2018
ms.author: ramankum
ms.openlocfilehash: d908cdcd9e77f91a726f985d21bdc5bbc80ffd27
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/03/2018
---
# <a name="find-and-delete-unattached-azure-managed-and-unmanaged-disks"></a>Suchen und Löschen von nicht angefügten verwalteten und nicht verwalteten Azure-Datenträgern
Beim Löschen eines virtuellen Computers (VM) in Azure werden standardmäßig Datenträger, die an den virtuellen Computer angefügt sind, nicht gelöscht. Dieses Feature verhindert Datenverluste aufgrund der versehentlich Löschung von virtuellen Computern. Nach dem Löschen eines virtuellen Computers bezahlen Sie nicht angefügte Datenträger weiterhin. In diesem Artikel erfahren Sie, wie Sie nicht angefügte Datenträger suchen und löschen, um unnötige Kosten zu verringern. 


## <a name="managed-disks-find-and-delete-unattached-disks"></a>Verwaltete Datenträger: Suchen und Löschen nicht angefügter Datenträger 

Das folgende Skript sucht nach nicht angefügten [verwalteten Datenträgern](managed-disks-overview.md), indem es den Wert der **ManagedBy**-Eigenschaft untersucht. Wenn ein verwalteter Datenträger an einen virtuellen Computer angefügt ist, enthält die **ManagedBy**-Eigenschaft die Ressourcen-ID des virtuellen Computers. Wenn ein verwalteter Datenträger nicht angefügt ist, ist die **ManagedBy**-Eigenschaft NULL. Das Skript überprüft alle verwalteten Datenträger in einem Azure-Abonnement. Wenn das Skript einen verwalteten Datenträger findet, dessen **ManagedBy**-Eigenschaft den Wert NULL hat, legt das Skript fest, dass der Datenträger nicht angefügt ist.

>[!IMPORTANT]
>Führen Sie zunächst das Skript aus, indem Sie die **deleteUnattachedDisks**-Variable auf „0“ festlegen. Auf diese Weise können Sie alle nicht angefügten verwalteten Datenträger suchen und anzeigen.
>
>Nachdem Sie alle nicht angefügten Datenträger überprüft haben, führen Sie das Skript erneut aus, legen die **deleteUnattachedDisks**-Variable dabei jedoch auf „1“ fest. Dadurch können Sie alle nicht angefügten verwalteten Datenträger löschen.
>

```azurepowershell-interactive

# Set deleteUnattachedDisks=1 if you want to delete unattached Managed Disks
# Set deleteUnattachedDisks=0 if you want to see the Id of the unattached Managed Disks
$deleteUnattachedDisks=0

$managedDisks = Get-AzureRmDisk

foreach ($md in $managedDisks) {
    
    # ManagedBy property stores the Id of the VM to which Managed Disk is attached to
    # If ManagedBy property is $null then it means that the Managed Disk is not attached to a VM
    if($md.ManagedBy -eq $null){

        if($deleteUnattachedDisks -eq 1){
            
            Write-Host "Deleting unattached Managed Disk with Id: $($md.Id)"

            $md | Remove-AzureRmDisk -Force

            Write-Host "Deleted unattached Managed Disk with Id: $($md.Id) "

        }else{

            $md.Id

        }
           
    }
     
 } 
```

## <a name="unmanaged-disks-find-and-delete-unattached-disks"></a>Nicht verwaltete Datenträger: Suchen und Löschen nicht angefügter Datenträger 

Nicht verwaltete Datenträger sind VHD-Dateien, die als [Seitenblobs](/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs#about-page-blobs) in [Azure Storage-Konten](../../storage/common/storage-create-storage-account.md) gespeichert sind. Das folgende Skript sucht nach nicht angefügten nicht verwalteten Datenträgern (Seitenblobs) durch Untersuchen des Werts der **LeaseStatus**-Eigenschaft. Wenn ein nicht verwalteter Datenträger an einen virtuellen Computer angefügt ist, weist die **LeaseStatus**-Eigenschaft den Wert **Locked** auf. Wenn ein nicht verwalteter Datenträger nicht angefügt ist, weist die **LeaseStatus**-Eigenschaft den Wert **Unlocked** auf. Das Skript überprüft alle nicht verwalteten Datenträger in allen Azure Storage-Konten in einem Azure-Abonnement. Wenn das Skript einen nicht verwalteten Datenträger findet, dessen **LeaseStatus**-Eigenschaft den Wert **Unlocked** hat, geht das Skript davon aus, dass der Datenträger nicht angefügt ist.

>[!IMPORTANT]
>Führen Sie zunächst das Skript aus, indem Sie die **deleteUnattachedVHDs**-Variable auf „0“ festlegen. Auf diese Weise können Sie alle nicht angefügten nicht verwalteten VHDs suchen und anzeigen.
>
>Nachdem Sie alle nicht angefügten Datenträger überprüft haben, führen Sie das Skript erneut aus, legen die **deleteUnattachedVHDs**-Variable dabei jedoch auf „1“ fest. Auf diese Weise können Sie alle nicht angefügten nicht verwalteten Datenträger löschen.
>

```azurepowershell-interactive
   
# Set deleteUnattachedVHDs=1 if you want to delete unattached VHDs
# Set deleteUnattachedVHDs=0 if you want to see the Uri of the unattached VHDs
$deleteUnattachedVHDs=1

$storageAccounts = Get-AzureRmStorageAccount

foreach($storageAccount in $storageAccounts){

    $storageKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $storageAccount.ResourceGroupName -Name $storageAccount.StorageAccountName)[0].Value

    $context = New-AzureStorageContext -StorageAccountName $storageAccount.StorageAccountName -StorageAccountKey $storageKey

    $containers = Get-AzureStorageContainer -Context $context

    foreach($container in $containers){

        $blobs = Get-AzureStorageBlob -Container $container.Name -Context $context

        #Fetch all the Page blobs with extension .vhd as only Page blobs can be attached as disk to Azure VMs
        $blobs | Where-Object {$_.BlobType -eq 'PageBlob' -and $_.Name.EndsWith('.vhd')} | ForEach-Object { 
        
            #If a Page blob is not attached as disk then LeaseStatus will be unlocked
            if($_.ICloudBlob.Properties.LeaseStatus -eq 'Unlocked'){
              
                  if($deleteUnattachedVHDs -eq 1){

                        Write-Host "Deleting unattached VHD with Uri: $($_.ICloudBlob.Uri.AbsoluteUri)"

                        $_ | Remove-AzureStorageBlob -Force

                        Write-Host "Deleted unattached VHD with Uri: $($_.ICloudBlob.Uri.AbsoluteUri)"
                  }
                  else{

                        $_.ICloudBlob.Uri.AbsoluteUri

                  }

            }
        
        }

    }

}
```

## <a name="next-steps"></a>Nächste Schritte

[Löschen eines Speicherkontos](../../storage/common/storage-create-storage-account.md)




