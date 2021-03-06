---
title: Informazioni sulla fattura per Azure | Microsoft Docs
description: Informazioni su come leggere e comprendere l'utilizzo e la fattura per la sottoscrizione di Azure
services: ''
documentationcenter: ''
author: tonguyen10
manager: tonguyen
editor: ''
tags: billing
ms.assetid: 32eea268-161c-4b93-8774-bc435d78a8c9
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/14/2018
ms.author: tonguyen
ms.openlocfilehash: 689ea9e0d029bb65bc579fc914c6ed3073b4a96b
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37064626"
---
# <a name="understand-your-bill-for-microsoft-azure"></a>Comprendere la fattura per Microsoft Azure
Per comprendere la fattura di Azure, confrontare la fattura con il file dei dettagli di utilizzo giornaliero e con i report di gestione dei costi nel portale di Azure.

>[!NOTE]
>Questo articolo non si applica ai clienti con contratto Enterprise Agreement (EA). Se si ha un contratto Enterprise Agreement (EA), [è possibile trovare la documentazione per la fatturazione in Enterprise Portal](https://ea.azure.com/helpdocs/understandingYourInvoice).  

Per ottenere la fattura in formato PDF e una copia del download del file dei dettagli di utilizzo giornaliero in formato CSV, vedere [Ottenere la fattura e i dati di utilizzo giornaliero di Azure](billing-download-azure-invoice-daily-usage-date.md). 

Per una spiegazione dettagliata dei termini e delle descrizioni nella fattura e nel file dei dettagli di utilizzo giornaliero, vedere [Understand terms on your Microsoft Azure invoice](billing-understand-your-invoice.md) (Comprendere i termini nella fattura di Microsoft Azure) e [Understand terms on your Microsoft Azure detailed usage](billing-understand-your-usage.md) (Comprendere i termini nel file dei dettagli di utilizzo giornaliero). 

Per informazioni sui report di gestione dei costi, vedere [Gestione dei costi del Portale di Azure](https://docs.microsoft.com/azure/billing/billing-getting-started).

## <a name="charges"></a>Come posso assicurarmi che gli addebiti nella fattura siano corretti?

>[!VIDEO https://www.youtube.com/embed/3YegFD769Pk]

Se nella fattura è presente un addebito su cui si vogliono ricevere altre informazioni, sono disponibili due opzioni.

### <a name="option-1-review-your-invoice-and-compare-the-usage-and-costs-with-the-detailed-usage-csv-file"></a>Opzione 1: Rivedere la fattura e confrontare l'utilizzo e i costi con il file dei dettagli di utilizzo in formato CSV

Il file con i dettagli di utilizzo in formato CSV illustra gli addebiti per periodo di fatturazione e utilizzo giornaliero. Per ottenere questi file con i dettagli di utilizzo in formato CSV, vedere [Ottenere la fattura e i dati di utilizzo giornaliero di Azure](https://docs.microsoft.com/azure/billing/billing-download-azure-invoice-daily-usage-date).

Gli addebiti relativi all'utilizzo vengono visualizzati a livello di contatore. I termini seguenti hanno lo stesso significato sia nella fattura che nel file con i dettagli di utilizzo. Ad esempio, il ciclo di fatturazione indicato nella fattura corrisponde al periodo di fatturazione indicato nel file con i dettagli di utilizzo.

 | Fattura (PDF) | Dettagli di utilizzo (CSV)|
 | --- | --- |
|Ciclo di fatturazione | Periodo di fatturazione |
 |NOME |Categoria misuratore |
 |type |Sottocategoria contatore |
 |Risorsa |Nome misuratore |
 |Region |Area misuratore |
 |Consumato |Quantità consumata |
 |Incluso |Quantità inclusa |
 |Fatturabile |Quantità in eccesso |

La sezione **Costi di utilizzo** nella fattura riporta il valore totale per ogni misuratore utilizzato durante il periodo di fatturazione. Ad esempio, lo screenshot seguente illustra l'addebito per l'utilizzo del servizio Utilità di pianificazione di Azure.

![Addebiti di utilizzo nella fattura](./media/billing-understand-your-bill/1.png)

La sezione **Statement** (Rendiconto) del file CSV con i dettagli di utilizzo indica lo stesso addebito. Sia la quantità indicata in *Consumato* che *Valore* hanno una corrispondenza nella fattura.

![Addebiti di utilizzo nel file CSV](./media/billing-understand-your-bill/2.png)

Per visualizzare una suddivisione di questo addebito su base giornaliera, andare alla sezione **Utilizzo giornaliero** del CSV. Filtrare "Utilità di pianificazione" in *Categoria misuratore* per visualizzare i giorni in cui il misuratore è stato usato e la quantità di consumo. Vengono elencate anche le informazioni *Risorsa* e *Gruppo di risorse* per il confronto. La somma dei valori in *Consumato* corrisponderà a quanto indicato nella fattura.

![Sezione Utilizzo giornaliero nel CSV](./media/billing-understand-your-bill/3.png)

Per ottenere il costo giornaliero, moltiplicare le quantità in *Consumato* con il valore *Tariffa* della sezione **Statement** (Rendiconto).

Per altre informazioni sulla fattura, vedere [Comprendere la fattura di Azure](billing-understand-your-invoice.md).

Per informazioni su ogni colonna del CSV, vedere [Informazioni sui dettagli di utilizzo di Azure](billing-understand-your-invoice.md).

### <a name="option-2-review-your-invoice-and-compare-with-the-usage-and-costs-in-the-azure-portal"></a>Opzione 2: Rivedere la fattura e confrontarla con l'utilizzo e i costi riportati nel portale di Azure

È anche possibile usare il portale di Azure per verificare gli addebiti. Il portale di Azure offre grafici di gestione dei costi per una rapida panoramica dell'utilizzo e degli addebiti nella fattura.

Per continuare con l'esempio precedente, vedere la [pagina Sottoscrizioni](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade), selezionare la sottoscrizione e quindi scegliere **Analisi dei costi**. È quindi possibile specificare l'intervallo di tempo e visualizzare i costi di utilizzo per il servizio Utilità di pianificazione di Azure.

![Visualizzazione dell'analisi dei costi nel Portale di Azure](./media/billing-understand-your-bill/4.png)

Per visualizzare la suddivisione dei costi giornalieri in **Cronologia dei costi**, fare clic sulla riga.

![Visualizzazione Cronologia dei costi nel portale di Azure](./media/billing-understand-your-bill/5.png)

Per altre informazioni, vedere [Evitare costi imprevisti con la gestione dei costi e alla fatturazione di Azure](billing-getting-started.md#costs).

## <a name="external"></a>A cosa si riferiscono gli addebiti per servizi esterni?
I servizi esterni, noti anche come ordini di Azure Marketplace, sono erogati da fornitori di servizi indipendenti e vengono fatturati separatamente. Questi addebiti non compaiono nella fattura di Azure. Per altre informazioni, vedere [Informazioni sugli addebiti per i servizi esterni](billing-understand-your-azure-marketplace-charges.md).

## <a name="payment"></a>Come si effettua il pagamento?

Se il metodo di pagamento è impostato su carta di credito o carta di debito, il pagamento viene addebitato automaticamente entro 10 giorni dal termine del periodo di fatturazione. Nell'estratto conto della carta di credito la voce sarà **MSFT Azure**.

Se si [paga tramite fattura](billing-how-to-pay-by-invoice.md), inviare il pagamento al destinatario elencato nella parte inferiore della fattura. Per altre informazioni, [contattare il supporto tecnico](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

## <a name="how-do-i-check-the-status-of-a-payment-made-by-credit-card"></a>Come si controlla lo stato di un pagamento effettuato con carta di credito?

[Creare un ticket di supporto](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) per richiedere lo stato del pagamento. 

## <a name="are-there-different-azure-customer-types-how-do-i-know-what-customer-type-i-am"></a>Sono disponibili diversi tipi di clienti Azure? Come posso sapere che tipo di cliente sono?
Esistono diversi tipi di clienti Azure. Per comprendere meglio la determinazione dei prezzi e la fattura, vedere le seguenti descrizioni relative ai tipi di cliente.

- **Enterprise**: i clienti Enterprise hanno sottoscritto un Contratto Enterprise con Azure per concludere impegni monetari negoziati e ottenere l'accesso alla determinazione dei prezzi personalizzata per le risorse di Azure.
- **Accesso Web diretto**: i clienti con accesso Web diretto non hanno sottoscritto alcun contratto personalizzato con Azure. Questi clienti hanno eseguito la sottoscrizione di Azure tramite azure.com e sono soggetti ai prezzi al pubblico per tutte le risorse Azure.
- **Provider di servizi cloud**: i provider di servizi Cloud sono in genere società assunte da un cliente finale per compilare soluzioni basate principalmente su Azure.

## <a name="why-dont-i-see-the-cost-the-resource-i-have-created-in-my-bill"></a>Per quale motivo non visualizzo il costo della risorsa creato nella fattura?
Azure non emette fatture basate direttamente sui costi delle risorse. La fatturazione avviene in base a uno o più contatori che consentono di tenere traccia dell'utilizzo di una risorsa nel suo intero ciclo di vita. Questi contatori vengono quindi usati per calcolare la fattura. Per altre informazioni sulle misurazioni in Azure, vedere di seguito.

## <a name="how-does-azure-charge-metering-work"></a>Come funzionano le misurazioni in Azure?
Quando si avvia una singola risorsa di Azure, ad esempio una macchina virtuale, verranno create anche una o più istanze di misurazione. Questi contatori vengono usati per tenere traccia dell'utilizzo delle risorse nel tempo. Ogni misuratore genera record di utilizzo che vengono quindi usati da Azure nel sistema di misurazione dei costi per calcolare la fattura. 

Ad esempio, in una singola macchina virtuale creata in Azure possono essere creati i contatori seguenti per tenere traccia dell'utilizzo:

- Ore di calcolo
- Ore indirizzo IP
- Importazione dati
- Trasferimento dati in uscita
- Managed Disks Standard
- Operazioni disco gestito Standard
- I/O Standard - Disco
- I/O Standard - Lettura BLOB in blocchi
- I/O Standard - Scrittura BLOB in blocchi
- I/O Standard - Eliminazione BLOB in blocchi

Dopo aver creato la macchina virtuale, ognuno dei contatori descritti sopra inizierà a emettere record di utilizzo. Questo utilizzo verrà quindi usato nel sistema di misurazione di Azure insieme al prezzo del contatore per determinare l'importo addebitato a un cliente.

> [!Note]
> I contatori di esempio precedenti possono essere solo un subset di contatori creato con una macchina virtuale.

## <a name="what-is-the-difference-between-azure-1st-party-charges-and-azure-marketplace-charges"></a>Qual è la differenza tra le spese per Azure di prime parti e gli addebiti per Marketplace?
Le spese per Azure di prime parti si riferiscono alle risorse direttamente sviluppate e offerte da Azure. Gli addebiti per Marketplace si riferiscono alle risorse che sono state create dai fornitori di software di terze parti e sono disponibili per l'uso tramite Marketplace. Ad esempio, un firewall Barracuda è una risorsa di Marketplace offerta da terze parti. Tutti gli addebiti per il firewall e i relativi contatori corrispondenti verranno visualizzati come addebiti Marketplace. 

## <a name="tips-for-cost-management"></a>Suggerimenti per la gestione dei costi
- Stimare i costi usando il [calcolatore dei prezzi](https://azure.microsoft.com/pricing/calculator/) e il [calcolatore del costo totale di proprietà](https://aka.ms/azure-tco-calculator) e ottenere [informazioni dettagliate sui prezzi per ogni servizio](https://azure.microsoft.com/pricing/).
- [Impostare avvisi di fatturazione](billing-set-up-alerts.md).
- [Controllare regolarmente utilizzo e costi nel portale di Azure](billing-getting-started.md#costs).

## <a name="need-help-contact-support"></a>Richiesta di assistenza Contattare il supporto tecnico.

Se si necessita ancora di assistenza, [contattare il supporto tecnico](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) per ottenere una rapida risoluzione del problema.
