---
title: Come creare un file di dump ed eseguire il ripristino in Database di Azure per PostgreSQL
description: Descrive come estrarre un database PostgreSQL in un file di dump ed eseguire il ripristino da un file creato da pg_dump in Database di Azure per PostgreSQL.
services: postgresql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.topic: conceptual
ms.date: 07/19/2018
ms.openlocfilehash: 94d196ceecc0b63b9f0b0fe94f71363dc2086c30
ms.sourcegitcommit: 248c2a76b0ab8c3b883326422e33c61bd2735c6c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/23/2018
ms.locfileid: "39213651"
---
# <a name="migrate-your-postgresql-database-using-dump-and-restore"></a>Eseguire la migrazione del database PostgreSQL usando dump e ripristino
È possibile usare [pg_dump](https://www.postgresql.org/docs/9.3/static/app-pgdump.html) per estrarre un database PostgreSQL in un file di dump e [pg_restore](https://www.postgresql.org/docs/9.3/static/app-pgrestore.html) per ripristinare il database PostgreSQL da un file di archivio creato da pg_dump.

## <a name="prerequisites"></a>Prerequisiti
Per proseguire con questa guida, si richiedono:
- Un [server di Database di Azure per PostgreSQL](quickstart-create-server-database-portal.md) con le regole del firewall per consentire l'accesso e il database sottostante.
- Utilità della riga di comando [pg_dump](https://www.postgresql.org/docs/9.6/static/app-pgdump.html) e [pg_restore](https://www.postgresql.org/docs/9.6/static/app-pgrestore.html) installate

Seguire questi passaggi per eseguire il dump e il ripristino del database PostgreSQL:

## <a name="create-a-dump-file-using-pgdump-that-contains-the-data-to-be-loaded"></a>Creare un file di dump usando pg_dump che contiene i dati da caricare
Per eseguire il backup di un database PostgreSQL in locale o in una macchina virtuale, eseguire questo comando:
```bash
pg_dump -Fc -v --host=<host> --username=<name> --dbname=<database name> > <database>.dump
```
Se ad esempio è presente un server locale che contiene un database denominato **testdb**, eseguire:
```bash
pg_dump -Fc -v --host=localhost --username=masterlogin --dbname=testdb > testdb.dump
```

> [!IMPORTANT]
> Copiare i file di backup in un archivio/BLOB di Azure ed eseguire il ripristino da tale posizione. In questo modo, l'operazione dovrebbe essere eseguita in modo molto più veloce rispetto al ripristino attraverso Internet.
> 

## <a name="restore-the-data-into-the-target-azure-database-for-postrgesql-using-pgrestore"></a>Ripristinare i dati nel database di Azure per PostrgeSQL di destinazione usando pg_restore
Dopo avere creato il database di destinazione, è possibile usare il comando pg_restore e il parametro -d, --dbname per ripristinare i dati nel database di destinazione dal file di dump.
```bash
pg_restore -v --no-owner –-host=<server name> --port=<port> --username=<user@servername> --dbname=<target database name> <database>.dump
```
Se si include il parametro --no-owner, tutti gli oggetti creati durante il ripristino saranno di proprietà dell'utente specificato con --username. Per altre informazioni, vedere la documentazione ufficiale di PostgreSQL per [pg_restore](https://www.postgresql.org/docs/9.6/static/app-pgrestore.html).

> [!NOTE]
> Se il server PostgreSQL richiede connessioni SSL (attive per impostazione predefinita nel Database di Azure per i server PostgreSQL), impostare una variabile di ambiente `PGSSLMODE=require` in modo che lo strumento pg_restore si connetta a SSL. Senza SSL, l'errore può riportare `FATAL:  SSL connection is required. Please specify SSL options and retry.`
>
> Nella riga di comando di Windows, eseguire il comando `SET PGSSLMODE=require` prima di eseguire il comando pg_restore. Nella riga di comando di Linux o Bash, eseguire il comando `export PGSSLMODE=require` prima di eseguire il comando pg_restore.
>

In questo esempio i dati vengono ripristinati dal file di dump **testdb.dump** nel database **mypgsqldb** disponibile sul server di destinazione **mydemoserver.postgres.database.azure.com**. 
```bash
pg_restore -v --no-owner --host=mydemoserver.postgres.database.azure.com --port=5432 --username=mylogin@mydemoserver --dbname=mypgsqldb testdb.dump
```

## <a name="next-steps"></a>Passaggi successivi
- Per eseguire la migrazione di un database PostgreSQL tramite esportazione e importazione, vedere [Eseguire la migrazione del database PostgreSQL usando le funzionalità di esportazione e importazione](howto-migrate-using-export-and-import.md).
- Per altre informazioni sulla migrazione dei database in Database di Azure per PostgreSQL, vedere [Database Migration Guide](http://aka.ms/datamigration) (Guida alla migrazione di database).
