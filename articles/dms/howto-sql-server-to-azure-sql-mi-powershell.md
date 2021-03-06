---
title: Migrer SQL Server vers Azure SQL Database Managed Instance avec Database Migration Service et PowerShell | Microsoft Docs
description: Apprenez à migrer à partir d’un serveur SQL Server local vers Azure SQL DB Managed Instance à l’aide d’Azure PowerShell.
services: database-migration
author: pochiraju
ms.author: rajpo
manager: craigg
ms.reviewer: douglasl
ms.service: dms
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 10/09/2018
ms.openlocfilehash: c4ccbe8a6f1e1923e83d2bdcbeb2d61429083aee
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/21/2018
ms.locfileid: "53724196"
---
# <a name="migrate-sql-server-on-premises-to-azure-sql-database-managed-instance-using-azure-powershell"></a>Migrer un serveur SQL Server local vers Azure SQL Database Managed Instance à l’aide d’Azure PowerShell
Dans cet article, vous allez migrer la base de données **Adventureworks2012** restaurée vers une instance locale de SQL Server 2005 ou une version ultérieure vers Azure SQL Database Managed Instance, à l’aide de Microsoft Azure PowerShell. Vous pouvez migrer des bases de données à partir d’une instance SQL Server locale vers Azure SQL Database Managed Instance, à l’aide du module `AzureRM.DataMigration`, dans Microsoft Azure PowerShell.

Dans cet article, vous apprendrez comment :
> [!div class="checklist"]
> * Créez un groupe de ressources.
> * Créer une instance Azure Database Migration Service.
> * Créer un projet de migration dans une instance Azure Database Migration Service.
> * Exécuter la migration.

## <a name="prerequisites"></a>Prérequis
Pour effectuer cette procédure, vous avez besoin de :

- [SQL Server 2016 ou version ultérieure](https://www.microsoft.com/sql-server/sql-server-downloads) (toute édition)
- Activer le protocole TCP/IP, qui est désactivé par défaut avec l’installation de SQL Server Express. Activer le protocole TCP/IP en suivant l’article [Activer ou désactiver un protocole réseau de serveur](https://docs.microsoft.com/sql/database-engine/configure-windows/enable-or-disable-a-server-network-protocol#SSMSProcedure).
- Pour configurer votre [pare-feu Windows pour accéder au moteur de base de données](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access).
- Une instance Azure SQL Database. Vous pouvez créer une instance Azure SQL Database en suivant les indications de l’article [Création d’une base de données SQL Azure à l’aide du portail Azure](https://docs.microsoft.com/azure/sql-database/sql-database-get-started-portal).
- [Assistant Migration des données](https://www.microsoft.com/download/details.aspx?id=53595) version 3.3 ou ultérieure.
- Avoir créé un réseau virtuel à l’aide du modèle de déploiement Azure Resource Manager, qui fournit une connectivité de site à site à Azure Database Migration Service pour vos serveurs sources locaux à l’aide [d’ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) ou d’un [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways).
- Avoir terminé l’évaluation de votre base de données locale et de la migration de schéma à l’aide de l’Assistant Migration de données, comme indiqué dans l’article [Évaluer votre migration vers SQL Server](https://docs.microsoft.com/sql/dma/dma-assesssqlonprem).
- Télécharger et installer le module AzureRM.DataMigration à partir de PowerShell Gallery en utilisant la [cmdlet PowerShell Install-Module](https://docs.microsoft.com/powershell/module/powershellget/Install-Module?view=powershell-5.1).
- Vous assurer que les informations d’identification utilisées pour se connecter à une instance SQL Server source disposent de l’autorisation [CONTROL SERVER](https://docs.microsoft.com/sql/t-sql/statements/grant-server-permissions-transact-sql).
- Vous assurer que les informations d’identification utilisées pour se connecter à une instance Azure SQL DB cible disposent de l’autorisation CONTROL DATABASE sur les bases de données Azure SQL Database cibles.
- Un abonnement Azure. Si vous n’en avez pas, créez un compte [gratuit](https://azure.microsoft.com/free/) avant de commencer.

## <a name="log-in-to-your-microsoft-azure-subscription"></a>Connexion à un abonnement Microsoft Azure
Suivez les instructions de l’article [Se connecter avec Azure PowerShell](https://docs.microsoft.com/powershell/azure/authenticate-azureps?view=azurermps-4.4.1) pour vous connecter à votre abonnement Azure à l’aide de PowerShell.

## <a name="create-a-resource-group"></a>Créer un groupe de ressources
Un groupe de ressources Azure est un conteneur logique dans lequel les ressources Azure sont déployées et gérées. Créez un groupe de ressources pour pouvoir créer une machine virtuelle.

Créez un groupe de ressources à l’aide de la commande [New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup?view=azurermps-4.4.1). 

L’exemple suivant permet de créer un groupe de ressources nommé *myResourceGroup* dans la région *Est des États-Unis*.

```powershell
New-AzureRmResourceGroup -ResourceGroupName myResourceGroup -Location EastUS
```
## <a name="create-an-instance-of-the-azure-database-migration-service"></a>Créer une instance Azure Database Migration Service 
Vous pouvez créer une instance Azure Database Migration Service à l’aide de l’applet de commande `New-AzureRmDataMigrationService`. Cette cmdlet attend les paramètres requis suivants :
- *Nom du groupe de ressources Azure*. Vous pouvez utiliser la commande [New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup?view=azurermps-4.4.1) pour créer le groupe de ressources Azure indiqué précédemment et indiquez son nom en tant que paramètre.
- *Nom du service*. Chaîne qui correspond au nom de service unique à donner à l’instance Azure Database Migration Service. 
- *Emplacement*. Spécifie l’emplacement du service. Indiquez un emplacement de centre de données Azure, tel que USA Ouest ou Asie Sud-Est.
- *Référence SKU*. Ce paramètre correspond au nom de référence SKU DMS. Les noms de référence SKU actuellement pris en charge sont *Basic_1vCore*, *Basic_2vCores* et *GeneralPurpose_4vCores*.
- *Identificateur de sous-réseau virtuel*. Créez un sous-réseau avec la cmdlet [New-AzureRmVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig?view=azurermps-4.4.1). 

L’exemple suivant crée un service nommé *MyDMS* dans le groupe de ressources *MyDMSResourceGroup*, qui se trouve dans la région *USA Est* à l’aide d’un réseau virtuel appelé *MyVNET* et d’un sous-réseau appelé *MySubnet*.

```powershell
 $vNet = Get-AzureRmVirtualNetwork -ResourceGroupName MyDMSResourceGroup -Name MyVNET

$vSubNet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vNet -Name MySubnet

$service = New-AzureRmDms -ResourceGroupName myResourceGroup `
  -ServiceName MyDMS `
  -Location EastUS `
  -Sku Basic_2vCores `  
  -VirtualSubnetId $vSubNet.Id`
```

## <a name="create-a-migration-project"></a>Créer un projet de migration
Une fois l’instance Azure Database Migration Service créée, générez un projet de migration. Un projet Azure Database Migration Service requiert des informations de connexion pour les instances source et cible, ainsi que la liste des bases de données que vous souhaitez migrer dans le cadre de ce projet.

### <a name="create-a-database-connection-info-object-for-the-source-and-target-connections"></a>Créer un objet d’informations de connexion de base de données pour les connexions source et cible
Vous pouvez créer un objet d’informations de connexion de base de données à l’aide de la cmdlet `New-AzureRmDmsConnInfo`. Cette cmdlet attend les paramètres suivants :
- *ServerType*. Type de connexion de base de données demandé, par exemple, MySQL, Oracle ou MySQL. Utilisez SQL pour SQL Server et Azure SQL.
- *DataSource*. Nom ou adresse IP d’une instance SQL Server ou d’une base de données Azure SQL.
- *AuthType*. Type d’authentification pour la connexion, qui peut être SqlAuthentication ou WindowsAuthentication.
- Le paramètre *TrustServerCertificate* définit une valeur qui indique si le canal est chiffré tout en ignorant l’analyse de la chaîne de certificat pour valider l’approbation. La valeur doit être « true » ou « false ».

L’exemple suivant crée l’objet d’informations de connexion pour l’instance SQL Server source, appelée MySourceSQLServer avec l’authentification SQL : 

```powershell
$sourceConnInfo = New-AzureRmDmsConnInfo -ServerType SQL `
  -DataSource MySourceSQLServer `
  -AuthType SqlAuthentication `
  -TrustServerCertificate:$true
```

L’exemple suivant illustre la création de l’objet d’informations de connexion pour un serveur Azure SQL Database Managed Instance appelé « targetmanagedinstance.database.windows.net » avec l’authentification SQL :

```powershell
$targetConnInfo = New-AzureRmDmsConnInfo -ServerType SQL `
  -DataSource "targetmanagedinstance.database.windows.net" `
  -AuthType SqlAuthentication `
  -TrustServerCertificate:$false
```

### <a name="provide-databases-for-the-migration-project"></a>Fournir des bases de données pour le projet de migration
Créez une liste d’objets `AzureRmDataMigrationDatabaseInfo` spécifiant les bases de données dans le cadre du projet Azure Database Migration qui peut être fourni en tant que paramètre de création du projet. La cmdlet `New-AzureRmDataMigrationDatabaseInfo` peut être utilisée pour créer AzureRmDataMigrationDatabaseInfo. 

L’exemple suivant crée le projet `AzureRmDataMigrationDatabaseInfo` pour la base de données **AdventureWorks** et l’ajoute à la liste qui doit être fournie en tant que paramètre de création du projet.

```powershell
$dbInfo1 = New-AzureRmDataMigrationDatabaseInfo -SourceDatabaseName AdventureWorks
$dbList = @($dbInfo1)
```

### <a name="create-a-project-object"></a>Créer un objet de projet
Enfin, vous pouvez créer le projet Azure Database Migration appelé *MyDMSProject*, situé dans la région *USA Est*, à l’aide du paramètre `New-AzureRmDataMigrationProject`, et en ajoutant les connexions source et cible créées précédemment et la liste des bases de données à migrer.

```powershell
$project = New-AzureRmDataMigrationProject -ResourceGroupName myResourceGroup `
  -ServiceName $service.Name `
  -ProjectName MyDMSProject `
  -Location EastUS `
  -SourceType SQL `
  -TargetType SQLMI `
  -SourceConnection $sourceConnInfo `
  -TargetConnection $targetConnInfo `
  -DatabaseInfo $dbList
```

## <a name="create-and-start-a-migration-task"></a>Créer et démarrer une tâche de migration
Enfin, créez et démarrez une tâche Azure Database Migration. La tâche Azure Database Migration nécessite des informations de connexion pour les systèmes source et cible, ainsi que la liste des tables de base de données à migrer, en plus des informations déjà fournies avec le projet créé, en tant que condition préalable. 

### <a name="create-credential-parameters-for-source-and-target"></a>Créer des paramètres d’informations d’identification pour la source et la cible
Vous pouvez créer des informations d’identification de sécurité de la connexion en tant qu’objet [PSCredential](https://docs.microsoft.com/dotnet/api/system.management.automation.pscredential?redirectedfrom=MSDN&view=powershellsdk-1.1.0). 

L’exemple suivant illustre la création d’objets *PSCredential* pour les connexions source et cible, en fournissant des mots de passe en tant que variables de chaîne *$sourcePassword* et *$targetPassword*. 

```powershell
$secpasswd = ConvertTo-SecureString -String $sourcePassword -AsPlainText -Force
$sourceCred = New-Object System.Management.Automation.PSCredential ($sourceUserName, $secpasswd)
$secpasswd = ConvertTo-SecureString -String $targetPassword -AsPlainText -Force
$targetCred = New-Object System.Management.Automation.PSCredential ($targetUserName, $secpasswd)
```

### <a name="create-backup-fileshare-object"></a>Créer l’objet de partage de fichiers de sauvegarde
Passez maintenant à la création de l’objet de partage de fichiers de sauvegarde représentant le partage réseau SMB local qu’Azure Database Migration Service peut utiliser pour les sauvegardes de bases de données source à l’aide de la cmdlet New-AzureRmDmsFileShare.

```powershell
$backupPassword = ConvertTo-SecureString -String $password -AsPlainText -Force
$backupCred = New-Object System.Management.Automation.PSCredential ($backupUserName, $backupPassword)

$backupFileSharePath="\\10.0.0.76\SharedBackup"
$backupFileShare = New-AzureRmDmsFileShare -Path $backupFileSharePath -Credential $backupCred
```

### <a name="create-selected-database-object"></a>Créer l’objet de base de données sélectionné
L’étape suivante consiste à sélectionner les bases de données source et cible à l’aide de la cmdlet New-AzureRmDmsSelectedDB, comme indiqué dans l’exemple suivant :

```powershell
$selectedDbs = @()
$selectedDbs += New-AzureRmDmsSelectedDB -MigrateSqlServerSqlDbMi `
  -Name AdventureWorks2016 `
  -TargetDatabaseName AdventureWorks2016 `
  -BackupFileShare $backupFileShare `
```

### <a name="sas-uri-for-azure-storage-container"></a>URI SAP pour un conteneur Stockage Azure
Créez une variable contenant l’URI SAP qui donne à Azure Database Migration Service l’accès au conteneur du compte de stockage dans lequel le service charge les fichiers de sauvegarde.

```powershell
$blobSasUri="https://mystorage.blob.core.windows.net/test?st=2018-07-13T18%3A10%3A33Z&se=2019-07-14T18%3A10%3A00Z&sp=rwdl&sv=2018-03-28&sr=c&sig=qKlSA512EVtest3xYjvUg139tYSDrasbftY%3D"
```

### <a name="select-logins"></a>Sélectionner des connexions
Dressez la liste des connexions à migrer comme indiqué dans l’exemple ci-dessous :  Notez que DMS ne prend actuellement en charge que la migration des connexions SQL. 

```powershell
$selectedLogins = @("user1", "user2")
```

### <a name="select-agent-jobs"></a>Sélectionner des travaux d’agents
Créez la liste des travaux d’agents à migrer comme indiqué dans l’exemple ci-dessous :

>[!NOTE]
>Database Migration Service prend actuellement en charge les travaux avec les étapes de travail du sous-système T-SQL uniquement.

```powershell
$selectedAgentJobs = @("agentJob1", "agentJob2")
```

### <a name="create-and-start-a-migration-task"></a>Créer et démarrer une tâche de migration

Utilisez la cmdlet `New-AzureRmDataMigrationTask` pour créer et démarrer une tâche de migration. Cette cmdlet attend les paramètres suivants :
- *TaskType*. Type de tâche de migration à créer pour une migration de SQL Server vers Azure SQL Database Managed Instance. Le type *MigrateSqlServerSqlDbMi* est attendu. 
- *Resource Group Name*. Nom du groupe de ressources Azure dans lequel créer la tâche.
- *ServiceName*. Instance Azure Database Migration Service dans laquelle créer la tâche.
- *ProjectName*. Nom du projet Azure Database Migration Service dans lequel créer la tâche. 
- *TaskName*. Nom de la tâche à créer. 
- *SourceConnection*. Objet AzureRmDmsConnInfo représentant la connexion SQL Server source.
- *TargetConnection*. Objet AzureRmDmsConnInfo représentant la connexion Azure SQL Database Managed Instance cible.
- *SourceCred*. Objet [PSCredential](https://docs.microsoft.com/dotnet/api/system.management.automation.pscredential?redirectedfrom=MSDN&view=powershellsdk-1.1.0) pour la connexion au serveur source.
- *TargetCred*. Objet [PSCredential](https://docs.microsoft.com/dotnet/api/system.management.automation.pscredential?redirectedfrom=MSDN&view=powershellsdk-1.1.0) pour la connexion au serveur cible.
- *SelectedDatabase*. Objet AzureRmDataMigrationSelectedDB représentant le mappage entre les bases de données source et cible.
- *BackupFileShare*. Objet FileShare représentant le partage réseau local qu’Azure Database Migration Service peut utiliser pour les sauvegardes de bases de données source.
- *BackupBlobSasUri*. L’URI SAP qui donne à Azure Database Migration Service l’accès au conteneur du compte de stockage dans lequel le service charge les fichiers de sauvegarde. Découvrez comment obtenir l’URI SAP du conteneur d’objets blob.
- *SelectedLogins*. Liste des connexions sélectionnées pour la migration.
- *SelectedAgentJobs*. Liste des travaux d’agents sélectionnés pour la migration.

L’exemple suivant crée et démarre une tâche de migration nommée myDMSTask :

```powershell
$migTask = New-AzureRmDataMigrationTask -TaskType MigrateSqlServerSqlDbMi `
  -ResourceGroupName myResourceGroup `
  -ServiceName $service.Name `
  -ProjectName $project.Name `
  -TaskName myDMSTask `
  -SourceConnection $sourceConnInfo `
  -SourceCred $sourceCred `
  -TargetConnection $targetConnInfo `
  -TargetCred $targetCred `
  -SelectedDatabase  $selectedDbs`
  -BackupFileShare $backupFileShare `
  -BackupBlobSasUri $blobSasUri `
  -SelectedLogins $selectedLogins `
  -SelectedAgentJobs $selectedJobs `
```

## <a name="monitor-the-migration"></a>Surveiller la migration
Vous pouvez surveiller la tâche de migration en cours d’exécution en interrogeant la propriété d’état de la tâche, comme indiqué dans l’exemple suivant :

```powershell
if (($mytask.ProjectTask.Properties.State -eq "Running") -or ($mytask.ProjectTask.Properties.State -eq "Queued"))
{
  write-host "migration task running"
}
```

## <a name="next-steps"></a>Étapes suivantes
- Lisez l’aide à la migration du [Guide de migration de bases de données](https://datamigration.microsoft.com/) de Microsoft.
