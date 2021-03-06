---
title: Informations de référence sur l’applet de commande Start-AzsReadinessChecker | Microsoft Docs
description: Aide sur l’applet de commande PowerShell pour le module Azure Stack Readiness Checker.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/04/2018
ms.author: sethm
ms.reviewer: ''
ms.openlocfilehash: 1dbfd668c2d233d299ee673da92ca203e72942fe
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/06/2018
ms.locfileid: "52957416"
---
# <a name="start-azsreadinesschecker-cmdlet-reference"></a>Informations de référence sur l’applet de commande Start-AzsReadinessChecker

Module : Microsoft.AzureStack.ReadinessChecker

Ce module contient une seule applet de commande.  Cette applet de commande effectue une ou plusieurs fonctions préalables au déploiement ou à la maintenance d’Azure Stack.

## <a name="syntax"></a>Syntaxe

```PowerShell
Start-AzsReadinessChecker
       [-CertificatePath <String>]
       -PfxPassword <SecureString>
       -RegionName <String>
       -FQDN <String>
       -IdentitySystem <String>
       [-CleanReport]
       [-OutputPath <String>]
       [-WhatIf]
       [-Confirm]
       [<CommonParameters>]
```

```PowerShell
Start-AzsReadinessChecker
       [-CertificatePath <String>]
       -PfxPassword <SecureString>
       -DeploymentDataJSONPath <String>
       [-CleanReport]
       [-OutputPath <String>]
       [-WhatIf]
       [-Confirm]
       [<CommonParameters>]
```

```PowerShell
Start-AzsReadinessChecker
       -PaaSCertificates <Hashtable>
       -DeploymentDataJSONPath <String>
       [-OutputPath <String>]
       [-WhatIf]
       [-Confirm]
       [<CommonParameters>]
```

```PowerShell
Start-AzsReadinessChecker
       -PaaSCertificates <Hashtable>
       -RegionName <String>
       -FQDN <String>
       -IdentitySystem <String>
       [-OutputPath <String>]
       [-WhatIf]
       [-Confirm]
       [<CommonParameters>]
```

```PowerShell
Start-AzsReadinessChecker
       -RegionName <String>
       -FQDN <String>
       -IdentitySystem <String>
       -Subject <OrderedDictionary>
       -RequestType <String>
       [-IncludePaaS]
       -OutputRequestPath <String>
       [-OutputPath <String>]
       [-WhatIf]
       [-Confirm]
       [<CommonParameters>]
```

```PowerShell
Start-AzsReadinessChecker
       -PfxPassword <SecureString>
       -PfxPath <String>
       -ExportPFXPath <String>
       [-OutputPath <String>]
       [-WhatIf]
       [-Confirm]
       [<CommonParameters>]
```

```PowerShell
Start-AzsReadinessChecker
       -AADServiceAdministrator <PSCredential>
       -AADDirectoryTenantName <String>
       -IdentitySystem <String>
       -AzureEnvironment <String>
       [-CleanReport]
       [-OutputPath <String>]
       [-WhatIf]
       [-Confirm]
       [<CommonParameters>]
```

```PowerShell
Start-AzsReadinessChecker
       -AADServiceAdministrator <PSCredential>
       -DeploymentDataJSONPath <String>
       [-CleanReport]
       [-OutputPath <String>]
       [-WhatIf]
       [-Confirm]
       [<CommonParameters>]
```

```PowerShell
Start-AzsReadinessChecker
       -RegistrationAccount <PSCredential>
       -RegistrationSubscriptionID <Guid>
       -AzureEnvironment <String>
       [-CleanReport]
       [-OutputPath <String>]
       [-WhatIf]
       [-Confirm]
       [<CommonParameters>]
```

```PowerShell
Start-AzsReadinessChecker
       -RegistrationAccount <PSCredential>
       -RegistrationSubscriptionID <Guid>
       -DeploymentDataJSONPath <String>
       [-CleanReport]
       [-OutputPath <String>]
       [-WhatIf]
       [-Confirm]
       [<CommonParameters>]
```

```PowerShell
Start-AzsReadinessChecker
       -ReportPath <String>
       [-ReportSections <String>]
       [-Summary]
       [-OutputPath <String>]
       [-WhatIf]
       [-Confirm]
       [<CommonParameters>]
```

## <a name="description"></a>Description

L’applet de commande **Start-AzsReadinessChecker** valide les certificats, comptes Azure, abonnements Azure et annuaires Azure Active Directory. Exécutez la validation avant de déployer Azure Stack ou avant d’exécuter des actions de maintenance sur Azure Stack comme la rotation des secrets. L’applet de commande peut également servir à générer des demandes de signature de certificat pour des certificats d’infrastructure et, éventuellement, des certificats PaaS.  Enfin, l’applet de commande peut repackager des certificats PFX pour corriger les problèmes de packaging courants.

## <a name="examples"></a>Exemples

### <a name="example-generate-certificate-signing-request"></a>Exemple : Générer la demande de signature de certificat

```PowerShell
$regionName = 'east'
$externalFQDN = 'azurestack.contoso.com'
$subjectHash = [ordered]@{"OU"="AzureStack";"O"="Microsoft";"L"="Redmond";"ST"="Washington";"C"="US"}
Start-AzsReadinessChecker -regionName $regionName -externalFQDN $externalFQDN -subject $subjectHash -IdentitySystem ADFS -requestType MultipleCSR
```

Dans cet exemple, Start-AzsReadinessChecker génère plusieurs demandes de signature de certificat pour des certificats adaptés à un déploiement ADFS d’Azure Stack, avec « east » comme nom de région et « azurestack.contoso.com » comme FQDN externe.

### <a name="example-validate-certificates"></a>Exemple : Valider des certificats

```PowerShell
$password = Read-Host -Prompt "Enter PFX Password" -AsSecureString
Start-AzsReadinessChecker -CertificatePath .\Certificates\ -PfxPassword $password -RegionName east -FQDN azurestack.contoso.com -IdentitySystem AAD
```

Dans cet exemple, le mot de passe PFX est demandé à des fins de sécurité, et Start-AzsReadinessChecker recherche dans le dossier relatif « Certificates » des certificats valides pour un déploiement AAD, avec « east » comme nom de région et « azurestack.contoso.com » comme FQDN externe.

### <a name="example-validate-certificates-with-deployment-data-deployment-and-support"></a>Exemple : Valider des certificats avec des données de déploiement (déploiement et support)

```PowerShell
$password = Read-Host -Prompt "Enter PFX Password" -AsSecureString
Start-AzsReadinessChecker -CertificatePath .\Certificates\ -PfxPassword $password -DeploymentDataJSONPath .\deploymentdata.json
```

Dans cet exemple de déploiement et de support, le mot de passe PFX est demandé à des fins de sécurité, et Start-AzsReadinessChecker recherche dans le dossier relatif « Certificats » des certificats valides pour un déploiement dans lequel l’identité, la région et le FQDN externe sont lus à partir du fichier JSON de données de déploiement généré pour le déploiement. 

### <a name="example-validate-paas-certificates"></a>Exemple : Valider des certificats PaaS

```PowerShell
$PaaSCertificates = @{
    'PaaSDBCert' = @{'pfxPath' = '<Path to DBAdapter PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
    'PaaSDefaultCert' = @{'pfxPath' = '<Path to Default PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
    'PaaSAPICert' = @{'pfxPath' = '<Path to API PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
    'PaaSFTPCert' = @{'pfxPath' = '<Path to FTP PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
    'PaaSSSOCert' = @{'pfxPath' = '<Path to SSO PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
}
Start-AzsReadinessChecker -PaaSCertificates $PaaSCertificates – RegionName east -FQDN azurestack.contoso.com
```

Dans cet exemple, une table de hachage est construite avec des chemins et des mots de passe pour chaque certificat PaaS. Les certificats peuvent être omis. Start-AzsReadinessChecker vérifie l’existence de chaque chemin PFX et les valide en utilisant la région « east » et le FQDN externe « azurestack.contoso.com ».

### <a name="example-validate-paas-certificates-with-deployment-data"></a>Exemple : Valider des certificats PaaS avec des données de déploiement

```PowerShell
$PaaSCertificates = @{
    'PaaSDBCert' = @{'pfxPath' = '<Path to DBAdapter PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
    'PaaSDefaultCert' = @{'pfxPath' = '<Path to Default PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
    'PaaSAPICert' = @{'pfxPath' = '<Path to API PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
    'PaaSFTPCert' = @{'pfxPath' = '<Path to FTP PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
    'PaaSSSOCert' = @{'pfxPath' = '<Path to SSO PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
}
Start-AzsReadinessChecker -PaaSCertificates $PaaSCertificates -DeploymentDataJSONPath .\deploymentdata.json
```

Dans cet exemple, une table de hachage est construite avec des chemins et des mots de passe pour chaque certificat PaaS. Les certificats peuvent être omis. Start-AzsReadinessChecker vérifie l’existence de chaque chemin PFX et les valide en utilisant la région et le FQDN externe lus à partir du fichier JSON de données de déploiement généré pour le déploiement. 

### <a name="example-validate-azure-identity"></a>Exemple : Valider l’identité Azure

```PowerShell
$serviceAdminCredential = Get-Credential -Message "Enter Credentials for Service Administrator of Azure Active Directory Tenant e.g. serviceadmin@contoso.onmicrosoft.com"
# Supported values for the <environment name> parameter are AzureCloud, AzureChinaCloud or AzureUSGovernment depending which Azure subscription you are using.
Start-AzsReadinessChecker -AADServiceAdministrator $serviceAdminCredential -AzureEnvironment "<environment name>" -AzureDirectoryTenantName azurestack.contoso.com
```

Dans cet exemple, les informations d’identification du compte Administrateur de service sont demandées à des fins de sécurité, et Start-AzsReadinessChecker vérifie que le compte Azure et que l’annuaire Azure Active Directory sont valides pour un déploiement AAD avec « azurestack.contoso.com » comme nom d’annuaire de locataire.

### <a name="example-validate-azure-identity-with-deployment-data-deployment-support"></a>Exemple : Valider l’identité Azure avec des données de déploiement (déploiement et support)

```PowerSHell
$serviceAdminCredential = Get-Credential -Message "Enter Credentials for Service Administrator of Azure Active Directory Tenant e.g. serviceadmin@contoso.onmicrosoft.com"
Start-AzsReadinessChecker -AADServiceAdministrator $serviceAdminCredential -DeploymentDataJSONPath .\contoso-depploymentdata.json
```

Dans cet exemple, les informations d’identification du compte Administrateur de service sont demandées à des fins de sécurité. Start-AzsReadinessChecker vérifie ensuite que le compte Azure et que l’annuaire Azure Active Directory sont valides pour un déploiement AAD, où AzureCloud et TenantName sont lus à partir du fichier JSON de données de déploiement généré pour le déploiement.

### <a name="example-validate-azure-registration"></a>Exemple : Valider l’inscription auprès d’Azure

```PowerShell
$registrationCredential = Get-Credential -Message "Enter Credentials for Subscription Owner e.g. subscriptionowner@contoso.onmicrosoft.com"
$subscriptionID = "<subscription ID"
# Supported values for the <environment name> parameter are AzureCloud, AzureChinaCloud or AzureUSGovernment depending which Azure subscription you are using.
Start-AzsReadinessChecker -RegistrationAccount $registrationCredential -RegistrationSubscriptionID $subscriptionID -AzureEnvironment "<environment name>"
```

Dans cet exemple, les informations d’identification du propriétaire de l’abonnement sont demandées à des fins de sécurité. Start-AzsReadinessChecker valide ensuite le compte et l’abonnement donnés pour vérifier qu’ils peuvent être utilisés pour l’inscription d’Azure Stack. 

### <a name="example-validate-azure-registration-with-deployment-data-deployment-team"></a>Exemple : Valider l’inscription auprès d’Azure avec des données de déploiement (équipe de déploiement)

```PowerShell
$registrationCredential = Get-Credential -Message "Enter Credentials for Subscription Owner e.g. subscriptionowner@contoso.onmicrosoft.com"
$subscriptionID = "<subscription ID>"
Start-AzsReadinessChecker -RegistrationAccount $registrationCredential -RegistrationSubscriptionID $subscriptionID -DeploymentDataJSONPath .\contoso-deploymentdata.json
```

Dans cet exemple, les informations d’identification du propriétaire de l’abonnement sont demandées à des fins de sécurité. Start-AzsReadinessChecker valide ensuite le compte et l’abonnement donnés pour vérifier qu’ils peuvent être utilisés pour l’inscription d’Azure Stack, où des détails supplémentaires sont lus à partir du fichier JSON de données de déploiement généré pour le déploiement.

### <a name="example-importexport-pfx-package"></a>Exemple : Importer/Exporter un package PFX

```PowerShell
$password = Read-Host -Prompt "Enter PFX Password" -AsSecureString
Start-AzsReadinessChecker -PfxPassword $password -PfxPath .\certificates\ssl.pfx -ExportPFXPath .\certificates\ssl_new.pfx
```

Dans cet exemple, le mot de passe PFX est demandé à des fins de sécurité. Le fichier ssl.pfx est importé dans le magasin de certificats de l’ordinateur local, réexporté avec le même mot de passe, puis enregistré en tant que ssl_new.pfx.  Suivez cette procédure quand la validation de certificat signale que l’attribut Ordinateur local n’est pas défini pour une clé privée, quand la chaîne d’approbation est rompue, quand des certificats inadaptés sont présents dans le PFX ou quand l’ordre de la chaîne d’approbation est incorrect.

### <a name="example-view-validation-report-deployment-support"></a>Exemple : Afficher le rapport de validation (déploiement et support)

```PowerShell
Start-AzsReadinessChecker -ReportPath Contoso-AzsReadinessReport.json
```

Dans cet exemple, l’équipe de déploiement ou de support reçoit le rapport de préparation du client (Contoso), et utilise Start-AzsReadinessChecker pour afficher l’état des exécutions de validation effectuées par Contoso.

### <a name="example-view-validation-report-summary-for-certificate-validation-only-deployment-and-support"></a>Exemple : Afficher le récapitulatif du rapport de validation pour la validation de certificat uniquement (déploiement et support)

```PowerShell
Start-AzsReadinessChecker -ReportPath Contoso-AzsReadinessReport.json -ReportSections Certificate -Summary
```

Dans cet exemple, l’équipe de déploiement ou de support reçoit le rapport de préparation du client Contoso, et utilise Start-AzsReadinessChecker pour afficher un récapitulatif de l’état des exécutions de validation de certificat effectuées par Contoso.

## <a name="required-parameters"></a>Paramètres obligatoires

> -RegionName

Spécifie le nom de la région du déploiement Azure Stack.
|  |  |
|----------------------------|--------------|
|Tapez :                       |Chaîne        |
|Position :                   |named         |
|Valeur par défaut :              |Aucun          |
|Accepter l’entrée de pipeline :      |False         |
|Accepter les caractères génériques : |False         |

> -FQDN

Spécifie le FQDN externe du déploiement Azure Stack (alias : ExternalFQDN et ExternalDomainName).
|  |  |
|----------------------------|--------------|
|Tapez :                       |Chaîne        |
|Position :                   |named         |
|Valeur par défaut :              |ExternalFQDN, ExternalDomainName |
|Accepter l’entrée de pipeline :      |False         |
|Accepter les caractères génériques : |False         |

> -IdentitySystem

Spécifie les valeurs valides du système d’identité du déploiement Azure Stack : AAD (Azure Active Directory) et ADFS (services de fédération Active Directory).
|  |  |
|----------------------------|--------------|
|Tapez :                       |Chaîne        |
|Position :                   |named         |
|Valeur par défaut :              |Aucun          |
|Valeurs valides :                |« AAD », « ADFS »  |
|Accepter l’entrée de pipeline :      |False         |
|Accepter les caractères génériques : |False         |

> -PfxPassword

Spécifie le mot de passe associé aux fichiers de certificat PFX.
|  |  |
|----------------------------|---------|
|Tapez :                       |SecureString |
|Position :                   |named    |
|Valeur par défaut :              |Aucun     |
|Accepter l’entrée de pipeline :      |False    |
|Accepter les caractères génériques : |False    |

> -PaaSCertificates

Spécifie la table de hachage contenant les chemins et mots de passe des certificats PaaS.
|  |  |
|----------------------------|---------|
|Tapez :                       |Hashtable |
|Position :                   |named    |
|Valeur par défaut :              |Aucun     |
|Accepter l’entrée de pipeline :      |False    |
|Accepter les caractères génériques : |False    |

> -DeploymentDataJSONPath

Spécifie le fichier de configuration JSON des données de déploiement Azure Stack. Ce fichier est généré pour le déploiement.
|  |  |
|----------------------------|---------|
|Tapez :                       |Chaîne   |
|Position :                   |named    |
|Valeur par défaut :              |Aucun     |
|Accepter l’entrée de pipeline :      |False    |
|Accepter les caractères génériques : |False    |

> -PfxPath

Spécifie le chemin à un certificat problématique dont la correction nécessite une routine d’importation/exportation, comme indiqué par la validation du certificat dans cet outil.
|  |  |
|----------------------------|---------|
|Tapez :                       |Chaîne   |
|Position :                   |named    |
|Valeur par défaut :              |Aucun     |
|Accepter l’entrée de pipeline :      |False    |
|Accepter les caractères génériques : |False    |

> -ExportPFXPath  

Spécifie le chemin de destination du fichier PFX résultant de la routine d’importation/exportation.  
|  |  |
|----------------------------|---------|
|Tapez :                       |Chaîne   |
|Position :                   |named    |
|Valeur par défaut :              |Aucun     |
|Accepter l’entrée de pipeline :      |False    |
|Accepter les caractères génériques : |False    |

> -Subject

Spécifie un dictionnaire ordonné de l’objet pour la génération de demande de certificat.
|  |  |
|----------------------------|---------|
|Tapez :                       |OrderedDictionary   |
|Position :                   |named    |
|Valeur par défaut :              |Aucun     |
|Accepter l’entrée de pipeline :      |False    |
|Accepter les caractères génériques : |False    |

> -RequestType

Spécifie le type SAN de la demande de certificat. Valeurs valides : MultipleCSR, SingleCSR.

- *MultipleCSR* génère plusieurs demandes de certificat, une pour chaque service.
- *SingleCSR* génère une demande de certificat pour tous les services.

|  |  |
|----------------------------|---------|
|Tapez :                       |Chaîne   |
|Position :                   |named    |
|Valeur par défaut :              |Aucun     |
|Valeurs valides :                |« MultipleCSR », « SingleCSR » |
|Accepter l’entrée de pipeline :      |False    |
|Accepter les caractères génériques : |False    |

> -OutputRequestPath

Spécifie le chemin de destination pour les fichiers de demande de certificat. Le répertoire doit déjà exister.
|  |  |
|----------------------------|---------|
|Tapez :                       |Chaîne   |
|Position :                   |named    |
|Valeur par défaut :              |Aucun     |
|Accepter l’entrée de pipeline :      |False    |
|Accepter les caractères génériques : |False    |

> -AADServiceAdministrator

Spécifie l’administrateur du service Azure Active Directory à utiliser pour le déploiement d’Azure Stack.
|  |  |
|----------------------------|---------|
|Tapez :                       |PSCredential   |
|Position :                   |named    |
|Valeur par défaut :              |Aucun     |
|Accepter l’entrée de pipeline :      |False    |
|Accepter les caractères génériques : |False    |

> -AADDirectoryTenantName

Spécifie le nom Azure Active Directory à utiliser pour le déploiement d’Azure Stack.
|  |  |
|----------------------------|---------|
|Tapez :                       |Chaîne   |
|Position :                   |named    |
|Valeur par défaut :              |Aucun     |
|Accepter l’entrée de pipeline :      |False    |
|Accepter les caractères génériques : |False    |

> -AzureEnvironment

Spécifie l’instance des services Azure contenant les comptes, annuaires et abonnements à utiliser pour l’inscription et le déploiement d’Azure Stack.
|  |  |
|----------------------------|---------|
|Tapez :                       |Chaîne   |
|Position :                   |named    |
|Valeur par défaut :              |Aucun     |
|Valeurs valides :                |« AzureCloud », « AzureChinaCloud », « AzureUSGovernment » |
|Accepter l’entrée de pipeline :      |False    |
|Accepter les caractères génériques : |False    |

> -RegistrationAccount

Spécifie le compte d’inscription à utiliser pour l’inscription d’Azure Stack.
|  |  |
|----------------------------|---------|
|Tapez :                       |Chaîne   |
|Position :                   |named    |
|Valeur par défaut :              |Aucun     |
|Accepter l’entrée de pipeline :      |False    |
|Accepter les caractères génériques : |False    |

> -RegistrationSubscriptionID

Spécifie l’ID d’abonnement d’inscription à utiliser pour l’inscription d’Azure Stack.
|  |  |
|----------------------------|---------|
|Tapez :                       |Guid     |
|Position :                   |named    |
|Valeur par défaut :              |Aucun     |
|Accepter l’entrée de pipeline :      |False    |
|Accepter les caractères génériques : |False    |

> -ReportPath

Spécifie le chemin du rapport de préparation. Le répertoire actif et le nom de rapport par défaut sont utilisés par défaut.
|  |  |
|----------------------------|---------|
|Tapez :                       |Chaîne   |
|Position :                   |named    |
|Valeur par défaut :              |Tous      |
|Accepter l’entrée de pipeline :      |False    |
|Accepter les caractères génériques : |False    |

## <a name="optional-parameters"></a>Paramètres facultatifs

> -CertificatePath

Spécifie le chemin sous lequel seuls les dossiers de certificat exigés sont présents.

Les dossiers exigés pour le déploiement d’Azure Stack avec le système d’identité d’Azure Active Directory sont les suivants :

ACSBlob, ACSQueue, ACSTable, Admin Portal, ARM Admin, ARM Public, KeyVault, KeyVaultInternal, Public Portal

Les dossiers exigés pour le déploiement d’Azure Stack avec le système d’identité des services de fédération Active Directory sont les suivants :

ACSBlob, ACSQueue, ACSTable, ADFS, Admin Portal, ARM Admin, ARM Public, Graph, KeyVault, KeyVaultInternal, Public Portal

|  |  |
|----------------------------|---------|
|Tapez :                       |Chaîne   |
|Position :                   |named    |
|Valeur par défaut :              |.\Certificates |
|Accepter l’entrée de pipeline :      |False    |
|Accepter les caractères génériques : |False    |

> -IncludePaaS  

Spécifie si les noms d’hôte/services PaaS doivent être ajoutés à la ou aux demandes de certificat.

|  |  |
|----------------------------|------------------|
|Tapez :                       |SwitchParameter   |
|Position :                   |named             |
|Valeur par défaut :              |False             |
|Accepter l’entrée de pipeline :      |False             |
|Accepter les caractères génériques : |False             |

> -ReportSections

Spécifie d’afficher uniquement le récapitulatif du rapport. Omet les détails.
|  |  |
|----------------------------|---------|
|Tapez :                       |Chaîne   |
|Position :                   |named    |
|Valeur par défaut :              |Tous      |
|Valeurs valides :                |« Certificat », « AzureRegistration », « AzureIdentity », « Jobs », « All » |
|Accepter l’entrée de pipeline :      |False    |
|Accepter les caractères génériques : |False    |

> -Summary

Spécifie d’afficher uniquement le récapitulatif du rapport. Omet les détails.
|  |  |
|----------------------------|------------------|
|Tapez :                       |SwitchParameter   |
|Position :                   |named             |
|Valeur par défaut :              |False             |
|Accepter l’entrée de pipeline :      |False             |
|Accepter les caractères génériques : |False             |

> -CleanReport

Supprime l’historique des exécutions et validations précédentes, et écrit les validations dans un nouveau rapport.
|  |  |
|----------------------------|------------------|
|Tapez :                       |SwitchParameter   |
|Alias :                    |cf                |
|Position :                   |named             |
|Valeur par défaut :              |False             |
|Accepter l’entrée de pipeline :      |False             |
|Accepter les caractères génériques : |False             |

> -OutputPath

Spécifie le chemin personnalisé pour enregistrer le rapport JSON de préparation et le fichier journal détaillé.  Si le chemin n’existe pas, l’outil tente de créer le répertoire.

|  |  |
|----------------------------|------------------|
|Tapez :                       |Chaîne            |
|Position :                   |named             |
|Valeur par défaut :              |$ENV:TEMP\AzsReadinessChecker  |
|Accepter l’entrée de pipeline :      |False             |
|Accepter les caractères génériques : |False             |

> -Confirm

Demande confirmation avant d’exécuter l’applet de commande.
|  |  |
|----------------------------|------------------|
|Tapez :                       |SwitchParameter   |
|Alias :                    |cf                |
|Position :                   |named             |
|Valeur par défaut :              |False             |
|Accepter l’entrée de pipeline :      |False             |
|Accepter les caractères génériques : |False             |

> -WhatIf

Montre ce qui se passe en cas d’exécution de l’applet de commande. L’applet de commande n’est pas exécutée.
|  |  |
|----------------------------|------------------|
|Tapez :                       |SwitchParameter   |
|Alias :                    |wi                |
|Position :                   |named             |
|Valeur par défaut :              |False             |
|Accepter l’entrée de pipeline :      |False             |
|Accepter les caractères génériques : |False             |
