---
title: Déployer App Service dans un environnement hors connexion dans Azure Stack | Microsoft Docs
description: Instructions détaillées sur le déploiement d’App Service dans un environnement Azure Stack déconnecté sécurisé par AD FS.
services: azure-stack
documentationcenter: ''
author: apwestgarth
manager: stefsch
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/13/2018
ms.author: anwestg
ms.openlocfilehash: a3703133e5dc5f0525638562fab98f9416bcf26e
ms.sourcegitcommit: 30d23a9d270e10bb87b6bfc13e789b9de300dc6b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/08/2019
ms.locfileid: "54104656"
---
# <a name="add-an-app-service-resource-provider-to-a-disconnected-azure-stack-environment-secured-by-ad-fs"></a>Ajouter un fournisseur de ressources App Service à un environnement Azure Stack déconnecté sécurisé par AD FS

*S’applique à : systèmes intégrés Azure Stack et Kit de développement Azure Stack*

> [!IMPORTANT]
> Appliquez la mise à jour 1809 à votre système intégré Azure Stack ou déployez le dernier Kit de développement Azure Stack avant de déployer Azure App Service 1.4.
>
>

En suivant les instructions de cet article, vous pouvez installer le [fournisseur de ressources App Service](azure-stack-app-service-overview.md) dans un environnement Azure Stack qui :

- n’est pas connecté à internet ;
- sécurisé par les services de fédération Active Directory (AD FS).

Pour ajouter le fournisseur de ressources App Service à votre déploiement Azure Stack hors connexion, vous devez effectuer ces tâches de niveau supérieur :

1. Effectuez les [étapes préalables](azure-stack-app-service-before-you-get-started.md) (telles que l’achat de certificats, ce qui peut prendre quelques jours avant leur réception).
2. [Téléchargez et extrayez les fichiers d’installation et d’assistance](azure-stack-app-service-before-you-get-started.md) sur un ordinateur connecté à Internet.
3. Créez un package d’installation hors connexion.
4. Exécutez le fichier du programme d’installation appservice.exe.

## <a name="create-an-offline-installation-package"></a>Créer un package d'installation hors connexion

Pour déployer App Service dans un environnement déconnecté, vous devez d’abord créer un package d’installation hors connexion sur une machine connectée à Internet.

1. Exécutez le programme d’installation AppService.exe sur une machine connectée à Internet.

2. Cliquez sur **Avancé** > **Créer un package d’installation hors connexion**.

    ![Programme d’installation App Service][1]

3. Le programme d’installation App Service crée un package d’installation hors connexion et affiche le chemin pour y accéder. Vous pouvez cliquer sur **Ouvrir le dossier** pour ouvrir le dossier dans l’Explorateur de fichiers.

    ![Programme d’installation App Service](media/azure-stack-app-service-deploy-offline/image02.png)

4. Copiez le programme d’installation AppService.exe et le package d’installation hors connexion sur l’ordinateur hôte Azure Stack.

## <a name="complete-the-offline-installation-of-app-service-on-azure-stack"></a>Terminer l’installation hors connexion d’App Service sur Azure Stack

1. Exécutez appservice.exe en tant qu’administrateur sur un ordinateur pouvant atteindre le point de terminaison de gestion des ressources Azure de l’administrateur Azure Stack.

2. Cliquez sur **Avancé** > **Effectuer l’installation hors connexion**.

    ![Programme d’installation App Service][2]

3. Accédez à l’emplacement du package d’installation hors connexion que vous avez créé, puis cliquez sur **Suivant**.

    ![Programme d’installation App Service](media/azure-stack-app-service-deploy-offline/image04.png)

4. Consultez et acceptez les termes du contrat de licence logiciel Microsoft, puis cliquez sur **Suivant**.

5. Consultez et acceptez les termes du contrat de licence tiers, puis cliquez sur **Suivant**.

6. Vérifiez l’exactitude des informations de configuration du Cloud App Service. Si vous avez utilisé les paramètres par défaut au cours du déploiement du Kit de développement Azure Stack, vous pouvez accepter les valeurs par défaut ici. Toutefois, si vous avez personnalisé les options lors du déploiement d’Azure Stack, ou que vous effectuez le déploiement sur un système intégré, vous devrez modifier les valeurs dans cette fenêtre de façon à l’indiquer. Par exemple, si vous utilisez le suffixe de domaine mycloud.com, vous devrez remplacer votre point de terminaison Azure Resource Manager de locataire Azure Stack par management.<region>.mycloud.com. Après avoir confirmé vos informations, cliquez sur **suivant**.

    ![Programme d’installation App Service][3]

7. Sur la page suivante :
    1. Cliquez sur le bouton **Se connecter** situé en regard de la zone **Abonnements Azure Stack**.
        - Indiquez votre compte Administrateur. Par exemple : cloudadmin@azurestack.local. Entrez votre mot de passe, puis cliquez sur **Se connecter**.
    2. Dans la zone **Abonnements Azure Stack**, sélectionnez **Abonnement au fournisseur par défaut**.
    
    > [!NOTE]
    > Le déploiement d’App Service n’est possible que sur l’**abonnement du fournisseur par défaut**.
    >
    
    3. Dans la zone **Emplacements Azure Stack**, sélectionnez l’emplacement qui correspond à la région où vous effectuez le déploiement. Par exemple, sélectionnez **local** si effectuez votre déploiement sur le Kit de développement Azure Stack.
    4. Cliquez sur **Suivant**.

    ![Programme d’installation App Service][4]

8. Vous avez maintenant la possibilité d’effectuer le déploiement sur un réseau virtuel existant en suivant la configuration indiquée dans les étapes mentionnées [ici](azure-stack-app-service-before-you-get-started.md#virtual-network), ou d’autoriser le programme d’installation App Service à créer un réseau virtuel et des sous-réseaux associés.
    1. Sélectionnez **Créer un réseau virtuel avec les paramètres par défaut**, acceptez les valeurs par défaut, puis cliquez sur **Suivant** ; ou
    2. Sélectionnez **Utiliser un réseau virtuel et des sous-réseaux existants**.
        1. Sélectionnez le **Groupe de ressources** qui contient votre réseau virtuel ;
        2. Choisissez le nom du **Réseau virtuel** sur lequel vous voulez effectuer le déploiement ;
        3. Sélectionnez les valeurs **Sous-réseau** correspondant à chacun des sous-réseaux de rôle nécessaires ;
        4. Cliquez sur **Suivant**

    ![Programme d’installation App Service][5]

9. Entrez les informations du partage de fichiers, puis cliquez sur **Suivant**. L’adresse du partage de fichiers doit utiliser le nom de domaine complet ou l’adresse IP de votre serveur de fichiers. Par exemple, \\\appservicefileserver.local.cloudapp.azurestack.external\websites ou \\\10.0.0.1\websites

    > [!NOTE]
    > Le programme d’installation tente de tester la connectivité au partage de fichiers avant de continuer.  Toutefois, si vous avez choisi d’effectuer le déploiement sur un réseau virtuel existant, il est possible que le programme d’installation ne puisse pas se connecter au partage de fichiers et affiche un avertissement vous demandant si vous souhaitez continuer.  Vérifiez les informations du partage de fichiers et continuez si elles sont correctes.
    >
    >

   ![Programme d’installation App Service][8]

10. Sur la page suivante :
    1. Dans la zone **ID d’application d’identité**, entrez le GUID de l’application que vous utilisez pour l’identité (à partir d’Azure AD).
    2. Dans la zone **fichier de certificat d’application d’identité**, entrez (ou accédez à) l’emplacement du fichier de certificat.
    3. Dans la zone **Mot de passe du certificat d’application d’identité**, entrez le mot de passe du certificat. Ce mot de passe est celui que vous avez noté lorsque vous avez utilisé le script pour créer les certificats.
    4. Dans la zone **fichier de certificat racine Azure Resource Manager**, entrez (ou accédez à) l’emplacement du fichier de certificat.
    5. Cliquez sur **Suivant**.

    ![Programme d’installation App Service][10]

11. Pour chacune des trois zones de fichier de certificat, cliquez sur **Parcourir**, puis accédez au fichier de certificat approprié. Vous devez fournir le mot de passe pour chaque certificat. Ces certificats sont ceux que vous avez créés lors de l’étape [Créer les certificats requis](azure-stack-app-service-before-you-get-started.md#get-certificates). Cliquez sur **Suivant** après avoir entré toutes les informations.

    | Box | Exemple de nom de fichier de certificat |
    | --- | --- |
    | **Fichier de certificat SSL par défaut d’App Service** | \_.appservice.local.AzureStack.external.pfx |
    | **Fichier de certificat SSL d’API App Service** | api.appservice.local.AzureStack.external.pfx |
    | **Fichier de certificat SSL Publisher App Service** | ftp.appservice.local.AzureStack.external.pfx |

    Si vous avez utilisé un suffixe de domaine différent lorsque vous avez créé les certificats, vos noms de fichier n’utilisent pas *local.AzureStack.external*. Utilisez alors vos informations de domaine personnalisées.

    ![Programme d’installation App Service][11]

12. Entrez les détails SQL Server de l’instance de serveur utilisée pour héberger les bases de données du fournisseur de ressources App Service, puis cliquez sur **Suivant**. Le programme d’installation valide les propriétés de connexion SQL. Vous **devez** entrer l’adresse IP interne ou le nom de domaine complet comme nom de serveur SQL Server.

    > [!NOTE]
    > Le programme d’installation tente de tester la connectivité au serveur SQL Server avant de continuer.  Toutefois, si vous avez choisi d’effectuer le déploiement sur un réseau virtuel existant, il est possible que le programme d’installation ne puisse pas se connecter au serveur SQL Server et affiche un avertissement vous demandant si vous souhaitez continuer.  Vérifiez les informations SQL Server et continuez si elles sont correctes.
    >
    > À compter d’Azure App Service sur Azure Stack 1.3 et versions ultérieures, le programme d’installation vérifie l’autonomie de la base de données est activée au niveau de SQL Server.  Si ce n’est pas le cas, l’exception suivante apparaît :
    > ```sql
    >    Enable contained database authentication for SQL server by running below command on SQL server (Ctrl+C to copy)
    >    ***********************************************************
    >    sp_configure 'contained database authentication', 1;  
    >    GO  
    >    RECONFIGURE;  
    >    GO
    >    ***********************************************************
    > ```
    > Reportez-vous aux [notes de publication pour Azure App Service sur Azure Stack 1.3](azure-stack-app-service-release-notes-update-three.md) pour plus d’informations.
   
   ![Programme d’installation App Service][12]

13. Passez en revue de l’instance de rôle et les options de la référence SKU. Les valeurs par défaut sont remplies avec le nombre minimal d’instances et la référence (SKU) minimale pour chaque rôle dans un déploiement ASDK. Un résumé des exigences en termes de processeur virtuel et de mémoire est fourni pour vous aider à planifier votre déploiement. Une fois vos sélections effectuées, cliquez sur **Suivant**.

     > [!NOTE]
     > Pour les déploiements de production, suivez l’aide fournie par la page [Planification de la capacité pour les rôles serveur Azure App Service dans Azure Stack](azure-stack-app-service-capacity-planning.md).
     >
     >

    | Rôle | Nombre minimal d’instances | Nombre minimal de références (SKU) | Notes |
    | --- | --- | --- | --- |
    | Controller | 1 | Standard_A2 - (2 processeurs virtuels, 3584 Mo) | Gère et maintient l’intégrité du Cloud App Service. |
    | gestion | 1 | Standard_A2 - (2 processeurs virtuels, 3 584 Mo) | Gère les points de terminaison App Service Azure Resource Manager et d’API, les extensions du portail (admin, locataire, portail Functions) et du service des données. Pour prendre en charge le basculement, augmenter les instances recommandées à 2. |
    | Publisher | 1 | Standard_A1 - (1 processeur virtuel, 1 792 Mo) | Publie du contenu via FTP et un déploiement web. |
    | FrontEnd | 1 | Standard_A1 - (1 processeur virtuel, 1 792 Mo) | Achemine les demandes vers les applications App Service. |
    | Worker partagé | 1 | Standard_A1 - (1 processeur virtuel, 1 792 Mo) | Héberge les applications web ou d’API et Azure Functions. Il peut être nécessaire d’ajouter plus d’instances. En tant qu’opérateur, vous pouvez définir votre offre et choisir n’importe quel niveau de référence. Les niveaux doivent avoir au minimum un processeur virtuel. |

    ![Programme d’installation App Service][14]

    > [!NOTE]
    > **Windows Server 2016 Core n’est pas une image de plateforme prise en charge pour une utilisation avec Azure App Service sur Azure Stack.  N’utilisez pas d’images d’évaluation pour les déploiements de production.  Azure App Service sur Azure Stack nécessite l’activation de Microsoft.NET 3.5.1 SP1 sur l’image utilisée pour le déploiement.   Cette fonctionnalité n’est pas activée sur les images Windows Server 2016 syndiquées sur la Place de marché. Dès lors, vous devez créer et utiliser une image Windows Server 2016 avec cette fonctionnalité pré-activée.**

14. Dans la zone **Sélectionnez l’image de plate-forme**, choisissez votre image de machine virtuelle Windows Server 2016 de déploiement parmi celles disponibles dans le fournisseur de ressources de calcul pour le cloud App Service. Cliquez sur **Suivant**.

15. Sur la page suivante :
     1. Entrez le nom d’utilisateur et le mot de passe de l’administrateur de la machine virtuelle du rôle Worker.
     2. Entrez le nom d’utilisateur et le mot de passe de l’administrateur de la machine virtuelle des autres rôles.
     3. Cliquez sur **Suivant**.

    ![Programme d’installation App Service][16]

16. Sur la page de résumé :
    1. Vérifiez les choix effectués. Pour apporter des modifications, utilisez les boutons **Précédent** pour visiter les pages précédentes.
    2. Si les configurations sont correctes, cochez la case.
    3. Cliquez sur **Suivant** pour commencer le déploiement.

    ![Programme d’installation App Service][17]

17. Sur la page suivante :
    1. Suivez la progression de l’installation. Le déploiement d’App Service sur Azure Stack prend environ 60 minutes avec les sélections par défaut.
    2. Une fois le programme d’installation terminé avec succès, cliquez sur **Quitter**.

    ![Programme d’installation App Service][18]

## <a name="validate-the-app-service-on-azure-stack-installation"></a>Valider l’installation App Service sur Azure Stack

1. Dans le portail administrateur d’Azure Stack, allez dans **Administration - App Service**.

2. Dans la vue d’ensemble, sous les statuts, vérifiez que le **Statut** affiche **Tous les rôles sont prêts**.

    ![Gestion d’App Service](media/azure-stack-app-service-deploy/image12.png)

> [!NOTE]
> Si vous avez choisi de déployer un modèle dans un réseau virtuel existant et une adresse IP interne pour vous connecter à votre serveur de fichiers, vous devez ajouter une règle de sécurité sortante, qui autorise le trafic SMB entre le sous-réseau Worker et le serveur de fichiers.  Pour ce faire, accédez au WorkersNsg dans le portail d’administration, puis ajoutez une règle de sécurité sortante comportant les propriétés suivantes :
> * Source : Quelconque
> * Plage de ports source : : *
> * Destination : Adresses IP
> * Plage d’adresses IP de destination : plage d’adresses IP de votre serveur de fichiers
> * Plage de ports de destination : 445
> * Protocole : TCP
> * Action : AUTORISER
> * Priorité : 700
> * Nom : Outbound_Allow_SMB445
>

## <a name="test-drive-app-service-on-azure-stack"></a>Tester App Service sur Azure Stack

Après avoir déployé et inscrit le fournisseur de ressources App Service, testez-le pour vous assurer que les utilisateurs peuvent déployer des applications web et d’API.

> [!NOTE]
> Vous devez créer une offre avec l’espace de noms Microsoft.Web dans le plan. Ensuite, vous devez avoir un abonnement locataire qui s’abonne à cette offre. Pour plus d’informations, consultez [Créer une offre](azure-stack-create-offer.md) et [Créer un plan](azure-stack-create-plan.md).
>
> Vous *devez* disposer d’un abonnement locataire pour créer des applications qui utilisent App Service sur Azure Stack. Les seules fonctionnalités qu’un administrateur de service peut effectuer dans le portail d’administration sont liées à l’administration du fournisseur de ressources App Service. Ces fonctionnalités incluent l’ajout de capacité, la configuration de sources de déploiement, l’ajout de niveaux Worker et de références.
>
> À compter de la troisième préversion technique, vous devez utiliser le portail du locataire et disposer d’un abonnement locataire pour créer des applications web, d’API et Azure Functions.

1. Dans le portail du locataire Azure Stack, cliquez sur **+Créer une ressource** > **Web + Mobile** > **Application web**.

2. Dans le panneau **Application web**, tapez un nom dans la zone **Application web**.

3. Sous **Groupe de ressources**, cliquez sur **Nouveau**. Tapez un nom dans la zone **Groupe de ressources**.

4. Cliquez sur **Plan App Service/Emplacement** > **Créer**.

5. Dans le panneau **Plan App Service**, tapez un nom dans la zone **Plan App Service**.

6. Cliquez sur **Niveau tarifaire** > **Libre-Partagé** ou **Partagé-Partagé** > **Sélectionnez** > **OK** > **Créer**.

7. En moins d’une minute, une vignette pour la nouvelle application web s’affiche dans le tableau de bord. Cliquez sur la vignette.

8. Dans le panneau **Application web**, cliquez sur **Parcourir** pour afficher le site web par défaut pour cette application.

## <a name="deploy-a-wordpress-dnn-or-django-website-optional"></a>Déployer un site web WordPress, DNN ou Django (facultatif)

1. Dans le portail du locataire Azure Stack, cliquez sur **+**, accédez à la Place de marché Azure, déployez un site web Django et attendez que l’opération se termine. La plateforme web Django utilise une base de données basée sur système de fichiers. Elle ne nécessite aucun fournisseur de ressources supplémentaire comme SQL ou MySQL.

2. Si vous avez également déployé un fournisseur de ressources MySQL, vous pouvez déployer un site web WordPress à partir de la Place de marché. Lorsque vous êtes invité à entrer les paramètres de la base de données, entrez le nom d’utilisateur *User1@Server1*, avec le nom d’utilisateur et le nom de serveur de votre choix.

3. Si vous avez également déployé un fournisseur de ressources SQL Server, vous pouvez déployer un site web DNN à partir de la Place de marché. Lorsque vous êtes invité à entrer les paramètres de la base de données, sélectionnez une base de données sur l’ordinateur exécutant SQL Server connecté à votre fournisseur de ressources.

## <a name="next-steps"></a>Étapes suivantes

Vous pouvez également tester d’autres [services PaaS](azure-stack-tools-paas-services.md).

- [Fournisseur de ressources SQL Server](azure-stack-sql-resource-provider-deploy.md)
- [Fournisseur de ressources MySQL](azure-stack-mysql-resource-provider-deploy.md)

<!--Links-->
[Azure_Stack_App_Service_preview_installer]: https://go.microsoft.com/fwlink/?LinkID=717531
[App_Service_Deployment]: https://go.microsoft.com/fwlink/?LinkId=723982
[AppServiceHelperScripts]: https://go.microsoft.com/fwlink/?LinkId=733525

<!--Image references-->
[1]: ./media/azure-stack-app-service-deploy-offline/app-service-exe-advanced-create-package.png
[2]: ./media/azure-stack-app-service-deploy-offline/app-service-exe-advanced-complete-offline.png
[3]: ./media/azure-stack-app-service-deploy-offline/app-service-azure-stack-arm-endpoints.png
[4]: ./media/azure-stack-app-service-deploy-offline/app-service-azure-stack-subscription-information.png
[5]: ./media/azure-stack-app-service-deploy-offline/app-service-default-VNET-config.png
[6]: ./media/azure-stack-app-service-deploy-offline/app-service-custom-VNET-config.png
[7]: ./media/azure-stack-app-service-deploy-offline/app-service-custom-VNET-config-with-values.png
[8]: ./media/azure-stack-app-service-deploy-offline/app-service-fileshare-configuration.png
[9]: ./media/azure-stack-app-service-deploy-offline/app-service-fileshare-configuration-error.png
[10]: ./media/azure-stack-app-service-deploy-offline/app-service-identity-app.png
[11]: ./media/azure-stack-app-service-deploy-offline/app-service-certificates.png
[12]: ./media/azure-stack-app-service-deploy-offline/app-service-sql-configuration.png
[13]: ./media/azure-stack-app-service-deploy-offline/app-service-sql-configuration-error.png
[14]: ./media/azure-stack-app-service-deploy-offline/app-service-cloud-quantities.png
[15]: ./media/azure-stack-app-service-deploy-offline/app-service-windows-image-selection.png
[16]: ./media/azure-stack-app-service-deploy-offline/app-service-role-credentials.png
[17]: ./media/azure-stack-app-service-deploy-offline/app-service-azure-stack-deployment-summary.png
[18]: ./media/azure-stack-app-service-deploy-offline/app-service-deployment-progress.png
