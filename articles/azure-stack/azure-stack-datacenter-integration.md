---
title: Considérations générales relatives à l’intégration au centre de données pour les systèmes intégrés Azure Stack | Microsoft Docs
description: Découvrez ce que vous pouvez faire pour planifier et préparer l’intégration d’un système Azure Stack à plusieurs nœuds au centre de données.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/12/2018
ms.author: jeffgilb
ms.reviewer: wfayed
ms.openlocfilehash: 3cbe7c1d458e95aa27c51f0d1c9f61326888a5ab
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/06/2018
ms.locfileid: "52962679"
---
# <a name="datacenter-integration-considerations-for-azure-stack-integrated-systems"></a>Considérations relatives à l’intégration au centre de données pour les systèmes intégrés Azure Stack
Si vous êtes intéressé par un système intégré Azure Stack, vous devez comprendre certaines considérations principales sur la planification traitant du déploiement et la façon dont le système s’adapte à votre centre de données. Cet article fournit une vue d’ensemble de ces considérations pour vous aider à prendre des décisions d’infrastructure importantes pour votre système Azure Stack à plusieurs nœuds. Comprendre ces considérations est utile pour collaborer avec votre fournisseur de matériel OEM lorsqu’il déploie Azure Stack vers votre centre de données.  

> [!NOTE]
> Les systèmes Azure Stack à plusieurs nœuds ne peuvent être achetés qu’auprès de fournisseurs de matériel autorisés. 

Pour déployer Azure Stack, vous devez remettre ces informations de planification à votre fournisseur de solutions avant le déploiement pour que le processus soit rapide et simple. Les informations nécessaires incluent la mise en réseau, la sécurité et les informations d’identité avec de nombreuses décisions importantes qui peuvent nécessiter des connaissances dans différents domaines et de différents décideurs. Par conséquent, vous devrez peut-être faire appel à des personnes de différentes équipes de votre organisation pour vous assurer que toutes les informations requises sont prêtes avant le début du déploiement. Il peut être utile de communiquer avec votre fournisseur de matériel lors de la collecte de ces informations, car il peut vous fournir des conseils utiles à la prise de décisions.

Lors de la recherche et de la collecte des informations nécessaires, vous devrez peut-être apporter des modifications de configuration avant le déploiement à votre environnement réseau. Ceci peut inclure la réservation d’espaces d’adresse IP pour la solution Azure Stack, la configuration de vos routeurs, commutateurs et pare-feu afin de préparer la connectivité aux nouveaux commutateurs de la solution Azure Stack. Assurez-vous que le spécialiste de la zone de l’objet peut vous aider dans votre planification.

## <a name="capacity-planning-considerations"></a>Considérations en matière de planification de capacité
Lorsque vous évaluez une solution Azure Stack pour l’acquisition, les options de configuration matérielle choisies ont un impact direct sur la capacité globale de la solution Azure Stack. Parmi ces choix se trouvent celui de l’UC, de la densité de mémoire, de la configuration du stockage et de la mise à l’échelle globale de la solution (p. ex. nombre de serveurs). Contrairement à une solution de virtualisation traditionnelle, l’arithmétique simple de ces composants pour déterminer la capacité utilisable ne s’applique pas. La première raison est que l’architecture d’Azure Stack permet d’héberger les composants de gestion ou d’infrastructure au sein-même de la solution. La deuxième raison est qu’une partie de la capacité de la solution est réservée pour prendre en charge la résilience ; la mise à jour des logiciels de la solution d’une façon permettant de minimiser l’interruption de charges de travail des locataires. 

La [feuille de calcul de planification de capacité Azure Stack](https://aka.ms/azstackcapacityplanner) vous permet de prendre des décisions éclairées par rapport à la capacité de planification de deux manières : soit en sélectionnant une offre matérielle et en essayant d’ajuster une combinaison de ressources, soit en définissant la charge de travail qu’Azure Stack doit exécuter pour afficher les références SKU matérielles disponibles qui peuvent assurer la prise en charge. Enfin, la feuille de calcul est conçue comme un guide pour faciliter la prise de décisions relatives à la planification et à la configuration d’Azure Stack. 

La feuille de calcul n’est pas destinée à remplacer vos propres recherches et analyses.  Microsoft ne fait aucune déclaration ou n’offre aucune garantie, expresse ou implicite, concernant les informations contenues dans la feuille de caclul.



## <a name="management-considerations"></a>Considérations relatives à la gestion
Azure Stack est un système fermé, dont l’infrastructure est verrouillée à la fois du point de vue des autorisations et du réseau. Les listes de contrôle d’accès réseau (ACL) sont utilisées pour bloquer tout trafic entrant non autorisé et toute communication inutile entre les composants de l’infrastructure. Cela rend l’accès au système difficile pour les utilisateurs non autorisés.

Pour les opérations et la gestion quotidiennes, il n’existe aucun accès administrateur non restreint à l’infrastructure. Les opérateurs Azure Stack doivent gérer le système via le portail administrateur ou via Azure Resource Manager (via PowerShell ou l’API REST). Il n’existe aucun accès au système par d’autres outils de gestion tels que le gestionnaire Hyper-V ou le gestionnaire du cluster de basculement. Pour protéger le système, les logiciels tiers (par exemple, les agents) ne peuvent pas être installés dans les composants de l’infrastructure Azure Stack. L’interopérabilité avec les logiciels de gestion et de sécurité externes est effectuée via PowerShell ou l’API REST.

Quand un niveau plus élevé d’accès est nécessaire pour résoudre des problèmes n’ayant pas été résolus à l’aide des étapes de médiation d’alerte, vous devez travailler avec le Support Microsoft. Les options de support proposent une méthode fournissant un accès administrateur complet temporaire au système pour effectuer des opérations plus avancées. 

## <a name="identity-considerations"></a>Identité - Éléments à prendre en compte

### <a name="choose-identity-provider"></a>Choisir un fournisseur d’identité
Vous devez prendre en compte le fournisseur d’identité que vous souhaitez utiliser pour le déploiement de Azure Stack, que ce soit Azure AD ou AD FS. Vous ne pouvez pas changer les fournisseurs d’identité après le déploiement sans effectuer un redéploiement complet du système. Si vous ne possédez pas de compte Azure AD et utilisez un compte fourni par votre fournisseur de services Cloud, et que vous décidez de changer de fournisseur pour utiliser un autre compte Active Directory, vous devrez à ce stade contacter votre fournisseur de solutions afin qu’il redéploie la solution pour vous, à vos frais.



Votre choix de fournisseur d’identité n’a aucune incidence sur les machines virtuelles du client, le système d’identité et les comptes qu’ils utilisent, la possibilité de joindre un domaine Active Directory, etc. C’est différent.

Pour en savoir plus sur le choix d’un fournisseur d’identité, voir l’[article relatif aux modèles de connexion des systèmes intégrés Azure Stack](./azure-stack-connection-models.md).

### <a name="ad-fs-and-graph-integration"></a>Intégration de AD FS et de Graph
Si vous choisissez de déployer Azure Stack en utilisant AD FS comme fournisseur d’identité, vous devez intégrer l’instance AD FS à Azure Stack avec une instance AD FS existante via une approbation de fédération. Ainsi, les identités dans une forêt Active Directory existante peuvent s’authentifier auprès des ressources dans Azure Stack.

Vous pouvez également intégrer le service Graph dans Azure Stack avec le Active Directory existant. Cela vous permet de gérer le contrôle d’accès en fonction du rôle (RBAC) dans Azure Stack. Lorsque l’accès à une ressource est délégué, le composant Graph recherche le compte d’utilisateur dans la forêt Active Directory existante à l’aide du protocole LDAP.

Le diagramme suivant montre le flux de trafic AD FS et Graph intégré.
![Diagramme montrant le flux de trafic AD FS et de Graph](media/azure-stack-datacenter-integration/ADFSIntegration.PNG)

## <a name="licensing-model"></a>Modèle de licence
Vous devez choisir le modèle de licence que vous souhaitez utiliser. Les options disponibles varient selon que vous déployez ou non Azure Stack en étant connecté à Internet :
- Pour un [déploiement connecté](azure-stack-connected-deployment.md), vous pouvez choisir une licence avec paiement à l’utilisation ou selon la capacité. Le paiement à l’utilisation requiert une connexion vers Azure pour rapporter l’utilisation, qui est ensuite facturée via Azure Commerce. 
- Seule la licence basée sur la capacité est prise en charge si vous effectuez un [déploiement en étant déconnecté](azure-stack-disconnected-deployment.md) d’internet. 

Pour plus d’informations sur les modèles de licence, consultez [Empaquetage et tarification de Microsoft Azure Stack](https://azure.microsoft.com/mediahandler/files/resourcefiles/5bc3f30c-cd57-4513-989e-056325eb95e1/Azure-Stack-packaging-and-pricing-datasheet.pdf).


## <a name="naming-decisions"></a>Décisions d’attribution de noms

Vous devez réfléchir à la façon dont vous souhaitez planifier votre espace de noms Azure Stack, notamment le nom de la région et le nom de domaine externe. Le nom de domaine complet (FQDN) de votre déploiement Azure Stack pour les points de terminaison publics est la combinaison de ces deux noms : &lt;*région*&gt;.&lt;*fqdn*&gt;. Par exemple, *east.cloud.fabrikam.com*. Dans cet exemple, les portails de Azure Stack sont accessibles aux URL suivantes :

- https://portal.east.cloud.fabrikam.com
- https://adminportal.east.cloud.fabrikam.com

> [!IMPORTANT]
> Le nom de région que vous choisissez pour votre déploiement Azure Stack doit être unique. Il apparaîtra dans les adresses de portail. 

Le tableau suivant récapitule ces décisions d’attribution de noms de domaine.

| NOM | Description | 
| -------- | ------------- | 
|Nom de la région | Le nom de votre première région Azure Stack. Ce nom est utilisé comme partie du nom de domaine complet pour les adresses IP virtuelles publiques (VIP) gérées par Azure Stack. En règle générale, le nom de la région est un identificateur d’emplacement physique tel qu’un emplacement de centre de données.<br><br>Le nom de région doit uniquement comporter des lettres et des nombres compris entre 0 et 9. Les caractères spéciaux tels que « - » ou « # », etc. ne sont pas autorisés.| 
| Nom de domaine externe | Le nom de la zone Domain Name System (DNS) pour les points de terminaison avec des adresses IP virtuelles externes. Utilisé dans le nom de domaine complet pour ces adresses IP virtuelles publiques. | 
| Nom de domaine privé (interne) | Le nom du domaine (et de la zone DNS interne) créé sur Azure Stack pour la gestion de l’infrastructure. 
| | |

## <a name="certificate-requirements"></a>Configuration requise des certificats

Pour le déploiement, vous devrez fournir les certificats SSL (Secure Sockets Layer) pour les points de terminaison publics. À un niveau élevé, les certificats présentent les exigences suivantes :

- Vous pouvez utiliser un certificat à caractères génériques unique, ou vous pouvez utiliser un jeu de certificats dédiés et utiliser des certificats à caractères génériques uniquement pour les points de terminaison tels que le stockage et le coffre de clés.
- Les certificats peuvent être émis par une autorité de certification (AC) approuvée publique ou une autorité de certification gérée par le client.

Pour plus d’informations sur les certificats pour infrastructure à clé publique requis pour déployer Azure Stack et comment les obtenir, consultez [Exigences de certificat pour infrastructure à clé publique Azure Stack](azure-stack-pki-certs.md).  


> [!IMPORTANT]
> Les informations de certificat pour infrastructure à clé publique fournies doivent être utilisées comme des conseils d’ordre général. Avant d’acquérir des certificats pour infrastructure à clé publique pour Azure Stack, travaillez avec votre fabricant partenaire de matériel OEM. Ce dernier fournit des conseils et des exigences plus détaillées en matière de certificat.


## <a name="time-synchronization"></a>Synchronisation temporelle
Vous devez choisir un serveur de temps spécifique, qui est utilisé pour synchroniser Azure Stack.  La synchronisation du temps est essentielle pour Azure Stack et ses rôles d’infrastructure, car elle est utilisée pour générer les tickets Kerberos utilisés pour authentifier les services internes avec eux.

Vous devez spécifier une adresse IP pour le serveur de synchronisation du temps. Bien que la plupart des composants de l’infrastructure soit en mesure de résoudre une URL, certains prennent uniquement en charge les adresses IP. Si vous utilisez l’option de déploiement déconnecté, vous devez spécifier un serveur de temps sur votre réseau d’entreprise accessible à partir du réseau d’infrastructure dans Azure Stack.

## <a name="connect-azure-stack-to-azure"></a>Connecter Azure Stack à Azure

Pour les scénarios de cloud hybrides, vous devrez planifier la façon dont vous souhaitez connecter Azure Stack à Azure. Deux méthodes vous permettent de connecter des réseaux virtuels Azure Stack à des réseaux virtuels Azure : 
- **Site à site**. Une connexion de réseau privé virtuel (VPN) sur IPsec (IKE v1 et IKE v2). Ce type de connexion requiert un périphérique VPN ou le service de routage et d’accès à distance (RRAS). Pour plus d’informations sur les passerelles VPN dans Azure, consultez l’article [À propos de la passerelle VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways). La communication via ce tunnel est chiffrée et sécurisée. Toutefois, la bande passante est limitée par le débit maximal du tunnel (100-200 Mbps).
- **NAT de trafic sortant**. Par défaut, toutes les machines virtuelles dans Azure Stack ont une connectivité aux réseaux externes via le NAT de trafic sortant. Chaque réseau virtuel créé dans Azure Stack se voit attribuer une adresse IP publique. Si la machine virtuelle se voit directement attribuée une adresse IP publique, ou se trouve derrière un équilibreur de charge avec une adresse IP publique, il aura un accès sortant via le NAT de trafic sortant à l’aide de l’adresse IP virtuelle du réseau virtuel. Cela fonctionne uniquement pour une communication initiée par la machine virtuelle et destinée à des réseaux externes (internet ou intranet). Il ne peut pas être utilisé pour communiquer avec la machine virtuelle à partir de l’extérieur.

### <a name="hybrid-connectivity-options"></a>Options de connectivité hybride

Pour une connectivité hybride, il est important de savoir quel type de déploiement vous souhaitez proposer et où il sera déployé. Vous devrez prendre en compte le fait de devoir isoler le trafic par client, et le fait d’effectuer un déploiement intranet ou internet.

- **Azure Stack à client unique**. Un déploiement de Azure Stack qui semble, au moins du point de vue de la mise en réseau, être un seul client. Il peut y avoir beaucoup d’abonnements clients, mais comme tout service intranet, tout le trafic transite sur les mêmes réseaux. Le trafic provenant d’un abonnement transite sur la même connexion réseau comme un autre abonnement et ne doit être pas être isolé via un tunnel chiffré.

- **Azure Stack mutualisé**. Un déploiement de Azure Stack où le trafic de l’abonnement de chaque client lié pour les réseaux externes à Azure Stack doit être isolé du trafic des autres clients.
 
- **Déploiement intranet**. Un déploiement de Azure Stack qui se trouve sur un intranet d’entreprise, en général sur l’espace d’adresse IP privée et derrière un ou plusieurs pare-feu. Les adresses IP publiques ne sont pas réellement publiques, car elles ne peuvent pas être routées directement via l’internet public.

- **Déploiement internet**. Un déploiement de Azure Stack qui est connecté à l’internet public et utilise des adresses IP publiques routables sur internet pour la plage d’adresses IP virtuelles publiques. Le déploiement peut toujours se placer derrière un pare-feu, mais la plage d’adresses IP virtuelles publiques est directement accessible depuis l’internet public et Azure.
 
Le tableau suivant résume les scénarios de connectivité hybride, avec les avantages, les inconvénients et les cas d’usage.

| Scénario | Méthode de connectivité | Avantages | Inconvénients | Bien pour |
| -- | -- | --| -- | --|
| Azure Stack avec un seul client, déploiement en intranet | NAT de trafic sortant | Meilleure bande passante pour des transferts plus rapides. Simple à implémenter ; aucune passerelle requise. | Trafic non chiffré ; aucun chiffrement ou isolation en dehors de la pile. | Déploiements d’entreprise où tous les clients sont fiables.<br><br>Entreprises qui ont un circuit Azure ExpressRoute vers Azure. |
| Azure Stack mutualisé, déploiement en intranet | VPN de site à site | Le trafic du réseau virtuel du client vers la destination est sécurisé. | La bande passante est limitée par le tunnel VPN de site à site.<br><br>Requiert une passerelle dans le réseau virtuel et un périphérique VPN sur le réseau de destination. | Les déploiements d’entreprise où certains trafics de client doivent être sécurisés par rapport aux autres clients. |
| Azure Stack avec un seul client, déploiement par internet | NAT de trafic sortant | Meilleure bande passante pour des transferts plus rapides. | Trafic non chiffré ; aucun chiffrement ou isolation en dehors de la pile. | Hébergement des scénarios où le client obtient son propre déploiement de Azure Stack et un circuit dédié à l’environnement Azure Stack. Par exemple, ExpressRoute et MPLS (Multiprotocol Label Switching).
| Azure Stack mutualisé, déploiement par internet | VPN de site à site | Le trafic du réseau virtuel du client vers la destination est sécurisé. | La bande passante est limitée par le tunnel VPN de site à site.<br><br>Requiert une passerelle dans le réseau virtuel et un périphérique VPN sur le réseau de destination. | Hébergement des scénarios où le fournisseur souhaite offrir un cloud mutualisé, où les clients ne se font pas confiance et où le trafic doit être chiffré.
|  |  |  |  |  |

### <a name="using-expressroute"></a>Utilisation de ExpressRoute

Vous pouvez connecter Azure Stack à Azure via [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) pour un intranet à un client et des scénarios mutualisés. Vous aurez besoin d’un circuit ExpressRoute approvisionné via un [fournisseur de connectivité](https://docs.microsoft.com/azure/expressroute/expressroute-locations).

Le diagramme suivant montre ExpressRoute pour un scénario à un client (où « Connexion du client » est le circuit ExpressRoute).

![Diagramme montrant un scénario ExpressRoute à un client](media/azure-stack-datacenter-integration/ExpressRouteSingleTenant.PNG)

Le diagramme suivant montre ExpressRoute pour un scénario mutualisé.

![Diagramme montrant un scénario ExpressRoute mutualisé](media/azure-stack-datacenter-integration/ExpressRouteMultiTenant.PNG)

## <a name="external-monitoring"></a>Surveillance externe
Pour obtenir une vue unique de toutes les alertes provenant de vos appareils et du déploiement d’Azure Stack, et intégrer des alertes dans les flux de travail existants de gestion des services informatiques pour la création de tickets, vous pouvez [intégrer Azure Stack aux solutions de surveillance du centre de données externe](azure-stack-integrate-monitor.md).

Inclus dans la solution Azure Stack, l’hôte de cycle de vie du matériel est un ordinateur extérieur à Azure Stack qui exécute les outils d’administration fournie par le fabricant OEM pour le matériel. Vous pouvez utiliser ces outils ou d’autres solutions s’intégrant directement avec les solutions de surveillance existantes dans votre centre de données.

Le tableau suivant récapitule la liste des options actuellement disponibles.

| Domaine | Solution de surveillance externe |
| -- | -- |
| Logiciel Azure Stack | [Pack d’administration Azure Stack pour Operations Manager](https://azure.microsoft.com/blog/management-pack-for-microsoft-azure-stack-now-available/)<br>[Plug-in Nagios](https://exchange.nagios.org/directory/Plugins/Cloud/Monitoring-AzureStack-Alerts/details)<br>Appels d’API basés sur REST | 
| Serveurs physiques (BMC via IPMI) | Matériel OEM : pack d’administration de fournisseur Operations Manager<br>Solution fournie par le fabricant de matériel OEM<br>Plu-gins Nagios du fabricant de matériel | Solution de surveillance prise en charge par le partenaire OEM (inclus) | 
| Périphériques réseau (SNMP) | Découverte des périphériques réseau Operations Manager<br>Solution fournie par le fabricant de matériel OEM<br>Plug-in de commutateur Nagios |
| Surveillance de l’intégrité de l’abonnement client | [Pack d’administration System Center pour Windows Azure](https://www.microsoft.com/download/details.aspx?id=50013) | 
|  |  | 

Notez les exigences suivantes :
- La solution que vous utilisez doit être sans agent. Vous ne pouvez pas installer d’agents tiers à l’intérieur des composants de Azure Stack. 
- Si vous souhaitez utiliser System Center Operations Manager, Operations Manager 2012 R2 ou Operations Manager 2016 sont requis.

## <a name="backup-and-disaster-recovery"></a>Sauvegarde et récupération d'urgence

La planification de la sauvegarde et de la récupération d’urgence implique une planification pour l’infrastructure Azure Stack sous-jacente hébergeant les machines virtuelles IaaS et les services PaaS, et pour les applications et les données du client. Vous devez faire ces planifications séparément.

### <a name="protect-infrastructure-components"></a>Protéger les composants d’infrastructure

Vous pouvez [sauvegarder les composants d’infrastructure Azure Stack](azure-stack-backup-back-up-azure-stack.md) sur un partage SMB que vous spécifiez :

- Vous aurez besoin d’un partage de fichiers SMB externe sur un serveur de fichiers Windows existant ou un périphérique tiers.
- Vous devriez utiliser ce même partage pour la sauvegarde des commutateurs réseau et l’hôte de cycle de vie du matériel. Votre fabricant de matériel OEM aidera à fournir des conseils pour la sauvegarde et la restauration de ces composants étant donné qu’ils ne font pas partie de Azure Stack. Vous êtes responsable d’exécuter les flux de travail de sauvegarde basés sur la recommandation du fabricant OEM.

En cas de perte catastrophique de données, vous pouvez utiliser la sauvegarde de l’infrastructure pour réimplanter les données de déploiement telles que les entrées et les identificateurs de déploiement, les comptes de service, le certificat racine d’autorité de certification, les ressources fédérés (dans les déploiements déconnectés), les plans, les offres, les abonnements, les quotas, les attributions de rôle et de stratégie RBAC et les secrets de coffre de clés.
 
### <a name="protect-tenant-applications-on-iaas-virtual-machines"></a>Protéger les applications du client sur des machines virtuelles IaaS

Azure Stack ne sauvegarde pas les applications et les données du client. Vous devez planifier la sauvegarde et la protection de récupération d’urgence vers une cible externe au Azure Stack. La protection du client est une activité qu’il conduit lui-même. Pour les machines virtuelles IaaS, les clients peuvent utiliser les technologies intégrées pour protéger des dossiers de fichiers, des données d’application et l’état du système. Toutefois, en tant qu’entreprise ou fournisseur de service, vous voudrez peut-être offrir une solution de sauvegarde et de restauration dans le même centre de données ou à l’extérieur, dans un cloud.

Pour sauvegarder des machines virtuelles IaaS Windows ou Linux, vous devez utiliser des produits de sauvegarde avec un accès au système d’exploitation invité pour protéger les fichiers, les dossiers, l’état du système d’exploitation et les données d’application. Vous pouvez utiliser Azure Backup, System Center Data Protection Center Manager, ou les produits tiers pris en charge.

Pour répliquer des données vers un emplacement secondaire et orchestrer le basculement d’application si un incident se produit, vous pouvez utiliser Azure Site Recovery ou des produits tiers pris en charge. En outre, les applications qui prennent en charge la réplication native, comme Microsoft SQL Server, peuvent répliquer des données vers un autre emplacement dans lequel l’application est en cours d’exécution.

## <a name="learn-more"></a>En savoir plus

- Pour plus d’informations sur les cas d’usage, l’achat, les partenaires et les fabricants de matériel OEM, consultez la page produit [Azure Stack](https://azure.microsoft.com/overview/azure-stack/).
- Pour plus d’informations sur la feuille de route et la disponibilité géographique des systèmes intégrés Azure Stack, consultez le livre blanc : [Azure Stack : An extension of Azure](https://azure.microsoft.com/resources/azure-stack-an-extension-of-azure/). 

## <a name="next-steps"></a>Étapes suivantes
[Modèles de connexion pour le déploiement d’Azure Stack](azure-stack-connection-models.md)
