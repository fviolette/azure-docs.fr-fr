---
title: Meilleures pratiques de Validation Azure Stack | Microsoft Docs
description: Cet article présente les meilleures pratiques concernant la validation en tant que service.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/26/2018
ms.author: mabrigg
ms.reviewer: John.Haskin
ms.openlocfilehash: d83eb501bbe685890decb3acbb8116164f4199db
ms.sourcegitcommit: c61c98a7a79d7bb9d301c654d0f01ac6f9bb9ce5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/27/2018
ms.locfileid: "52422658"
---
# <a name="best-practices-for-validation-as-a-service"></a>Meilleures pratiques concernant la validation en tant que service

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Cet article traite des meilleures pratiques concernant la gestion des ressources dans Validation en tant que Service (VaaS). Pour une vue d’ensemble des ressources VaaS, consultez [Concepts clés de Validation en tant que service](azure-stack-vaas-key-concepts.md).

## <a name="solution-management"></a>Gestion de solution

### <a name="naming-convention-for-vaas-solutions"></a>Convention d’affectation de noms pour les solutions VaaS

Utilisez une convention d’affectation de noms cohérente pour toutes les solutions inscrites dans VaaS. Par exemple, le nom de la solution peut être construit à partir des propriétés de matériel ci-dessous :

|Nom du produit | Élément matériel unique 1 | Élément matériel unique 2 | Nom de la solution
|---|---|---|---|
Ma solution XYZ |  Flash | My Switch X01 | MySolutionXYZ_AllFlash_MySwitchX01

### <a name="when-to-create-a-new-vaas-solution"></a>Quand créer une nouvelle solution VaaS

Utilisez une même solution VaaS lors de l’exécution de flux de travail sur une même référence (SKU) matériel. Une nouvelle solution VaaS doit être créée uniquement en cas de modification de la référence (SKU) matériel.

## <a name="workflow-management"></a>Gestion de flux de travail

### <a name="naming-convention-for-vaas-workflows"></a>Convention d’affectation de noms pour les flux de travail VaaS

Utilisez une convention d’affectation de noms cohérente pour toutes les exécutions de flux de travail VaaS. Par exemple, la construction présente un nom de flux de travail déduit des propriétés de build ci-dessous, comme suit :

|Numéro de build (majeur) | Date | Taille de la solution | Nom du flux de travail
|---|---|---| ---|
1808 | 081518 | 4NODE | 1808_081518_4NODE

## <a name="next-steps"></a>Étapes suivantes

- Découvrez les [concepts clés concernant la validation en tant que service](azure-stack-vaas-key-concepts.md)
