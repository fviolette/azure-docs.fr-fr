---
title: Exécuter Ansible avec Bash dans Azure Cloud Shell
description: Découvrir comment effectuer diverses tâches Ansible avec Bash dans Azure Cloud Shell
ms.service: ansible
keywords: ansible, azure, devops, bash, cloudshell, playbook, bash
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.date: 08/07/2018
ms.topic: article
ms.openlocfilehash: 6bfac47e4afa41b4c75a8d33b4eea1ff5103296d
ms.sourcegitcommit: d61faf71620a6a55dda014a665155f2a5dcd3fa2
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/04/2019
ms.locfileid: "54050893"
---
# <a name="run-ansible-with-bash-in-azure-cloud-shell"></a>Exécuter Ansible avec Bash dans Azure Cloud Shell

Dans ce didacticiel, vous allez apprendre à utiliser Bash dans Cloud Shell pour configurer un abonnement Azure en tant qu’espace de travail Ansible. 

## <a name="prerequisites"></a>Prérequis

- **Abonnement Azure** : si vous n’avez pas d’abonnement Azure, créez un [compte gratuit](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).

- **Configurez Azure Cloud Shell** : si vous ne connaissez pas encore Azure Cloud Shell, l’article [Démarrage rapide de Bash dans Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/quickstart) vous indique comment démarrer et configurer Cloud Shell. 

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="automatic-credential-configuration"></a>Configuration automatique des informations d’identification

Lorsque vous vous connectez à Cloud Shell, Ansible s’authentifie avec Azure pour gérer l’infrastructure sans aucune configuration supplémentaire. Si vous avez plusieurs abonnements, vous pouvez choisir l’abonnement à utiliser par Ansible en exportant la variable d’environnement `AZURE_SUBSCRIPTION_ID`. Pour répertorier tous vos abonnements Azure, exécutez la commande suivante :

```azurecli-interactive
az account list
```

À l’aide de l’**id** de l’abonnement avec lequel vous souhaitez travailler, définissez l’**AZURE_SUBSCRIPTION_ID** comme suit :

```azurecli-interactive
export AZURE_SUBSCRIPTION_ID=<your-subscription-id>
```

## <a name="verify-the-configuration"></a>Vérifier la configuration
Pour vous assurer que la configuration a réussi, utilisez Ansible pour créer un groupe de ressources.

[!INCLUDE [create-resource-group-with-ansible.md](../../includes/ansible-create-resource-group.md)]

## <a name="next-steps"></a>Étapes suivantes

> [!div class="nextstepaction"] 
> [Créer une machine virtuelle Linux de base dans Azure avec Ansible](/azure/virtual-machines/linux/ansible-create-vm)