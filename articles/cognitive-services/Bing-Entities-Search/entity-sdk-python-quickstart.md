---
title: 'Démarrage rapide : Kit de développement logiciel (SDK) pour Recherche d’entités Bing, Python'
titlesuffix: Azure Cognitive Services
description: Configuration de l’application console du Kit de développement logiciel (SDK) pour Recherche d’entités Bing.
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-entity-search
ms.topic: quickstart
ms.date: 02/15/2018
ms.author: v-gedod
ms.openlocfilehash: 4e79fdbf465c04075320b71b5471b196e603d046
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/26/2018
ms.locfileid: "52316002"
---
# <a name="quickstart-bing-entity-search-sdk-with-python"></a>Démarrage rapide : Kit de développement logiciel (SDK) pour Recherche d’entités Bing avec Python

Le Kit de développement logiciel (SDK) Recherche d’entités fournit les fonctionnalités de l’API REST pour les requêtes web et l’analyse des résultats.

Le [code source des exemples du Kit de développement logiciel (SDK) Python pour Recherche d’entités Bing](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples/blob/master/samples/search/entity_search_samples.py) est disponible sur GitHub.

## <a name="application-dependencies"></a>Dépendances de l’application
Obtenez une [clé d’accès Cognitive Services](https://azure.microsoft.com/try/cognitive-services/) sous **Recherche**.  Consultez également [Tarification Cognitive Services - API Recherche Bing](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/).

Si ce n’est déjà fait, installez Python. Le Kit de développement logiciel (SDK) est compatible avec Python 2.7, 3.3, 3.4, 3.5 et 3.6.

Il est généralement conseillé d’utiliser un [environnement virtuel](https://docs.python.org/3/tutorial/venv.html) pour le développement Python. Installez et initialisez l’environnement virtuel avec le [module venv](https://pypi.python.org/pypi/virtualenv). Vous devez installer virtualenv pour Python 2.7.
```
python -m venv mytestenv
```
Installez les dépendances du Kit de développement logiciel (SDK) pour Recherche d’entités :
```
cd mytestenv
python -m pip install azure-cognitiveservices-search-entitysearch
```
## <a name="entity-search-client"></a>Client Recherche d’entités
Obtenez une [clé d’accès Cognitive Services](https://azure.microsoft.com/try/cognitive-services/) sous *Recherche*. Ajoutez des importations :
```
from azure.cognitiveservices.search.entitysearch import EntitySearchAPI
from azure.cognitiveservices.search.entitysearch.models import Place, ErrorResponseException
from msrest.authentication import CognitiveServicesCredentials

subscription_key = "YOUR-SUBSCRIPTION-KEY"
```
Créez une instance de `CognitiveServicesCredentials`. Instanciez le client :
```
client = EntitySearchAPI(CognitiveServicesCredentials(subscription_key))
```
Recherchez une entité (Gibralter) et affichez une brève description :
```
entity_data = client.entities.search(query="Gibralter")

if entity_data.entities.value:
    # find the entity that represents the dominant one

    main_entities = [entity for entity in entity_data.entities.value
                     if entity.entity_presentation_info.entity_scenario == "DominantEntity"]

    if main_entities:
        print('Searched for "Gibralter" and found a dominant entity with this description:')
        print(main_entities[0].description)
    else:
        print("Couldn't find main entity Gibralter!")

else:
    print("Didn't see any data..")

```
Recherchez et levez l’ambiguïté des résultats pour une requête ambiguë (William Gates).
```
def handling_disambiguation(subscription_key):

    client = EntitySearchAPI(CognitiveServicesCredentials(subscription_key))

    try:
        entity_data = client.entities.search(query="william gates")

        if entity_data.entities.value:
            # find the entity that represents the dominant one

            main_entities = [entity for entity in entity_data.entities.value
                             if entity.entity_presentation_info.entity_scenario == "DominantEntity"]

            disambig_entities = [entity for entity in entity_data.entities.value
                                 if entity.entity_presentation_info.entity_scenario == "DisambiguationItem"]

            if main_entities:
                main_entity = main_entities[0]
                type_hint = main_entity.entity_presentation_info.entity_type_display_hint
                
                print('Searched for "William Gates" and found a dominant entity {}with this description:'.format(
                    '"with type hint "{}" '.format(type_hint) if type_hint else ''))
                print(main_entity.description)
            else:
                print("Couldn't find a reliable dominant entity for William Gates!")
        
            if disambig_entities:
                print("\nThis query is pretty ambiguous and can be referring to multiple things. Did you mean one of these:")
                suggestions = []
                for disambig_entity in disambig_entities:
                    suggestions.append("{} the {}".format(disambig_entity.name, disambig_entity.entity_presentation_info.entity_type_display_hint))
                print(", or ".join(suggestions))
            else:
                print("We didn't find any disambiguation items for William Gates, so we must be certain what you're talking about!")

        else:
            print("Didn't see any data..")

    except Exception as err:
        print("Encountered exception. {}".format(err))

```
Recherchez un magasin (Microsoft Store) et affichez son numéro de téléphone.
```
def store_lookup(subscription_key):

    client = EntitySearchAPI(CognitiveServicesCredentials(subscription_key))

    try:
        entity_data = client.entities.search(query="microsoft store")

        if entity_data.places.value:

            store = entity_data.places.value[0]

            # Some local entities will be places, others won't be. Depending on what class contains the data you want, you can check 
            # using isinstance one of the class, or try to get the attribute and handle the exception (EAFP principle).
            # The recommended Python way is usually EAFP (see https://docs.python.org/3/glossary.html)
            # In this case, the item being returned is technically a store, but the Place schema has the data we want (telephone)

            # Pythonic approach : EAFP "Easier to ask for forgiveness than permission"
            try:
                telephone = store.telephone
                print('Searched for "Microsoft Store" and found a store with this phone number:')
                print(telephone)
            except AttributeError:
                print("Couldn't find a place!")

            # More cross language approach
            if isinstance(store, Place):
                print('Searched for "Microsoft Store" and found a store with this phone number:')
                print(store.telephone)
            else:
                print("Couldn't find a place!")


        else:
            print("Didn't see any data..")

    except Exception as err:
        print("Encountered exception. {}".format(err))

```
Recherchez une liste de restaurants (Seattle restaurants) et afficher leurs noms et numéros de téléphone.
```
def multiple_restaurant_lookup(subscription_key):

    client = EntitySearchAPI(CognitiveServicesCredentials(subscription_key))

    try:
        restaurants = client.entities.search(query="seattle restaurants")

        if restaurants.places.value:

            # get all the list items that relate to this query
            list_items = [entity for entity in restaurants.places.value
                          if entity.entity_presentation_info.entity_scenario == "ListItem"]

            if list_items:

                suggestions = []
                for place in list_items:
                    # Pythonic approach : EAFP "Easier to ask for forgiveness than permission"
                    # see https://docs.python.org/3/glossary.html
                    try:
                        suggestions.append("{} ({})".format(place.name, place.telephone))
                    except AttributeError:
                        print("Unexpectedly found something that isn\'t a place named '{}'", place.name)

                print("Ok, we found these places: ")
                print(", ".join(suggestions))

            else:
                print("Couldn't find any relevant results for \"seattle restaurants\"")

        else:
            print("Didn't see any data..")

    except Exception as err:
        print("Encountered exception. {}".format(err))

```
Déclenchez une demande incorrecte et lisez la réponse d’erreur.
```
def error(subscription_key):

    client = EntitySearchAPI(CognitiveServicesCredentials(subscription_key))

    try:
        entity_data = client.entities.search(query="satya nadella", market="no-ty")
    except ErrorResponseException as err:
        # The status code of the error should be a good indication of what occurred. However, if you'd like more details, you can dig into the response.
        # Please note that depending on the type of error, the response schema might be different, so you aren't guaranteed a specific error response schema.
                    
        print("Exception occurred, status code {} with reason {}.\n".format(err.response.status_code, err))

        # if you'd like more descriptive information (if available)
        if err.error.errors:
            print("This is the errors I have:")
            for error in err.error.errors:
                print("Parameter \"{}\" has an invalid value \"{}\". SubCode is \"{}\". Detailed message is \"{}\"".format(error.parameter, error.value, error.sub_code, error.message))
        else:
            print("There was no details on the error.")
```
## <a name="next-steps"></a>Étapes suivantes

[Exemples du SDK Python pour Cognitive Services](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples)

