---
title: Évaluer les charges de travail locales pour la migration de Contoso vers Azure | Microsoft Docs
description: Découvrez comment Contoso évalue ses ordinateurs locaux pour la migration vers Azure à l’aide d’Azure Migrate et de l’Assistant Migration de données.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 10/23/2018
ms.author: raynew
ms.openlocfilehash: c2afd1c5a85ec393acfb0f754dd4ca9d77162061
ms.sourcegitcommit: 3ab534773c4decd755c1e433b89a15f7634e088a
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/07/2019
ms.locfileid: "54065492"
---
# <a name="contoso-migration-assess-on-premises-workloads-for-migration-to-azure"></a>Migration de Contoso : Évaluer facilement vos charges de travail locales en vue d’une migration vers Azure

Dans cet article, Contoso évalue son application SmartHotel360 locale en vue de sa migration vers Azure.

Cet article fait partie d’une série décrivant comment la société fictive Contoso migre ses ressources locales vers le cloud Microsoft Azure. La série comprend des informations générales et des scénarios de déploiement détaillés qui illustrent comment configurer une infrastructure de migration, évaluer l’adéquation des ressources locales pour la migration et exécuter différents types de migration. Les scénarios augmentent en complexité. De nouveaux articles seront ajoutés à la série au fil du temps.

Article | Détails | Statut
--- | --- | ---
[Article 1 : Vue d’ensemble](contoso-migration-overview.md) | Vue d’ensemble de la série d’articles, de la stratégie de migration de Contoso et des exemples d’applications utilisés dans la série. | Disponible
[Article 2 : Déployer une infrastructure Azure](contoso-migration-infrastructure.md) | Contoso prépare son infrastructure locale et son infrastructure Azure pour la migration. La même infrastructure est utilisée pour tous les articles de migration de la série. | Disponible
Article 3 : Évaluer les ressources locales à migrer vers Azure | Contoso évalue son application locale SmartHotel360 qui s’exécute sur VMware. Contoso évalue les machines virtuelles de l’application à l’aide du service Azure Migrate et la base de données SQL Server de l’application à l’aide de l’Assistant Migration de données. | Cet article
[Article 4 : Réhéberger une application sur une machine virtuelle Azure et SQL Database Managed Instance](contoso-migration-rehost-vm-sql-managed-instance.md) | Contoso exécute une migration lift-and-shift vers Azure pour son application SmartHotel360 locale. Il migre le serveur frontal de l’application à l’aide du service Azure Site Recovery. Il migre la base de données de l’application vers Azure SQL Database Managed Instance à l’aide d’Azure Database Migration Service. | Disponible
[Article 5 : Réhéberger une application sur des machines virtuelles Azure](contoso-migration-rehost-vm.md) | Contoso migre les machines virtuelles de son application SmartHotel360 vers des machines virtuelles Azure avec le service Site Recovery. | Disponible
[Article 6 : Réhéberger une application sur des machines virtuelles Azure et dans un groupe de disponibilité SQL Server](contoso-migration-rehost-vm-sql-ag.md) | Contoso migre l’application SmartHotel360 en utilisant Site Recovery pour migrer les machines virtuelles de l’application, et en utilisant Database Migration Service pour migrer la base de données de l’application vers un cluster SQL Server qui est protégé par un groupe de disponibilité AlwaysOn. | Disponible
[Article 7 : Réhéberger une application Linux sur des machines virtuelles Azure](contoso-migration-rehost-linux-vm.md) | Contoso effectue une migration lift-and-shift de son application osTicket Linux sur des machines virtuelles Azure à l’aide du service Site Recovery. | Disponible
[Article 8 : Réhéberger une application Linux sur des machines virtuelles Azure et Azure Database pour MySQL](contoso-migration-rehost-linux-vm-mysql.md) | Contoso migre son application osTicket Linux vers des machines virtuelles Azure à l’aide de Site Recovery. La société migre la base de données d’application vers Azure Database pour MySQL à l’aide de MySQL Workbench. | Disponible
[Article 9 : Refactoriser une application vers une application web Azure et Azure SQL Database](contoso-migration-refactor-web-app-sql.md) | Contoso migre son application SmartHotel360 vers une application web Azure, et la base de données de l’application vers une instance Azure SQL Server avec l’Assistant Migration de données. | Disponible
[Article 10 : Refactoriser une application Linux vers une application web Azure et Azure Database pour MySQL](contoso-migration-refactor-linux-app-service-mysql.md) | Contoso migre son application osTicket Linux vers Azure Web App dans plusieurs régions Azure à l’aide d’Azure Traffic Manager, intégré à GitHub de façon à assurer une livraison continue. Contoso migre la base de données d’application vers une instance d’Azure Database pour MySQL. | Disponible
[Article 11 : Refactoriser Team Foundation Server sur Azure DevOps Services](contoso-migration-tfs-vsts.md) | Contoso migre son déploiement local de Team Foundation Server vers Azure DevOps Services dans Azure. | Disponible
[Article 12 : Réarchitecturer une application dans des conteneurs Azure et Azure SQL Database](contoso-migration-rearchitect-container-sql.md) | Contoso migre son application SmartHotel vers Azure. Ensuite, l’entreprise restructure la couche web de l’application comme un conteneur Windows s’exécutant dans Azure Service Fabric, et la base de données avec Azure SQL Database. | Disponible
[Article 13 : Regénérer une application dans Azure](contoso-migration-rebuild.md) | Contoso régénère son application SmartHotel à l’aide d’une série de fonctionnalités et services Azure, notamment Azure App Service, Azure Kubernetes Service (AKS), Azure Functions, Azure Cognitive Services et Azure Cosmos DB. | Disponible
[Article 14 : Mettre à l’échelle une migration vers Azure](contoso-migration-scale.md) | Après des essais de différentes combinaisons de migration, Contoso se prépare à une migration complète vers Azure. | Disponible


## <a name="overview"></a>Vue d’ensemble

Puisqu’elle envisage d’effectuer une migration vers Azure, la société Contoso souhaite réaliser une évaluation technique et financière pour déterminer si ses charges de travail locales peuvent être migrées vers le cloud. L’équipe Contoso souhaite évaluer la compatibilité des bases de données et des machines à migrer. Elle souhaite estimer la capacité et les coûts nécessaires à l’exécution des ressources Contoso dans Azure.

Pour démarrer et pour mieux comprendre les technologies impliquées, Contoso va évaluer deux de ses applications locales, reprises dans le tableau suivant. La société évalue les scénarios de migration qui réhébergent et refactorisent les applications pour la migration. Apprenez-en davantage sur le ré-hébergement et la refactorisation en consultant la [vue d’ensemble de la migration de Contoso](contoso-migration-overview.md).

Nom de l’application | Plateforme | Niveaux de l’application | Détails
--- | --- | --- | ---
SmartHotel360<br/><br/> (gère les déplacements de Contoso) | S’exécute sur Windows avec une base de données SQL Server | Application à deux niveaux. Le site web ASP.NET frontend s’exécute sur une machine virtuelle (**WEBVM**), et SQL Server s’exécute sur une autre machine virtuelle (**SQLVM**). | Les machines virtuelles sont des machines VMware s’exécutant sur un hôte ESXi géré par vCenter Server.<br/><br/> Vous pouvez télécharger l’exemple d’application sur [GitHub](https://github.com/Microsoft/SmartHotel360).
osTicket<br/><br/> (application Service Desk de Contoso) | S’exécute sur Linux/Apache avec MySQL PHP (LAMP) | Application à deux niveaux. Le site web PHP frontend s’exécute sur une machine virtuelle (**OSTICKETWEB**) et la base de données MySQL s’exécute sur une autre machine virtuelle (**OSTICKETMYSQL**). | L’application est utilisée par les applications de service clientèle pour effectuer le suivi des problèmes pour les employés et les clients externes.<br/><br/> Vous pouvez télécharger l’exemple sur [GitHub](https://github.com/osTicket/osTicket).

## <a name="current-architecture"></a>Architecture actuelle

Le diagramme suivant montre l’infrastructure locale actuelle de Contoso :

![Architecture actuelle de Contoso](./media/contoso-migration-assessment/contoso-architecture.png)  

- Contoso dispose d’un centre de données principal. Le centre de données principal se trouve dans la ville de New York dans l’Est des États-Unis.
- Contoso a trois branches locales supplémentaires aux États-Unis.
- Le centre de données principal est connecté à Internet par le biais d’une connexion Ethernet Fiber Metro (500 Mbits/s).
- Chaque branche est connectée localement à Internet par le biais de connexions de qualité professionnelle, avec des tunnels VPN IPsec vers le centre de données principal. Cette configuration permet au réseau entier d’être connecté en permanence, et elle optimise la connectivité Internet.
- Le centre de données principal est entièrement virtualisé avec VMware. Contoso dispose de deux hôtes de virtualisation ESXi 6.5 gérés par vCenter Server 6.5.
- Contoso utilise Active Directory pour la gestion d’identité. Contoso utilise des serveurs DNS sur le réseau interne.
- Les contrôleurs de domaine dans le centre de données s’exécutent sur des machines virtuelles VMware. Les contrôleurs de domaine au niveau des branches locales s’exécutent sur des serveurs physiques.

## <a name="business-drivers"></a>Axes stratégiques

L’équipe informatique de Contoso a travaillé en étroite collaboration avec ses partenaires commerciaux pour comprendre le résultat que l’entreprise souhaite obtenir avec cette migration :

- **Répondre à la croissance de l’entreprise** : Contoso se développe. Il en découle une pression accrue sur l’infrastructure et les systèmes locaux et de l’entreprise.
- **Augmenter l’efficacité** : Contoso doit supprimer les procédures inutiles et rationaliser les processus pour ses développeurs et ses utilisateurs. L’entreprise a besoin d’une informatique rapide et doit éviter les pertes de temps ou d’argent afin de répondre plus rapidement aux exigences des clients.
- **Accroître l’agilité** :  le service informatique de Contoso doit être plus réactif face aux besoins de l’entreprise. Elle doit être en mesure de réagir plus rapidement que l’évolution du marché pour réussir dans une économie mondiale. L’informatique chez Contoso ne doit pas devenir une entrave à l’activité.
- **Mise à l’échelle** : à mesure que l’entreprise croît, l’informatique de Contoso doit fournir des systèmes capables de croître au même rythme.

## <a name="assessment-goals"></a>Objectifs d’évaluation

L’équipe cloud de Contoso a identifié des objectifs pour ses évaluations de migration :

- Après la migration, les applications dans Azure devront offrir les mêmes performances qu’aujourd’hui dans l’environnement VMWare local de Contoso. Migrer vers le cloud ne signifie nullement que les performances de l’application deviennent moins importantes.
- Contoso doit savoir si ses applications et ses bases de données sont compatibles avec les exigences d’Azure. Contoso doit également connaître les options d’hébergement Azure dont elle dispose.
- Le passage au cloud permet de réduire les tâches d’administration de la base de données de Contoso.  
- Contoso souhaite connaître non seulement les options de migration, mais aussi les coûts associés à l’infrastructure après déplacement de celle-ci vers le cloud.

## <a name="assessment-tools"></a>Outils d’évaluation

Contoso utilise les outils Microsoft pour son évaluation de la migration. Ces outils s’alignent sur les objectifs de Contoso et doivent lui fournir toutes les informations dont elle a besoin.

Technology | Description | Coût
--- | --- | ---
[Assistant de migration des données](https://docs.microsoft.com/sql/dma/dma-overview?view=ssdt-18vs2017) | Contoso utilise l’Assistant Migration de données pour évaluer et détecter les problèmes de compatibilité susceptibles d’affecter les fonctionnalités de base de données dans Azure. L’Assistant Migration de données évalue la parité des fonctionnalités entre les instances sources et cibles de SQL. Il recommande des améliorations au niveau des performances et de la fiabilité. | L’Assistant Migration de données est un outil gratuit et téléchargeable.
[Azure Migrate](https://docs.microsoft.com/azure/migrate/migrate-overview) | Contoso utilise le service Azure Migrate pour évaluer ses machines virtuelles VMware. Azure Migrate évalue la pertinence de la migration des ordinateurs. Il fournit des estimations de dimensionnement et de coût pour l’exécution des machines dans Azure.  | À compter de mai 2018, Azure Migrate devient un service gratuit.
[Service Map](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-service-map) | Azure Migrate utilise Service Map pour afficher les dépendances qui existent entre les ordinateurs que vous souhaitez migrer. | Service Map fait partie d’Azure Log Analytics. Contoso peut utiliser Service Map gratuitement pendant 180 jours.

Dans ce scénario, Contoso télécharge et exécute l’Assistant Migration de données pour évaluer la base de données SQL Server locale pour son application de voyage. Contoso utilise Azure Migrate avec un mappage des dépendances pour évaluer les machines virtuelles de l’application avant d’opérer la migration vers Azure.

## <a name="assessment-architecture"></a>Architecture pour l’évaluation

![Architecture pour l’évaluation de la migration](./media/contoso-migration-assessment/migration-assessment-architecture.png)

- Contoso est un nom fictif représentant une entreprise classique.
- Contoso dispose d’un centre de données local (**contoso-datacenter**), et de contrôleurs de domaine locaux (**CONTOSODC1**, **CONTOSODC2**).
- Les machines virtuelles VMware sont situées sur des hôtes VMware ESXi exécutant la version 6.5 (**contosohost1**, **contosohost2**).
- L’environnement VMware est géré par vCenter Server 6.5 (**vcenter.contoso.com** s’exécutant sur une machine virtuelle).
- L’application de voyage SmartHotel360 présente les caractéristiques suivantes :
    - L’application est répartie entre deux machines virtuelles VMware (**WEBVM** et **SQLVM**).
    - Les machines virtuelles sont situées sur un hôte VMware ESXi, **contosohost1.contoso.com**.
    - Les machines virtuelles exécutent Windows Server 2008 R2 Datacenter avec SP1.
- L’environnement VMware est géré par le serveur vCenter Server (**vcenter.contoso.com**) s’exécutant sur une machine virtuelle.
- L’application Service Desk osTicket :
    - L’application est répartie entre deux machines virtuelles (**OSTICKETWEB** et **OSTICKETMYSQL**).
    - Les machines virtuelles s’exécutent sur Ubuntu Linux Server 16.04-LTS.
    - **OSTICKETWEB** exécute Apache 2 et PHP 7.0.
    - **OSTICKETMYSQL** exécute MySQL 5.7.22.

## <a name="prerequisites"></a>Prérequis

Contoso et les autres utilisateurs doivent respecter les prérequis suivants pour cette évaluation :

- Des autorisations Propriétaire ou Contributeur pour l’abonnement Azure ou pour un groupe de ressources de l’abonnement Azure.
- Une instance locale de vCenter Server qui exécute la version 6.5, 6.0 ou 5.5
- Un compte en lecture seule dans vCenter Server, ou des autorisations pour en créer un.
- Des autorisations pour créer une machine virtuelle dans l’instance vCenter Server à l’aide d’un modèle .OVA.
- Au moins un hôte ESXi exécutant la version 5.0 ou une version ultérieure.
- Au moins deux machines virtuelles VMware locales, dont l’une exécutant une base de données SQL Server.
- Les autorisations nécessaires pour installer des agents Azure Migrate sur chaque machine virtuelle.
- Les machines virtuelles doivent avoir une connectivité Internet directe.  
    - Vous pouvez limiter l’accès Internet aux [URL requises](https://docs.microsoft.com/azure/migrate/concepts-collector#collector-pre-requisites).  
    - Si vos machines virtuelles ne sont pas connectées à Internet, la [passerelle Azure Log Analytics](../azure-monitor/platform/gateway.md) doit être installée et le trafic de l’agent y transiter.
- Le nom de domaine complet de la machine virtuelle qui exécute l’instance SQL Server, pour l’évaluation de la base de données.
- Le Pare-feu Windows exécuté sur la machine virtuelle SQL Server doit autoriser les connexions externes via le port TCP 1433 (par défaut). Cette installation permet à l’Assistant Migration de données de se connecter.

## <a name="assessment-overview"></a>Vue d’ensemble de l’évaluation

Voici comment Contoso effectue son évaluation :

> [!div class="checklist"]
> * **Étape 1 : Télécharger et installer l’Assistant Migration de données** : Contoso prépare l’Assistant Migration de données pour l’évaluation de la base de données SQL Server locale.
> * **Étape 2 : Évaluer la base de données à l’aide de l’Assistant Migration des données** : Contoso effectue l'évaluation de la base de données et analyse les résultats.
> * **Étape 3 : Préparer l’évaluation de la machine virtuelle avec Azure Migrate** : Contoso configure les comptes locaux et ajuste les paramètres VMware.
> * **Étape 4 : Découvrir les machines virtuelles locales avec Azure Migrate** : Contoso crée une machine virtuelle collecteur Azure Migrate. Ensuite, Contoso exécute le collecteur pour découvrir les machines virtuelles dans le cadre de l’évaluation.
> * **Étape 5 : Préparer l’analyse des dépendances avec Azure Migrate** : Contoso installe les agents Azure Migrate sur les machines virtuelles, afin de voir le mappage des dépendances entre les machines virtuelles.
> * **Étape 6 : Évaluer les machines virtuelles à l’aide d’Azure Migrate** : Contoso vérifie les dépendances, regroupe les machines virtuelles et lance l’évaluation. Une fois l’évaluation effectuée, Contoso analyse les résultats en vue de préparer la migration.

## <a name="step-1-download-and-install-data-migration-assistant"></a>Étape 1 : Télécharger et installer l’Assistant Migration de données

1. Contoso télécharge l’Assistant Migration de données à partir du [Centre de téléchargement Microsoft](https://www.microsoft.com/download/details.aspx?id=53595).
    - Vous pouvez installer l’Assistant Migration de données sur toutes les machines pouvant se connecter à l’instance SQL Server. Contoso n’a pas besoin de l’exécuter sur la machine SQL Server.
    - L’Assistant Migration de données ne doit pas être exécuté sur la machine hôte SQL Server.
2. Contoso exécute le fichier d’installation téléchargé (DownloadMigrationAssistant.msi) pour démarrer l’installation.
3. Dans la page **Terminer**, Contoso sélectionne **Lancer l’Assistant Migration de données Microsoft** avant la fin de l’exécution de l’Assistant.

## <a name="step-2-run-and-analyze-the-database-assessment-for-smarthotel360"></a>Étape 2 : Exécuter et analyser l’évaluation de la base de données pour SmartHotel360

Contoso peut à présent lancer une évaluation pour analyser sa base de données SQL Server locale pour l’application SmartHotel360.

1. Dans l’Assistant Migration de données, Contoso sélectionne **Nouveau** > **Évaluation**, puis attribue un nom de projet à l’évaluation.
2. Pour **Type de serveur source**, Contoso sélectionne **SQL Server sur des machines virtuelles Azure**.

    ![Assistant Migration de données - Sélectionner une source](./media/contoso-migration-assessment/dma-assessment-1.png)

    > [!NOTE]
      L’Assistant Migration de données ne permet pas d’effectuer une évaluation pour une migration vers Azure SQL Database Managed Instance. Pour contourner ce problème, Contoso utilise SQL Server sur une machine virtuelle Azure en tant que cible supposée pour l’évaluation.

3. Dans **Sélectionner une version cible**, Contoso sélectionne SQL Server 2017 comme version cible. Contoso doit sélectionner cette version, car c’est celle qui est utilisée par SQL Database Managed Instance.
4. Contoso sélectionne l’option Rapports pour découvrir des informations sur la compatibilité et les nouvelles fonctionnalités :
    - La section **Problèmes de compatibilité** signale des modifications qui peuvent interrompre la migration, ou qui nécessitent un ajustement mineur avant celle-ci. Ce rapport tient Contoso informé des fonctionnalités en cours d’utilisation qui sont dépréciées. Les problèmes sont classés par niveau de compatibilité.
    - La section **Recommandation de nouvelles fonctionnalités** signale de nouvelles fonctionnalités de la plateforme SQL Server cible qui peuvent être utilisées pour la base de données après la migration. Les recommandations concernant les nouvelles fonctionnalités sont répertoriées sous les en-têtes **Performances**, **Sécurité**, et **Stockage**.

    ![Assistant Migration de données - Problèmes de compatibilité et nouvelles fonctionnalités](./media/contoso-migration-assessment/dma-assessment-2.png)

2. Dans **Se connecter à un serveur**, Contoso entre le nom de la machine virtuelle qui exécute la base de données et les informations d’identification permettant d’y accéder. Contoso sélectionne **Faire confiance au certificat de serveur** pour vérifier que la machine virtuelle peut accéder à SQL Server. Ensuite, Contoso sélectionne **Se connecter**.

    ![Assistant Migration de données - Se connecter à un serveur](./media/contoso-migration-assessment/dma-assessment-3.png)

3. Dans **Ajouter une source**, Contoso ajoute la base de données qu’elle souhaite évaluer, puis sélectionne **Suivant** pour démarrer l’évaluation.
4. L’évaluation est créée.

    ![Assistant Migration de données - Créer une évaluation](./media/contoso-migration-assessment/dma-assessment-4.png)

5. Dans **Examiner les résultats**, Contoso peut voir les résultats de l’évaluation.

### <a name="analyze-the-database-assessment"></a>Analyser l’évaluation de la base de données

Les résultats s’affichent dès qu’elles sont disponibles. Si Contoso a corrigé les problèmes, elle doit sélectionner **Redémarrer l’évaluation** pour réexécuter l’évaluation.

1. Dans le rapport **Problèmes de compatibilité**, Contoso vérifie la présence éventuelle de problèmes à chaque niveau de compatibilité. Vous trouverez ci-dessous le mappage des niveaux de compatibilité aux versions SQL Server :

    - 100 : SQL Server 2008/Azure SQL Database
    - 110 : SQL Server 2012/Azure SQL Database
    - 120 : SQL Server 2014/Azure SQL Database
    - 130 : SQL Server 2016/Azure SQL Database
    - 140 : SQL Server 2017/Azure SQL Database

    ![Assistant Migration de données - Rapport Problèmes de compatibilité](./media/contoso-migration-assessment/dma-assessment-5.png)

2. Dans le rapport **Recommandations de fonctionnalités**, Contoso peut voir les fonctionnalités de performances, de sécurité et de stockage que l’évaluation recommande après la migration. De nombreuses fonctionnalités sont recommandées, notamment OLTP en mémoire, les index columnstore, Stretch Database, Always Encrypted, Dynamic Data Masking et Transparent Data Encryption.

    ![Assistant Migration de données - Rapport Recommandations de fonctionnalités](./media/contoso-migration-assessment/dma-assessment-6.png)

    > [!NOTE]
    > Contoso doit [activer Transparent Data Encryption](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption?view=sql-server-2017) pour toutes les bases de données SQL Server. Ceci est plus important pour les bases de données situées dans le cloud que pour celles qui sont hébergées localement. Transparent Data Encryption doit être activé après la migration. Si Transparent Data Encryption est déjà activé, Contoso doit déplacer le certificat ou la clé asymétrique vers la base de données MASTER du serveur cible. Découvrez comment [déplacer une base de données protégée par Transparent Data Encryption vers une autre instance de SQL Server](https://docs.microsoft.com/sql/relational-databases/security/encryption/move-a-tde-protected-database-to-another-sql-server?view=sql-server-2017)

2. Contoso peut exporter l’évaluation au format JSON ou CSV.

> [!NOTE]
> Pour les évaluations à grande échelle :
> - Vous pouvez exécuter plusieurs évaluations simultanément et afficher l’état des évaluations en ouvrant la page **Toutes les évaluations**.
> - Vous pouvez centraliser des évaluations dans une [ base de données SQL Server](https://docs.microsoft.com/sql/dma/dma-consolidatereports?view=ssdt-18vs2017#import-assessment-results-into-a-sql-server-database).
> - Vous pouvez centraliser des évaluations dans un [rapport Power BI](https://docs.microsoft.com/sql/dma/dma-powerbiassesreport?view=ssdt-18vs2017).

## <a name="step-3-prepare-for-vm-assessment-by-using-azure-migrate"></a>Étape 3 : Préparer l’évaluation de la machine virtuelle avec Azure Migrate

Contoso doit créer un compte VMware qu’Azure Migrate pourra utiliser pour découvrir automatiquement les machines virtuelles à évaluer, vérifier les autorisations permettant de créer une machine virtuelle, noter les ports qui doivent être ouverts et définir le niveau des paramètres de statistiques.

### <a name="set-up-a-vmware-account"></a>Configurer un compte VMware

La découverte des machines virtuelles nécessite un compte en lecture seule dans vCenter Server, avec les propriétés suivantes :

- **Type d’utilisateur** : Au moins un utilisateur en lecture seule.
- **Autorisations** : pour l’objet centre de données, cochez la case **Propager vers les objets enfants**. Pour **Rôle**, sélectionnez **En lecture seule**.
- **Détails** : l’utilisateur est affecté au niveau du centre de données et a donc accès à tous les objets du centre de données.
- Pour restreindre l’accès, attribuez le rôle **Aucun accès** avec l’objet **Propager vers l’objet enfant** défini sur les objets enfants (hôtes vSphere, banques de données, machines virtuelles et réseaux).

### <a name="verify-permissions-to-create-a-vm"></a>Vérifier les autorisations pour créer une machine virtuelle

Contoso vérifie qu’elle dispose des autorisations nécessaires pour créer une machine virtuelle en important un fichier au format .OVA. Découvrez comment [créer et affecter un rôle avec des privilèges](https://kb.vmware.com/s/article/1023189?other.KM_Utility.getArticleLanguage=1&r=2&other.KM_Utility.getArticleData=1&other.KM_Utility.getArticle=1&ui-comm-runtime-components-aura-components-siteforce-qb.Quarterback.validateRoute=1&other.KM_Utility.getGUser=1)

### <a name="verify-ports"></a>Vérifier les ports

L’évaluation de Contoso utilise le mappage de dépendance. Le mappage des dépendances nécessite que vous installiez un agent sur les machines virtuelles qui doivent être évaluées. Cet agent doit pouvoir se connecter à Azure via le port TCP 443 sur chaque machine virtuelle. En savoir plus sur les [exigences relatives à la connexion](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid)

### <a name="set-statistics-settings"></a>Définir les paramètres de statistiques

Avant de commencer le déploiement, Contoso doit définir les paramètres de statistiques de vCenter Server sur le niveau 3.

> [!NOTE]
> - Après avoir défini le niveau, Contoso doit attendre au moins un jour avant de lancer l’évaluation. Sinon, l’évaluation peut ne pas fonctionner comme prévu.
> - Si le niveau est supérieur à 3, l’évaluation fonctionnera, mais :
>    - Les données de performance concernant les disques et les réseaux ne seront pas collectées.
>    - Pour le stockage, Azure Migrate recommande un disque standard dans Azure, de la même taille que le disque local.
>    - Concernant les réseaux, il est recommandé d’avoir une carte réseau Azure pour chaque carte réseau locale.
>    - Pour le calcul, Azure Migrate examine le nombre de cœurs et la taille de la mémoire de la machine virtuelle, et recommande une machine virtuelle Azure avec la même configuration. S’il existe plusieurs tailles de machine virtuelle Azure éligibles, celle dont le coût est le plus faible est recommandée.
> - Pour plus d’informations sur le dimensionnement à l’aide du niveau 3, consultez [Dimensionnement](https://docs.microsoft.com/azure/migrate/concepts-assessment-calculation#sizing).

Pour définir le niveau :

1. Dans le client web vSphere, Contoso ouvre l’instance vCenter Server.
2. Contoso sélectionne **Gérer** > **Paramètres** > **Général** > **Modifier**.
3. Dans **Statistiques**, Contoso définit le niveau des paramètres de statistiques sur **Niveau 3**.

    ![Niveau de statistiques vCenter Server](./media/contoso-migration-assessment/vcenter-statistics-level.png)

## <a name="step-4-discover-vms"></a>Étape 4 : Détection des machines virtuelles

Pour détecter les machines virtuelles, Contoso crée un projet Azure Migrate. Contoso télécharge et configure la machine virtuelle collecteur. Ensuite, Contoso exécute le collecteur pour découvrir ses machines virtuelles locales.

### <a name="create-a-project"></a>Création d’un projet

1. Dans le [portail Azure](https://portal.azure.com), Contoso recherche **Azure Migrate**. Ensuite, Contoso crée un projet.
2. Contoso spécifie un nom de projet (**ContosoMigration**) et l’abonnement Azure. Elle crée un groupe de ressources Azure (**ContosoFailoverRG**).
    > [!NOTE]
    > - Vous ne pouvez créer un projet Azure Migrate que dans la région USA Centre-Ouest ou USA Est.
    > - Vous pouvez planifier une migration pour n’importe quel emplacement cible.
    > - L’emplacement du projet est utilisé uniquement pour stocker les métadonnées collectées à partir des machines virtuelles locales.

    ![Azure Migrate - Créer un projet de migration](./media/contoso-migration-assessment/project-1.png)

### <a name="download-the-collector-appliance"></a>Télécharger l’appliance collecteur

Azure Migrate crée une machine virtuelle locale connue comme étant *l’appliance collecteur*. Cette machine virtuelle découvre les machines virtuelles VMware sur site et envoie les métadonnées les concernant au service Azure Migrate. Pour configurer l’appliance collecteur, Contoso télécharge un modèle .OVA, puis l’importe dans l’instance locale de vCenter Server pour créer la machine virtuelle.

1. Dans le projet Azure Migrate, Contoso sélectionne **Prise en main** > **Découvrir et évaluer** > **Découvrir des machines**. Contoso télécharge le fichier du modèle .OVA.
2. Contoso copie l’ID et la clé du projet. Le projet et l’ID sont nécessaires pour la configuration du collecteur.

    ![Azure Migrate - Télécharger le fichier OVA](./media/contoso-migration-assessment/download-ova.png)

### <a name="verify-the-collector-appliance"></a>Vérifier l’appliance collecteur

Avant de déployer la machine virtuelle, Contoso vérifie que le fichier .OVA est sûr :

1. Sur la machine où le fichier a été téléchargé, Contoso ouvre une fenêtre d’invite de commandes Administrateur.
2. Contoso exécute la commande suivante pour générer le code de hachage du modèle OVA :

    ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```

    **Exemple**

    ```C:\>CertUtil -HashFile C:\AzureMigrate\AzureMigrate.ova SHA256```
3. Le hachage généré doit correspondre aux paramètres ci-après (version 1.0.9.15) :

    **Algorithme** | **Valeur de hachage**
    --- | ---
    MD5 | e9ef16b0c837638c506b5fc0ef75ebfa
    SHA1 | 37b4b1e92b3c6ac2782ff5258450df6686c89864
    SHA256 | 8a86fc17f69b69968eb20a5c4c288c194cdcffb4ee6568d85ae5ba96835559ba

### <a name="create-the-collector-appliance"></a>Créer l’appliance de collecteur

Contoso peut maintenant importer le fichier téléchargé dans l’instance vCenter Server et approvisionner la machine virtuelle d’appliance collecteur :

1. Dans la console du client vSphere, Contoso sélectionne **File** (Fichier) > **Deploy OVF Template** (Déployer le modèle OVF).

    ![Client web vSphere - Déployer le modèle OVF](./media/contoso-migration-assessment/vcenter-wizard.png)

2. Dans l’Assistant de déploiement du modèle OVF, Contoso sélectionne **Source**, puis spécifie l’emplacement du fichier .OVA.
3. Dans **Name and Location** (Nom et emplacement), Contoso spécifie un nom d’affichage pour la machine virtuelle collecteur. Ensuite, elle sélectionne l’emplacement de stockage dans lequel héberger la machine virtuelle. Contoso spécifie aussi l’hôte ou le cluster sur lequel exécuter l’appliance collecteur.
4. Dans **Storage** (Stockage), Contoso spécifie l’emplacement de stockage. Dans **Disk Format** (Format du disque), Contoso sélectionne la méthode de provisionnement du stockage.
5. Dans **Network Mapping** (Mappage réseau), Contoso spécifie le réseau auquel connecter la machine virtuelle collecteur. Le réseau nécessite une connexion à Internet pour envoyer des métadonnées vers Azure.
6. Contoso examine les paramètres, puis sélectionne **Power on after deployment (Mettre sous tension après le déploiement)** > **Finish (Terminer)**. Un message confirmant la réussite est affiché après la création de l’appliance.

### <a name="run-the-collector-to-discover-vms"></a>Exécutez le collecteur pour découvrir les machines virtuelles

Ensuite, Contoso exécute le collecteur pour découvrir les machines virtuelles. Notez que le collecteur prend uniquement en charge **Anglais (États-Unis)** comme langue du système d’exploitation et langue de l’interface du collecteur.

1. Dans la console du client vSphere, Contoso sélectionne **Open Console** (Ouvrir la console). Contoso indique les préférences de langue, de fuseau horaire et de mot de passe pour la machine virtuelle collecteur.
2. Sur le Bureau, Contoso sélectionne le raccourci **Run collector** (Exécuter le collecteur).

    ![Console du client vSphere - Raccourci du collecteur](./media/contoso-migration-assessment/collector-shortcut.png)

3. Dans Azure Migrate Collector, ouvrez **Configurer les prérequis**. Contoso accepte les termes de licence et lit les informations relatives aux tiers.
4. Le collecteur vérifie que la machine virtuelle a accès à Internet, que l’heure est synchronisée et que le service du collecteur est en cours d’exécution (le collecteur est installé par défaut sur la machine virtuelle). Contoso installe également VMware PowerCLI.

    > [!NOTE]
    > Nous partons du principe que la machine virtuelle a un accès direct à Internet, sans proxy.

    ![Azure Migrate Collector - Vérifier la configuration requise](./media/contoso-migration-assessment/collector-verify-prereqs.png)

5. Dans **Spécifier les détails vCenter Server**, Contoso spécifie le nom (FQDN) ou l’adresse IP de l’instance vCenter Server, ainsi que les informations d’identification en lecture seule utilisées pour la découverte.
6. Contoso sélectionne une étendue pour la découverte des machines virtuelles. Le collecteur peut uniquement découvrir les machines virtuelles situées dans l’étendue spécifiée. L’étendue peut être définie au niveau d’un dossier, d’un centre de données ou d’un cluster. L’étendue ne doit pas inclure plus de 1 500 machines virtuelles.

    ![Spécifier les détails vCenter Server](./media/contoso-migration-assessment/collector-connect-vcenter.png)

7. Dans **Spécifier le projet de migration**, Contoso entre l’ID et la clé du projet Azure Migrate qui ont été copiés à partir du portail. Pour obtenir l’ID et la clé du projet, Contoso peut accéder à la page **Vue d’ensemble** du projet > **Découvrir des machines**.  

    ![Spécifier un projet de migration](./media/contoso-migration-assessment/collector-connect-azure.png)

8. Dans **Afficher la progression de la collecte**, Contoso peut surveiller le processus de découverte et vérifier que les métadonnées collectées à partir des machines virtuelles s’inscrivent dans l’étendue spécifiée. Le collecteur fournit une durée approximative de la découverte.

    ![Afficher la progression de la collecte](./media/contoso-migration-assessment/collector-collection-process.png)

### <a name="verify-vms-in-the-portal"></a>Vérifier les machines virtuelles dans le portail

Une fois la collecte terminée, Contoso vérifie que les machines virtuelles apparaissent dans le portail :

1. Dans le projet Azure Migrate, Contoso sélectionne **Gérer** > **Machines**. Contoso vérifie que les machines virtuelles à découvrir sont affichées.

    ![Azure Migrate - Machines découvertes](./media/contoso-migration-assessment/discovery-complete.png)

2. Les agents Azure Migrate ne sont pas encore installés sur les machines. Contoso doit installer les agents pour voir les dépendances.

    ![Azure Migrate - Installation de l’agent nécessaire](./media/contoso-migration-assessment/machines-no-agent.png)

## <a name="step-5-prepare-for-dependency-analysis"></a>Étape 5 : Préparer l’analyse des dépendances

Pour voir les dépendances qui existent entre les machines virtuelles à évaluer, Contoso télécharge et installe des agents sur les machines virtuelles de l’application. Contoso installe les agents sur toutes les machines virtuelles pour ses applications (Windows et Linux).

### <a name="take-a-snapshot"></a>Prendre un instantané

Avant de modifier les machines virtuelles, Contoso fait une copie des machines en prenant un instantané avant l’installation des agents.

![Instantané de la machine virtuelle](./media/contoso-migration-assessment/snapshot-vm.png)

### <a name="download-and-install-the-vm-agents"></a>Téléchargement et installation des agents de machines virtuelles

1. Dans **Machines**, Contoso sélectionne la machine. Dans la colonne **Dépendances**, Contoso sélectionne **Installation requise**.
2. Dans le volet **Découvrir des machines**, Contoso :
    - Télécharge Microsoft Monitoring Agent (MMA) et Dependency Agent pour chaque machine virtuelle Windows
    - Télécharge Microsoft Monitoring Agent (MMA) et Dependency Agent pour chaque machine virtuelle Linux
3. Contoso copie à présent l’ID et la clé de l’espace de travail. Contoso a besoin de l’ID et de la clé de l’espace de travail pour installer Microsoft Monitoring Agent.

    ![Téléchargement de l’agent](./media/contoso-migration-assessment/download-agents.png)

### <a name="install-the-agents-on-windows-vms"></a>Installer les agents sur les machines virtuelles Windows

Contoso exécute l’installation sur chaque machine virtuelle.

#### <a name="install-the-mma-on-windows-vms"></a>Installer l’agent MMA sur les machines virtuelles Windows

1. Contoso double-clique sur l’agent téléchargé.
2. Dans la page **Dossier de destination**, Contoso conserve le dossier d’installation par défaut, puis sélectionne **Suivant**.
3. Dans **Options d’installation de l’agent**, Contoso sélectionne **Connecter l’agent à Azure Log Analytics** > **Suivant**.

    ![Installation de Microsoft Monitoring Agent - Options d’installation de l’agent](./media/contoso-migration-assessment/mma-install.png)

4. Dans **Azure Log Analytics**, Contoso colle l’ID et la clé de l’espace de travail copiés à partir du portail.

    ![Installation de Microsoft Monitoring Agent - Azure Log Analytics](./media/contoso-migration-assessment/mma-install2.png)

5. Dans **Prêt pour l’installation**, Contoso installe Microsoft Monitoring Agent.

#### <a name="install-the-dependency-agent-on-windows-vms"></a>Installer l’agent de dépendances sur les machines virtuelles Windows

1. Contoso double-clique sur le fichier Dependency Agent téléchargé.
2. Contoso accepte les termes du contrat de licence et attend que l’installation se termine.

    ![Installation de Dependency Agent - Installation](./media/contoso-migration-assessment/dependency-agent.png)

### <a name="install-the-agents-on-linux-vms"></a>Installer les agents sur les machines virtuelles Linux

Contoso exécute l’installation sur chaque machine virtuelle.

#### <a name="install-the-mma-on-linux-vms"></a>Installer l’agent MMA sur les machines virtuelles Linux

1. Contoso installe la bibliothèque ctypes Python sur chaque machine virtuelle à l’aide de la commande suivante :

    `sudo apt-get install python-ctypeslib`
2. Contoso doit exécuter la commande pour installer Microsoft Monitoring Agent comme agent racine. Pour qu’il devienne racine, Contoso doit exécuter la commande suivante et entrer le mot de passe racine :

    `sudo -i`
3. Contoso installe Microsoft Monitoring Agent :
    - Contoso entre l’ID et la clé de l’espace de travail dans la commande.
    - Ces commandes sont de type 64 bits.
    - L’ID de l’espace de travail et la clé primaire se trouvent dans l’espace de travail Log Analytics dans le portail Azure. Sélectionnez **Paramètres**, puis l’onglet **Sources connectées**.
    - Exécutez les commandes suivantes pour télécharger l’agent Log Analytics, valider la somme de contrôle, puis installer et intégrer l’agent :

    ```
    wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w 6b7fcaff-7efb-4356-ae06-516cacf5e25d -s k7gAMAw5Bk8pFVUTZKmk2lG4eUciswzWfYLDTxGcD8pcyc4oT8c6ZRgsMy3MmsQSHuSOcmBUsCjoRiG2x9A8Mg==
    ```

#### <a name="install-the-dependency-agent-on-linux-vms"></a>Installer Dependency Agent sur des machines virtuelles Linux

Une fois Microsoft Monitoring Agent installé, Contoso installe Dependency Agent sur les machines virtuelles Linux :

1. Dependency Agent est installé sur les ordinateurs Linux avec InstallDependencyAgent-Linux64.bin, un script shell comprenant un fichier binaire à extraction automatique. Contoso exécute le fichier à l’aide de sh ou y ajoute des autorisations d’exécution.

2. Contoso installe l’instance de Dependency Agent pour Linux en tant que racine :

    ```
    wget --content-disposition https://aka.ms/dependencyagentlinux -O InstallDependencyAgent-Linux64.bin && sudo sh InstallDependencyAgent-Linux64.bin -s
    ```

## <a name="step-6-run-and-analyze-the-vm-assessment"></a>Étape 6 : Exécuter et analyser l’évaluation de la machine virtuelle

Contoso peut à présent vérifier les dépendances de machine et créer un groupe. Ensuite, Contoso effectue l’évaluation pour le groupe.

### <a name="verify-dependencies-and-create-a-group"></a>Vérifier les dépendances et créer un groupe

1. Pour déterminer quelles machines doivent être analysées, Contoso sélectionne **Afficher les dépendances**.

    ![Azure Migrate - Afficher les dépendances des machines](./media/contoso-migration-assessment/view-machine-dependencies.png)

2. Pour la machine virtuelle SQLVM, la carte des dépendances montre les informations suivantes :

    - Groupes de processus ou processus avec des connexions réseau actives s’exécutant sur la machine virtuelle SQLVM au cours d’une période spécifiée (une heure, par défaut).
    - Connexion TCP entrantes (client) et sortantes (serveur) vers et depuis toutes les machines dépendantes.
    - Les machines dépendantes sur lesquelles sont installés des agents Azure Migrate sont affichées dans des zones distinctes.
    - Les machines sans agents affichent des informations de port et d’adresse IP.

3. Pour les machines sur lesquelles est installé l’agent (WEBVM), Contoso sélectionne la zone de la machine pour afficher plus d’informations. Ces informations comprennent le nom de domaine complet, le système d’exploitation et l’adresse MAC.

    ![Azure Migrate - Afficher les dépendances des groupes](./media/contoso-migration-assessment/sqlvm-dependencies.png)

4. Contoso sélectionne les machines virtuelles à ajouter au groupe (SQLVM et WEBVM). Contoso utilise CTRL + clic pour sélectionner plusieurs machines virtuelles.
5. Contoso sélectionne **Créer un groupe**, puis entre un nom (**smarthotelapp**).

    > [!NOTE]
    > Pour afficher des dépendances plus précises, vous pouvez étendre l’intervalle de temps. Vous pouvez sélectionner une durée, ou une date de début et de fin.

### <a name="run-an-assessment"></a>Exécuter une évaluation

1. Dans **Groupes**, Contoso ouvre le groupe (**smarthotelapp**), puis sélectionne **Créer une évaluation**.

    ![Azure Migrate - Créer une évaluation](./media/contoso-migration-assessment/run-vm-assessment.png)

2. Pour afficher l’évaluation, Contoso sélectionne **Gérer** > **Évaluations**.

Contoso utilise les paramètres d’évaluation par défaut, mais vous pouvez [personnaliser les paramètres](how-to-modify-assessment.md).

### <a name="analyze-the-vm-assessment"></a>Analyser l’évaluation de la machine virtuelle

Une évaluation Azure Migrate inclut des informations sur la compatibilité des ordinateurs locaux avec Azure, des suggestions concernant la taille idéale des machines virtuelles Azure et une estimation des coûts mensuels Azure.

![Azure Migrate - Rapport d’évaluation](./media/contoso-migration-assessment/assessment-overview.png)

#### <a name="review-confidence-rating"></a>Examiner le niveau de confiance

![Azure Migrate - Affichage de l’évaluation](./media/contoso-migration-assessment/assessment-display.png)

Une évaluation obtient un niveau de confiance compris entre 1 étoile et 5 étoiles (1 étoile étant le niveau le plus bas et 5 étoiles le niveau le plus élevé).
- Le niveau de confiance est affecté à une évaluation en fonction de la disponibilité des points de données qui sont nécessaires pour calculer l’évaluation.
- Ce niveau vous permet d’évaluer la fiabilité des recommandations de taille fournies par Azure Migrate.
- Le niveau de confiance est utile lorsque vous effectuez un *dimensionnement basé sur les performances*. Azure Migrate peut ne pas avoir suffisamment de points de données pour le dimensionnement basé sur l’utilisation. Pour un dimensionnement effectué *Localement*, le niveau de confiance s’élève toujours à 5 étoiles, car Azure Migrate dispose de tous les points de données nécessaires au dimensionnement de la machine virtuelle.
- Selon le pourcentage de points de données disponibles, le niveau de confiance pour l’évaluation est fourni :

   Disponibilité des points de données | Niveau de confiance
   --- | ---
   0 %-20 % | 1 étoile
   21 %-40 % | 2 étoiles
   41 %-60 % | 3 étoiles
   61 %-80 % | 4 étoiles
   81 %-100 % | 5 étoiles

#### <a name="verify-azure-readiness"></a>Vérifier la compatibilité avec Azure

![Azure Migrate - Évaluation de la préparation](./media/contoso-migration-assessment/azure-readiness.png)  

Le rapport d’évaluation affiche les informations qui sont regroupées dans le tableau. Pour afficher le dimensionnement basé sur les performances, Azure Migrate a besoin des informations suivantes. Si ces informations ne peuvent pas être collectées, l’évaluation du dimensionnement risque de ne pas être suffisamment précise.

- Données d’utilisation pour le processeur et la mémoire.
- E/S par seconde en lecture et en écriture de chaque disque attaché à la machine virtuelle.
- Informations entrantes/sortantes du réseau pour chaque carte réseau attachée à la machine virtuelle.

Paramètre | Indication | Détails
--- | --- | ---
**Préparation des machines virtuelles pour Azure** | Indique si la machine virtuelle est prête pour la migration. | États possibles :</br><br/>- Prête pour Azure<br/><br/>- Prête sous conditions <br/><br/>- Pas prête pour Azure<br/><br/>- État de préparation inconnue<br/><br/> Si une machine virtuelle n’est pas prête, Azure Migrate fournit des étapes de correction.
**Taille de la machine virtuelle Azure** | Pour les machines virtuelles prêtes, Azure Migrate fournit des recommandations concernant la taille des machines virtuelles Azure. | La recommandation de taille dépend des propriétés de l’évaluation :<br/><br/>- Si vous avez utilisé un dimensionnement basé sur les performances, le dimensionnement prend en compte l’historique des performances des machines virtuelles.<br/><br/>- Si vous avez sélectionné l’option *Localement*, le dimensionnement est basé sur la taille de la machine virtuelle locale, et les données d’utilisation ne sont pas utilisées.
**Outil suggéré** | Étant donné que les machines Azure exécutent des agents, Azure Migrate examine les processus qui sont exécutés sur les machines. Il détermine si la machine est une machine de base de données.
**VM information** (Informations de machine virtuelle) | Le rapport affiche les paramètres de la machine virtuelle locale, y compris des informations sur le système d’exploitation, le type de démarrage, le disque et le stockage.

#### <a name="review-monthly-cost-estimates"></a>Examiner les estimations des coûts mensuels

Cette vue affiche le coût total du calcul et du stockage liés à l’exécution des machines virtuelles dans Azure. Elle montre également des informations détaillées sur chaque machine.

![Azure Migrate - Coûts Azure](./media/contoso-migration-assessment/azure-costs.png)

- Les estimations de coûts sont calculées à l’aide des recommandations de taille pour une machine.
- L’estimation des coûts mensuels de calcul et de stockage est agrégée pour toutes les machines virtuelles dans le groupe.

## <a name="clean-up-after-assessment"></a>Nettoyer après évaluation

- Une fois l’évaluation terminée, Contoso conserve l’appliance Azure Migrate en vue d’évaluations ultérieures.
- Contoso désactive la machine virtuelle VMware. Contoso la réutilisera au moment d’évaluer d’autres machines virtuelles.
- Contoso conserve le projet **Migration de Contoso** dans Azure. Le projet est actuellement déployé dans le groupe de ressources **ContosoFailoverRG**, dans la région USA Est Azure.
-  La licence d’évaluation de la machine virtuelle collecteur a une validité de 180 jours. Au-delà de cette limite, Contoso devra de nouveau télécharger et installer le collecteur.

## <a name="conclusion"></a>Conclusion

Dans ce scénario, Contoso évalue son application de base de données SmartHotel360 avec l’outil d’évaluation de la migration de données. Contoso évalue les machines virtuelles locales à l’aide du service Azure Migrate. Contoso étudie les évaluations pour vérifier que les ressources locales sont prêtes pour la migration vers Azure.

## <a name="next-steps"></a>Étapes suivantes

Dans l’article suivant de cette série, Contoso réhéberge son application SmartHotel360 dans Azure avec une migration de type lift-and-shift. Contoso utilise Azure Site Recovery afin de migrer la machine virtuelle frontend WEBVM pour l’application. Contoso migre la base de données d’application vers Azure SQL Database Managed Instance avec Database Migration Service. [Commencez](contoso-migration-rehost-vm-sql-managed-instance.md) avec ce déploiement.
