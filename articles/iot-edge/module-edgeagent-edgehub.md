---
title: Référence des propriétés désirées de EdgeAgent et EdgeHub - Azure IoT Edge | Microsoft Docs
description: Passez en revue les propriétés spécifiques et les valeurs des jumeaux de module edgeAgent et edgeHub.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 09/21/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: a0834e5886a1a088486109f967baf357e375ad05
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/08/2018
ms.locfileid: "53100257"
---
# <a name="properties-of-the-edge-agent-and-edge-hub-module-twins"></a>Propriétés des jumeaux de module de l’agent Edge et du hub Edge

L’agent Edge et le hub Edge sont deux modules qui constituent le runtime IoT Edge. Pour plus d’informations sur les rôles de chaque module, consultez [Présentation du runtime Azure IoT Edge et de son architecture](iot-edge-runtime.md). 

Cet article fournit les propriétés souhaitées et signalées des jumeaux de module du runtime. Pour plus d’informations sur le déploiement de modules sur des appareils IoT Edge, consultez [Déploiement et supervision](module-deployment-monitoring.md).

## <a name="edgeagent-desired-properties"></a>Propriétés souhaitées pour EdgeAgent

La représentation de module de l’agent Edge est appelée `$edgeAgent` et coordonne les communications entre l’agent Edge exécuté sur un appareil et IoT Hub. Les propriétés souhaitées sont définies lors de l’application d’un manifeste de déploiement sur un appareil spécifique dans le cadre d’un déploiement d’appareil unique ou à grande échelle. 

| Propriété | Description | Obligatoire |
| -------- | ----------- | -------- |
| schemaVersion | Doit être "1.0" | Oui |
| runtime.type | Doit être "docker" | Oui |
| runtime.settings.minDockerVersion | Définie sur la version Docker minimale requise par ce manifeste de déploiement | Oui |
| runtime.settings.loggingOptions | Champ de chaîne JSON contenant les options de journalisation du conteneur d’agent Edge. [Options de journalisation Docker](https://docs.docker.com/engine/admin/logging/overview/) | Non  |
| runtime.settings.registryCredentials<br>.{registryId}.username | Nom d’utilisateur du registre de conteneurs. Pour Azure Container Registry, le nom d’utilisateur est généralement le nom du registre.<br><br> Les informations d’identification du registre sont nécessaires pour toutes les images de modules qui ne sont pas publiques. | Non  |
| runtime.settings.registryCredentials<br>.{registryId}.password | Mot de passe du registre de conteneurs. | Non  |
| runtime.settings.registryCredentials<br>.{registryId}.address | Adresse du registre de conteneurs. Pour Azure Container Registry, l’adresse est généralement *{registryname}.azurecr.io*. | Non  |  
| systemModules.edgeAgent.type | Doit être "docker" | Oui |
| systemModules.edgeAgent.settings.image | URI de l’image de l’agent Edge. Actuellement, l’agent Edge ne peut pas se mettre à jour lui-même. | Oui |
| systemModules.edgeAgent.settings<br>.createOptions | Champ de chaîne JSON contenant les options de création du conteneur d’agent Edge. [Options de création Docker](https://docs.docker.com/engine/api/v1.32/#operation/ContainerCreate) | Non  |
| systemModules.edgeAgent.configuration.id | ID du déploiement ayant déployé ce module. | Cette propriété est définie par IoT Hub quand ce manifeste est appliqué à l’aide d’un déploiement. Ne fait pas partie d’un manifeste de déploiement. |
| systemModules.edgeHub.type | Doit être "docker" | Oui |
| systemModules.edgeHub.type | Doit être "running" | Oui |
| systemModules.edgeHub.restartPolicy | Doit être "always" | Oui |
| systemModules.edgeHub.settings.image | URI de l’image de Edge Hub. | Oui |
| systemModules.edgeHub.settings<br>.createOptions | Champ de chaîne JSON contenant les options de création du conteneur Edge Hub. [Options de création Docker](https://docs.docker.com/engine/api/v1.32/#operation/ContainerCreate) | Non  |
| systemModules.edgeHub.configuration.id | ID du déploiement ayant déployé ce module. | Cette propriété est définie par IoT Hub quand ce manifeste est appliqué à l’aide d’un déploiement. Ne fait pas partie d’un manifeste de déploiement. |
| modules.{moduleId}.version | Chaîne définie par l’utilisateur représentant la version de ce module. | Oui |
| modules.{moduleId}.type | Doit être "docker" | Oui |
| modules.{moduleId}.status | {"running" \| "stopped"} | Oui |
| modules.{moduleId}.restartPolicy | {"never" \| "on-failed" \| "on-unhealthy" \| "always"} | Oui |
| modules.{moduleId}.settings.image | URI de l’image du module. | Oui |
| modules.{moduleId}.settings.createOptions | Champ de chaîne JSON contenant les options de création du conteneur de module. [Options de création Docker](https://docs.docker.com/engine/api/v1.32/#operation/ContainerCreate) | Non  |
| modules.{moduleId}.configuration.id | ID du déploiement ayant déployé ce module. | Cette propriété est définie par IoT Hub quand ce manifeste est appliqué à l’aide d’un déploiement. Ne fait pas partie d’un manifeste de déploiement. |

## <a name="edgeagent-reported-properties"></a>Propriétés signalées pour EdgeAgent

Les propriétés signalées pour l’agent Edge incluent trois informations principales :

1. l’état de l’application de dernières propriétés souhaitées affichées ;
2. l’état des modules en cours d’exécution sur l’appareil, comme signalé par l’agent Edge ; et
3. une copie des propriétés souhaitées en cours d’exécution sur l’appareil.

Cette dernière information est utile au cas où les dernières propriétés souhaitées ne seraient pas appliquées avec succès par le runtime, et que l’appareil exécuterait encore un manifeste de déploiement précédent.

> [!NOTE]
> Les propriétés signalées de l’agent Edge sont utiles car elles peuvent être interrogées avec le [langage de requête IoT Hub](../iot-hub/iot-hub-devguide-query-language.md) afin de connaître l’état de déploiements à grande échelle. Pour plus d’informations sur l’utilisation des propriétés d’agent Edge pour l’état, consultez [Comprendre les déploiements IoT Edge pour les appareils uniques ou à grande échelle](module-deployment-monitoring.md).

Le tableau suivant n’inclut pas les informations copiées à partir des propriétés souhaitées.

| Propriété | Description |
| -------- | ----------- |
| lastDesiredVersion | Cet entier fait référence à la dernière version des propriétés souhaitées traitées par l’agent Edge. |
| lastDesiredStatus.code | Il s’agit du code d’état faisant référence à dernières propriétés souhaitées observées par l’agent Edge. Valeurs autorisées : `200` Réussite, `400` Configuration non valide, `412` Version de schéma non valide, `417` les propriétés souhaitées sont vides, `500` Échec |
| lastDesiredStatus.description | Texte de description de l’état |
| deviceHealth | `healthy` si l’état du runtime de tous les modules est `running` ou `stopped`, sinon, `unhealthy` |
| configurationHealth.{deploymentId}.health | `healthy` si l’état du runtime de tous les modules définis par le déploiement {deploymentId} est `running` ou `stopped`, sinon, `unhealthy` |
| runtime.platform.OS | Signalement du système d’exploitation exécuté sur l’appareil |
| runtime.platform.architecture | Signalement de l’architecture de l’UC sur l’appareil |
| systemModules.edgeAgent.runtimeStatus | État signalé de l’agent Edge : {"running" \| "unhealthy"} |
| systemModules.edgeAgent.statusDescription | Texte de description de l’état signalé de l’agent Edge. |
| systemModules.edgeHub.runtimeStatus | État actuel du hub Edge : { "running" \| "stopped" \| "failed" \| "backoff" \| "unhealthy" } |
| systemModules.edgeHub.statusDescription | Texte de description de l’état actuel de Edge Hub en cas de défaillance. |
| systemModules.edgeHub.exitCode | En cas de sortie, le code de sortie signalé par le conteneur Edge Hub |
| systemModules.edgeHub.startTimeUtc | Heure du dernier démarrage de Edge Hub |
| systemModules.edgeHub.lastExitTimeUtc | Heure de la dernière sortie de Edge Hub |
| systemModules.edgeHub.lastRestartTimeUtc | Heure du dernier redémarrage de Edge Hub |
| systemModules.edgeHub.restartCount | Nombre de redémarrages de ce module dans le cadre de la stratégie de redémarrage. |
| modules.{moduleId}.runtimeStatus | État actuel du module : { "running" \| "stopped" \| "failed" \| "backoff" \| "unhealthy" } |
| modules.{moduleId}.statusDescription | Texte de description de l’état actuel du module en cas de défaillance. |
| modules.{moduleId}.exitCode | En cas de sortie, le code de sortie signalé par le conteneur de module |
| modules.{moduleId}.startTimeUtc | Heure du dernier démarrage du module |
| modules.{moduleId}.lastExitTimeUtc | Heure de la dernière sortie du module |
| modules.{moduleId}.lastRestartTimeUtc | Heure du dernier redémarrage du module |
| modules.{moduleId}.restartCount | Nombre de redémarrages de ce module dans le cadre de la stratégie de redémarrage. |

## <a name="edgehub-desired-properties"></a>Propriétés souhaitées pour EdgeHub

La représentation de module de Edge Hub est appelée `$edgeHub` et coordonne les communications entre Edge Hub exécuté sur un appareil et IoT Hub. Les propriétés souhaitées sont définies lors de l’application d’un manifeste de déploiement sur un appareil spécifique dans le cadre d’un déploiement d’appareil unique ou à grande échelle. 

| Propriété | Description | Requise dans le manifeste de déploiement |
| -------- | ----------- | -------- |
| schemaVersion | Doit être "1.0" | Oui |
| routes.{routeName} | Chaîne représentant un itinéraire Edge Hub. | L’élément `routes` peut être présent mais vide. |
| storeAndForwardConfiguration.timeToLiveSecs | Durée en secondes pendant laquelle le hub Edge conserve les messages en cas de points de terminaison de routage déconnectés, par exemple, déconnectés d’IoT Hub ou d’un module local | Oui |

## <a name="edgehub-reported-properties"></a>Propriétés signalées pour EdgeHub

| Propriété | Description |
| -------- | ----------- |
| lastDesiredVersion | Cet entier fait référence à la dernière version des propriétés souhaitées traitées par le hub Edge. |
| lastDesiredStatus.code | Il s’agit du code d’état faisant référence à dernières propriétés souhaitées observées par Edge Hub. Valeurs autorisées : `200` Réussite, `400` Configuration non valide, `500` Échec |
| lastDesiredStatus.description | Texte de description de l’état |
| clients.{device or moduleId}.status | État de connectivité de cet appareil ou module. Valeurs possibles {"connected" \| "disconnected"}. Seules les identités de module peuvent être à l’état déconnecté. Les appareils en aval se connectant à Edge Hub ne s’affichent que lorsqu’ils sont connectés. |
| clients.{device or moduleId}.lastConnectTime | Dernière connexion de l’appareil ou du module |
| clients.{device or moduleId}.lastDisconnectTime | Dernière déconnexion de l’appareil ou du module |

## <a name="next-steps"></a>Étapes suivantes

Pour savoir comment utiliser ces propriétés pour générer des manifestes de déploiement, consultez [Comprendre comment les modules IoT Edge peuvent être utilisés, configurés et réutilisées](module-composition.md).
