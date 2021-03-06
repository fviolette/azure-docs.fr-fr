---
title: Caractéristiques
titleSuffix: Language Understanding - Azure Cognitive Services
description: Ajoutez des fonctionnalités à un modèle de langage afin de fournir des conseils sur la façon de reconnaître les entrées que vous souhaitez étiqueter ou classer.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: conceptual
ms.date: 01/02/2019
ms.author: diberry
ms.openlocfilehash: 2d6f7e2fd332e1687db1564befeb6f531045c5dd
ms.sourcegitcommit: fd488a828465e7acec50e7a134e1c2cab117bee8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/03/2019
ms.locfileid: "53993058"
---
# <a name="phrase-list-features-in-your-luis-app"></a>Caractéristiques de liste d’expressions dans votre application LUIS

En Machine Learning, une *caractéristique* (« feature ») est un trait ou un attribut distinctif des données observées par le système. 

Ajoutez des fonctionnalités à un modèle de langage afin de fournir des conseils sur la façon de reconnaître les entrées que vous souhaitez étiqueter ou classer. Les fonctionnalités aident LUIS à reconnaître les intentions et les entités, mais les fonctionnalités ne sont pas elles-mêmes des intentions ou des entités. Les fonctionnalités peuvent toutefois fournir des exemples de termes connexes.  

## <a name="what-is-a-phrase-list-feature"></a>Qu’est-ce qu’une fonctionnalité de liste d’expressions ?
Une liste d’expressions inclut un groupe de valeurs (mots ou expressions) qui appartiennent à la même classe et qui doivent être traitées de la même façon (par exemple, des noms de villes ou des produits). Ce que LUIS apprend sur l’un d’entre elles s’applique automatiquement également aux autres. Cette liste n’est pas une [entité de liste](luis-concept-entity-types.md#types-of-entities) (correspondances de texte exactes) de mots correspondants.

Une liste d’expressions complète le vocabulaire du domaine d’application sous la forme d’un deuxième signal vers LUIS sur ces mots.

## <a name="phrase-lists-help-all-models"></a>Les listes d’expressions aident tous les modèles

Les listes d’expressions ne sont pas liées à une intention ou à une entité spécifique, mais sont ajoutées pour améliorer tous les modèles. Leur objectif est d’améliorer la détection de l’intention et la classification des entités.

## <a name="how-to-use-phrase-lists"></a>Comment utiliser des listes d’expressions
Dans le [tutoriel d’entité simple](luis-quickstart-primary-and-secondary-data.md) de l’application de ressources humaines, l’application utilise une liste d’expressions **travail** de types d’emplois tels que programmeur, couvreur et secrétaire. Si vous étiquetez une des valeurs suivantes en tant qu’entité ayant bénéficié du machine-learning, LUIS apprend à reconnaître les autres. 

Une liste d’expressions peut être interchangeable ou non. Une liste d’expressions *interchangeable* concerne les valeurs qui sont des synonymes, et une liste d’expressions *non interchangeable* est conçue comme une liste de vocabulaire propre à une application. À mesure que la liste d’expressions de vocabulaire croît, vous constaterez peut-être que certains termes ont de nombreuses formes (synonymes). Répartissez-les dans une autre liste d’expressions interchangeable. 

|Type de liste|Objectif|
|--|--|
|Interchangeable|Synonymes ou mots qui, quand ils sont remplacés par un autre mot dans la liste, ont la même intention et extraction d’entité.|
|Non-interchangeable|Vocabulaire d’application propre à votre application (généralement plus que d’autres mots dans cette langue).|

Les listes d’expressions facilitent non seulement la détection d’entité, mais également la classification d’intention où le caractère non interchangeable est plus logique, comme par exemple pour l’ajout de mots de vocabulaire qui ne sont pas connus en langue française.


<a name="phrase-lists-help-identify-simple-exchangeable-entities"></a>

## <a name="phrase-lists-help-identify-simple-interchangeable-entities"></a>Les listes d’expressions permettent d’identifier les entités interchangeables simples
Les listes d’expressions interchangeables sont une bonne méthode pour optimiser la performance de votre application LUIS. Si votre application rencontre des difficultés à prédire des énoncés sur l’intention appropriée, ou à reconnaître des entités, vérifiez si les énoncés contiennent des mots inhabituels, ou des mots dont le sens peut être ambigu. Ces mots sont de bons candidats à inclure dans une liste d’expressions.

## <a name="phrase-lists-help-identify-intents-by-better-understanding-context"></a>Les listes d’expressions permettent d’identifier les intentions grâce à une meilleure compréhension du contexte
Une liste d’expressions n’est pas une instruction pour que LUIS effectue une correspondance stricte ni n’étiquette exactement pareil les termes dans la liste d’expressions. Il s’agit simplement d’un conseil. Par exemple, vous pourriez avoir une liste d’expressions qui indique que « Patti » et « Selma » sont des noms, mais LUIS peut toujours utiliser des informations contextuelles pour reconnaître qu’ils signifient autre chose dans « Réserver pour 2 chez Patti’s Diner pour le dîner » et « Me trouver des itinéraires routiers pour Selma, Géorgie ». 

L’ajouter d’une liste d’expressions est une alternative à l’ajout de plusieurs exemples d’énoncés à une intention. 

## <a name="an-interchangeable-phrase-list"></a>Une liste d’expressions interchangeables
Utilisez une liste d’expressions interchangeable quand la liste de mots ou d’expressions crée une classe ou un groupe. Un exemple est une liste de mois comme « Janvier », « Février », « Mars » ; ou des noms comme « John », « Mary », « Frank ».  Ces listes sont interchangeables en ce que l’énoncé serait étiqueté avec la même intention ou entité même si un autre mot de la liste d’expressions était utilisé. Par exemple, si « Afficher le calendrier de janvier » a les mêmes intentions qu’« afficher le calendrier de février », les mots doivent être sur une liste interchangeable. 

## <a name="a-non-interchangeable-phrase-list"></a>Une liste d’expressions non interchangeables
Utilisez une liste d’expressions non interchangeable pour des mots non synonymes ou des expressions qui peuvent être regroupés dans votre domaine. 

Par exemple, utilisez une liste d’expressions non interchangeables pour les mots rares, propriétaires et étrangers. LUIS peut ne pas être en mesure de reconnaître des mots rares et propriétaires, ainsi que des mots étrangers (en dehors de la culture de l’application). Le paramètre non interchangeable indique que l’ensemble de mots rares constitue une classe que LUIS doit apprendre à reconnaître, mais que ces mots ne sont ni synonymes, ni interchangeables entre eux.

## <a name="when-to-use-phrase-lists-versus-list-entities"></a>Quand utiliser des listes d’expressions et des entités de liste
Même si une liste d’expressions et des entités de liste peuvent affecter des énoncés dans toutes les intentions, chacune le fait d’une manière différente. Utilisez une liste d’expressions pour affecter le score de prédiction d’intention. Utilisez une entité de liste pour affecter l’extraction d’entité pour une correspondance texte exacte. 

### <a name="use-a-phrase-list"></a>Utiliser une liste d’expressions
Avec une liste d’expressions, LUIS peut toujours prendre en compte le contexte et généraliser afin d’identifier les éléments qui sont similaires, mais pas une correspondance exacte, à des éléments d’une liste. Si votre application LUIS doit être en mesure de généraliser et d’identifier les nouveaux éléments dans une catégorie, utilisez une liste d’expressions. 

Lorsque vous souhaitez être en mesure de reconnaître les nouvelles instances d’une entité, comme un planificateur de réunion qui doit reconnaître les noms de nouveaux contacts, ou une application d’inventaire qui doit reconnaître de nouveaux produits, utilisez un autre type d’entité appris par ordinateur comme une entité simple ou hiérarchique. Créez ensuite une liste d’expressions de mots et d’expressions qui permet à LUIS de rechercher d’autres mots similaires à l’entité. Cette liste guide LUIS afin qu’il reconnaisse des exemples de l’entité en ajoutant une précision supplémentaire à la valeur de ces mots. 

Les listes d’expression sont comme du vocabulaire spécifique à un domaine qui permet d’améliorer la qualité de compréhension des intentions et des entités. Une utilisation courante d’une liste d’expressions s’applique aux noms propres comme les noms de villes. Un nom de ville peut contenir plusieurs mots avec des traits d’union ou des apostrophes.
 
### <a name="dont-use-a-phrase-list"></a>Ne pas utiliser une liste d’expressions 
Une entité de liste définit explicitement chaque valeur qu’une entité peut prendre et n’identifie que les valeurs qui correspondent exactement. Une entité de liste peut être appropriée pour une application dans laquelle toutes les instances d’une entité sont connues et ne changent pas souvent. Les aliments dans un menu de restaurant qui change rarement en sont des exemples. Si vous avez besoin d’une correspondance texte exacte d’une entité, n’utilisez pas une liste d’expressions. 

## <a name="best-practices"></a>Bonnes pratiques
Découvrir les [meilleures pratiques](luis-concept-best-practices.md).

## <a name="next-steps"></a>Étapes suivantes

Consultez [Ajouter des fonctionnalités](luis-how-to-add-features.md) pour découvrir comment ajouter des fonctionnalités à votre application LUIS.
