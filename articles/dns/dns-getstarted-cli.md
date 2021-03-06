---
title: "Guida introduttiva: Creare una zona e un record DNS di Azure con l'interfaccia della riga di comando di Azure"
description: "Guida introduttiva: Informazioni su come creare una zona e un record DNS in DNS di Azure. Si tratta di una guida dettagliata per creare e gestire la prima zona e il primo record DNS con l'interfaccia della riga di comando di Azure."
services: dns
author: vhorne
ms.service: dns
ms.topic: quickstart
ms.date: 7/16/2018
ms.author: victorh
ms.openlocfilehash: 3fb39558ff99c35786dedc133a9d1d1a450b5928
ms.sourcegitcommit: e32ea47d9d8158747eaf8fee6ebdd238d3ba01f7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/17/2018
ms.locfileid: "39090123"
---
# <a name="quickstart-create-an-azure-dns-zone-and-record-using-azure-cli"></a>Guida introduttiva: Creare una zona e un record DNS di Azure con l'interfaccia della riga di comando di Azure

Questo articolo illustra i passaggi per creare la prima zona e il primo record DNS con l'interfaccia della riga di comando di Azure disponibile per Windows, Mac e Linux. È anche possibile eseguire questi passaggi tramite il [portale di Azure](dns-getstarted-portal.md) o [Azure PowerShell](dns-getstarted-powershell.md).

Una zona DNS viene usata per ospitare i record DNS per un particolare dominio. Per iniziare a ospitare il dominio in DNS di Azure, è necessario creare una zona DNS per il nome di dominio. Ogni record DNS per il dominio viene quindi creato all'interno di questa zona DNS. Per pubblicare infine la zona DNS su Internet, è necessario configurare i server dei nomi per il dominio. Ogni passaggio è illustrato di seguito.

DNS di Azure supporta ora anche le zone DNS private, attualmente disponibili in anteprima pubblica. Per altre informazioni sulle zone DNS private, vedere [Using Azure DNS for private domains](private-dns-overview.md) (Uso di Azure DNS per domini privati). Per un esempio di come creare una zona DNS privata, vedere [Introduzione alle Zone private di DNS di Azure con l'interfaccia della riga di comando](./private-dns-getstarted-cli.md).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Se non si ha una sottoscrizione di Azure, creare un [account gratuito](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) prima di iniziare.

## <a name="create-the-resource-group"></a>Creare il gruppo di risorse.

Prima di creare la zona DNS, creare un gruppo di risorse per contenere la zona DNS:

```azurecli
az group create --name MyResourceGroup --location "East US"
```

## <a name="create-a-dns-zone"></a>Creare una zona DNS

Una zona DNS viene creata utilizzando il comando `az network dns zone create` . Per visualizzare la guida per questo comando, digitare `az network dns zone create -h`.

L'esempio seguente crea una zona DNS denominata *contoso.com* nel gruppo di risorse *MyResourceGroup*. Usare l'esempio per creare una zona DNS, sostituendo i valori con quelli personalizzati.

```azurecli
az network dns zone create -g MyResourceGroup -n contoso.com
```

## <a name="create-a-dns-record"></a>Creare un record DNS

Per creare un record DNS, usare il comando `az network dns record-set [record type] add-record`. Per informazioni, ad esempio per i record A, vedere `azure network dns record-set A add-record -h`.

L'esempio seguente crea un record con il nome relativo "www" nella zona DNS "contoso.com" nel gruppo di risorse "MyResourceGroup". Il nome completo del set di record sarà "www.contoso.com". Il tipo di record è "A" con indirizzo IP "1.2.3.4" e viene usato un valore TTL predefinito di 3600 secondi (1 ora).

```azurecli
az network dns record-set a add-record -g MyResourceGroup -z contoso.com -n www -a 1.2.3.4
```

## <a name="view-records"></a>Visualizzare i record

Per elencare i record DNS nella zona, usare:

```azurecli
az network dns record-set list -g MyResourceGroup -z contoso.com
```

## <a name="update-name-servers"></a>Aggiornare i server dei nomi

Dopo essersi assicurati che la zona e i record DNS siano stati configurati correttamente, è necessario configurare il nome di dominio in modo che usi i server dei nomi di DNS Azure. Ciò consente agli altri utenti su Internet di trovare i record DNS.

I server dei nomi per la zona vengono specificati tramite il comando `az network dns zone show`. Per visualizzare i nomi dei server dei nomi, usare l'output JSON, come mostrato nell'esempio seguente.

```azurecli
az network dns zone show -g MyResourceGroup -n contoso.com -o json

{
  "etag": "00000003-0000-0000-b40d-0996b97ed101",
  "id": "/subscriptions/a385a691-bd93-41b0-8084-8213ebc5bff7/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com",
  "location": "global",
  "maxNumberOfRecordSets": 5000,
  "name": "contoso.com",
  "nameServers": [
    "ns1-01.azure-dns.com.",
    "ns2-01.azure-dns.net.",
    "ns3-01.azure-dns.org.",
    "ns4-01.azure-dns.info."
  ],
  "numberOfRecordSets": 3,
  "resourceGroup": "myresourcegroup",
  "tags": {},
  "type": "Microsoft.Network/dnszones"
}
```

I server dei nomi devono essere configurati con il registrar dei nomi di dominio, in cui è stato acquistato il nome di dominio. Il registrar offrirà l'opzione per la configurazione dei server dei nomi per il dominio. Per altre informazioni, vedere [Delegare un dominio al servizio DNS Azure](dns-domain-delegation.md).

## <a name="delete-all-resources"></a>Eliminare tutte le risorse
 
Quando non sono più necessarie, è possibile eliminare tutte le risorse create in questa guida introduttiva eliminando il gruppo di risorse:

```azurecli
az group delete --name MyResourceGroup
```

## <a name="next-steps"></a>Passaggi successivi

Dopo aver creato la prima zona e il primo record DNS con l'interfaccia della riga di comando di Azure, è possibile creare i record per un'app Web in un dominio personalizzato.

> [!div class="nextstepaction"]
> [Creare record DNS per un'app Web in un dominio personalizzato](./dns-web-sites-custom-domain.md)
