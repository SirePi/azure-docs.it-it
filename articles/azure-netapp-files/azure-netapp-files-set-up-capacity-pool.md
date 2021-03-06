---
title: Configurare un pool di capacità per Azure NetApp Files | Microsoft Docs
description: Descrive come configurare un pool di capacità in modo da poter creare volumi al suo interno.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/28/2018
ms.author: b-juche
ms.openlocfilehash: 0e9203f5b4e2a9043e242b804c82017cf6fc3ee1
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/13/2018
ms.locfileid: "39010804"
---
# <a name="set-up-a-capacity-pool"></a>Configurare un pool di capacità
La configurazione di un pool di capacità consente di creare volumi al suo interno.  

## <a name="before-you-begin"></a>Prima di iniziare 
È necessario avere già creato un account di NetApp.   

[Creare un account di NetApp](azure-netapp-files-create-netapp-account.md)

## <a name="steps"></a>Passaggi 

1. Passare al pannello di gestione per l'account di NetApp e quindi, nel riquadro di spostamento, selezionare **Pool di capacità**.

2. Fare clic su **+ Aggiungi pool** per creare un nuovo pool di capacità.   
    Verrà visualizzata la finestra New Capacity Pool (Nuovo pool di capacità).

3. Specificare le informazioni seguenti per il nuovo pool di capacità:  
  * **Nome**  
    Specificare il nome per il pool di capacità.  
    Il nome del pool di capacità deve essere univoco per ogni account di NetApp.

  * **Livello di servizio**   
    Questo campo mostra le prestazioni di destinazione per il pool di capacità.  
    Attualmente è disponibile solo il livello di servizio Premium. 

  *  **Dimensioni**     
      Specificare le dimensioni del pool di capacità da acquistare.        
      Le dimensioni minime del pool di capacità sono di 4 TiB. È possibile creare un pool con dimensioni in multipli di 4 TiB.   
      
      ![Nuovo pool di capacità](../media/azure-netapp-files/azure-netapp-files-new-capacity-pool.png)

4. Fare clic su **OK**.

## <a name="next-steps"></a>Passaggi successivi 

1. [Creare un volume per Azure NetApp Files](azure-netapp-files-create-volumes.md)
2. [Configurare i criteri di esportazione per un volume (facoltativo)](azure-netapp-files-configure-export-policy.md)

