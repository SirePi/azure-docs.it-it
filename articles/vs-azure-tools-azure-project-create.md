---
title: Creare un progetto di servizio cloud di Azure con Visual Studio | Microsoft Docs
description: Informazioni su come creare un progetto di servizio cloud di Azure con Visual Studio
services: visual-studio-online
author: ghogen
manager: douge
assetId: ec580df7-3dcc-45a9-a1d9-8c110678dfb5
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 03/21/2017
ms.author: ghogen
ms.openlocfilehash: 3e6ab2078c6b9233360d7cabdc49eddf557b9b57
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/29/2018
ms.locfileid: "43189241"
---
# <a name="creating-an-azure-cloud-service-project-with-visual-studio"></a>Creazione di un progetto di servizio cloud di Azure con Visual Studio
Gli strumenti di Azure per Visual Studio offrono un modello di progetto che consente di creare un [servizio cloud di Azure](/azure/cloud-services/cloud-services-choose-me), ovvero un semplice servizio di Azure per uso generico. Dopo aver creato il progetto, Visual Studio consente di configurare, eseguire il debug e distribuire il servizio cloud in Azure.

## <a name="steps-to-create-an-azure-cloud-service-project-in-visual-studio"></a>Procedura per creare un progetto di servizio cloud di Azure in Visual Studio
In questa sezione viene illustrata la creazione di un progetto di servizio cloud di Azure in Visual Studio con uno o più ruoli Web.  

1. Avviare Visual Studio come amministratore.

1. Nel menu principale, selezionare **File** > **Nuovo** > **Progetto**.

1. Selezionare **Cloud** dai nodi del modello del progetto di Visual C# o Visual Basic e selezionare **Servizio cloud di Azure** dall'elenco dei modelli.

    ![Nuovo servizio cloud di Azure](./media/vs-azure-tools-azure-project-create/new-project-wizard-for-cloud-service.png)

1. Specificare la versione di .NET Framework che si vuole usare per sviluppare il progetto.

1. Immettere un nome e un percorso per il progetto e un nome per la soluzione. 

1. Selezionare **OK**.

1. Nella finestra di dialogo **Nuovo servizio cloud Microsoft Azure** scegliere i ruoli che si desidera aggiungere e fare clic sul pulsante con la freccia destra per aggiungerli alla soluzione.

    ![Selezionare i ruoli del nuovo servizio cloud di Azure](./media/vs-azure-tools-azure-project-create/new-cloud-service.png)

1. Per rinominare un ruolo che è stato aggiunto, passare il mouse sul ruolo nella finestra di dialogo **Nuovo servizio cloud Microsoft Azure** e dal menu di scelta rapida, selezionare **Rinomina**. Una volta aggiunto, un ruolo può essere rinominato (in **Esplora soluzioni**) anche nella soluzione.

    ![Rinominare un ruolo dei servizi cloud di Azure](./media/vs-azure-tools-azure-project-create/new-cloud-service-rename.png)

Il progetto Azure in Visual Studio contiene le associazioni ai progetti di ruolo nella soluzione. Il progetto include anche i *file di definizione del servizio* e i *file di configurazione del servizio*:

- Il **file di definizione del servizio** definisce le impostazioni di runtime per l'applicazione e include i ruoli richiesti, gli endpoint e le dimensioni della macchina virtuale. 
- Il **file di configurazione del servizio** configura il numero delle istanze di un ruolo eseguite e i valori delle impostazioni definiti per un ruolo. 

Per altre informazioni su queste impostazioni, vedere [Configurare i ruoli di un servizio cloud di Azure con Visual Studio](vs-azure-tools-configure-roles-for-cloud-service.md).

## <a name="next-steps"></a>Passaggi successivi
- [Gestione dei ruoli nei progetti di servizi cloud di Azure con Visual Studio](./vs-azure-tools-cloud-service-project-managing-roles.md)
