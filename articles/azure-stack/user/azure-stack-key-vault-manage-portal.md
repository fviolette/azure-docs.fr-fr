---
title: Gérer Key Vault dans Azure Stack en utilisant le portail | Microsoft Docs
description: Découvrez comment gérer Key Vault dans Azure Stack en utilisant le portail
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: D4300668-461F-45F6-BF3B-33B502C39D17
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/04/201
ms.author: sethm
ms.openlocfilehash: 5e0462c533be11241908cafc593c89588f61f349
ms.sourcegitcommit: 63b996e9dc7cade181e83e13046a5006b275638d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/10/2019
ms.locfileid: "54192628"
---
# <a name="manage-key-vault-in-azure-stack-by-using-the-portal"></a>Gérer Key Vault dans Azure Stack en utilisant le portail

Vous pouvez gérer Key Vault dans Azure Stack à l’aide du portail Azure Stack. Cet article explique comment créer et gérer un coffre de clés dans Azure Stack.

## <a name="prerequisites"></a>Prérequis

Vous devez vous abonner à une offre qui inclut le service Azure Key Vault.

## <a name="create-a-key-vault"></a>Création d’un coffre de clés

1. Connectez-vous au [portail utilisateur](https://portal.local.azurestack.external).

2. Sur le tableau de bord, sélectionnez **+ Créer une ressource**, **Sécurité + Identité** et **Coffre de clés**.

    ![Écran Coffre de clés](media/azure-stack-key-vault-manage-portal/image1.png)

3. Dans le volet **Créer un coffre de clés**, affectez un **Nom** à votre coffre. Les noms de coffre ne peuvent contenir que des caractères alphanumériques et des traits d'union (-). Ils ne peuvent pas commencer par un chiffre.

4. Choisissez un **Abonnement** dans la liste des abonnements disponibles. Tous les abonnements qui offrent le service Key Vault sont affichés dans la liste déroulante.

5. Sélectionnez un **Groupe de ressources** existant ou créez-en un.

6. Sélectionnez le **Niveau tarifaire**. Dans le Kit de développement Azure Stack, les coffres de clés ne prennent en charge que les références SKU **Standard**.

7. Choisissez une des **Stratégies d’accès** existantes ou créez-en une. Une stratégie d’accès vous permet d’accorder des autorisations à un utilisateur, à une application ou à un groupe de sécurité pour effectuer des opérations avec ce coffre.

8. Choisissez éventuellement une **Stratégie d’accès avancé** pour activer l’accès aux fonctionnalités. Par exemple : machines virtuelles (VM) pour le déploiement, Gestionnaire de ressources pour le déploiement d’un modèle et accès à Azure Disk Encryption pour le chiffrement de volumes.

9. Après avoir configuré les paramètres, sélectionnez **OK**, puis sélectionnez **Créer**. Le déploiement de Key Vault débute.

## <a name="manage-keys-and-secrets"></a>Gérer les clés et les secrets

Après avoir créé un coffre, procédez comme suit pour créer et gérer des clés et des secrets au sein de celui-ci.

### <a name="create-a-key"></a>Créer une clé

1. Connectez-vous au [portail utilisateur](https://portal.local.azurestack.external).

2. Dans le tableau de bord, sélectionnez **Toutes les ressources**, sélectionnez le coffre de clés que vous avez créé, puis sélectionnez la vignette **Clés**.

3. Dans le volet **Clés**, sélectionnez **Ajouter**.

4. Dans le volet **Créer une clé**, dans la liste **Options**, choisissez la méthode que vous voulez utiliser pour créer une clé. Vous pouvez **Générer** une nouvelle clé, **Charger** une clé existante ou utiliser **Restaurer la sauvegarde** pour sélectionner une sauvegarde d’une clé.

5. Entrez un **Nom** pour votre clé. Le nom de la clé ne peut contenir que des caractères alphanumériques et des traits d'union (-).

6. Si vous le souhaitez, configurez les valeurs **Définir la date d’activation** et **Définir la date d’expiration** pour votre clé.

7. Sélectionnez **Créer** pour démarrer le déploiement.

Une fois la clé créée, vous pouvez la sélectionner sous **Clés** et afficher ou modifier ses propriétés. La section Propriétés contient l'**identificateur de clé**, qui est un URI (Uniform Resource Identifier) par lequel les applications externes peuvent accéder à cette clé. Pour limiter les opérations sur cette clé, configurez les paramètres sous **Opérations autorisées**.

![URI de clé](media/azure-stack-key-vault-manage-portal/image4.png)

### <a name="create-a-secret"></a>Créer un secret

1. Connectez-vous au [portail utilisateur](https://portal.local.azurestack.external).

2. Dans le tableau de bord, sélectionnez **Toutes les ressources**, sélectionnez le coffre de clés que vous avez créé, puis sélectionnez la vignette **Secrets**.

3. Sous **Secrets**, sélectionnez **Ajouter**.

4. Sous **Créer un secret**, accédez à la liste **Options de chargement** et choisissez comment créer un secret. Vous pouvez créer un secret **Manuellement** en entrant une valeur pour le secret, ou en chargeant un **Certificat** à partir de votre ordinateur local.

5. Entrez un **Nom** pour le secret. Le nom du secret ne peut contenir que des caractères alphanumériques et des traits d'union (-).

6. Si vous le souhaitez, spécifiez le **Type de contenu** et configurez des valeurs pour **Définir la date d’activation** et **Définir la date d’expiration** pour le secret.

7. Sélectionnez **Créer** pour démarrer le déploiement.

Une fois le secret créé, vous pouvez le sélectionner sous **Secrets**, et afficher ou modifier ses propriétés. **L’identificateur de clé secrète** est une URI que les applications externes peuvent utiliser pour accéder à cette clé secrète.

![URI de secret](media/azure-stack-key-vault-manage-portal/image5.png)

## <a name="next-steps"></a>Étapes suivantes

* [Déployer une machine virtuelle en récupérant le mot de passe stocké dans Key Vault](azure-stack-key-vault-deploy-vm-with-secret.md)
* [Déployer une machine virtuelle avec un certificat stocké dans un coffre de clés](azure-stack-key-vault-push-secret-into-vm.md)
