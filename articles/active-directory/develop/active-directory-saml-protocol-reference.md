---
title: Informazioni di riferimento sul protocollo SAML in Azure AD | Microsoft Docs
description: Questo articolo offre una panoramica dei profili SAML Single Sign-On e Single Sign-Out in Azure Active Directory.
services: active-directory
documentationcenter: .net
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 88125cfc-45c1-448b-9903-a629d8f31b01
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: celested
ms.custom: aaddev
ms.reviewer: hirsin, dastrock
ms.openlocfilehash: 067924294838459c866a0603ab092d139f1e6331
ms.sourcegitcommit: 615403e8c5045ff6629c0433ef19e8e127fe58ac
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/06/2018
ms.locfileid: "39579232"
---
# <a name="how-azure-active-directory-uses-the-saml-protocol"></a>Uso del protocollo SAML in Azure Active Directory
Azure Active Directory (Azure AD) usa il protocollo SAML 2.0 per consentire alle applicazioni di offrire agli utenti un'esperienza di accesso Single Sign-On. I profili SAML [Single Sign-On](single-sign-on-saml-protocol.md) e [Single Sign-Out](single-sign-out-saml-protocol.md) di Azure AD specificano come vengono usati i protocolli, le associazioni e le asserzioni SAML nel servizio del provider di identità.

Il protocollo SAML richiede che il provider di identità (Azure AD) e il provider del servizio (l'applicazione) si scambino informazioni su se stessi.

Quando un'applicazione viene registrata in Azure AD, lo sviluppatore dell'app registra le informazioni relative alla federazione con Azure AD. Tali informazioni includono l'**URI di reindirizzamento** e l'**URI dei metadati** dell'applicazione.

Azure AD usa l'**URI dei metadati** del servizio cloud per recuperare la chiave di firma e l'URI di disconnessione. Se l'applicazione non supporta un URI dei metadati, lo sviluppatore deve contattare il supporto tecnico Microsoft per fornire l'URI di disconnessione e la chiave di firma.

Azure Active Directory espone endpoint di Single Sign-On e Single Sign-Out specifici del tenant e comuni (indipendenti dal tenant). Questi URL non sono semplici identificatori, ma rappresentano posizioni indirizzabili. È quindi possibile accedere all'endpoint per leggere i metadati.

* L'endpoint specifico del tenant si trova all'indirizzo `https://login.microsoftonline.com/<TenantDomainName>/FederationMetadata/2007-06/FederationMetadata.xml`. Il segnaposto *<TenantDomainName>* rappresenta un nome di dominio registrato o un GUID TenantID di un tenant di Azure AD. Ad esempio, i metadati di federazione del tenant contoso.com sono in: https://login.microsoftonline.com/contoso.com/FederationMetadata/2007-06/FederationMetadata.xml

* L'endpoint indipendente dal tenant si trova all'indirizzo `https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml`. Questo indirizzo dell'endpoint contiene **common** anziché l'ID o il nome di un domino tenant.

Per informazioni sui documenti di metadati della federazione pubblicati da Azure AD, vedere [Metadati della federazione](azure-ad-federation-metadata.md).
