---
title: Domande frequenti su Personality Chat - Servizi cognitivi Azure | Microsoft Docs
description: Domande frequenti su Personality Chat
services: cognitive-services
author: noellelacharite
manager: nolachar
ms.service: cognitive-services
ms.topic: faq
ms.date: 05/07/2018
ms.author: nolachar
comment: As a bot developer, I want my bot to be able to handle small talk in a consistent tone so that my bot appears more complete and conversational.
ms.openlocfilehash: 5304b25672f9951e004876dfd19af75ea7d86502
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35378389"
---
# <a name="frequently-asked-questions"></a>Domande frequenti

## <a name="why-doesnt-this-answer-every-question-i-ask-it-like-other-chat-bots"></a>Perché questo bot non risponde a tutte le domande che vengono poste, come altri chatbot?

Project Personality Chat consentirà di migliorare un bot con una breve conversazione che dimostra personalità e crea un'esperienza utente più completa. Questa funzionalità non è progettata per supportare lunghe conversazioni su argomenti non rilevanti per la funzione principale del bot. Anche se potrebbe rispondere a tutte le conversazioni, nella versione beta è limitata a domini di conversazione ridotti.

## <a name="how-can-i-customize-the-personality-to-suit-my-brand"></a>Come si può personalizzare la personalità in base alla propria identità di marchio?

Selezionare l'utente tipo più appropriato tra quelli predefiniti disponibili. Attualmente, è possibile modificare le risposte nella libreria editoriale per adattarle alle proprie esigenze. In futuro, sarà possibile caricare un set di enunciati di esempio dalla personalità scelta e trovare la versione più vicina per l'utente tipo. Esistono anche modi per ripetere il training del modello e personalizzarlo.

## <a name="is-this-service-powering-existing-intelligent-agents-such-as-zo"></a>Questo servizio è alla base di agenti intelligenti esistenti, come Zo?

I servizi alla base di Zo, Cortana e Project Personality Chat condividono alcune tecniche simili, ma sono stack diversi. In questo servizio sono state incorporate le conoscenze apprese dalle esperienze con Zo e Cortana.

## <a name="can-this-service-lead-to-bad-customer-experiences"></a>Questo servizio può causare esperienze spiacevoli per i clienti?

Per offrire un'esperienza più completa, Personality Chat può generare risposte oltre a quelle nel set di dati editoriale e prova a interpretare tutto l'input utente. Sono quindi possibili risposte apparentemente non corrette nel contesto. Sono stati implementati numerosi controlli per evitare risposte sfavorevoli, sulla base delle conoscenze acquisite per agenti intelligenti come Zo. Per impostazione predefinita, Project Personality Chat è impostato per rispondere solo a finalità dell'utente riconosciute. È consigliabile verificare se Project Personality Chat è adatto alle proprie esigenze. I clienti sono invitati a inviare commenti e suggerimenti se riscontrano situazioni che richiedono ulteriore training. Se si prevede di usare questo servizio con i clienti in futuro, è consigliabile implementare la registrazione anonimizzata per facilitare l'individuazione dei problemi con le interazioni con gli utenti in diretta.