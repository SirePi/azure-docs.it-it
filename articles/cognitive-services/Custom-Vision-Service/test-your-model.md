---
title: Testare e ripeterne il training di un modello - Servizio visione artificiale personalizzato - Servizi cognitivi di Azure | Microsoft Docs
description: Informazioni su come testare un'immagine e quindi usarla per ripetere il training del modello.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: article
ms.date: 05/03/2018
ms.author: anroth
ms.openlocfilehash: 1933b1a45844ac99308baebe59b49687a957abfa
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35375268"
---
# <a name="test-and-retrain-a-model-with-custom-vision-service"></a>Test e ripetere il training di un modello con il Servizio visione artificiale personalizzato

Dopo avere eseguito il training del modello, è possibile testarlo rapidamente, usando un'immagine archiviata in locale o un'immagine online. Il test usa l'iterazione più recente di cui è stato eseguito il training.

## <a name="test-your-model"></a>Testare il modello

1. Selezionare il progetto nella [pagina Web di Visione personalizzata](https://customvision.ai). Selezionare **Quick Test** (Test rapido) a destra della barra dei menu in alto. Questa azione apre una finestra denominata **Quick Test** (Test rapido).

    ![Il pulsante Quick Test (Test rapido) viene visualizzato nell'angolo in alto a destra della finestra.](./media/test-your-model/quick-test-button.png)

2. Nella finestra **Quick Test** (Test rapido) fare clic sul campo **Submit Image** (Invia immagine) e immettere l'URL dell'immagine da usare per il test. Se invece si vuole usare invece un'immagine archiviata in locale, fare clic sul pulsante **Browse local files** (Esplora file locali) e selezionare un file di immagine locale.

    ![Immagine della pagina Submit image (Invia immagine)](./media/test-your-model/submit-image.png)

L'immagine selezionata verrà visualizzata al centro della pagina. I risultati vengono visualizzati quindi sotto l'immagine in formato tabella con due colonne denominate **Tags** (Tag) e **Confidence** (Attendibilità). Dopo aver visualizzato i risultati, è possibile chiudere la finestra **Quick Test** (Test rapido).

È possibile ora aggiungere questa immagine di test al modello e quindi ripetere il training del modello.

## <a name="use-the-predicted-image-for-training"></a>Usare l'immagine prevista per il training.

Per usare l'immagine inviata in precedenza per il training, usare la procedura seguente:

1. Per visualizzare le immagini inviate al classificatore aprire la [pagina Web di Visione personalizzata](https://customvision.ai) e selezionare la scheda __Predictions__ (Previsioni).

    ![Immagine della scheda Predictions (Previsioni)](./media/test-your-model/predictions-tab.png)

    > [!TIP]
    > La visualizzazione predefinita mostra le immagini dall'iterazione corrente. È possibile usare l'elenco a discesa __Iteration__ (Iterazione) per visualizzare le immagini inviate durante le iterazioni precedenti.

2. Passare il mouse su un'immagine per visualizzare i tag previsti dal classificatore.

    > [!TIP]
    > Le immagini vengono classificate in modo che quelle che possono portare più miglioramenti al classificatore siano al primo posto. Per selezionare un ordinamento diverso, usare la sezione __Sort__ (Ordinamento).

    Per aggiungere un'immagine ai dati di training, selezionare l'immagine, selezionare il tag e quindi selezionare __Save and close__ (Salva e chiudi). L'immagine viene rimossa da __Predictions__ (Previsioni) e aggiunta alle immagini di training. È possibile visualizzarla selezionando la scheda __Training Images__ (Immagini di training).

    ![Immagine della pagina dei tag](./media/test-your-model/tag-image.png)

3. Usare il pulsante __Train__ (Esegui il training) per ripetere il training del classificatore.

## <a name="next-steps"></a>Passaggi successivi

[Migliorare il classificatore](getting-started-improving-your-classifier.md)