---
title: Créer des insights vidéo à partir de vidéos existantes
titlesuffix: Azure Media Services
description: Cette rubrique vous montre comment créer et publier des insights vidéo à partir de fichiers vidéo existants.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.topic: article
ms.date: 11/19/2018
ms.author: juliako
ms.openlocfilehash: 7ba3fab514729b7b5645254fa9d34dd42b6718d5
ms.sourcegitcommit: beb4fa5b36e1529408829603f3844e433bea46fe
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/22/2018
ms.locfileid: "52291810"
---
# <a name="tutorial-create-highlights-from-existing-videos"></a>Tutoriel : Créer des points importants à partir de vidéos existantes

Cette rubrique vous montre comment créer et publier des insights vidéo à partir d’autres vidéos.

1. Accédez au site web [Video Indexer](https://www.videoindexer.ai/) et connectez-vous.
2. Recherchez une vidéo à partir de laquelle vous souhaitez créer vos insights vidéo.
3. Appuyez sur **Lecture**.

    La page affiche les insights résumés de la vidéo. 

    ![Insights](./media/video-indexer-create-new/video-indexer-summarized-insights.png)

3. Appuyez sur le bouton **Modifier**.

    Cette page affiche la répartition complète d’une vidéo. La répartition est divisée en blocs. Les blocs sont là pour simplifier le parcours des données. Par exemple, un bloc peut être défini par un changement d’intervenant ou une pause prolongée. Vous pouvez créer votre propre playlist contenant uniquement les lignes souhaitées. Pour afficher uniquement certaines parties de la vidéo source, vous pouvez filtrer par rubriques/mots clés, sentiments, personnes, intervenants. Vous pouvez choisir d’afficher uniquement la transcription ou la reconnaissance optique des caractères (OCR) de la vidéo.    

    ![Insights](./media/video-indexer-create-new/video-indexer-create-new-playlist.png)

4. Créez votre playlist.

    Pour ajouter ou supprimer des lignes de votre playlist, appuyez sur **+**/**-**.

5. Affichez un aperçu de votre playlist.

    Une fois que vous avez terminé la création de la playlist, appuyez sur **Aperçu**.
6. Publiez la playlist.

    Après avoir affiché un aperçu de la playlist, vous pouvez la publier.

    Une fois la playlist publiée, elle est ajoutée à la liste de vos insights vidéo.


## <a name="next-steps"></a>Étapes suivantes 

Une fois votre nouvelle playlist créée, vous pouvez continuer de la traiter, comme décrit dans l’une des rubriques suivantes : 

- [Traiter du contenu avec l’API REST de Video Indexer](video-indexer-use-apis.md)
- [Incorporer des widgets visuels dans votre application](video-indexer-embed-widgets.md)

## <a name="see-also"></a>Voir aussi

[Présentation de Video Indexer](video-indexer-overview.md) 
