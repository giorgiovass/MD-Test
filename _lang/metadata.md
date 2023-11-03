---
nav_title: Docs Métadonnées
page_order: 0
noindex: vrai
---
# Docs Métadonnées

> Cet article présente les options permettant d'ajouter des métadonnées aux pages Docs. Nous optimisons notre recherche en fonction du type de page et d'autres métadonnées, notamment : les [balises yaml](#yaml-tags) et les [types de page](#page-types) (basés sur des [modèles]({{site.baseurl}}/home/templates/)).

{% alert important %}

Les [balises de contenu](#content-tags) énumérées sur cette page sont actuellement en cours d'élaboration et ne peuvent pas être appliquées aux pages sans provoquer d'erreurs. Revenez plus tard pour la version finale, lorsqu'ils pourront être appliqués en toute sécurité ! Notez que vous **pouvez** actuellement utiliser la balise de contenu `platform`.

{% endalert %}

## Tags YAML

Ils sont indépendants. Si vous avez besoin de voir du contenu YAML optionnel supplémentaire basé sur "Layout", consultez les décompositions des [modèles]({{site.baseurl}}/home/templates/) et des layouts (TBD).

{% alert important %}
Une note sur la capitalisation...
<br> 
<br> 
Laissez toutes les valeurs des balises (à l'exception du contenu de la balise `description` ) en minuscules. Cela permettra d'assurer la cohérence. Il est possible que nous changions cela à l'avenir, mais pour l'instant, les minuscules sont préférables et plus faciles à rechercher et à remplacer en masse en cas de mise à jour du format.
{% endalert %}

### Tags de configuration

Ils modifient automatiquement la présentation ou la fonction d'une page.

| Balise de contenu YAML | Description | Nécessaire ? | Type de données | Valeurs disponibles |
| ----------------- | ----------- | --------- | -------------------- |
| `nav_title` | Il s'agit du titre de l'article qui apparaîtra dans la navigation de gauche du site Docs. Encapsuler dans des guillemets. | Oui, sauf si la page est `hidden`. | Chaîne de caractères. | N'importe lequel - le titre d'une page vous appartient. Il est recommandé de ne pas dépasser 30 caractères, espaces compris. |
| `page_order` | C'est l'ordre dans lequel l'article apparaîtra dans la navigation de gauche du site Docs. | Oui, sauf si la page est cachée. | Entier. | Tout nombre (y compris les décimales multiples) compris entre `1` et `100`. Vous pouvez également utiliser `1.1`, `1.2`, `1.3`, etc. pour ordonner les pages.|
| `hidden` | Indique si la page sera visible ou non dans la barre de navigation de gauche. En réglant ce paramètre sur `false`, la page n'apparaîtra pas dans les recherches (à la fois sur le site et par l'intermédiaire des fournisseurs de recherche en ligne). | Non. | Booléen. | Vous avez le choix entre `true` et `false`. |
| `config_only` | Indique si une page doit être considérée comme une page ou comme une catégorie dans le panneau de navigation de gauche. La valeur par défaut est `false`. | Non. |  Booléen. | Vous avez le choix entre `true` et `false`. |
| `permalink` | Définit l'url de la page. Par exemple : `permalink: /this_page_name/` définira l'URL de la page à `https://www.braze.com/docs/this_page_name/`. | Non, sauf si la page est `hidden`. | Chaîne de caractères. | N'importe lequel - cette URL est à votre discrétion. Encapsuler dans des barres obliques (`/`). |
| `layout` | Définit des caractéristiques spécifiques sur la page qui s'alignent sur les mises en page développées. Defaults est une page normale. | Non. | Chaîne de caractères. | Si vous ne définissez pas ce paramètre, une page de contenu normale sera créée par défaut. Vous avez le choix entre : `api_page`, `dev_guide`, `featured_video`, `featured`, `glossary_page`, `blank_config`, et `redirect`. Il en existe d'autres, mais ils sont principalement destinés à des usages internes et de configuration. |
| `hide_toc` | Détermine si la table des matières située à droite de la page est incluse ou non. | Non. | Booléen. | Vous avez le choix entre `true` et `false`. |
| `noindex` | Détermine si l'article sera affiché dans Algolia et dans les recherches Google. La valeur par défaut est `false`, sauf si la balise YAML `hidden` est définie comme `true`. | Non. | Booléen. | `true` ou `false`. | 
{: .reset-td-br-1 .reset-td-br-2 .reset-td-br-3  .reset-td-br-4}

### Tags de contenu
Ceux-ci contribueront au référencement externe et interne, en informant le contenu et la mise en forme des pages, ainsi que d'autres structures basées sur le contenu.

| Balise de contenu YAML | Description  | Nécessaire ? | Exclusif ou multiple ? | Type de données | Valeurs disponibles |
| ----------------- | ----------- | --------- | ---------------------------------- | --------- | ---------------- |
| `description` | Description de la page qui apparaîtra dans les recherches en ligne. Encapsuler dans des guillemets. | Oui. | Exclusif jusqu'à 160 caractères. | Chaîne de caractères. | N'importe laquelle - la description d'une page est laissée à votre discrétion. Nous recommandons de ne pas dépasser 3 phrases. <br> <br> Modèle : `This {page_type} {lists, describes, walks you through} {topic or task} for {platform and/or channel} using {tool}.` Bien que la formulation exacte puisse varier, elle doit inclure au moins le type de page, ce que la page vise à faire (par exemple, elle "vous guidera dans l'exécution de la tâche indiquée" ou "vous apprendra à lire un certain rapport" ou "décrira les exigences d'une certaine intégration de partenaire"). <br> <br> Exemple : `This glossary lists all of the terms you need to know while onboarding with Braze and preparing for the Integration Phase.` Ou `This reference article describes the different kinds of Canvas Steps and how they affect iOS or Android push campaigns.` Ou même `This solutions article will walk you through a custom integration.` |
| `page_type` | Type de page, déterminé par les modèles de page. Informer sur le formatage et le contenu. | Oui. | Exclusif ; un seul peut être utilisé par page. | Chaîne de caractères. | Voir [Types de pages](#page-types). |
| `platform` | Note les plateformes (iOS, Android, etc.) auxquelles l'article est associé. | Non, sauf sur une page du guide de développement.  | Plusieurs valeurs peuvent être utilisées. | Chaîne de caractères. | Toutes les plateformes sur lesquelles Braze s'intègre : `iOS`, `Android`, `Web`, `API`, et tous les SDKs de wrapper. |
| `channel` | Note les canaux de messagerie (push, messages in-app, etc.) auxquels l'article est associé. | Non, sauf si le contenu mentionne une ou plusieurs chaînes spécifiques. | Plusieurs valeurs peuvent être utilisées. | Chaîne de caractères. | N'importe lequel des canaux de messagerie que Braze envoie : `content cards`, `email`, `news feed`, `in-app messages`, `push`, `sms`, et `webhooks`.|
| `tool` | Indique les outils d'engagement (Canvas, campagnes, etc.) auxquels l'article est associé. | Oui. | Plusieurs valeurs peuvent être utilisées. | Chaîne de caractères. | L'un des sites de Braze tools: `dashboard`, `docs`, `canvas`, `campaigns`, `segments`, `templates`, `media`, `location`, `currents`, `reports`. |
{: .reset-td-br-1 .reset-td-br-2 .reset-td-br-3  .reset-td-br-4}

### Valeurs d'étiquettes multiples
Il peut arriver qu'une balise de contenu pour une page puisse être classée avec plusieurs valeurs (par exemple, un article peut parler à la fois de Canvas et de campagnes, ou couvrir une intégration personnalisée pour Android et iOS).

Vous pouvez formuler cela de la manière suivante : 
```
key:
  - string1
  - string2      
  - string3
  - string4
  - string5
  - string6
```
{% alert important %}
Notez qu'il ne peut y avoir qu'une seule valeur `page_type` pour la page. Une page ne peut pas être à la fois `reference` et `glossary`. Les différents types de pages ont pour but de limiter le champ d'application et l'objectif de chaque article.
{% endalert %}

### Exemple de YAML

Le début de chaque page markdown doit commencer par une section de yaml pour définir la page.

```html
---
nav_title: "Docs Metadata"
page_order: 0

description: "This page walks through the options for adding metadata to Docs pages. We optimize our search based on page type and other bits of metadata. This is a great resource for contributors via our GitHub page."
page_type: reference
tool: 
  - docs
  - dashboard
---
```


## Types de pages

Pour les appliquer, assurez-vous que votre paramètre yaml pour les types de page est : `page_type:`

Par exemple : `page_type: glossary`

| Type de page <br> <br> Étiquette de type de page | Description | Modèles disponibles |
| ------------- | ------------- | ------------- |
| Page du glossaire <br> <br> `glossary`| Cette page fournit une description consultable de certains termes ou éléments (métriques, mots à connaître, points de terminaison de l'API, etc.) | [Glossaire de l'API ou du code]({{site.baseurl}}/home/templates/api_glossary/) <br> <br> [Glossaire général]({{site.baseurl}}/home/templates/glossary_test_page/)
| Page de solution <br> <br> `solution` | Un article de dépannage ou d'"options" qui guide les utilisateurs vers une solution à un problème ou vers des étapes permettant de résoudre ou de réduire un problème. | [Article d'aide]({{site.baseurl}}/home/templates/help_article_template/) <br> <br> [Article de solution]({{site.baseurl}}/home/templates/solution/) |
| Page de référence <br> <br> `reference` | Un article qui explique un concept et contient des informations spécifiques sur les processus techniques et le contenu des produits. (étapes du canevas, segmentation, caractéristiques spécifiques du produit, etc.) | [Article de référence avec vidéo]({{site.baseurl}}/home/templates/reference_vide/) <br> <br> [Article de référence]({{site.baseurl}}/home/templates/reference/) |
| Page de tutoriel <br> <br> `tutorial` | Une présentation générale d'un concept pédagogique. Doit contenir des connaissances pratiques. Se concentre sur un seul sujet (par exemple, comment créer une campagne, comment créer un Canvas, etc.) Article axé sur un objectif ou une tâche, qui explique étape par étape comment résoudre un problème spécifique (comment cibler des utilisateurs spécifiques, comment segmenter en fonction de la localisation, etc.) | [Tutoriel Article avec vidéo]({{site.baseurl}}/home/templates/tutorial_video/) <br> <br> [Tutoriel Article]({{site.baseurl}}/home/templates/tutorial/) <br> <br> [Article sur les cas d'utilisation avec vidéo]({{site.baseurl}}/home/templates/use_case_video/) <br> <br> [Article sur les cas d'utilisation]({{site.baseurl}}/home/templates/use_case/) |
| Page d'atterrissage <br> <br> `landing` | La page propose une sélection d'options dans une certaine section, ainsi qu'une description ou une vue d'ensemble de cette section. | [Page d'atterrissage à section unique avec FA Icons]({{site.baseurl}}/home/templates/landing_single/) <br> <br> [Page d'atterrissage à section unique utilisant des images]({{site.baseurl}}/home/templates/landing_images/) <br> <br> [Page d'atterrissage à sections multiples utilisant les icônes FA]({{site.baseurl}}/home/templates/landing_multiple/) <br> <br> [Page d'atterrissage à sections multiples utilisant des images]({{site.baseurl}}/home/templates/landing_multiple_images/)
| Page des partenaires <br> <br> `partner` | Une page qui combine plusieurs des types de pages précédents en une seule page. Ces pages décrivent un partenaire, ses avantages, comment l'intégrer, puis comment utiliser cette intégration et les meilleures pratiques associées à cette utilisation. | [Page partenaire avec vidéo]({{site.baseurl}}/home/templates/partner_page_template_video/) <br> <br> [Page des partenaires]({{site.baseurl}}/home/templates/partner_page_template/) |
| Mises à jour et notes de version <br> <br> `update` | Une page qui répertorie les mises à jour d'un produit ou d'un kit de développement logiciel (SDK) les unes après les autres. Une simple mise à jour d'une page plus importante ou une page consacrée à une nouvelle fonctionnalité **n'est pas** considérée comme un type de page `update`. | Voir les pages de notes de version et les pages de modifications du SDK. | 
{: .reset-td-br-1 .reset-td-br-2 .reset-td-br-3}

Type de page potentielle : Meilleures pratiques
