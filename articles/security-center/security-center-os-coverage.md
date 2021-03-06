---
title: Fonctionnalités et plateformes prises en charge par Azure Security Center | Microsoft Docs
description: Ce document fournit une liste des fonctionnalités et des plateformes prises en charge par Azure Security Center.
services: security-center
documentationcenter: na
author: rkarlin
manager: MBaldwin
editor: ''
ms.assetid: 70c076ef-3ad4-4000-a0c1-0ac0c9796ff1
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/18/2018
ms.author: rkarlin
ms.openlocfilehash: 2dcc72e0e3b9caef9ab01d9f754671cb0365a358
ms.sourcegitcommit: 4eeeb520acf8b2419bcc73d8fcc81a075b81663a
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/19/2018
ms.locfileid: "53608832"
---
# <a name="platforms-and-features-supported-by-azure-security-center"></a>Plateformes et fonctionnalités prises en charge par Azure Security Center

La supervision de l’état de la sécurité et des recommandations sont disponibles pour les machines virtuelles créées à l’aide des modèles de déploiement classique et Resource Manager, ainsi que des ordinateurs.

> [!NOTE]
> En savoir plus sur les [modèles de déploiement de type Classique et Resource Manager](../azure-classic-rm.md) pour les ressources Azure.
>
>

## <a name="platforms-that-support-the-data-collection-agent"></a>Plateformes prenant en charge l’agent de collecte de données 

Cette section liste les plateformes sur lesquelles l’agent Azure Security Center peut s’exécuter et à partir desquelles il peut collecter des données.

### <a name="supported-platforms-for-windows-computers-and-vms"></a>Plateformes prises en charge pour les ordinateurs et machines virtuelles Windows
Les systèmes d’exploitation Windows suivants sont pris en charge :

* Windows Server 2008
* Windows Server 2008 R2
* Windows Server 2012
* Windows Server 2012 R2
* Windows Server 2016

> [!NOTE]
> L’intégration à Windows Defender ATP prend en charge seulement Windows Server 2012 R2 et Windows Server 2016.
>
>

### <a name="supported-platforms-for-linux-computers-and-vms"></a>Plateformes prises en charge pour les ordinateurs et machines virtuelles Linux
Les systèmes d’exploitation Linux suivants sont pris en charge :

* Ubuntu versions 12.04 LTS, 14.04 LTS et 16.04 LTS.
* Debian versions 6, 7, 8 et 9.
* CentOS versions 5, 6 et 7.
* Red Hat Enterprise Linux (RHEL) versions 5, 6 et 7.
* SUSE Linux Enterprise Server (SLES) versions 11 et 12.
* Oracle Linux versions 5, 6 et 7.
* Amazon Linux, des versions 2012.09 à 2017.
* OpenSSL 1.1.0 n’est pris en charge que sur les plateformes x86_64 (64 bits).

## <a name="vms-and-cloud-services"></a>Machines virtuelles et services cloud
Les machines virtuelles en cours d’exécution dans un service cloud sont également prises en charge. Seuls les rôles de travail et web des services cloud en cours d’exécution dans des emplacements de production sont surveillés. Pour en savoir plus sur les services cloud, consultez [Vue d’ensemble d’Azure Cloud Services](../cloud-services/cloud-services-choose-me.md).


## <a name="supported-iaas-features"></a>Fonctionnalités IaaS prises en charge

> [!div class="mx-tableFixed"]
> 

|Serveur| Windows||Linux||
|----|----|----|----|----|
|Environnement|Azure|Non-Azure|Azure|Non-Azure|
|Alertes de détection des menaces VMBA|✔|✔|✔ (sur les versions prises en charge)|✔|
|Alertes de détection des menaces réseau|✔|X|✔|X|
|Intégration de Windows Defender ATP*|✔ (sur les versions prises en charge)|✔|X|X|
|Correctifs manquants|✔|✔|✔|✔|
|Configurations de sécurité|✔|✔|✔|✔|
|Logiciels anti-programme malveillant|✔|✔|X|X|
|Accès JIT à la machine virtuelle|✔|X|✔|X|
|Contrôles d’application adaptative|✔|X|X|X|
|FIM|✔|✔|✔|✔|
|Chiffrement de disque|✔|X|✔|X|
|Déploiement tiers|✔|X|✔|X|
|Groupes de sécurité réseau|✔|X|✔|X|
|Détection des menaces sans fichier|✔|✔|X|X|
|Mappage réseau|✔|X|✔|X|
|Contrôles réseau adaptatifs|✔|X|✔|X|

\* Ces fonctionnalités sont actuellement prises en charge en préversion publique.


## <a name="supported-paas-features"></a>Fonctionnalités PaaS prises en charge 


|de diffusion en continu|Recommandations|Détection de menaces|
|----|----|----|
|SQL|✔| ✔|
|PostGreSQL*|✔| ✔|
|MySQL*|✔| ✔|
|Comptes de stockage Azure blob*|✔| ✔|
|App Services|✔| ✔|
|Cloud Services|✔| X|
|Réseaux virtuels|✔| N/D|
|Sous-réseaux|✔| N/D|
|Cartes réseau|✔| ✔|
|Groupes de sécurité réseau|✔| N/D|
|Abonnement|✔| ✔|

\* Ces fonctionnalités sont actuellement prises en charge en préversion publique. 

## <a name="next-steps"></a>Étapes suivantes

- [Découvrez comment planifier l’adoption d’Azure Security Center et prenez connaissance des considérations relatives à la conception](security-center-planning-and-operations-guide.md).
- En savoir plus sur [l’analyse comportementale de machine virtuelle et l’analyse de mémoire de vidage sur incident dans Security Center](security-center-alerts-type.md#virtual-machine-behavioral-analysis).
- Découvrez [les réponses aux questions le plus souvent posées sur l’utilisation d’Azure Security Center](security-center-faq.md).
- Accédez à des [billets de blog sur la sécurité et la conformité Azure](https://blogs.msdn.com/b/azuresecurity/).
