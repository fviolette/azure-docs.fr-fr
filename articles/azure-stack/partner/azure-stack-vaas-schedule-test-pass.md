---
title: Utiliser le portail de Validation en tant que service pour le portail Azure Stack pour planifier votre premier test | Microsoft Docs
description: Utilisez le portail de Validation en tant que service pour le portail Azure Stack pour planifier votre premier test.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: How to
ms.date: 11/26/2018
ms.author: mabrigg
ms.reviewer: johnhas
ms.openlocfilehash: 3021fafe7d74f71450eec9b3720c974b04ada224
ms.sourcegitcommit: c61c98a7a79d7bb9d301c654d0f01ac6f9bb9ce5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/27/2018
ms.locfileid: "52426925"
---
# <a name="schedule-your-first-test"></a>Planifier votre premier test

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Planifiez un test dans le portail Validation en tant que Service (VaaS) pour votre solution Azure Stack. Une solution VaaS est une solution Azure Stack associée à une nomenclature matérielle (BoM) particulière. Vous pouvez planifier un test pour vérifier que votre matériel peut exécuter Azure Stack.

Pour vérifier votre solution, créez le flux de travail pour un test. Un flux de travail VaaS intervient dans le contexte d’une solution VaaS. Il représente un ensemble de suites de tests qui testent les fonctionnalités d’un déploiement Azure Stack sur votre matériel. Ajoutez les paramètres de l’environnement de votre solution et sélectionnez un ou plusieurs tests à exécuter sur votre solution.

Si le flux de travail Passe de test peut être utilisé pour exécuter n’importe quel test fourni par VaaS, y compris des tests des flux de travail de validation, les résultats du flux de travail Passe de test ne sont pas considérés comme *officiels*. Pour plus d’informations sur les workflows de validation officiels, consultez [Flux de travail](azure-stack-vaas-key-concepts.md#workflows).

## <a name="prerequisites"></a>Prérequis

Avant de procéder à ce démarrage rapide, vous devez avoir terminé les éléments suivants :

- [Configurer les ressources de la validation en tant que service](azure-stack-vaas-set-up-resources.md)
- [Déployer l’agent local](azure-stack-vaas-local-agent.md) (Recommandé)
- [Concepts clés à propos de la validation en tant que service](azure-stack-vaas-key-concepts.md) (Recommandé)

## <a name="start-a-workflow"></a>Démarrer un flux de travail

![Se connecter au portail VaaS](media/vaas_portalsignin.png)

Connectez-vous au portail, sélectionnez ou créez une solution, puis sélectionnez la solution.

1. Connectez-vous au [portail VaaS](https://azurestackvalidation.com).
2. Tapez le nom d’une solution existante ou sélectionnez **Nouvelle solution** pour créer une nouvelle solution. Pour obtenir des instructions, consultez [Create a solution in the VaaS portal](azure-stack-vaas-key-concepts.md#create-a-solution-in-the-vaas-portal) (Créer une solution dans le portail VaaS).
3. Sélectionnez **Démarrer** au niveau de la vignette **Passes de Test**.

## <a name="specify-parameters"></a>Spécifier les paramètres

![Texte de remplacement](media/vaas_test_pass_parameters.png)

Définissez le flux de travail pour votre solution. Ce dernier comporte les étapes du processus permettant de tester votre solution.

1. [!INCLUDE [azure-stack-vaas-workflow-step_naming](includes/azure-stack-vaas-workflow-step_naming.md)]
2. [!INCLUDE [azure-stack-vaas-workflow-step_upload-stampinfo](includes/azure-stack-vaas-workflow-step_upload-stampinfo.md)]
3. [!INCLUDE [azure-stack-vaas-workflow-step_test-params](includes/azure-stack-vaas-workflow-step_test-params.md)]
4. [!INCLUDE [azure-stack-vaas-workflow-step_tags](includes/azure-stack-vaas-workflow-step_tags.md)]
5. Sélectionnez **Suivant** pour sélectionner les tests à planifier.

## <a name="select-tests-to-run"></a>Sélectionner les tests à exécuter

Choisissez les tests que vous souhaitez exécuter dans votre flux de travail.

1. Sélectionnez le ou les tests à exécuter dans votre flux de travail.

    Si vous souhaitez remplacer les paramètres communs (autrement dit, les paramètres fournis dans la section précédente) pour tous les tests, sélectionnez ensuite le lien **Modifier** pour spécifier les nouvelles valeurs.

1. [!INCLUDE [azure-stack-vaas-workflow-step_select-agent](includes/azure-stack-vaas-workflow-step_select-agent.md)]
1. Sélectionnez **Suivant** pour réviser le flux de travail.

## <a name="review-and-submit"></a>Passer en revue et soumettre

Passez en revue, créez, puis planifiez vos flux de travail.

1. Vérifiez les informations affichées.

    Les services créent votre flux de travail avec les informations fournies et les tests sélectionnés seront planifiés.

    Si vous voyez une information incorrecte, utilisez les boutons **Précédent** pour accéder à une section antérieure.

1. [!INCLUDE [azure-stack-vaas-workflow-step_submit](includes/azure-stack-vaas-workflow-step_submit.md)]

## <a name="next-steps"></a>Étapes suivantes

- [Surveiller et gérer les tests dans le portail VaaS](azure-stack-vaas-monitor-test.md)
