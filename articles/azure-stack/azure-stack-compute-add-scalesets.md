---
title: Rendere il set di scalabilità di macchine virtuali disponibili in Azure Stack | Microsoft Docs
description: Informazioni su come un operatore cloud può aggiungere set di scalabilità di macchine virtuali in Azure Stack Marketplace
services: azure-stack
author: brenduns
manager: femila
editor: ''
ms.service: azure-stack
ms.topic: article
ms.date: 09/05/2018
ms.author: brenduns
ms.reviewer: kivenkat
ms.openlocfilehash: 3fbc3047688fed877280ca2d0f079ddea66bceb8
ms.sourcegitcommit: d211f1d24c669b459a3910761b5cacb4b4f46ac9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/06/2018
ms.locfileid: "44024732"
---
# <a name="make-virtual-machine-scale-sets-available-in-azure-stack"></a>Rendere il set di scalabilità di macchine virtuali disponibili in Azure Stack

*Si applica a: Azure Stack Development Kit e i sistemi integrati di Azure Stack*
  
Set di scalabilità di macchine virtuali sono una risorsa di calcolo di Azure Stack. È possibile utilizzarli per distribuire e gestire un set di macchine virtuali identiche. Con tutte le macchine virtuali configurato allo stesso modo, i set di scalabilità non necessitano di pre-provisioning di macchine virtuali. È più semplice creare servizi su larga scala destinati a big compute, big data e i carichi di lavoro in contenitori.

Questo articolo illustra il processo per rendere disponibili i set di scalabilità in Azure Stack Marketplace. Dopo aver completato questa procedura, gli utenti possono aggiungere set di scalabilità di macchine virtuali per le sottoscrizioni.

Set di scalabilità di macchine virtuali in Azure Stack sono simili a set di scalabilità di macchine virtuali in Azure. Per altre informazioni, vedere i video seguenti:
* [Mark Russinovich talks Azure scale sets](https://channel9.msdn.com/Blogs/Regular-IT-Guy/Mark-Russinovich-Talks-Azure-Scale-Sets/) (Mark Russinovich illustra i set di scalabilità di Azure)
* [Set di scalabilità di macchine virtuali con Guy Bowerman](https://channel9.msdn.com/Shows/Cloud+Cover/Episode-191-Virtual-Machine-Scale-Sets-with-Guy-Bowerman)

In Azure Stack, il set di scalabilità di macchine virtuali non supporta la scalabilità automatica. È possibile aggiungere più istanze per un set di scalabilità tramite PowerShell, CLI o modelli di Resource Manager.

## <a name="prerequisites"></a>Prerequisiti

- **Il Marketplace**  
    Registrare Azure Stack con Azure globale, per abilitare la disponibilità dei prodotti in Marketplace. Seguire le istruzioni in [registrare Azure Stack con Azure](azure-stack-registration.md).
- **Immagine del sistema operativo**  
  Prima di poter creare un set di scalabilità di macchine virtuali (VMSS), è necessario scaricare le immagini di macchina virtuale per l'utilizzo in set di scalabilità dal [Marketplace Azure Stack](azure-stack-download-azure-marketplace-item.md). Le immagini devono essere già presente prima che un utente può creare un nuovo set di scalabilità. 


## <a name="use-the-azure-stack-portal"></a>Usa il portale di Azure Stack 

>[!NOTE]  
> Le informazioni contenute in questa sezione si applicano quando si usa Azure Stack 1808 o versione successiva. Se la versione è 1807 o versioni precedenti, vedere [aggiungere il Set di scalabilità (antecedente a 1808)](#add-the-virtual-machine-scale-set-(prior-to-version-1808)).

1. Accedere al portale di Azure Stack. Quindi, passare alla **tutti i servizi** > **set di scalabilità di macchine virtuali**, quindi in *calcolo*selezionare **deisetdiscalabilitàdimacchinevirtuali**. 
   ![Set di scalabilità di macchine virtuali selezionate](media/azure-stack-compute-add-scalesets/all-services.png)

2. Selezionare Crea ***set di scalabilità di macchine virtuali***.
   ![Creare un set di scalabilità di macchine virtuali](media/azure-stack-compute-add-scalesets/create-scale-set.png)

3. Compilare i campi vuoti, scegliere tra gli elenchi a discesa per *immagine del sistema operativo*, *sottoscrizione*, e *dimensioni istanze*. Selezionare **Yes** per *Usa dischi gestiti*. Scegliere quindi **Create** (Crea).
    ![Configurare e creare](media/azure-stack-compute-add-scalesets/create.png)

4. Per visualizzare lo scalabilità di macchine virtuali nuove impostato, visitare **tutte le risorse**, eseguire la ricerca per nome del set di scalabilità di macchine virtuali e quindi selezionare il relativo nome nella ricerca. 
   ![Visualizzare il set di scalabilità](media/azure-stack-compute-add-scalesets/search.png)



## <a name="add-the-virtual-machine-scale-set-prior-to-version-1808"></a>Aggiungere il Set di scalabilità (precedenti alla versione 1808)
>[!NOTE]  
> Le informazioni contenute in questa sezione si applicano quando si usa una versione di Azure Stack precedente 1808. Se si usa 1808 o versione successiva, vedere [usare il portale di Azure Stack](#use-the-azure-stack-portal).

1. Aprire il Marketplace di Azure Stack e connettersi ad Azure. Selezionare **gestione Marketplace**> **+ Aggiungi da Azure**.

    ![Gestione di Marketplace](media/azure-stack-compute-add-scalesets/image01.png)

2. Aggiungere e scaricare l'elemento del marketplace Set di scalabilità di macchine virtuali.

    ![Set di scalabilità di macchine virtuali](media/azure-stack-compute-add-scalesets/image02.png)

## <a name="update-images-in-a-virtual-machine-scale-set"></a>Aggiornare le immagini in un Set di scalabilità di macchine virtuali

Dopo aver creato un set di scalabilità di macchine virtuali, gli utenti possono aggiornare le immagini in set di scalabilità senza dover essere ricreato di set di scalabilità. Il processo per aggiornare un'immagine dipende gli scenari seguenti:

1. Modello di distribuzione di set di scalabilità di macchine virtuali **specifica più recente** per *versione*:  

   Quando la *versione* viene impostato come **più recente** nel *imageReference* sezione del modello per una scala impostata, scalare le operazioni relative all'utilizzo di set di scalabilità la versione più recente disponibile Imposta le istanze dell'immagine per la scala. Dopo aver completato un scalabilità verticale, è possibile eliminare precedenti istanze di set di scalabilità di macchine virtuali.  (I valori per *server di pubblicazione*, *offrono*, e *sku* restano invariati). 

   L'esempio JSON seguente specifica *più recente*:  

    ```Json  
    "imageReference": {
        "publisher": "[parameters('osImagePublisher')]",
        "offer": "[parameters('osImageOffer')]",
        "sku": "[parameters('osImageSku')]",
        "version": "latest"
        }
    ```

   Prima di aumento delle prestazioni è possibile usare una nuova immagine, è necessario scaricare che la nuova immagine:  

   - Quando l'immagine in Marketplace è una versione più recente rispetto all'immagine del set di scalabilità: scaricare la nuova immagine che sostituisce l'immagine precedente. Dopo la sostituzione dell'immagine, un utente può continuare per aumentare le prestazioni. 

   - Quando la versione dell'immagine nel Marketplace è quello utilizzato per l'immagine del set di scalabilità: Elimina l'immagine è in uso nel set di scalabilità e quindi scaricare la nuova immagine. Durante l'intervallo tra la rimozione dell'immagine originale e il download della nuova immagine, è possibile scalare in verticale. 
      
     Questo processo è obbligatorio per resyndicate immagini che utilizzano il formato di file sparse, introdotto con la versione 1803. 
 

2. Modello di distribuzione di set di scalabilità di macchine virtuali **specificato non è più recente** per *versione* e specifica invece un numero di versione:  

    Se si scarica un'immagine con una versione più recente (che cambia la versione disponibile), il set di scalabilità non è possibile aumentare le prestazioni. È per impostazione predefinita la versione dell'immagine specificata nel modello di set di scalabilità deve essere disponibile.  

Per altre informazioni, vedere [dischi del sistema operativo e immagini](.\user\azure-stack-compute-overview.md#operating-system-disks-and-images).  


## <a name="scale-a-virtual-machine-scale-set"></a>Ridimensionare un set di scalabilità di macchine virtuali
È possibile scalare le dimensioni di un *set di scalabilità di macchine virtuali* per renderla superiori o inferiori.  

1. Nel portale, selezionare il set di scalabilità e quindi selezionare **Scaling**.
2. Usare il dispositivo di scorrimento per impostare il nuovo livello di scalabilità per questo set di scalabilità di macchine virtuali e quindi selezionare **salvare**.
     ![Il set di scalabilità](media/azure-stack-compute-add-scalesets/scale.png)





## <a name="remove-a-virtual-machine-scale-set"></a>Rimuovere un Set di scalabilità di macchine virtuali

Per rimuovere una macchina virtuale di elemento della raccolta di set di scalabilità, eseguire il comando PowerShell seguente:

```PowerShell  
    Remove-AzsGalleryItem
````

> [!NOTE]
> L'elemento della raccolta non può essere rimosso immediatamente. Si potrebbe essere necessario aggiornare il portale più volte prima che l'elemento viene illustrato come rimuovere dal Marketplace.

## <a name="next-steps"></a>Passaggi successivi
[Domande frequenti per Azure Stack](azure-stack-faq.md)