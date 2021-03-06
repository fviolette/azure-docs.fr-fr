---
title: Intégrer Azure Key Vault à un déploiement de modèle Resource Manager | Microsoft Docs
description: Découvrez comment utiliser Azure Key Vault pour transmettre des valeurs de paramètre sécurisées lors d’un déploiement de modèle Resource Manager.
services: azure-resource-manager
documentationcenter: ''
author: mumian
manager: dougeby
editor: tysonn
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 11/13/2018
ms.topic: tutorial
ms.author: jgao
ms.custom: seodec18
ms.openlocfilehash: 3a84f9ed35bac7f56d4a6aa2af94d1c28e335b74
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/08/2018
ms.locfileid: "53093197"
---
# <a name="tutorial-integrate-azure-key-vault-in-resource-manager-template-deployment"></a>Tutoriel : Intégrer Azure Key Vault à un déploiement de modèle Resource Manager

Découvrez comment récupérer des valeurs secrètes auprès d’Azure Key Vault et comment transmettre ces valeurs sous forme de paramètres lors d’un déploiement Resource Manager. La valeur n’est jamais exposée, car vous référencez uniquement son ID Key Vault. Pour plus d’informations, consultez l’article [Utiliser Azure Key Vault pour transmettre une valeur de paramètre sécurisée pendant le déploiement](./resource-manager-keyvault-parameter.md).

Dans le didacticiel [Définir l’ordre de déploiement des ressources](./resource-manager-tutorial-create-templates-with-dependent-resources.md), vous créez une machine virtuelle, un réseau virtuel et d’autres ressources dépendantes. Dans ce didacticiel, vous personnalisez le modèle pour récupérer le mot de passe administrateur de la machine virtuelle depuis Azure Key Vault.

Ce tutoriel décrit les tâches suivantes :

> [!div class="checklist"]
> * Préparer le coffre Key Vault
> * Ouvrir un modèle de démarrage rapide
> * Modifier le fichier de paramètres
> * Déployer le modèle
> * Valider le déploiement
> * Supprimer des ressources

Si vous ne disposez pas d’abonnement Azure, créez un [compte gratuit](https://azure.microsoft.com/free/) avant de commencer.

## <a name="prerequisites"></a>Prérequis

Pour effectuer ce qui est décrit dans cet article, vous avez besoin des éléments suivants :

* [Visual Studio Code](https://code.visualstudio.com/) avec l’[extension Outils Resource Manager](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#prerequisites).
* Pour une sécurité optimale, utilisez un mot de passe généré pour le compte administrateur de la machine virtuelle. Voici un exemple pour générer un mot de passe :

    ```azurecli-interactive
    openssl rand -base64 32
    ```
    Azure Key Vault a été conçu pour protéger les clés et autres secrets de chiffrement. Pour plus d’informations, consultez [Tutoriel : Intégrer Azure Key Vault à un déploiement de modèle Resource Manager](./resource-manager-tutorial-use-key-vault.md). Nous vous recommandons également de mettre à jour votre mot de passe tous les trois mois.

## <a name="prepare-the-key-vault"></a>Préparer le coffre Key Vault

Dans cette section, vous allez utiliser un modèle Resource Manager pour créer un coffre Key Vault et un secret. Ce modèle effectue les opérations suivantes :

* Créez un coffre de clés avec la propriété `enabledForTemplateDeployment` activée. Cette propriété doit être définie sur la valeur True pour que le processus de déploiement de modèle puisse accéder aux secrets définis dans ce coffre Key Vault.
* Il ajoute un secret au coffre Key Vault.  Ce secret stocke le mot de passe d’administrateur de la machine virtuelle.

Si, en votre qualité d’utilisateur déployant le modèle de machine virtuelle, vous n’êtes pas le propriétaire ou le contributeur du coffre Key Vault, le propriétaire ou un contributeur du Key Vault doit vous accorder l’accès à l’autorisation Microsoft.KeyVault/vaults/deploy/action pour ce Key Vault. Pour plus d’informations, consultez l’article [Utiliser Azure Key Vault pour transmettre une valeur de paramètre sécurisée pendant le déploiement](./resource-manager-keyvault-parameter.md).

Le modèle requiert votre ID objet utilisateur Azure AD pour la configuration des autorisations. La procédure ci-après obtient l’ID objet (GUID) et génère également le mot de passe d’administrateur. Pour éviter tout risque d’attaque par pulvérisation de mots de passe, il est recommandé d’utiliser des mots de passe générés.

1. Exécutez la commande Azure PowerShell ou Azure CLI suivante.  

    ```azurecli-interactive
    echo "Enter your email address that is associated with your Azure subscription):" &&
    read upn &&
    az ad user show --upn-or-object-id $upn --query "objectId" &&
    openssl rand -base64 32
    ```
    ```azurepowershell-interactive
    $upn = Read-Host -Prompt "Input your user principal name (email address) used to sign in to Azure"
    (Get-AzureADUser -ObjectId $upn).ObjectId
    openssl rand -base64 32
    ```
2. Notez par écrit l’ID objet et le mot de passe généré. Vous en aurez besoin ultérieurement.
3. Assurez-vous que le mot de passe généré répond aux exigences en matière de mot de passe de machine virtuelle. Chaque service Azure présente des exigences de mot de passe spécifiques. Pour connaître les exigences relatives aux mots de passe de machine virtuelle, consultez la section [Quelles sont les exigences en matière de mot de passe lors de la création d’une machine virtuelle ?](../virtual-machines/windows/faq.md#what-are-the-password-requirements-when-creating-a-vm).

Pour créer un coffre Key Vault :

1. Cliquez sur l’image ci-après pour vous connecter à Azure et ouvrir un modèle. Le modèle crée un coffre Key Vault et un secret Key Vault.

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Farmtutorials.blob.core.windows.net%2Fcreatekeyvault%2FCreateKeyVault.json"><img src="./media/resource-manager-tutorial-use-key-vault/deploy-to-azure.png" alt="deploy to azure"/></a>

2. Sélectionnez ou entrez les valeurs suivantes.  Ne sélectionnez pas **Acheter** après avoir entré ces valeurs.

    ![Portail de déploiement d’intégration de coffre Key Vault à un modèle Resource Manager](./media/resource-manager-tutorial-use-key-vault/resource-manager-tutorial-create-key-vault-portal.png)

    * **Abonnement** : sélectionnez un abonnement Azure.
    * **Groupe de ressources**: attribuez un nom unique. Notez ce nom par écrit, car vous utiliserez le même groupe de ressources pour déployer la machine virtuelle dans la session suivante. Le fait de placer le coffre Key Vault et la machine virtuelle dans le même groupe de ressources facilite le nettoyage de la ressource à la fin du didacticiel.
    * **Emplacement** : sélectionnez un emplacement.  L’emplacement par défaut est **USA Centre**.
    * **Nom du coffre de clés**: attribuez un nom unique. 
    * **Id de locataire**: la fonction de modèle récupère automatiquement votre id de locataire.  Ne modifiez pas la valeur par défaut.
    * **ID d’utilisateur Azure Active Directory** : entrez votre ID objet utilisateur Azure AD que vous avez récupéré à partir de la dernière procédure.
    * **Nom du secret** : le nom par défaut est **vmAdminPassword**. Si vous modifiez le nom du secret à cet emplacement, vous devez mettre à jour ce nom lorsque vous déployez la machine virtuelle.
    * **Valeur du secret** : entrez votre secret.  Le secret correspond au mot de passe utilisé pour la connexion à la machine virtuelle. Il est recommandé d’utiliser le mot de passe généré que vous avez créé au cours de la dernière procédure.
    * **J’accepte les termes et conditions mentionnés ci-dessus** : cochez la case.
3. Sélectionnez **Modifier les paramètres** en haut de la page pour examiner le modèle.
4. Accédez à la ligne 28 du fichier JSON du modèle. Il s’agit de la définition de la ressource Key Vault.
5. Accédez à la ligne 35 :

    ```json
    "enabledForTemplateDeployment": true,
    ```
    `enabledForTemplateDeployment` est une propriété Key Vault. Cette propriété doit présenter la valeur True pour que vous puissiez récupérer les secrets de ce coffre Key Vault pendant le déploiement.
6. Accédez à la ligne 89. Il s’agit de la définition du secret Key Vault.
7. Sélectionnez **Abandonner** au bas de la page, car vous n’avez apporté aucune modification.
8. Vérifiez que vous avez fourni toutes les valeurs indiquées dans la capture d’écran précédente, puis cliquez sur **Acheter** au bas de la page.
9. Sélectionnez l’icône représentant une cloche (notification) en haut de la page pour ouvrir le volet **Notifications**. Attendez que la ressource ait été correctement déployée.
10. Dans le volet **Notifications**, sélectionnez **Accéder au groupe de ressources**. 
11. Sélectionnez le nom du coffre Key Vault pour l’ouvrir.
12. Dans le volet gauche, sélectionnez **Stratégies d’accès**. Votre nom (Active Directory) doit être répertorié ; dans le cas contraire, vous n’êtes pas autorisé à accéder au coffre de clés.
13. Sélectionnez **Cliquez ici pour afficher les stratégies d'accès avancé**. Notez que l’option **Activer l’accès à Azure Resource Manager pour le déploiement de modèles** est sélectionnée. Il s’agit d’une autre condition requise pour le bon fonctionnement de l’intégration de Key Vault.

    ![Stratégies d’accès d’intégration de coffre Key Vault à un modèle Resource Manager](./media/resource-manager-tutorial-use-key-vault/resource-manager-tutorial-key-vault-access-policies.png)
14. Dans le volet gauche, sélectionnez **Propriétés**.
15. Effectuez une copie de la valeur **ID de ressource**. Vous devez disposer de cet ID lorsque vous déployez la machine virtuelle.  Le format d’ID de ressource est le suivant :

    ```json
    /subscriptions/<SubscriptionID>/resourceGroups/mykeyvaultdeploymentrg/providers/Microsoft.KeyVault/vaults/<KeyVaultName>
    ```

## <a name="open-a-quickstart-template"></a>Ouvrir un modèle de démarrage rapide

Modèles de démarrage rapide Azure est un référentiel pour les modèles Resource Manager. Au lieu de créer un modèle à partir de zéro, vous pouvez chercher un exemple de modèle et le personnaliser. Le modèle utilisé dans ce didacticiel se nomme [Déployer une machine virtuelle Windows simple](https://azure.microsoft.com/resources/templates/101-vm-simple-windows/).

1. À partir de Visual Studio Code, sélectionnez **Fichier**>**Ouvrir un fichier**.
2. Collez l’URL suivante dans **Nom de fichier** :

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.json
    ```
3. Sélectionnez **Ouvrir** pour ouvrir le fichier. Il s’agit du même scénario que celui utilisé dans l’article [Didacticiel : créer des modèles Azure Resource Manager avec des ressources dépendantes](./resource-manager-tutorial-create-templates-with-dependent-resources.md).
4. Il existe cinq ressources définies par le modèle :

    * `Microsoft.Storage/storageAccounts`. Consultez la [référence de modèle](https://docs.microsoft.com/azure/templates/Microsoft.Storage/storageAccounts).
    * `Microsoft.Network/publicIPAddresses`. Consultez la [référence de modèle](https://docs.microsoft.com/azure/templates/microsoft.network/publicipaddresses).
    * `Microsoft.Network/virtualNetworks`. Consultez la [référence de modèle](https://docs.microsoft.com/azure/templates/microsoft.network/virtualnetworks).
    * `Microsoft.Network/networkInterfaces`. Consultez la [référence de modèle](https://docs.microsoft.com/azure/templates/microsoft.network/networkinterfaces).
    * `Microsoft.Compute/virtualMachines`. Consultez la [référence de modèle](https://docs.microsoft.com/azure/templates/microsoft.compute/virtualmachines).

    Il est préférable de comprendre quelques notions basiques du modèle avant de le personnaliser.
5. Sélectionnez **Fichier**>**Enregistrer sous** pour enregistrer une copie du fichier sur votre ordinateur local avec le nom **azuredeploy.json**.
6. Répétez les étapes 1 à 4 pour accéder à l’URL ci-après, puis enregistrez le fichier sous le nom **azuredeploy.parameters.json**.

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.parameters.json
    ```

## <a name="edit-the-parameters-file"></a>Modifier le fichier de paramètres

Aucune modification de ce fichier de modèle n’est nécessaire.

1. Ouvrez le fichier **azuredeploy.parameters.json** dans Visual Studio Code s’il n’est pas déjà ouvert.
2. Mettez à jour le paramètre **adminPassword** comme suit :

    ```json
    "adminPassword": {
        "reference": {
            "keyVault": {
            "id": "/subscriptions/<SubscriptionID>/resourceGroups/mykeyvaultdeploymentrg/providers/Microsoft.KeyVault/vaults/<KeyVaultName>"
            },
            "secretName": "vmAdminPassword"
        }
    },
    ```
    Remplacez la valeur de l’élément **id** par l’ID de ressource du coffre Key Vault que vous avez créé au cours de la dernière procédure.  

    ![Fichier de paramètres de déploiement de machine virtuelle pour l’intégration d’un coffre Key Vault à un modèle Resource Manager](./media/resource-manager-tutorial-use-key-vault/resource-manager-tutorial-create-vm-parameters-file.png)
3. Fournissez les valeurs suivantes :

    * **adminUsername** : nommez le compte administrateur de la machine virtuelle.
    * **dnsLabelPrefix** : nommez l’élément dnsLabelPrefix.
4. Enregistrez les modifications.

## <a name="deploy-the-template"></a>Déployer le modèle

Suivez les instructions de la section [Déployer le modèle](./resource-manager-tutorial-create-templates-with-dependent-resources.md#deploy-the-template) pour déployer le modèle. Vous devez charger les fichiers **azuredeploy.json** et **azuredeploy.parameters.json** dans le service Cloud Shell, puis utiliser le script PowerShell ci-après pour déployer le modèle :

```azurepowershell
$resourceGroupName = Read-Host -Prompt "Enter the resource group name of the Key Vault"
$deploymentName = Read-Host -Prompt "Enter the name for this deployment"
New-AzureRmResourceGroupDeployment -Name $deploymentName -ResourceGroupName $resourceGroupName `
    -TemplateFile azuredeploy.json -TemplateParameterFile azuredeploy.parameters.json
```

Lorsque vous déployez le modèle, utilisez le même groupe de ressources que celui du coffre Key Vault. Cette approche simplifie le nettoyage des ressources. En effet, vous ne devez supprimer qu’un groupe de ressources au lieu de deux.

## <a name="valid-the-deployment"></a>Valider le déploiement

Une fois que vous avez correctement déployé la machine virtuelle, testez la connexion à l’aide du mot de passe stocké dans le coffre Key Vault.

1. Ouvrez le [portail Azure](https://portal.azure.com).
2. Sélectionnez **Groupes de ressources**/**NomDeVotreGroupeDeRessources>**/**simpleWinVM**.
3. Sélectionnez **Connexion** dans la partie supérieure.
4. Sélectionnez **Télécharger le fichier RDP**, puis suivez les instructions pour vous connecter à la machine virtuelle à l’aide du mot de passe stocké dans le coffre Key Vault.

## <a name="clean-up-resources"></a>Supprimer des ressources

Lorsque vous n’en avez plus besoin, nettoyez les ressources Azure que vous avez déployées en supprimant le groupe de ressources.

1. Dans le portail Azure, sélectionnez **Groupe de ressources** dans le menu de gauche.
2. Entrez le nom du groupe de ressources dans le champ **Filtrer par nom**.
3. Sélectionnez le nom du groupe de ressources.  Vous devriez voir six ressources au total dans le groupe de ressources.
4. Sélectionnez **Supprimer le groupe de ressources** dans le menu supérieur.

## <a name="next-steps"></a>Étapes suivantes

Dans le cadre de ce didacticiel, vous avez récupéré un secret auprès d’Azure Key Vault, puis vous avez utilisé ce secret dans votre déploiement de modèle.  Pour savoir comment créer des modèles liés, consultez l’article suivant :

> [!div class="nextstepaction"]
> [Créer des modèles liés](./resource-manager-tutorial-create-linked-templates.md)
