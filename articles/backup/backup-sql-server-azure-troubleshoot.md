---
title: Guide de dépannage Sauvegarde Azure pour les machines virtuelles SQL Server | Microsoft Docs
description: Informations de dépannage pour la sauvegarde des machines virtuelles SQL Server sur Azure.
services: backup
documentationcenter: ''
author: rayne-wiselman
manager: carmonm
editor: ''
keywords: ''
ms.assetid: ''
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 06/19/2018
ms.author: anuragm
ms.custom: ''
ms.openlocfilehash: 89344b6e06dbc62fe56c0aebc30a049aebf5c097
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/13/2018
ms.locfileid: "53339516"
---
# <a name="troubleshoot-back-up-sql-server-on-azure"></a>Résoudre les problèmes de sauvegarde SQL Server sur Azure

Cet article fournit des informations de dépannage pour la protection des machines virtuelles SQL Server sur Azure (préversion).

## <a name="public-preview-limitations"></a>Limitations de la préversion publique

Pour consulter les limitations relatives à la préversion publique, reportez-vous à l’article [Sauvegarder une base de données SQL Server dans Azure](backup-azure-sql-database.md#public-preview-limitations).

## <a name="sql-server-permissions"></a>Autorisations SQL Server

Pour configurer la protection d’une base de données SQL Server sur une machine virtuelle, l’extension **AzureBackupWindowsWorkload** doit être installée sur la machine virtuelle concernée. Si vous recevez l’erreur **UserErrorSQLNoSysadminMembership**, cela signifie que votre instance SQL n’a pas les autorisations de sauvegarde requises. Pour corriger cette erreur, suivez les étapes indiquées dans [Définir des autorisations pour les machines virtuelles SQL autres que de la Place de marché](backup-azure-sql-database.md#set-permissions-for-non-marketplace-sql-vms).

## <a name="troubleshooting-errors"></a>Résolution des erreurs

Utilisez les informations des tableaux suivants pour résoudre les problèmes et les erreurs rencontrés lorsque vous protégez SQL Server sur Azure.

## <a name="backup-failures"></a>Échecs de sauvegarde

Les tableaux suivants sont organisés par code d’erreur.

### <a name="usererrorsqlpodoesnotsupportbackuptype"></a>UserErrorSQLPODoesNotSupportBackupType

| Message d’erreur | Causes possibles | Action recommandée |
|---|---|---|
| This SQL database does not support the requested backup type. (Cette base de données SQL ne prend pas en charge le type de sauvegarde demandé.) | Se produit lorsque le mode de récupération de la base de données n’autorise pas le type de sauvegarde demandé. L’erreur peut se produire dans les situations suivantes : <br/><ul><li>Une base de données utilisant un mode de récupération simple n’autorise pas la sauvegarde de fichier journal.</li><li>Les sauvegardes de fichier journal et différentielles ne sont pas autorisées pour une base de données MASTER.</li></ul>Pour plus d’informations, consultez le document [Modes de récupération (SQL Server)](https://docs.microsoft.com/sql/relational-databases/backup-restore/recovery-models-sql-server). | Si la sauvegarde de fichier journal échoue pour la base de données en mode de récupération simple, essayez l’une des options suivantes :<ul><li>Si la base de données est en mode de récupération simple, désactivez les sauvegardes de fichier journal.</li><li>Consultez la [documentation SQL](https://docs.microsoft.com/sql/relational-databases/backup-restore/view-or-change-the-recovery-model-of-a-database-sql-server) pour modifier le mode de récupération de la base de données en le définissant sur Complet ou Journalisation en bloc. </li><li> Si vous ne souhaitez pas modifier le mode de récupération et si vous disposez d’une stratégie standard pour sauvegarder plusieurs bases de données ne pouvant être changée, ignorez l’erreur. Vos sauvegardes complètes et différentielles fonctionneront par planification. Les sauvegardes de fichier journal seront ignorées, ce qui est attendu dans ce cas.</li></ul>S’il s’agit d’une base de données MASTER et si vous avez configuré la sauvegarde différentielle ou de fichier journal, suivez l’une des étapes ci-après :<ul><li>Utilisez le portail pour modifier la planification de la stratégie de sauvegarde pour la base de données MASTER en la définissant sur Complète.</li><li>Si vous disposez d’une stratégie standard pour sauvegarder plusieurs bases de données ne pouvant être changée, ignorez l’erreur. Votre sauvegarde complète fonctionnera par planification. Les sauvegardes différentielles ou de fichier journal n’auront pas lieu, ce qui est attendu dans ce cas.</li></ul> |
| Operation canceled as a conflicting operation was already running on the same database. (Opération annulée car une opération conflictuelle était déjà en cours d’exécution sur la même base de données.) | Consultez le [billet de blog sur les limitations relatives à la sauvegarde et à la restauration](https://blogs.msdn.microsoft.com/arvindsh/2008/12/30/concurrency-of-full-differential-and-log-backups-on-the-same-database) qui s’exécutent simultanément.| [Utilisez SQL Server Management Studio (SSMS) pour surveiller les travaux de sauvegarde.](backup-azure-sql-database.md#manage-azure-backup-operations-for-sql-on-azure-vms) Après l’échec de l’opération en conflit, recommencez l’opération.|

### <a name="usererrorsqlpodoesnotexist"></a>UserErrorSQLPODoesNotExist

| Message d’erreur | Causes possibles | Action recommandée |
|---|---|---|
| SQL database does not exist. (La base de données SQL n’existe pas.) | La base de données a été supprimée ou renommée. | <ul><li>Vérifiez si la base de données a été supprimée ou renommée par inadvertance.</li><li>Si la base de données a été supprimée par inadvertance, restaurez la base de données à l’emplacement d’origine pour poursuivre les sauvegardes.</li><li>Si vous avez supprimé la base de données et si vous n’avez pas besoin de sauvegardes ultérieures, dans le coffre Recovery Services cliquez sur [Arrêter la sauvegarde, puis « Conserver les données de sauvegarde » ou « Supprimer les données de sauvegarde »](backup-azure-sql-database.md#manage-azure-backup-operations-for-sql-on-azure-vms).</li>|

### <a name="usererrorsqllsnvalidationfailure"></a>UserErrorSQLLSNValidationFailure

| Message d’erreur | Causes possibles | Action recommandée |
|---|---|---|
| Log chain is broken. (La séquence de journaux de transactions consécutifs est altérée.) | La base de données ou la machine virtuelle est sauvegardée à l’aide d’une autre solution de sauvegarde, ce qui tronque la séquence de journaux de transactions consécutifs.|<ul><li>Vérifiez si un autre script ou une autre solution de sauvegarde est en cours d’utilisation. Si c’est le cas, arrêtez l’autre solution de sauvegarde. </li><li>Si la sauvegarde était de type sauvegarde de fichier journal ad hoc, déclenchez une sauvegarde complète pour commencer une nouvelle séquence de journaux de transactions consécutifs. Pour les sauvegardes de fichier journal planifiées, aucune action n’est requise étant donné que le service Sauvegarde Azure va automatiquement déclencher une sauvegarde complète pour corriger ce problème.</li>|

### <a name="usererroropeningsqlconnection"></a>UserErrorOpeningSQLConnection

| Message d’erreur | Causes possibles | Action recommandée |
|---|---|---|
| Azure Backup is not able to connect to the SQL instance. (Sauvegarde Azure n’est pas en mesure de se connecter à l’instance SQL.) | Sauvegarde Azure ne peut pas se connecter à l’instance SQL. | Utilisez les informations supplémentaires dans le menu d’erreur du Portail Azure pour mieux déterminer les causes racines. Consultez [Résoudre les problèmes de connexion au moteur de base de données SQL Server](https://docs.microsoft.com/sql/database-engine/configure-windows/troubleshoot-connecting-to-the-sql-server-database-engine) pour corriger l’erreur.<br/><ul><li>Si les paramètres SQL par défaut n’autorisent pas les connexions à distance, modifiez les paramètres. Consultez les liens ci-dessous pour modifier les paramètres.<ul><li>[https://msdn.microsoft.com/library/bb326495.aspx](https://msdn.microsoft.com/library/bb326495.aspx)</li><li>[https://docs.microsoft.com/sql/relational-databases/errors-events/mssqlserver-2-database-engine-error](https://docs.microsoft.com/sql/relational-databases/errors-events/mssqlserver-2-database-engine-error)</li><li>[https://docs.microsoft.com/sql/relational-databases/errors-events/mssqlserver-53-database-engine-error](https://docs.microsoft.com/sql/relational-databases/errors-events/mssqlserver-53-database-engine-error)</li></ul></li></ul><ul><li>En cas de problèmes de connexion, reportez-vous aux liens ci-dessous pour apporter les corrections :<ul><li>[https://docs.microsoft.com/sql/relational-databases/errors-events/mssqlserver-18456-database-engine-error](https://docs.microsoft.com/sql/relational-databases/errors-events/mssqlserver-18456-database-engine-error)</li><li>[https://docs.microsoft.com/sql/relational-databases/errors-events/mssqlserver-18452-database-engine-error](https://docs.microsoft.com/sql/relational-databases/errors-events/mssqlserver-18452-database-engine-error)</li></ul></li></ul> |

### <a name="usererrorparentfullbackupmissing"></a>UserErrorParentFullBackupMissing

| Message d’erreur | Causes possibles | Action recommandée |
|---|---|---|
| First full backup is missing for this data source. (La première sauvegarde complète est manquante pour cette source de données.) | La sauvegarde complète est manquante pour la base de données. Parent des sauvegardes différentielles et de fichier journal manquant sur une sauvegarde complète, des sauvegardes complètes doivent donc être effectuées avant de déclencher des sauvegardes différentielles ou de fichier journal. | Déclenchez une sauvegarde complète ad hoc.   |

### <a name="usererrorbackupfailedastransactionlogisfull"></a>UserErrorBackupFailedAsTransactionLogIsFull

| Message d’erreur | Causes possibles | Action recommandée |
|---|---|---|
| Cannot take backup as transaction log for the data source is full. (Impossible de prendre la sauvegarde car le journal des transactions de la source de données est plein.) | L’espace dédié au journal des transactions de la base de données est plein. | Pour résoudre ce problème, reportez-vous à la [documentation SQL](https://docs.microsoft.com/sql/relational-databases/errors-events/mssqlserver-9002-database-engine-error). |
| This SQL database does not support the requested backup type. (Cette base de données SQL ne prend pas en charge le type de sauvegarde demandé.) | Les réplicas secondaires de groupes de disponibilité AlwaysOn ne prennent pas en charge les sauvegardes différentielles et complètes. | <ul><li>Si vous avez déclenché une sauvegarde ad hoc, déclenchez les sauvegardes sur le nœud principal.</li><li>Si la sauvegarde a été planifiée par une stratégie, vérifiez que le nœud principal est inscrit. Pour inscrire le nœud, [suivez les étapes pour détecter une base de données SQL Server](backup-azure-sql-database.md#discover-sql-server-databases).</li></ul> |

## <a name="restore-failures"></a>Échecs de restauration

Les codes d’erreur suivants sont affichés en cas d’échec des travaux de restauration.

### <a name="usererrorcannotrestoreexistingdbwithoutforceoverwrite"></a>UserErrorCannotRestoreExistingDBWithoutForceOverwrite

| Message d’erreur | Causes possibles | Action recommandée |
|---|---|---|
| Database with same name already exists at the target location (Une base de données portant le même nom existe déjà dans l’emplacement cible) | La destination de restauration cible a déjà une base de données portant le même nom.  | <ul><li>Modifiez le nom de la base de données cible.</li><li>Ou, utilisez l’option de remplacement disponible sur la page de restauration.</li> |

### <a name="usererrorrestorefaileddatabasecannotbeofflined"></a>UserErrorRestoreFailedDatabaseCannotBeOfflined

| Message d’erreur | Causes possibles | Action recommandée |
|---|---|---|
| Restore failed as the database could not be brought offline. (Échec de la restauration car la base de données n’a pas pu être mise hors connexion.) | La base de données cible doit être mise hors connexion au cours de la restauration. Sauvegarde Azure n’est pas en mesure de mettre ces données hors connexion. | Utilisez les informations supplémentaires dans le menu d’erreur du Portail Azure pour mieux déterminer les causes racines. Pour plus d’informations, consultez la [documentation SQL](https://docs.microsoft.com/sql/relational-databases/backup-restore/restore-a-database-backup-using-ssms). |


###  <a name="usererrorcannotfindservercertificatewiththumbprint"></a>UserErrorCannotFindServerCertificateWithThumbprint

| Message d’erreur | Causes possibles | Action recommandée |
|---|---|---|
| Cannot find the server certificate with thumbprint on the target. (Impossible de trouver le certificat de serveur avec l’empreinte sur la cible.) | La base de données MASTER sur l’instance de destination n’a pas une empreinte de chiffrement valide. | Importez l’empreinte de certificat valide utilisée sur l’instance source vers l’instance cible. |

## <a name="registration-failures"></a>Échecs d’enregistrement

Les codes d’erreur suivants correspondent aux échecs d’inscription.

### <a name="fabricsvcbackuppreferencecheckfailedusererror"></a>FabricSvcBackupPreferenceCheckFailedUserError

| Message d’erreur | Causes possibles | Action recommandée |
|---|---|---|
| Backup preference for SQL Always On Availability Group cannot be met as some nodes of the Availability Group are not registered. (Les préférences de sauvegarde pour le groupe de disponibilité Always On ne peuvent pas être respectées car certains nœuds du groupe de disponibilité ne sont pas inscrits.) | Les nœuds requis pour effectuer des sauvegardes ne sont pas inscrits ou sont inaccessibles. | <ul><li>Vérifiez que tous les nœuds requis pour effectuer des sauvegardes de cette base de données sont inscrits et intègres, puis recommencez l’opération.</li><li>Modifiez les préférences de sauvegarde du groupe de disponibilité Always On.</li></ul> |

### <a name="vmnotinrunningstateusererror"></a>VMNotInRunningStateUserError

| Message d’erreur | Causes possibles | Action recommandée |
|---|---|---|
| SQL server VM is either shutdown and not accessible to Azure Backup service. (La machine virtuelle SQL Server est arrêtée ou indisponible pour le service Sauvegarde Azure.) | La machine virtuelle est arrêtée | Assurez-vous que le serveur SQL est en cours d’exécution. |

### <a name="guestagentstatusunavailableusererror"></a>GuestAgentStatusUnavailableUserError

| Message d’erreur | Causes possibles | Action recommandée |
|---|---|---|
| Azure Backup service uses Azure VM guest agent for doing backup but guest agent is not available on the target server. (Le service Sauvegarde Azure utilise l’agent invité de machine virtuelle Azure pour effectuer la sauvegarde, mais l’agent invité n’est pas disponible sur le serveur cible.) | L’agent invité n’est pas activé ou n’est pas intègre | [Installez l’agent invité de machine virtuelle](../virtual-machines/extensions/agent-windows.md) manuellement. |

## <a name="configure-backup-failures"></a>Échecs de la configuration de sauvegarde

Les codes d’erreur suivants correspondent aux échecs de la configuration de sauvegarde.

### <a name="autoprotectioncancelledornotvalid"></a>AutoProtectionCancelledOrNotValid

| Message d’erreur | Causes possibles | Action recommandée |
|---|---|---|
| L’intention de protection automatique a été supprimée ou n’est pas plus valide. | Lorsque vous activez la protection automatique sur une instance SQL, les tâches **Configurer la sauvegarde** s’exécutent pour toutes les bases de données de cette instance. Si vous désactivez la protection automatique pendant l’exécution des tâches, les tâches **En cours** sont annulées avec ce code d’erreur. | Activez la protection automatique de nouveau pour protéger toutes les bases de données restantes. |

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur Sauvegarde Azure pour les machines virtuelles SQL Server (préversion publique), consultez [Sauvegarde Azure pour les machines virtuelles SQL (préversion publique)](../virtual-machines/windows/sql/virtual-machines-windows-sql-backup-recovery.md#azbackup).
