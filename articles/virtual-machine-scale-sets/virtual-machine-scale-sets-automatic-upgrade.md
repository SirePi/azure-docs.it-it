---
title: Aggiornamenti automatici del sistema operativo con i set di scalabilità di macchine virtuali di Azure | Microsoft Docs
description: Informazioni su come aggiornare automaticamente il sistema operativo nelle istanze di macchina virtuale in un set di scalabilità
services: virtual-machine-scale-sets
documentationcenter: ''
author: yeki
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/03/2018
ms.author: yeki
ms.openlocfilehash: 6b20ef98e008d9c5d984ba29eed894b1c5ec8c09
ms.sourcegitcommit: a5eb246d79a462519775a9705ebf562f0444e4ec
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/26/2018
ms.locfileid: "39263249"
---
# <a name="azure-virtual-machine-scale-set-automatic-os-upgrades"></a>Aggiornamenti automatici del sistema operativo per un set di scalabilità di macchine virtuali di Azure

L'aggiornamento automatico dell'immagine del sistema operativo è una funzionalità di anteprima per i set di scalabilità di macchine virtuali di Azure che consente di aggiornare automaticamente tutte le macchine virtuali all'immagine del sistema operativo più recente.

L'aggiornamento automatico del sistema operativo presenta le caratteristiche seguenti:

- Una volta configurato, l'immagine del sistema operativo più recente pubblicata dagli editori di immagini viene applicata automaticamente al set di scalabilità senza l'intervento dell'utente.
- Aggiorna batch di istanze in sequenza ogni volta che viene pubblicata una nuova immagine della piattaforma dall'editore.
- Si integra con i probe di integrità delle applicazioni (facoltativo, ma consigliato per motivi di sicurezza).
- È adatto per le macchine virtuali di qualsiasi dimensione.
- È adatto per le immagini della piattaforma Windows e Linux.
- È possibile rifiutare esplicitamente gli aggiornamenti automatici in qualsiasi momento. Gli aggiornamenti del sistema operativo possono anche essere avviati manualmente.
- Il disco del sistema operativo di una macchina virtuale viene sostituito con il nuovo disco del sistema operativo creato con una versione più recente dell'immagine. Le estensioni configurate e gli script di dati personalizzati vengono eseguiti, mentre i dischi dati persistenti vengono mantenuti.


## <a name="preview-notes"></a>Note sull'anteprima 
Nella versione di anteprima si applicano le limitazioni e restrizioni seguenti:

- Gli aggiornamenti automatici del sistema operativo supportano solo [quattro SKU del sistema operativo](#supported-os-images). Non sono previsti contratti di servizio o garanzie. È consigliabile non usare gli aggiornamenti automatici sui carichi di lavoro critici di produzione durante l'anteprima.
- La crittografia dei dischi di Azure attualmente **non** supporta l'aggiornamento automatico del sistema operativo dei set di scalabilità di macchine virtuali.


## <a name="register-to-use-automatic-os-upgrade"></a>Eseguire la registrazione per usare l'aggiornamento automatico del sistema operativo
Per usare la funzionalità di aggiornamento automatico del sistema operativo, registrare il provider di anteprima con Azure Powershell o l'interfaccia della riga di comando di Azure 2.0.

### <a name="powershell"></a>PowerShell

1. Registrare con [Register-AzureRmProviderFeature](/powershell/module/azurerm.resources/register-azurermproviderfeature):

     ```powershell
     Register-AzureRmProviderFeature -ProviderNamespace Microsoft.Compute -FeatureName AutoOSUpgradePreview
     ```

2. Occorrono circa 10 minuti perché lo stato di registrazione venga segnalato come *Registered* (Registrato). È possibile controllare lo stato di registrazione corrente con [Get-AzureRmProviderFeature](/powershell/module/AzureRM.Resources/Get-AzureRmProviderFeature). 

3. Dopo aver eseguito la registrazione, verificare che il provider *Microsoft.Compute* sia registrato. L'esempio seguente usa Azure Powershell con [Register-AzureRmResourceProvider](/powershell/module/AzureRM.Resources/Register-AzureRmResourceProvider):

     ```powershell
     Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Compute
     ```


### <a name="cli-20"></a>Interfaccia della riga di comando 2.0

1. Registrare con [az feature register](/cli/azure/feature#az-feature-register):

     ```azurecli
     az feature register --name AutoOSUpgradePreview --namespace Microsoft.Compute
     ```

2. Occorrono circa 10 minuti perché lo stato di registrazione venga segnalato come *Registered* (Registrato). È possibile controllare lo stato di registrazione corrente con [az feature show](/cli/azure/feature#az-feature-show). 
 
3. Dopo aver eseguito la registrazione, assicurarsi che il provider *Microsoft.Compute* sia registrato. L'esempio seguente usa l'interfaccia della riga di comando di Azure 2.0.20 o versione successiva con [az provider register](/cli/azure/provider#az-provider-register):

     ```azurecli
     az provider register --namespace Microsoft.Compute
     ```

> [!NOTE]
> I cluster di Service Fabric hanno un concetto specifico di integrità dell'applicazione, ma i set di scalabilità senza Service Fabric usano il probe di integrità di bilanciamento del carico per monitorare l'integrità dell'applicazione. 
>
> ### <a name="azure-powershell"></a>Azure Powershell
>
> 1. Registrare la funzionalità del provider per i probe di integrità con [Register-AzureRmProviderFeature](/powershell/module/azurerm.resources/register-azurermproviderfeature):
>
>      ```powershell
>      Register-AzureRmProviderFeature -ProviderNamespace Microsoft.Network -FeatureName AllowVmssHealthProbe
>      ```
>
> 2. Anche in questo caso, occorrono circa 10 minuti perché lo stato di registrazione venga segnalato come *Registered* (Registrato). È possibile controllare lo stato di registrazione corrente con [Get-AzureRmProviderFeature](/powershell/module/AzureRM.Resources/Get-AzureRmProviderFeature)
>
> 3. Dopo aver eseguito la registrazione, verificare che il provider *Microsoft.Network* sia registrato con [Register-AzureRmResourceProvider](/powershell/module/AzureRM.Resources/Register-AzureRmResourceProvider):
>
>      ```powershell
>      Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network
>      ```
>
>
> ### <a name="cli-20"></a>Interfaccia della riga di comando 2.0
>
> 1. Registrare la funzionalità del provider per i probe di integrità con [az feature register](/cli/azure/feature#az-feature-register):
>
>      ```azurecli
>      az feature register --name AllowVmssHealthProbe --namespace Microsoft.Network
>      ```
>
> 2. Anche in questo caso, occorrono circa 10 minuti perché lo stato di registrazione venga segnalato come *Registered* (Registrato). È possibile controllare lo stato di registrazione corrente con [az feature show](/cli/azure/feature#az-feature-show). 
>
> 3. Dopo aver eseguito la registrazione, verificare che il provider *Microsoft.Network* sia registrato con [az provider register](/cli/azure/provider#az-provider-register) come segue:
>
>      ```azurecli
>      az provider register --namespace Microsoft.Network
>      ```

## <a name="portal-experience"></a>Funzionalità del portale
Dopo aver seguito la procedura di registrazione descritta in precedenza, è possibile passare al [portale di Azure](https://aka.ms/managed-compute) per abilitare gli aggiornamenti automatici del sistema operativo per i set di scalabilità e visualizzare lo stato degli aggiornamenti:

![](./media/virtual-machine-scale-sets-automatic-upgrade/automatic-upgrade-portal.png)


## <a name="supported-os-images"></a>Immagini del sistema operativo supportate
Attualmente sono supportate solo alcune immagini della piattaforma del sistema operativo. Al momento non è possibile usare immagini personalizzate create autonomamente. La proprietà *version* dell'immagine della piattaforma deve essere impostata su *latest*.

Attualmente sono supportati i seguenti SKU (ne verranno aggiunti altri a breve):
    
| Editore               | Offerta         |  Sku               | Version  |
|-------------------------|---------------|--------------------|----------|
| Canonical               | UbuntuServer  | 16.04-LTS          | più recenti   |
| MicrosoftWindowsServer  | WindowsServer | 2012-R2-Datacenter | più recenti   |
| MicrosoftWindowsServer  | WindowsServer | 2016-Datacenter    | più recenti   |
| MicrosoftWindowsServer  | WindowsServer | 2016-Datacenter-Smalldisk | più recenti   |
| MicrosoftWindowsServer  | WindowsServer | 2016-Datacenter-with-Containers | più recenti   |



## <a name="application-health-without-service-fabric"></a>Integrità dell'applicazione senza Service Fabric
> [!NOTE]
> Questa sezione si applica solo ai set di scalabilità senza Service Fabric. Service Fabric ha un concetto proprio di integrità dell'applicazione. Quando si usano gli aggiornamenti automatici del sistema operativo con Service Fabric, la nuova immagine del sistema operativo viene implementata in un dominio di aggiornamento alla volta per mantenere un'elevata disponibilità dei servizi in esecuzione in Service Fabric. Per altre informazioni sulle caratteristiche di durabilità dei cluster di Service Fabric, vedere [questa documentazione](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity#the-durability-characteristics-of-the-cluster).

Durante un aggiornamento del sistema operativo, le istanze di macchina virtuale in un set di scalabilità vengono aggiornate un batch alla volta. L'aggiornamento deve continuare solo se l'applicazione del cliente è integra nelle istanze di macchina virtuale aggiornate. Per questo motivo, l'applicazione deve offrire segnali di integrità al motore di aggiornamento del sistema operativo del set di scalabilità. Durante gli aggiornamenti del sistema operativo la piattaforma prende in considerazione lo stato di alimentazione della macchina virtuale e lo stato di provisioning dell'estensione per determinare se un'istanza di macchina virtuale è integra dopo un aggiornamento. Durante l'aggiornamento del sistema operativo di un'istanza di macchina virtuale, il disco del sistema operativo in un'istanza di macchina virtuale viene sostituito con un nuovo disco in base alla versione più recente dell'immagine. Una volta completato l'aggiornamento del sistema operativo, le estensioni configurate vengono eseguite in queste macchine virtuali. L'applicazione viene considerata integra solo dopo che è stato effettuato correttamente il provisioning di tutte le estensioni in una macchina virtuale. 

In aggiunta il set di scalabilità *deve* essere configurato con probe di integrità dell'applicazione per fornire alla piattaforma informazioni corrette sullo stato dell'applicazione. I probe di integrità delle applicazioni sono probe del servizio di bilanciamento del carico personalizzati usati come un segnale di integrità. L'applicazione in esecuzione in un'istanza di macchina virtuale del set di scalabilità può rispondere a richieste HTTP o TCP esterne per indicare se è integra. Per altre informazioni sul funzionamento dei probe di bilanciamento del carico personalizzati, vedere [Informazioni sui probe di bilanciamento del carico](../load-balancer/load-balancer-custom-probe-overview.md).

Se il set di scalabilità è configurato per l'uso di più gruppi di posizionamento, è necessario usare probe con un [servizio di bilanciamento del carico standard](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-overview).

### <a name="important-keep-credentials-up-to-date"></a>Importante: mantenere le credenziali aggiornate
Se il set di scalabilità usa credenziali per accedere alle risorse esterne, è necessario assicurarsi che le credenziali vengano mantenute aggiornate. Ad esempio, un'estensione macchina virtuale può essere configurata per usare un token di firma di accesso condiviso per l'account di archiviazione. In caso di scadenza delle credenziali, inclusi i certificati e i token, l'aggiornamento avrà esito negativo e il primo batch di macchine virtuali verrà lasciato in uno stato di errore.

Le procedure consigliate per recuperare le macchine virtuali e abilitare di nuovo l'aggiornamento automatico del sistema operativo se si verifica un errore di autenticazione delle risorse sono:

* Rigenerare il token (o qualsiasi altra credenziale) passato alle estensioni.
* Assicurarsi che eventuali credenziali usate all'interno della macchina virtuale per comunicare con le entità esterne siano aggiornate.
* Aggiornare le estensioni nel modello di set di scalabilità con eventuali nuovi token.
* Distribuire il set di scalabilità aggiornato, che aggiornerà tutte le istanze di macchina virtuale, incluse quelle in errore. 

### <a name="configuring-a-custom-load-balancer-probe-as-application-health-probe-on-a-scale-set"></a>Configurazione di un probe di bilanciamento del carico personalizzato come probe di integrità dell'applicazione in un set di scalabilità
È *necessario* creare un probe di bilanciamento del carico in modo esplicito per l'integrità del set di scalabilità. Può essere usato lo stesso endpoint per un probe HTTP o TCP esistente, ma un probe di integrità può richiedere un comportamento diverso da un probe di bilanciamento del carico tradizionale. Ad esempio un probe di bilanciamento del carico tradizionale può essere restituito non integro se il carico nell'istanza è troppo elevato. Potrebbe invece non essere appropriato per determinare l'integrità dell'istanza durante l'aggiornamento automatico del sistema operativo. Configurare il probe con un tasso di probing elevato, inferiore a due minuti.

È possibile fare riferimento al probe di bilanciamento del carico nell'impostazione *networkProfile* del set di scalabilità e il probe può essere associato a un servizio di bilanciamento del carico interno o pubblico, come segue:

```json
"networkProfile": {
  "healthProbe" : {
    "id": "[concat(variables('lbId'), '/probes/', variables('sshProbeName'))]"
  },
  "networkInterfaceConfigurations":
  ...
```


## <a name="enforce-an-os-image-upgrade-policy-across-your-subscription"></a>Applicare i criteri di aggiornamento dell'immagine del sistema operativo nella sottoscrizione
Per aggiornamenti sicuri, è consigliabile applicare criteri di aggiornamento. Questi criteri possono richiedere probe di integrità delle applicazioni nella sottoscrizione. I criteri seguenti di Azure Resource Manager rifiutano le distribuzioni per cui non sono configurate impostazioni automatizzate di aggiornamento dell'immagine del sistema operativo:

### <a name="powershell"></a>PowerShell
1. Ottenere la definizione dei criteri predefiniti di Azure Resource Manager con [Get-AzureRmPolicyDefinition](/powershell/module/AzureRM.Resources/Get-AzureRmPolicyDefinition) come segue:

    ```powershell
    $policyDefinition = Get-AzureRmPolicyDefinition -Id "/providers/Microsoft.Authorization/policyDefinitions/465f0161-0087-490a-9ad9-ad6217f4f43a"
    ```

2. Assegnare i criteri a una sottoscrizione con [New-AzureRmPolicyAssignment](/powershell/module/AzureRM.Resources/New-AzureRmPolicyAssignment) come segue:

    ```powershell
    New-AzureRmPolicyAssignment `
        -Name "Enforce automatic OS upgrades with app health checks" `
        -Scope "/subscriptions/<SubscriptionId>" `
        -PolicyDefinition $policyDefinition
    ```

### <a name="cli-20"></a>Interfaccia della riga di comando 2.0
Assegnare i criteri a una sottoscrizione con criteri predefiniti di Azure Resource Manager:

```azurecli
az policy assignment create --display-name "Enforce automatic OS upgrades with app health checks" --name "Enforce automatic OS upgrades" --policy 465f0161-0087-490a-9ad9-ad6217f4f43a --scope "/subscriptions/<SubscriptionId>"
```

## <a name="configure-auto-updates"></a>Configurare gli aggiornamenti automatici
Per configurare gli aggiornamenti automatici, verificare che *automaticOSUpgrade* sia impostato su *true* nella definizione del modello del set di scalabilità. È possibile configurare questa proprietà con Azure PowerShell o l'interfaccia della riga di comando di Azure 2.0.

### <a name="powershell"></a>PowerShell
Nell'esempio seguente viene usato Azure PowerShell (4.4.1 o versione successiva) per configurare gli aggiornamenti automatici per il set di scalabilità denominato *myVMSS* nel gruppo di risorse denominato *myResourceGroup*:

```powershell
$rgname = myResourceGroup
$vmssname = myVMSS
$vmss = Get-AzureRmVMss -ResourceGroupName $rgname -VmScaleSetName $vmssname
$vmss.UpgradePolicy.AutomaticOSUpgrade = $true
Update-AzureRmVmss -ResourceGroupName $rgname -VMScaleSetName $vmssname -VirtualMachineScaleSet $vmss
```

### <a name="cli-20"></a>Interfaccia della riga di comando 2.0
Nell'esempio seguente viene usata l'interfaccia della riga di comando di Azure (2.0.20 o versione successiva) per configurare gli aggiornamenti automatici per il set di scalabilità denominato *myVMSS* nel gruppo di risorse denominato *myResourceGroup*:

```azurecli
rgname="myResourceGroup"
vmssname="myVMSS"
az vmss update --name $vmssname --resource-group $rgname --set upgradePolicy.AutomaticOSUpgrade=true
```


## <a name="check-the-status-of-an-automatic-os-upgrade"></a>Controllare lo stato di un aggiornamento automatico del sistema operativo
È possibile controllare lo stato dell'aggiornamento del sistema operativo più recente eseguito nel set di scalabilità con Azure PowerShell, l'interfaccia della riga di comando di Azure 2.0 o le API REST.

### <a name="powershell"></a>PowerShell
Nell'esempio seguente viene usato Azure PowerShell (4.4.1 o versione successiva) per controllare lo stato per il set di scalabilità denominato *myVMSS* nel gruppo di risorse denominato *myResourceGroup*:

```powershell
Get-AzureRmVmssRollingUpgrade -ResourceGroupName myResourceGroup -VMScaleSetName myVMSS
```

### <a name="cli-20"></a>Interfaccia della riga di comando 2.0
Nell'esempio seguente viene usata l'interfaccia della riga di comando di Azure (2.0.20 o versione successiva) per controllare lo stato per il set di scalabilità denominato *myVMSS* nel gruppo di risorse denominato *myResourceGroup*:

```azurecli
az vmss rolling-upgrade get-latest --resource-group myResourceGroup --name myVMSS
```

### <a name="rest-api"></a>API REST
Nell'esempio seguente viene usata l'API REST per controllare lo stato per il set di scalabilità denominato *myVMSS* nel gruppo di risorse denominato *myResourceGroup*:

```
GET on `/subscriptions/subscription_id/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet/rollingUpgrades/latest?api-version=2017-03-30`
```

La chiamata GET restituisce proprietà simili all'output dell'esempio seguente:

```json
{
  "properties": {
    "policy": {
      "maxBatchInstancePercent": 20,
      "maxUnhealthyInstancePercent": 5,
      "maxUnhealthyUpgradedInstancePercent": 5,
      "pauseTimeBetweenBatches": "PT0S"
    },
    "runningStatus": {
      "code": "Completed",
      "startTime": "2017-06-16T03:40:14.0924763+00:00",
      "lastAction": "Start",
      "lastActionTime": "2017-06-22T08:45:43.1838042+00:00"
    },
    "progress": {
      "successfulInstanceCount": 3,
      "failedInstanceCount": 0,
      "inprogressInstanceCount": 0,
      "pendingInstanceCount": 0
    }
  },
  "type": "Microsoft.Compute/virtualMachineScaleSets/rollingUpgrades",
  "location": "southcentralus"
}
```


## <a name="automatic-os-upgrade-execution"></a>Esecuzione dell'aggiornamento automatico del sistema operativo
Per espandere l'uso dei probe di integrità delle applicazioni, gli aggiornamenti del sistema operativo dei set di scalabilità eseguono le operazioni seguenti:

1. Se più del 20% delle istanze non è integro, interrompere l'aggiornamento; in caso contrario, continuare.
2. Identificare il batch successivo di istanze di macchina virtuale per l'aggiornamento, dove un batch che comprende al massimo il 20% del numero totale di istanze.
3. Aggiornare il sistema operativo del batch successivo di istanze di macchina virtuale.
4. Se più del 20% delle istanze aggiornate non è integro, interrompere l'aggiornamento; in caso contrario, continuare.
5. Per i set di scalabilità che non fanno parte di un cluster di Service Fabric, l'aggiornamento rimane in attesa dell'integrità dei probe fino a 5 minuti, quindi procede immediatamente al batch successivo. Per i set di scalabilità che fanno parte di un cluster di Service Fabric, il set di scalabilità attende 30 minuti prima di passare al batch successivo.
6. Se sono presenti ulteriori istanze da aggiornare, procedere al passaggio 1) per il batch successivo; in caso contrario, l'aggiornamento è stato completato.

Il motore di aggiornamento del sistema operativo del set di scalabilità verifica l'integrità complessiva delle istanze di macchina virtuale prima di eseguire l'aggiornamento di ogni batch. Durante l'aggiornamento di un batch, possono essere in corso altre attività di manutenzione pianificate o non pianificate nei data center di Azure che potrebbero influire sulla disponibilità delle macchine virtuali. Di conseguenza, è possibile che temporaneamente più del 20% delle istanze risulti inattivo. In questi casi, alla fine del batch corrente, l'aggiornamento del set di scalabilità si interrompe.


## <a name="deploy-with-a-template"></a>Eseguire la distribuzione con un modello

È possibile usare il modello seguente per distribuire un set di scalabilità che usa gli aggiornamenti automatici <a href='https://github.com/Azure/vm-scale-sets/blob/master/preview/upgrade/autoupdate.json'>Aggiornamenti automatici in sequenza - Ubuntu 16.04-LTS</a>

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fvm-scale-sets%2Fmaster%2Fpreview%2Fupgrade%2Fautoupdate.json" target="_blank">
    <img src="http://azuredeploy.net/deploybutton.png"/>
</a>


## <a name="next-steps"></a>Passaggi successivi
Per altri esempi su come usare gli aggiornamenti automatici del sistema operativo con i set di scalabilità, vedere il [repository GitHub per le funzionalità di anteprima](https://github.com/Azure/vm-scale-sets/tree/master/preview/upgrade).
