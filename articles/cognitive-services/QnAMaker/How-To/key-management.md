---
title: Gestion des clés et des ressources - QnA Maker
titleSuffix: Azure Cognitive Services
description: Votre service QnA Maker porte sur deux types de clés, les clés d’abonnement et les clés de point de terminaison.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 11/26/2018
ms.author: tulasim
ms.custom: seodec18
ms.openlocfilehash: 3984ac84271728b9066aba15a582065ea3121c76
ms.sourcegitcommit: 7cd706612a2712e4dd11e8ca8d172e81d561e1db
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/18/2018
ms.locfileid: "53581757"
---
# <a name="how-to-manage-keys-in-qna-maker"></a>Comment gérer les clés dans QnA Maker

Votre service QnA Maker porte sur deux types de clés, les **clés d’abonnement** et les **clés de point de terminaison**.

![gestion des clés](../media/qnamaker-how-to-key-management/key-management.png)

1. **Clés d’abonnement** : ces clés sont utilisées pour accéder aux [API du service d’administration QnA Maker](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75ff). Ces API vous permettent de modifier votre base de connaissances.  

2. **Clés de point de terminaison** : ces clés sont utilisées pour accéder au point de terminaison de la base de connaissances afin d’obtenir une réponse à une question de l’utilisateur. En général, vous utilisez ce point de terminaison dans le code de votre bot conversationnel ou application cliente qui utilise le service QnA Maker.
 
## <a name="subscription-keys"></a>Clés d’abonnement
Vous pouvez afficher et réinitialiser vos clés d’abonnement à partir du portail Azure où la ressource QnA Maker a été créée. 
1. Accédez à la ressource QnA Maker dans le portail Azure.

    ![Liste des ressources QnA Maker](../media/qnamaker-how-to-key-management/qnamaker-resource-list.png)

2. Accédez à **Clés**.

    ![clé d’abonnement](../media/qnamaker-how-to-key-management/subscription-key.PNG)

## <a name="endpoint-keys"></a>Clés de point de terminaison

Les clés de point de terminaison peuvent être gérées à partir du [portail QnA Maker](https://qnamaker.ai).

1. Connectez-vous au [portail QnA Maker](https://qnamaker.ai) et accédez à **Paramètres de service**.

    ![clé de point de terminaison](../media/qnamaker-how-to-key-management/Endpoint-keys.png)

2. Affichez ou réinitialisez vos clés.

    ![gestionnaire de la clé de point de terminaison](../media/qnamaker-how-to-key-management/Endpoint-keys1.png)

    >[!NOTE]
    >Actualisez vos clés si vous pensez qu’elles ont été compromises. Cette opération peut nécessiter des modifications correspondantes de votre code de bot ou d’application cliente.

## <a name="next-steps"></a>Étapes suivantes

> [!div class="nextstepaction"]
> [Créer une base de connaissances dans une autre langue](./language-knowledge-base.md)
