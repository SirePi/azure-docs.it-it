---
title: Monitorare gli aggiornamenti in Azure Stack tramite l'endpoint con privilegi | Microsoft Docs
description: Informazioni su come usare l'endpoint con privilegi per monitorare lo stato di aggiornamento per i sistemi integrati di Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 449ae53e-b951-401a-b2c9-17fee2f491f1
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2018
ms.author: mabrigg
ms.openlocfilehash: 8f384a79811c9a9b104acb98c8f6b6e162946ab8
ms.sourcegitcommit: 974c478174f14f8e4361a1af6656e9362a30f515
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/20/2018
ms.locfileid: "42139452"
---
# <a name="monitor-updates-in-azure-stack-using-the-privileged-endpoint"></a>Monitorare gli aggiornamenti in Azure Stack tramite l'endpoint con privilegi

*Si applica a: i sistemi integrati di Azure Stack*

È possibile usare l'endpoint con privilegi per monitorare lo stato di avanzamento di un'operazione di aggiornamento di Azure Stack e riprendere un aggiornamento non riuscito eseguito dall'ultimo passaggio ha esito positivo devono Azure Stack portale non saranno più disponibili.  Tramite il portale di Azure Stack è il metodo consigliato per gestire gli aggiornamenti in Azure Stack.

I seguenti nuovi cmdlet di PowerShell per gestire gli aggiornamenti sono incluse nell'aggiornamento 1710 per i sistemi integrati di Azure Stack.

| Cmdlet  | DESCRIZIONE  |
|---------|---------|
| `Get-AzureStackUpdateStatus` | Restituisce lo stato dell'aggiornamento attualmente in esecuzione, completato o non riuscito. Fornisce lo stato di alto livello dell'operazione di aggiornamento e un documento XML che descrive sia il passaggio corrente e lo stato corrispondente. |
| `Resume-AzureStackUpdate` | Riprende un aggiornamento non riuscito nel punto in cui non è riuscita. In alcuni scenari, potrebbe essere necessario completare i passaggi di mitigazione dei rischi prima di riprendere l'aggiornamento.         |
| | |

## <a name="verify-the-cmdlets-are-available"></a>Verificare che siano disponibili i cmdlet
Poiché i cmdlet sono di nuovi nel pacchetto di aggiornamento 1710 per Azure Stack, il processo di aggiornamento 1710 deve ottenere un certo punto prima che sia disponibile la funzionalità di monitoraggio. In genere, i cmdlet sono disponibili se lo stato nel portale di amministrazione indica che l'aggiornamento 1710 è il **riavviare gli host di archiviazione** passaggio. In particolare, l'aggiornamento di cmdlet rientra **passaggio: esecuzione del passaggio 2.6 - PrivilegedEndpoint aggiornare elenco elementi consentiti**.

È anche possibile determinare se i cmdlet sono disponibili a livello di codice eseguendo una query di elenco dei comandi dall'endpoint con privilegi. A tale scopo, eseguire i comandi seguenti dall'host del ciclo di vita dell'hardware o da una Workstation con privilegi di accesso. Inoltre, assicurarsi che l'endpoint con privilegi è un host attendibile. Per altre informazioni, vedere il passaggio 1 [accedere all'endpoint con privilegi](azure-stack-privileged-endpoint.md#access-the-privileged-endpoint). 

1. Creare una sessione di PowerShell in una o più macchine virtuali ERCS nell'ambiente Azure Stack (*Prefix*-ERCS01, *prefisso*-ERCS02, o *prefisso*-ERCS03). Sostituire *prefisso* con la stringa di prefisso di macchina virtuale specifico per l'ambiente.

   ```powershell
   $cred = Get-Credential

   $pepSession = New-PSSession -ComputerName <Prefix>-ercs01 -Credential $cred -ConfigurationName PrivilegedEndpoint 
   ```
   Alla richiesta delle credenziali, usare il &lt; *dominio di Azure Stack*&gt;\cloudadmin account o un account membro del gruppo CloudAdmins. Per l'account CloudAdmin, immettere la stessa password che è stata specificata durante l'installazione per l'account di amministratore di dominio AzureStackAdmin.

2. Ottiene l'elenco completo dei comandi disponibili nell'endpoint con privilegi. 

   ```powershell
   $commands = Invoke-Command -Session $pepSession -ScriptBlock { Get-Command } 
   ```
3. Determinare se l'endpoint con privilegi è stato aggiornato.

   ```powershell
   $updateManagementModuleName = "Microsoft.Azurestack.UpdateManagement"
    if (($commands | ? Source -eq $updateManagementModuleName)) {
   Write-Host "Privileged endpoint was updated to support update monitoring tools."
    } else {
   Write-Host "Privileged endpoint has not been updated yet. Please try again later."
    } 
   ```

4. Elencare i comandi specifici al modulo Microsoft.AzureStack.UpdateManagement.

   ```powershell
   $commands | ? Source -eq $updateManagementModuleName 
   ```
   Ad esempio: 
   ```powershell
   $commands | ? Source -eq $updateManagementModuleName
   
   CommandType     Name                                               Version    Source                                                  PSComputerName
    -----------     ----                                               -------    ------                                                  --------------
   Function        Get-AzureStackUpdateStatus                         0.0        Microsoft.Azurestack.UpdateManagement                   Contoso-ercs01
   Function        Resume-AzureStackUpdate                            0.0        Microsoft.Azurestack.UpdateManagement                   Contoso-ercs01
   ``` 

## <a name="use-the-update-management-cmdlets"></a>Usare i cmdlet di gestione aggiornamenti

> [!NOTE]
> Eseguire i comandi seguenti dall'host del ciclo di vita dell'hardware o da una Workstation con privilegi di accesso. Inoltre, assicurarsi che l'endpoint con privilegi è un host attendibile. Per altre informazioni, vedere il passaggio 1 [accedere all'endpoint con privilegi](azure-stack-privileged-endpoint.md#access-the-privileged-endpoint).

### <a name="connect-to-the-privileged-endpoint-and-assign-session-variable"></a>Connettersi all'endpoint con privilegi e assegnare la variabile di sessione

Eseguire i comandi seguenti per creare una sessione di PowerShell in una o più macchine virtuali ERCS nell'ambiente Azure Stack (*Prefix*-ERCS01, *prefisso*-ERCS02, o *prefisso*-ERCS03) e assegnare una variabile di sessione.

```powershell
$cred = Get-Credential

$pepSession = New-PSSession -ComputerName <Prefix>-ercs01 -Credential $cred -ConfigurationName PrivilegedEndpoint 
```
 Alla richiesta delle credenziali, usare il &lt; *dominio di Azure Stack*&gt;\cloudadmin account o un account membro del gruppo CloudAdmins. Per l'account CloudAdmin, immettere la stessa password che è stata specificata durante l'installazione per l'account di amministratore di dominio AzureStackAdmin.

### <a name="get-high-level-status-of-the-current-update-run"></a>Ottenere lo stato generale della sequenza di aggiornamento corrente 

Per ottenere lo stato generale della sequenza di aggiornamento corrente, eseguire i comandi seguenti: 

```powershell
$statusString = Invoke-Command -Session $pepSession -ScriptBlock { Get-AzureStackUpdateStatus -StatusOnly }

$statusString.Value 
```

I valori possibili sono:

- In esecuzione
- Completed
- Operazione non riuscita 
- Canceled

È possibile eseguire questi comandi più volte per visualizzare lo stato più aggiornato. Si è autorizzati a ristabilire una connessione al nuovo controllo.

### <a name="get-the-full-update-run-status-with-details"></a>Ottenere l'aggiornamento completo di stato di esecuzione con i dettagli 

È possibile ottenere l'aggiornamento completo di riepilogo come stringa XML dell'esecuzione. È possibile scrivere la stringa in un file per un esame, o convertirlo in un documento XML e usare PowerShell per analizzarlo. Il comando seguente consente di analizzare il codice XML per ottenere un elenco gerarchico dei passaggi attualmente in esecuzione.

```powershell
[xml]$updateStatus = Invoke-Command -Session $pepSession -ScriptBlock { Get-AzureStackUpdateStatus }

$updateStatus.SelectNodes("//Step[@Status='InProgress']")
```

Nell'esempio seguente, il passaggio di primo livello (aggiornamento del Cloud) ha un piano figlio per aggiornare e riavviare l'host di archiviazione. Viene illustrato che il piano di riavviare gli host di archiviazione sta aggiornando il servizio di archiviazione Blob in uno degli host.

```powershell
[xml]$updateStatus = Invoke-Command -Session $pepSession -ScriptBlock { Get-AzureStackUpdateStatus }

$updateStatus.SelectNodes("//Step[@Status='InProgress']") 

    FullStepIndex : 2
    Index         : 2
    Name          : Cloud Update
    Description   : Perform cloud update.
    StartTimeUtc  : 2017-10-13T12:50:39.9020351Z
    Status        : InProgress
    Task          : Task
    
    FullStepIndex  : 2.9
    Index          : 9
    Name           : Restart Storage Hosts
    Description    : Restart Storage Hosts.
    EceErrorAction : Stop
    StartTimeUtc   : 2017-10-13T15:44:06.7431447Z
    Status         : InProgress
    Task           : Task
    
    FullStepIndex : 2.9.2
    Index         : 2
    Name          : PreUpdate ACS Blob Service
    Description   : Check function level, update deployment artifacts, configure Blob service settings
    StartTimeUtc  : 2017-10-13T15:44:26.0708525Z
    Status        : InProgress
    Task          : Task
```

### <a name="resume-a-failed-update-operation"></a>Riprendere un'operazione di aggiornamento non riuscito

Se l'aggiornamento non riesce, è possibile riprendere l'aggiornamento eseguito in cui è stata interrotta.

```powershell
Invoke-Command -Session $pepSession -ScriptBlock { Resume-AzureStackUpdate } 
```

## <a name="troubleshoot"></a>Risolvere problemi

L'endpoint con privilegi è disponibile in tutte le macchine virtuali ERCS nell'ambiente Azure Stack. Poiché non viene stabilita la connessione a un endpoint a disponibilità elevata, è possibile che si riscontrino occasionalmente interruzioni, avviso o messaggi di errore. Questi messaggi possono indicare che la sessione era disconnessa o che si è verificato un errore durante la comunicazione con il servizio ECE. Questo comportamento è previsto. È possibile ripetere l'operazione tra qualche minuto o crearne una nuova sessione di con privilegi endpoint su una delle altre macchine virtuali ERCS. 

## <a name="next-steps"></a>Passaggi successivi

- [Gestione degli aggiornamenti in Azure Stack](azure-stack-updates.md) 


