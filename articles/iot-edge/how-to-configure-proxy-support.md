---
title: Configurare i dispositivi Azure IoT Edge per i proxy di rete | Microsoft Docs
description: Come configurare il runtime Azure IoT Edge e tutti i moduli IoT Edge con connessione Internet per comunicare tramite un server proxy.
author: kgremban
manager: ''
ms.author: kgremban
ms.date: 09/13/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: cf8f6197c65b0169e2bc61f46ab4a22f212512a6
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46996724"
---
# <a name="configure-an-iot-edge-device-to-communicate-through-a-proxy-server"></a>Configurare un dispositivo IoT Edge per comunicare tramite un server proxy

I dispositivi IoT Edge inviano le richieste HTTPS per comunicare con l'hub IoT. Se il dispositivo è connesso a una rete che usa un server proxy, è necessario configurare il runtime IoT Edge per comunicare tramite il server. I server proxy possono influire anche sui singoli moduli IoT Edge se vengono effettuate richieste HTTP o HTTPS che non vengono indirizzate tramite l'hub di Edge. 

Per configurare un dispositivo IoT Edge da usare con un server proxy, seguire questi passaggi di base: 

1. Configurare il daemon Docker e il daemon IoT Edge nel dispositivo per usare il server proxy.
2. Configurare le proprietà di edgeAgent nel file config.yaml nel dispositivo.
3. Impostare le variabili di ambiente per il runtime IoT Edge e altri moduli IoT Edge nel manifesto della distribuzione. 

## <a name="configure-the-daemons"></a>Configurare i daemon

I daemon Docker e IoT Edge in esecuzione nel dispositivo IoT Edge devono essere configurati per usare il server proxy. Il daemon Docker esegue richieste Web per eseguire il pull di immagini di contenitori dai registri contenitori. Il daemon IoT Edge esegue richieste Web per comunicare con l'hub IoT.

### <a name="docker-daemon"></a>Daemon docker

Vedere la documentazione di Docker per configurare il daemon Docker con le variabili di ambiente. La maggior parte dei registri contenitori (compresi DockerHub e registri contenitori Azure) supporta le richieste HTTPS, per cui la variabile da impostare è **HTTPS_PROXY**. Se si esegue il pull di immagini da un registro che non supporta il protocollo TLS (Transport Layer Security), potrà essere necessario impostare **HTTP_PROXY**. 

Scegliere l'articolo che si applica alla versione Docker usata: 

* [Docker](https://docs.docker.com/config/daemon/systemd/#httphttps-proxy)
* [Docker per Windows](https://docs.docker.com/docker-for-windows/#proxies)

### <a name="iot-edge-daemon"></a>Daemon IoT Edge

Il daemon IoT Edge è configurato in modo simile al daemon Docker. Tutte le richieste inviate da IoT Edge all'hub IoT usano HTTPS. Usare la procedura seguente per impostare una variabile di ambiente per il servizio, in base al sistema operativo usato. 

#### <a name="linux"></a>Linux

Aprire un editor nel terminale per configurare il daemon IoT Edge. 

```bash
sudo systemctl edit iotedge
```

Immettere il testo seguente, sostituendo l'**\<URL del proxy>** con la porta e l'indirizzo del server proxy. Quindi, salvare e chiudere. 

```text
[Service]
Environment="https_proxy=<proxy URL>"
```

Aggiornare Service Manager in modo da scegliere la nuova configurazione per iotedge.

```bash
sudo systemctl daemon-reload
```

Riavviare IoT Edge per rendere effettive le modifiche apportate.

```bash
sudo systemctl restart iotedge
```

Verificare che la variabile di ambiente sia stata creata e che la nuova configurazione sia stata caricata. 

```bash
systemctl show --property=Environment iotedge
```

#### <a name="windows"></a>Windows

Aprire una finestra di PowerShell come amministratore ed eseguire il comando seguente per modificare il registro con la nuova variabile di ambiente. Sostituire l'**\<UEL del proxy>** con la porta e l'indirizzo del server proxy. 

```powershell
reg add HKLM\SYSTEM\CurrentControlSet\Services\iotedge /v Environment /t REG_MULTI_SZ /d https_proxy=<proxy URL>
```

Riavviare IoT Edge per rendere effettive le modifiche apportate.

```powershell
Restart-Service iotedge
```

## <a name="configure-the-edge-agent"></a>Configurare l'agente Edge

L'agente Edge è il primo modulo che viene avviato in un dispositivo IoT Edge. Viene avviato per la prima volta in base alle informazioni presenti nel file config.yaml di IoT Edge. L'agente Edge si connette quindi all'hub IoT per recuperare i manifesti della distribuzione, che dichiarano quali altri moduli devono essere distribuiti nel dispositivo.

Aprire il file config.yaml nel dispositivo IoT Edge. Nei sistemi Linux questo file si trova in **/etc/iotedge/config.yaml**. Nei sistemi Windows questo file si trova in **C:\ProgramData\iotedge\config.yaml**. Il file di configurazione è protetto, pertanto sono necessari i privilegi amministrativi per accedervi. Nei sistemi Linux sarà quindi necessario usare il comando `sudo` prima di aprire il file nell'editor di testo preferito. In Windows sarà necessario aprire un editor di testo come Blocco note da eseguire come amministratore e quindi aprire il file. 

Nel file config.yaml trovare la sezione **Edge Agent module spec**. La definizione dell'agente Edge include un parametro **env** in cui è possibile aggiungere le variabili di ambiente. 

![Definizione di edgeAgent](./media/how-to-configure-proxy-support/edgeagent-unedited.png)

Rimuovere le parentesi graffe che sono un segnaposto per il parametro env e aggiungere la nuova variabile in una nuova riga. Tenere presente che i rientri in YAML sono rappresentati da due spazi. 

```yaml
https_proxy: "<proxy URL>"
```

Per impostazione predefinita, il runtime IoT Edge usa AMQP per comunicare con l'hub IoT. Alcuni server proxy bloccano le porte AMQP. In tal caso, sarà anche necessario configurare edgeAgent per usare AMQP su WebSocket. Aggiungere una seconda variabile di ambiente.

```yaml
UpstreamProtocol: "AmqpWs"
```

![definizione di edgeAgent con variabili di ambiente](./media/how-to-configure-proxy-support/edgeagent-edited.png)

Salvare le modifiche apportate a config.yaml e chiudere l'editor. Riavviare IoT Edge per rendere effettive le modifiche apportate. 

* Linux: 

   ```bash
   sudo systemctl restart iotedge
   ```

* Windows:

   ```powershell
   Restart-Service iotedge
   ```

## <a name="configure-deployment-manifests"></a>Configurare i manifesti della distribuzione  

Dopo aver configurato il dispositivo IoT Edge per l'uso con il server proxy, è necessario anche dichiarare le variabili di ambiente in tutti i manifesti della distribuzione futuri. I due moduli di runtime, edgeAgent ed edgeHub, devono avere sempre il server proxy configurato per mantenere la comunicazione con l'hub IoT. È possibile configurare un modulo IoT Edge per comunicare tramite un server proxy, ma non è necessario per i moduli che instradare i messaggi tramite edgeHub o che comunicano solo con altri moduli nel dispositivo. 

È possibile creare i manifesti della distribuzione usando il portale di Azure o manualmente modificando un file JSON. 

### <a name="azure-portal"></a>Portale di Azure

Quando si usa la procedura guidata **Imposta moduli** per creare le distribuzioni per i dispositivi IoT Edge, ogni modulo avrà una sezione **Variabili di ambiente** che è possibile usare per configurare le connessioni al server proxy. 

Per configurare i moduli dell'agente Edge e dell'hub Edge, selezionare **Configura impostazioni avanzate per il runtime di IoT Edge** nel primo passaggio della procedura guidata. 

![Configura impostazioni avanzate per il runtime di IoT Edge](./media/how-to-configure-proxy-support/configure-runtime.png)

Aggiungere la variabile di ambiente **https_proxy** alle definizioni dei moduli sia dell'agente Edge che dell'hub Edge. Se nel file config.yaml è stata inclusa la variabile di ambiente **UpstreamProtocol** nel dispositivo IoT Edge, aggiungerla anche alla definizione del modulo dell'agente Edge. 

![Impostare le variabili di ambiente](./media/how-to-configure-proxy-support/edgehub-environmentvar.png)

Tutti gli altri moduli che vengono aggiunti a un manifesto della distribuzione seguono lo stesso criterio. Nella pagina in cui si imposta il nome del modulo e l'immagine, è presente una sezione variabili di ambiente.

### <a name="json-deployment-manifest-files"></a>File JSON del manifesto della distribuzione

Se le distribuzioni per i dispositivi IoT Edge vengono create usando i modelli in Visual Studio Code o creando manualmente i file JSON, è possibile aggiungere le variabili di ambiente direttamente a ogni definizione del modulo. 

Usare il formato JSON seguente: 

```json
"env": {
    "https_proxy": {
        "value": "<proxy URL>"
    }
}
```

Con le variabili di ambiente incluse la definizione del modulo sarà simile a quella illustrata nell'esempio di edgeHub seguente:

```json
"edgeHub": {
    "type": "docker",
    "settings": {
        "image": "mcr.microsoft.com/azureiotedge-hub:1.0",
        "createOptions": ""
    },
    "env": {
        "https_proxy": {
            "value": "https://proxy.example.com:3128"
        }
    },
    "status": "running",
    "restartPolicy": "always"
}
```

Se nel file config.yaml è stata inclusa la variabile di ambiente **UpstreamProtocol** nel dispositivo IoT Edge, aggiungerla anche alla definizione del modulo dell'agente Edge. 

```json
"env": {
    "https_proxy": {
        "value": "<proxy URL"
    },
    "UpstreamProtocol": {
        "value": "AmqpWs"
    }
}
```

## <a name="next-steps"></a>Passaggi successivi

Altre informazioni sui ruoli del [runtime IoT Edge](iot-edge-runtime.md).

Risolvere gli errori di installazione e configurazione con [Problemi comuni e soluzioni per Azure IoT Edge](troubleshoot.md)

