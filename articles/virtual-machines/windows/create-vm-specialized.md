---
title: Creare una macchina virtuale Windows da un disco rigido virtuale specializzato in Azure | Microsoft Docs
description: Creare una nuova macchina virtuale Windows collegando un disco gestito specializzato come disco del sistema operativo usando il modello di distribuzione Resource Manager.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 3b7d3cd5-e3d7-4041-a2a7-0290447458ea
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 01/09/2018
ms.author: cynthn
ms.openlocfilehash: 34bfe7733c60337d6ab7d81c498d2fb0fd15e1fd
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/31/2018
ms.locfileid: "43338487"
---
# <a name="create-a-windows-vm-from-a-specialized-disk-using-powershell"></a>Creare una macchina virtuale Windows da un disco specializzato tramite PowerShell

Creare una nuova macchina virtuale collegando un disco gestito specializzato come disco del sistema operativo. Un disco specializzato è una copia del disco rigido virtuale proveniente da una macchina virtuale esistente che gestisce gli account utente, le applicazioni e altri dati di stato dalla macchina virtuale originale. 

Quando si usa un disco rigido virtuale specializzato per creare una nuova VM, la nuova VM mantiene il nome computer della VM originale. Vengono mantenute anche altre informazioni specifiche del computer e, in alcuni casi, queste informazioni duplicate possono causare problemi. Quando si copia una VM, tenere presente quali tipi di informazioni specifiche del computer vengono usate dalle applicazioni.

Sono disponibili diverse opzioni:
* [Usare un disco gestito esistente](#option-1-use-an-existing-disk). Questa opzione è utile se la macchina virtuale disponibile non funziona correttamente. È possibile eliminare la macchina virtuale e riutilizzare il disco gestito per creare una nuova macchina virtuale. 
* [Caricare un VHD](#option-2-upload-a-specialized-vhd) 
* [Copiare una macchina virtuale di Azure esistente con gli snapshot](#option-3-copy-an-existing-azure-vm)

È anche possibile usare il portale di Azure per [creare una nuova macchina virtuale da un disco rigido virtuale specializzato](create-vm-specialized-portal.md).

Questo argomento illustra come usare i dischi gestiti. Se è presente una distribuzione legacy che richiede l'uso di un account di archiviazione, vedere [Create a VM from a specialized VHD in a storage account](sa-create-vm-specialized.md) (Creare una VM da un disco rigido virtuale specializzato in un account di archiviazione)

## <a name="before-you-begin"></a>Prima di iniziare
Se si usa PowerShell, verificare di avere la versione più recente del modulo di PowerShell AzureRM.Compute. 

```powershell
Install-Module AzureRM -RequiredVersion 6.0.0
```
Per altre informazioni, vedere [Controllo delle versioni di Azure PowerShell](/powershell/azure/overview).

## <a name="option-1-use-an-existing-disk"></a>Opzione 1: Usare un disco esistente

Se è presente una macchina virtuale che è stata eliminata e si vuole riutilizzare il disco del sistema operativo per creare una nuova macchina virtuale, usare [Get-AzureRmDisk](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermdisk?view=azurermps-6.8.1).

```powershell
$resourceGroupName = 'myResourceGroup'
$osDiskName = 'myOsDisk'
$osDisk = Get-AzureRmDisk `
-ResourceGroupName $resourceGroupName `
-DiskName $osDiskName
```
Sarà quindi possibile collegare questo disco come disco del sistema operativo a una [nuova macchina virtuale](#create-the-new-vm).

## <a name="option-2-upload-a-specialized-vhd"></a>Opzione 2: Caricare un disco rigido virtuale specializzato

È possibile caricare il disco rigido virtuale da una VM specializzata creata con uno strumento di virtualizzazione locale, ad esempio Hyper-V, o da una VM esportata da un altro cloud.

### <a name="prepare-the-vm"></a>Preparare la macchina virtuale
Se si intende usare il disco rigido virtuale così come è per creare una nuova macchina virtuale, assicurare il completamento delle operazioni seguenti. 
  
  * [Preparare un disco rigido virtuale (VHD) di Windows per il caricamento in Azure](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). **Non** generalizzare la macchina Virtuale con Sysprep.
  * Rimuovere tutti gli strumenti di virtualizzazione guest e gli agenti installati nella macchina virtuale, ad esempio gli strumenti VMware.
  * Assicurarsi che la macchina virtuale sia configurata per eseguire il pull dell'indirizzo IP e delle impostazioni DNS tramite DHCP. In questo modo il server ottiene un indirizzo IP all'interno della rete virtuale all'avvio. 


### <a name="get-the-storage-account"></a>Ottenere l'account di archiviazione
Per archiviare il disco rigido virtuale caricato, è necessario un account di archiviazione di Azure. È possibile usare un account di archiviazione esistente o crearne uno nuovo. 

Per mostrare gli account di archiviazione disponibili, digitare:

```powershell
Get-AzureRmStorageAccount
```

Per usare un account di archiviazione esistente, passare alla sezione [Caricare il disco rigido virtuale](#upload-the-vhd-to-your-storage-account).

Per creare un account di archiviazione, seguire questa procedura:

1. È necessario il nome del gruppo di risorse in cui deve essere creato l'account di archiviazione. Per trovare tutti i gruppi di risorse inclusi nella sottoscrizione digitare:
   
    ```powershell
    Get-AzureRmResourceGroup
    ```

    Per creare un gruppo di risorse denominato *MyResourceGroup* nell'area *Stati Uniti occidentali*, digitare:

    ```powershell
    New-AzureRmResourceGroup `
       -Name myResourceGroup `
       -Location "West US"
    ```

2. Creare un account di archiviazione denominato *mystorageaccount* in questo gruppo di risorse con il cmdlet [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount).
   
    ```powershell
    New-AzureRmStorageAccount `
       -ResourceGroupName myResourceGroup `
       -Name mystorageaccount `
       -Location "West US" `
       -SkuName "Standard_LRS" `
       -Kind "Storage"
    ```

### <a name="upload-the-vhd-to-your-storage-account"></a>Caricare il disco rigido virtuale nell'account di archiviazione 
Usare il cmdlet [Add-AzureRmVhd](/powershell/module/azurerm.compute/add-azurermvhd) per caricare il disco rigido virtuale in un contenitore nell'account di archiviazione. In questo esempio, il file *myVHD.vhd* viene caricato da `"C:\Users\Public\Documents\Virtual hard disks\"` a un account di archiviazione denominato *mystorageaccount* nel gruppo di risorse *myResourceGroup*. Il file viene archiviato nel contenitore denominato *mycontainer* e il nuovo nome del file sarà *myUploadedVHD.vhd*.

```powershell
$resourceGroupName = "myResourceGroup"
$urlOfUploadedVhd = "https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd"
Add-AzureRmVhd -ResourceGroupName $resourceGroupName `
   -Destination $urlOfUploadedVhd `
   -LocalFilePath "C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd"
```


Se l'operazione riesce, si ottiene una risposta simile alla seguente:

```powershell
MD5 hash is being calculated for the file C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd.
MD5 hash calculation is completed.
Elapsed time for the operation: 00:03:35
Creating new page blob of size 53687091712...
Elapsed time for upload: 01:12:49

LocalFilePath           DestinationUri
-------------           --------------
C:\Users\Public\Doc...  https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd
```

L'esecuzione del comando potrebbe richiedere del tempo, a seconda della connessione di rete e delle dimensioni del file VHD.

### <a name="create-a-managed-disk-from-the-vhd"></a>Creare un disco gestito dal disco rigido virtuale

Creare un disco gestito dal disco rigido virtuale specializzato nell'account di archiviazione usando [New-AzureRMDisk](/powershell/module/azurerm.compute/new-azurermdisk). Questo esempio usa **myOSDisk1** per il nome del disco, inserisce il disco nella risorsa di archiviazione *Standard_LRS* e usa  *https://storageaccount.blob.core.windows.net/vhdcontainer/osdisk.vhd* come URI per il disco rigido virtuale di origine.

Creare un nuovo gruppo di risorse per la nuova VM.

```powershell
$destinationResourceGroup = 'myDestinationResourceGroup'
New-AzureRmResourceGroup -Location $location `
   -Name $destinationResourceGroup
```

Creare il nuovo disco del sistema operativo dal disco rigido virtuale caricato. 

```powershell
$sourceUri = (https://storageaccount.blob.core.windows.net/vhdcontainer/osdisk.vhd)
$osDiskName = 'myOsDisk'
$osDisk = New-AzureRmDisk -DiskName $osDiskName -Disk `
    (New-AzureRmDiskConfig -AccountType Standard_LRS  `
    -Location $location -CreateOption Import `
    -SourceUri $sourceUri) `
    -ResourceGroupName $destinationResourceGroup
```

## <a name="option-3-copy-an-existing-azure-vm"></a>Opzione 3: Copiare una macchina virtuale di Azure esistente

È possibile creare una copia di una VM che usa dischi gestiti acquisendo uno snapshot della VM, quindi usando tale snapshot per creare un nuovo disco gestito e una nuova VM.


### <a name="take-a-snapshot-of-the-os-disk"></a>Acquisire uno snapshot del disco del sistema operativo

È possibile acquisire uno snapshot di un'intera VM (tutti i dischi inclusi) o solo di un singolo disco. I passaggi seguenti illustrano come acquisire uno snapshot solo del disco del sistema operativo della VM usando il cmdlet [New-AzureRmSnapshot](/powershell/module/azurerm.compute/new-azurermsnapshot). 

Impostare alcuni parametri. 

 ```powershell
$resourceGroupName = 'myResourceGroup' 
$vmName = 'myVM'
$location = 'westus' 
$snapshotName = 'mySnapshot'  
```

Ottenere l'oggetto macchina virtuale.

```powershell
$vm = Get-AzureRmVM -Name $vmName `
   -ResourceGroupName $resourceGroupName
```
Ottenere il nome del disco del sistema operativo.

 ```powershell
$disk = Get-AzureRmDisk -ResourceGroupName $resourceGroupName `
   -DiskName $vm.StorageProfile.OsDisk.Name
```

Creare la configurazione dello snapshot. 

 ```powershell
$snapshotConfig =  New-AzureRmSnapshotConfig `
   -SourceUri $disk.Id `
   -OsType Windows `
   -CreateOption Copy `
   -Location $location 
```

Ottenere lo snapshot.

```powershell
$snapShot = New-AzureRmSnapshot `
   -Snapshot $snapshotConfig `
   -SnapshotName $snapshotName `
   -ResourceGroupName $resourceGroupName
```


Se si prevede di usare lo snapshot per creare una macchina virtuale a prestazioni elevate, usare il parametro `-AccountType Premium_LRS` con il comando New-AzureRmSnapshot. Il parametro crea lo snapshot in modo tale che venga archiviato come un disco gestito Premium. I dischi gestiti Premium sono più costosi di quelli Standard. Assicurarsi quindi che l'opzione Premium sia realmente necessaria prima di usare il parametro.

### <a name="create-a-new-disk-from-the-snapshot"></a>Creare un nuovo disco dallo snapshot

Creare un disco gestito dallo snapshot usando [New-AzureRMDisk](/powershell/module/azurerm.compute/new-azurermdisk). Questo esempio usa *myOSDisk* come nome del disco.

Creare un nuovo gruppo di risorse per la nuova VM.

```powershell
$destinationResourceGroup = 'myDestinationResourceGroup'
New-AzureRmResourceGroup -Location $location `
   -Name $destinationResourceGroup
```

Impostare il nome del disco del sistema operativo. 

```powershell
$osDiskName = 'myOsDisk'
```

Creare il disco gestito.

```powershell
$osDisk = New-AzureRmDisk -DiskName $osDiskName -Disk `
    (New-AzureRmDiskConfig  -Location $location -CreateOption Copy `
    -SourceResourceId $snapshot.Id) `
    -ResourceGroupName $destinationResourceGroup
```


## <a name="create-the-new-vm"></a>Creare la nuova VM 

Creare le risorse di rete e le altre risorse da usare nella nuova VM.

### <a name="create-the-subnet-and-vnet"></a>Creare la subNet e la vNet

Creare la rete virtuale e la subnet della [rete virtuale](../../virtual-network/virtual-networks-overview.md).

Creare la subnet. In questo esempio viene creata una subnet denominata **mySubNet** nel gruppo di risorse **myDestinationResourceGroup** e il prefisso dell'indirizzo della subnet viene impostato su **10.0.0.0/24**.
   
```powershell
$subnetName = 'mySubNet'
$singleSubnet = New-AzureRmVirtualNetworkSubnetConfig `
   -Name $subnetName `
   -AddressPrefix 10.0.0.0/24
```

Creare la rete virtuale. In questo esempio il nome della rete virtuale è **myVnetName**, la posizione specificata è **Stati Uniti occidentali** e il prefisso dell'indirizzo per la rete virtuale è **10.0.0.0/16**. 
   
```powershell
$vnetName = "myVnetName"
$vnet = New-AzureRmVirtualNetwork `
   -Name $vnetName -ResourceGroupName $destinationResourceGroup `
   -Location $location `
   -AddressPrefix 10.0.0.0/16 `
   -Subnet $singleSubnet
```    


### <a name="create-the-network-security-group-and-an-rdp-rule"></a>Creare il gruppo di sicurezza di rete e una regola RDP
Per essere in grado di accedere alla VM tramite RDP, è necessario disporre di una regola di sicurezza che consenta l'accesso RDP sulla porta 3389. Poiché il disco rigido virtuale per la nuova macchina virtuale è stato creato da una VM specializzata esistente, è possibile usare un account della macchina virtuale di origine per RDP.

In questo esempio il nome NSG impostato è **myNsg**, mentre il nome della regola RDP è **myRdpRule**.

```powershell
$nsgName = "myNsg"

$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name myRdpRule -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389
$nsg = New-AzureRmNetworkSecurityGroup `
   -ResourceGroupName $destinationResourceGroup `
   -Location $location `
   -Name $nsgName -SecurityRules $rdpRule
    
```

Per altre informazioni sugli endpoint e sulle regole NSG, vedere [Apertura di porte a una VM tramite PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

### <a name="create-a-public-ip-address-and-nic"></a>Creare un indirizzo IP pubblico e NIC
Per abilitare la comunicazione con la macchina virtuale nella rete virtuale, sono necessari un [indirizzo IP pubblico](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) e un'interfaccia di rete.

Creare l'IP pubblico. In questo esempio, il nome dell'indirizzo IP pubblico è **myIP**.
   
```powershell
$ipName = "myIP"
$pip = New-AzureRmPublicIpAddress `
   -Name $ipName -ResourceGroupName $destinationResourceGroup `
   -Location $location `
   -AllocationMethod Dynamic
```       

Creare la scheda NIC. In questo esempio, il nome specificato della scheda NIC è **myNicName**.
   
```powershell
$nicName = "myNicName"
$nic = New-AzureRmNetworkInterface -Name $nicName `
   -ResourceGroupName $destinationResourceGroup `
   -Location $location -SubnetId $vnet.Subnets[0].Id `
   -PublicIpAddressId $pip.Id `
   -NetworkSecurityGroupId $nsg.Id
```



### <a name="set-the-vm-name-and-size"></a>Impostare il nome e le dimensioni della macchina virtuale

Questo esempio imposta il nome della macchina virtuale su *myVM* e le dimensioni su *Standard_A2*.

```powershell
$vmName = "myVM"
$vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize "Standard_A2"
```

### <a name="add-the-nic"></a>Aggiungere la scheda di interfaccia di rete
    
```powershell
$vm = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic.Id
```
    

### <a name="add-the-os-disk"></a>Aggiungere il disco del sistema operativo 

Aggiungere il disco del sistema operativo alla configurazione usando [Set-AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk). In questo esempio le dimensioni del disco vengono impostate su *128 GB* e viene collegato il disco gestito come disco del sistema operativo *Windows*.
 
```powershell
$vm = Set-AzureRmVMOSDisk -VM $vm -ManagedDiskId $osDisk.Id -StorageAccountType Standard_LRS `
    -DiskSizeInGB 128 -CreateOption Attach -Windows
```

### <a name="complete-the-vm"></a>Completare la VM 

Creare la macchina virtuale usando le configurazioni [New-AzureRMVM](/powershell/module/azurerm.compute/new-azurermvm) appena create.

```powershell
New-AzureRmVM -ResourceGroupName $destinationResourceGroup -Location $location -VM $vm
```

Se il comando ha esito positivo, viene visualizzato un output simile al seguente:

```powershell
RequestId IsSuccessStatusCode StatusCode ReasonPhrase
--------- ------------------- ---------- ------------
                         True         OK OK   

```

### <a name="verify-that-the-vm-was-created"></a>Verificare che la VM sia stata creata
La VM appena creata verrà visualizzata nel [portale di Azure](https://portal.azure.com) in **Sfoglia** > **Macchine virtuali** oppure usando i comandi di PowerShell seguenti:

```powershell
$vmList = Get-AzureRmVM -ResourceGroupName $destinationResourceGroup
$vmList.Name
```

## <a name="next-steps"></a>Passaggi successivi
Accedere alla nuova macchina virtuale. Per altre informazioni, vedere [Come connettersi e accedere a una macchina virtuale di Azure che esegue Windows](connect-logon.md).

