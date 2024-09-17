---
page_order: 0.5
nav_title: Page de test vidéo
layout: featured_video
video_id: XY5uXoKIvFY
video_source: youtube
noindex: vrai
---

# Collecte de données sur les utilisateurs

Avant d'achever la mise en œuvre de Braze, veillez à ce que votre équipe de marketing et votre équipe de développement discutent de vos objectifs de marketing. Lorsque vous décidez de ce que vous voulez suivre et comment vous voulez le faire avec Braze, il est utile de prendre en compte ces objectifs et de travailler en amont. Vous trouverez un exemple de ce processus dans notre cas d'une [application de taxi/de covoiturage][16] à la fin de ce guide.

Ce guide de bonnes pratiques vous aidera à comprendre exactement ce que Braze considère comme un événement personnalisé ou un attribut personnalisé.

## Données collectées automatiquement

Les événements et attributs suivants sont capturés et mis à jour automatiquement par le SDK Braze dans le cadre des points de données `Session Start` et `Session End`, ou par le backend Braze. Il n'est pas nécessaire de les enregistrer séparément en tant qu'événements ou attributs personnalisés.

#### Informations sur l'utilisation
- Première application utilisée (date)
- Dernière application utilisée (date)
- Nombre total de sessions (Integer)
- Nombre d'éléments de retour d'information (Entier)
- Nombre de sessions au cours des Y derniers jours (nombre entier et date)
- Email disponible (booléen)
- Nombre de vues du fil d'actualité (Entier)

#### Campagne de reciblage
- Dernier message reçu (date)
- Dernière campagne de courrier électronique reçue (date)
- Dernière campagne de push reçue (date)
- Dernier fil d'actualité consulté (date)
- Carte cliquée (Integer)
- Message reçu de la campagne
  - Ce filtre vous permet de cibler les utilisateurs en fonction du fait qu'ils ont (ou n'ont pas) reçu une campagne précédente.
- Message reçu d'une campagne avec étiquette
  - Ce filtre vous permet de cibler les utilisateurs en fonction du fait qu'ils ont (ou n'ont pas) reçu une campagne qui comporte actuellement une étiquette.
- Campagne de reciblage
  - Ce filtre vous permet de cibler les utilisateurs en fonction du fait qu'ils ont ou non ouvert ou cliqué sur un e-mail, un push ou un slideup spécifique dans le passé.

#### Informations sur l'appareil
- Lieu disponible (booléen)
- Emplacement le plus récent (si l'autorisation de localisation est accordée à votre application)
- Poussée activée (booléen)
- Locale de l'appareil
- Langue (extrait de Device Locale)
- Pays (premier extrait de l'adresse IP). Si cette information n'est pas disponible, elle est tirée de Device Locale)
- Version la plus récente de l'application
- Modèle d'appareil
- Version du système d'exploitation de l'appareil
- Résolution de l'appareil
- Dispositif Opérateur sans fil
- Fuseau horaire de l'appareil
- Identifiant de l'appareil
- Désinstallé (date et booléen)

## Événements sur mesure

Les événements personnalisés sont des actions entreprises par vos utilisateurs ; ils conviennent le mieux pour suivre les interactions de grande valeur entre les utilisateurs et votre application. L'enregistrement d'un événement personnalisé peut déclencher un nombre quelconque de campagnes de suivi avec des délais configurables, et permet les filtres de segmentation suivants autour de la récurrence et de la fréquence de cet événement :

| Options de segmentation | Filtre déroulant | Options d'entrée |
| ---------------------| --------------- | ------------- |
| Vérifier si l'événement personnalisé s'est produit **plus de X** fois | **PLUS QUE** | **INTEGER** |
| Vérifier si l'événement personnalisé s'est produit **moins de X** fois | **MOINS DE** | **INTEGER** |
| Vérifier si l'événement personnalisé s'est produit **exactement X** fois. | **EXACTEMENT** | **INTEGER** |
| Vérifier si l'événement personnalisé s'est produit pour la dernière fois **après la date X** | **APRÈS** | **DATE** |
| Vérifier si l'événement personnalisé s'est produit pour la dernière fois **avant la date X** | **AVANT** | **DATE** |
| Vérifier si l'événement personnalisé s'est produit pour la dernière fois **il y a plus de X jours** | **PLUS QUE** | **NOMBRE DE JOURS ANCIENS** (Entier) positif) |
| Vérifier si l'événement personnalisé s'est produit pour la dernière fois **il y a moins de X jours** | **MOINS DE** | **NOMBRE DE JOURS ANCIENS** (Entier) positif) |
| Vérifier si l'événement personnalisé s'est produit **plus de X (Max = 50)** fois. | **PLUS QUE** | au cours des **Y** derniers **jours (Y = 1,3,7,14,21,30)** |
| Vérifier si l'événement personnalisé s'est produit **moins de X (Max = 50)** fois. | **MOINS DE** | au cours des **Y** derniers **jours (Y = 1,3,7,14,21,30)** |
| Vérifier si l'événement personnalisé s'est produit **exactement X (Max = 50)** fois. | **EXACTEMENT** | au cours des **Y** derniers **jours (Y = 1,3,7,14,21,30)** |
{: .reset-td-br-1 .reset-td-br-2 .reset-td-br-3}

Braze note le nombre de fois que ces événements se sont produits ainsi que la dernière fois qu'ils ont été effectués par chaque utilisateur pour la segmentation. Sur la page d'analyse des événements personnalisés, vous pouvez visualiser la fréquence globale de chaque événement personnalisé, ainsi que par segment dans le temps pour une analyse plus détaillée. Ceci est particulièrement utile pour voir comment vos campagnes ont affecté l'activité des événements personnalisés en regardant les lignes grises que Braze superpose sur les séries temporelles pour indiquer la dernière fois qu'une campagne a été envoyée.

![custom_event_analytics_example.png][8]

>  [L'incrémentation des attributs personnalisés][10] peut être utilisée pour conserver un compteur sur une action de l'utilisateur similaire à un événement personnalisé. Cependant, vous ne pourrez pas visualiser les données d'attributs personnalisés dans une série temporelle. Les actions des utilisateurs qui n'ont pas besoin d'être analysées dans des séries temporelles doivent être enregistrées par cette méthode.

### Stockage d'événements sur mesure

Toutes les données du profil de l'utilisateur (événements personnalisés, attributs personnalisés, données personnalisées) sont stockées tant que ces profils sont actifs. Les propriétés des événements sont conservées pendant soixante (60) jours à des fins de segmentation.

### Propriétés des événements personnalisés

Avec les propriétés d'événements personnalisés, Braze vous permet de définir des propriétés sur des événements et des achats personnalisés. Ces propriétés peuvent ensuite être utilisées pour mieux qualifier les conditions de déclenchement, accroître la personnalisation des messages et générer des analyses plus sophistiquées grâce à l'exportation de données brutes. Les valeurs des propriétés peuvent être des chaînes de caractères, des nombres, des booléens ou des objets de type Date. Cependant, les valeurs des propriétés ne peuvent pas être des objets de type tableau.

Par exemple, si une application de commerce électronique souhaite envoyer un message à un utilisateur lorsqu'il abandonne son panier, elle pourrait en outre améliorer son public cible et permettre une personnalisation accrue de la campagne en ajoutant une propriété d'événement personnalisée de la "valeur du panier" des paniers des utilisateurs.

![customEventProperties.png][18]

Les propriétés des événements personnalisés peuvent également être utilisées pour la personnalisation dans le modèle de messagerie. Toute campagne utilisant [Action-Based Delivery][19] avec un événement déclencheur peut utiliser les propriétés personnalisées de cet événement pour la personnalisation de la messagerie. Si une application de jeu souhaite envoyer un message aux utilisateurs qui ont terminé un niveau, elle peut personnaliser le message en y ajoutant une propriété correspondant au temps qu'il a fallu aux utilisateurs pour terminer ce niveau. Dans cet exemple, le message est personnalisé pour trois segments différents à l'aide de la [logique conditionnelle][18].  La propriété d'événement personnalisée appelée ``time_spent``, peut être incluse dans le message en appelant ``{% raw %} {{event_properties.${time_spent}}} {% endraw %}``.

{% raw %}
```liquid
{% if {{event_properties.${time_spent}}} < 600 %}
Congratulations on beating that level so fast! Check out our online portal where you can play against top players fromm around the world!
{% elsif {{event_properties.${time_spent}}} < 1800 %}
Don't forget to visit the town store between levels to upgrade your tools.
{% else %}
Talk to villagers for essential tips on how to beat levels!
{% endif %}
```
{% endraw %}

Les propriétés d'événements personnalisés sont conçues pour vous aider à personnaliser vos messages ou à élaborer des campagnes de livraison basées sur des actions granulaires. Si vous souhaitez créer des segments basés sur la récurrence et la fréquence des propriétés des événements, contactez votre responsable de la réussite des clients, car cela peut entraîner des coûts de données supplémentaires.

## Attributs personnalisés
Les attributs personnalisés sont idéaux pour stocker des attributs concernant vos utilisateurs ou des informations sur des actions de faible valeur au sein de votre application. N'oubliez pas que nous ne stockons pas d'informations sur les séries temporelles pour les attributs personnalisés. Vous n'obtiendrez donc pas de graphiques basés sur ces attributs, comme dans l'exemple précédent pour les événements personnalisés.

### Stockage d'attributs personnalisés

Toutes les données du profil de l'utilisateur (événements personnalisés, attributs personnalisés, données personnalisées) sont stockées tant que ces profils sont actifs. Les propriétés des événements sont conservées pendant soixante (60) jours à des fins de segmentation.

### Types de données d'attributs personnalisés
Les attributs personnalisés sont des outils extraordinairement flexibles qui permettent un ciblage efficace. Les types de données suivants peuvent être stockés en tant qu'attributs personnalisés :

#### Chaînes (caractères alphanumériques)
Les attributs de type chaîne sont utiles pour stocker les données de l'utilisateur, telles qu'une marque préférée, un numéro de téléphone ou une dernière chaîne de recherche dans votre application. Les attributs des chaînes de caractères peuvent comporter jusqu'à 255 caractères.

| Options de segmentation | Filtre déroulant | Options d'entrée |
| ---------------------| --------------- | ------------- |
| Vérifier si l'attribut string **correspond exactement à la** chaîne saisie| **ÉGAUX** | **CHAÎNE** |
| Vérifier si l'attribut string **correspond partiellement à** une chaîne saisie **OU** Expression régulière | **MATCHES REGEX** | **CHAÎNE** **OU** **EXPRESSION RÉGULIÈRE** |
| Vérifier si l'attribut string **ne correspond pas partiellement à** une chaîne saisie **OU** Expression régulière | **NE CORRESPOND PAS À UNE EXPRESSION RATIONNELLE*** | **CHAÎNE** **OU** **EXPRESSION RÉGULIÈRE** |
| Vérifier si l'attribut string **ne correspond pas à** une chaîne saisie| **N'EST PAS ÉGAL** | **CHAÎNE** |
| Vérifier si l'attribut string **existe** dans le profil d'un utilisateur | **N'EST PAS EN BLANC** | **N/A** |
| Vérifier si l'attribut string **n'existe pas** dans le profil d'un utilisateur | **EST BLANC** | **N/A** |
{: .reset-td-br-1 .reset-td-br-2 .reset-td-br-3}

{% alert important %}
\* Lors de la segmentation à l'aide du filtre **DOES NOT MATCH REGEX**, il est nécessaire qu'il existe déjà un attribut personnalisé avec une valeur assignée dans ce profil d'utilisateur. Braze suggère d'utiliser la logique "OR" pour vérifier si un attribut personnalisé est vide afin de s'assurer que les utilisateurs sont correctement ciblés.
{% endalert %}

{% alert tip %}
Pour en savoir plus sur l'utilisation de notre filtre d'expressions régulières, consultez cette documentation sur les [expressions régulières compatibles avec Perl (PCRE)](http://www.regextester.com/pregsyntax.html).
<br>
Plus de ressources sur les expressions rationnelles :
- [Regex avec Braze]({{site.baseurl}}/user_guide/engagement_tools/segments/regex/)
- [Débogueur et testeur de regex](https://regex101.com/)
- [Tutoriel sur les expressions rationnelles](https://medium.com/factory-mind/regex-tutorial-a-simple-cheatsheet-by-examples-649dc1c3f285)
{% endalert %}

#### Tableaux
Les attributs de type tableau sont utiles pour stocker des listes d'informations relatives à vos utilisateurs. Par exemple, le stockage des 100 derniers contenus regardés par un utilisateur dans un tableau permettrait une segmentation spécifique des intérêts.

Les tableaux d'attributs personnalisés sont des ensembles unidimensionnels ; les tableaux multidimensionnels ne sont pas pris en charge. **L'ajout d'un élément à un tableau d'attributs personnalisés ajoute l'élément à la fin du tableau, sauf s'il est déjà présent, auquel cas il est déplacé de sa position actuelle à la fin du tableau.** Par exemple, si un tableau ['hotdog', 'hotdog', 'hotdog', 'pizza'] est importé, il apparaîtra dans l'attribut tableau sous la forme ['hotdog', 'pizza'], car nous ne prenons en charge que les valeurs uniques.

Si le tableau contient le nombre maximum d'éléments, le premier élément est rejeté et le nouvel élément est ajouté à la fin. Voici un exemple de code illustrant le comportement des tableaux dans le SDK web :

```
var abUser = appboy.getUser();
// initialize array for this user, assuming max length of favorite_foods is set to 4.
abUser.setCustomUserAttribute('favorite_foods', ['pizza', 'wings', 'pasta']); // => ['pizza', 'wings', 'pasta']
abUser.addToCustomAttributeArray('favorite_foods', 'fries'); // => ['pizza', 'wings', 'pasta', 'fries']
abUser.addToCustomAttributeArray('favorite_foods', 'pizza'); // => ['wings', 'pasta', 'fries', 'pizza']
abUser.addToCustomAttributeArray('favorite_foods', 'ice cream'); // => ['pasta', 'fries', 'pizza', 'ice cream']

```

Le nombre maximal d'éléments dans les tableaux d'attributs personnalisés est fixé par défaut à 25. Le nombre maximum de réseaux individuels peut être augmenté jusqu'à 100 dans le tableau de bord de Braze, sous **Data Settings** > **Custom Attributes**. Si vous souhaitez que ce maximum soit augmenté, contactez votre responsable du service clientèle. Les tableaux dépassant le nombre maximal d'éléments seront tronqués pour contenir le nombre maximal d'éléments.

| Options de segmentation | Filtre déroulant | Options d'entrée |
| ---------------------| --------------- | ------------- |
| Vérifier si l'attribut du tableau **contient une valeur qui correspond exactement à la** valeur saisie.| **INCLUT LA VALEUR** | **CHAÎNE** |
| Vérifier si l'attribut du tableau **ne contient pas une valeur qui correspond exactement à la** valeur saisie.| **NE COMPREND PAS DE VALEUR** | **CHAÎNE** |
| Vérifier si l'attribut du tableau **contient une valeur qui correspond partiellement à** une valeur saisie **OU** Expression régulière | **MATCHES REGEX** | **CHAÎNE** **OU** **EXPRESSION RÉGULIÈRE** |
| Vérifier si l'attribut du tableau **a une valeur** | **A UNE VALEUR** | **N/A** |
| Vérifier si l'attribut du tableau **est vide** | **EST VIDE** | **N/A** |
{: .reset-td-br-1 .reset-td-br-2 .reset-td-br-3}

>  Nous utilisons les [expressions régulières compatibles avec Perl (PCRE)][11].

#### Dates
Les attributs de date sont utiles pour enregistrer la dernière fois qu'une action spécifique a été effectuée, afin que vous puissiez proposer à vos utilisateurs des messages de réengagement spécifiques au contenu.

>  La dernière date à laquelle un événement personnalisé ou un événement d'achat s'est produit est automatiquement enregistrée et ne doit pas être enregistrée en double par le biais d'un attribut de date personnalisé.

Les filtres de date utilisant des dates relatives (par exemple, il y a plus d'un jour, il y a moins de deux jours) mesurent 1 jour comme 24 heures. Toute campagne menée à l'aide de ces filtres inclura tous les utilisateurs par tranches de 24 heures. Par exemple, l'expression "dernière utilisation de l'application il y a plus d'un jour" permet de capturer tous les utilisateurs qui ont "utilisé l'application pour la dernière fois il y a plus de 24 heures" à partir de l'heure exacte à laquelle la campagne est lancée. Il en va de même pour les campagnes dont les dates sont plus longues - ainsi, cinq jours après l'activation correspondront aux 120 heures précédentes.

| Options de segmentation | Filtre déroulant | Options d'entrée |
| ---------------------| --------------- | ------------- |
| Vérifier si l'attribut date **est antérieur à** une **date sélectionnée**| **AVANT** | **SÉLECTEUR DE DATE DU CALENDRIER** |
| Vérifier si l'attribut date **est postérieur à** une **date sélectionnée**| **APRÈS** | **SÉLECTEUR DE DATE DU CALENDRIER** |
| Vérifier si l'attribut date est **antérieur de** plus de **X** **jours**. | **PLUS QUE** | **NOMBRE DE JOURS ÉCOULÉS** |
| Vérifier si l'attribut date est **inférieur à X** **jours**.| **MOINS DE** | **NOMBRE DE JOURS ÉCOULÉS** |
| Vérifier si l'attribut date se **situe dans** plus de **X** **jours dans le futur** | **DANS PLUS DE** | **NOMBRE DE JOURS À VENIR** |
| Vérifier si l'attribut date est **inférieur à X** **jours dans le futur** | **EN MOINS DE** | **NOMBRE DE JOURS À VENIR**  |
| Vérifier si l'attribut date **existe** dans le profil d'un utilisateur | **EXISTE** | **N/A** |
| Vérifier si l'attribut date n **'existe pas** dans le profil d'un utilisateur | **N'EXISTE PAS** | **N/A** |
{: .reset-td-br-1 .reset-td-br-2 .reset-td-br-3}

#### Entiers (standard et incrémentaux) et décimaux (flottants/doubles) {#integers}
Les attributs numériques ont une grande variété de cas d'utilisation. Les attributs personnalisés de type entier incrémentiel sont utiles pour enregistrer le nombre de fois qu'une action ou un événement donné s'est produit. Les nombres entiers et décimaux standard ont toutes sortes d'utilisations, par exemple : (enregistrement de la pointure, du tour de taille, du nombre de fois qu'un utilisateur a consulté une certaine caractéristique du produit ou une certaine catégorie).
>  L'argent dépensé dans l'application ne doit pas être enregistré par cette méthode. Elle doit plutôt être enregistrée par le biais de nos [méthodes d'achat][4].

| Options de segmentation | Filtre déroulant | Options d'entrée |
| ---------------------| --------------- | ------------- |
| Vérifier si l'attribut numérique **est supérieur à** une **valeur entière ou décimale**| **PLUS QUE** | **INTEGER** ou **DECIMAL** |
| Vérifier si l'attribut numérique **est inférieur à** une **valeur entière ou décimale**| **MOINS DE** | **INTEGER** ou **DECIMAL** |
| Vérifier si l'attribut numérique **est exactement** un **entier ou une valeur décimale**.| **EXACTEMENT** | **INTEGER** ou **DECIMAL** |
| Vérifier si l'attribut numérique n'est **pas égal à** un **entier ou à une valeur décimale**| **N'EST PAS ÉGAL** | **INTEGER** ou **DECIMAL** |
| Vérifier si l'attribut numérique **existe** dans le profil d'un utilisateur | **EXISTE** | **N/A** |
| Vérifier si l'attribut numérique **n'existe pas** dans le profil d'un utilisateur | **N'EXISTE PAS** | **N/A** |
{: .reset-td-br-1 .reset-td-br-2 .reset-td-br-3}

#### Booléens (vrai/faux)
Les attributs booléens sont utiles pour stocker les statuts d'abonnement et d'autres données binaires simples concernant vos utilisateurs. Les options de saisie que nous proposons vous permettent de trouver les utilisateurs pour lesquels une variable a été explicitement définie comme une valeur vrai/faux, ainsi que ceux pour lesquels cet attribut n'a pas encore été enregistré.

| Options de segmentation | Filtre déroulant | Options d'entrée |
| ---------------------| --------------- | ------------- |
| Vérifier si la valeur booléenne **est** | **IS**  | **VRAI**, **FAUX**, **VRAI OU NON ENREGISTRÉ**, ou **FAUX OU NON ENREGISTRÉ** |
| Vérifier si la valeur booléenne **existe** dans le profil d'un utilisateur | **EXISTE**  | **N/A** |
| Vérifier si la valeur booléenne n **'existe pas** dans le profil d'un utilisateur | **N'EXISTE PAS**  | **N/A** |
{: .reset-td-br-1 .reset-td-br-2 .reset-td-br-3}

## Événements d'achat / Suivi des recettes

L'utilisation de nos méthodes d'achat pour enregistrer les achats in-app établit la valeur à vie (LTV) pour chaque profil d'utilisateur individuel. Ces données peuvent être consultées dans notre page sur les revenus sous forme de séries chronologiques.

| Options de segmentation | Filtre déroulant | Options d'entrée |
| ---------------------| --------------- | ------------- |
| Vérifier si le nombre total de dollars dépensés **est supérieur à** une **valeur entière ou décimale**.| **PLUS GRAND QUE** | **INTEGER** ou **DECIMAL** |
| Vérifier si le nombre total de dollars dépensés **est inférieur à** une **valeur entière ou décimale**.| **MOINS DE** | **INTEGER** ou **DECIMAL** |
| Vérifier si le nombre total de dollars dépensés **est exactement** un **entier ou une valeur décimale**.| **EXACTEMENT** | **INTEGER** ou **DECIMAL** |
| Vérifier si le dernier achat a eu lieu **après la date X** | **APRÈS** | **DATE** |
| Vérifier si le dernier achat a eu lieu **avant la date X** | **AVANT** | **DATE** |
| Vérifier si l'achat a eu lieu **il y a plus de X jours** | **PLUS QUE** | **DATE** |
| Vérifier si l'achat a eu lieu **il y a moins de X jours** | **MOINS DE** | **DATE** |
| Vérifier si l'achat a eu lieu **plus de X fois (Max = 50)** | **PLUS QUE** | au cours des **Y** derniers **jours (Y = 1,3,7,14,21,30)** |
| Vérifier si l'achat a eu lieu **moins de X fois (Max = 50)** | **MOINS DE** | au cours des **Y** derniers **jours (Y = 1,3,7,14,21,30)** |
| Vérifier si l'achat a eu lieu **exactement X (Max = 50)** fois. | **EXACTEMENT** | au cours des **Y** derniers **jours (Y = 1,3,7,14,21,30)** |
{: .reset-td-br-1 .reset-td-br-2 .reset-td-br-3}

>  Si vous souhaitez segmenter le nombre de fois qu'un achat spécifique a été effectué, vous devez également enregistrer cet achat individuellement en tant qu'[attribut personnalisé incrémentiel][12].

## Cas d'utilisation d'une application de taxi ou de covoiturage {#example-case}
Pour cet exemple, considérons une application de taxi ou de covoiturage (telle que Hailo, Lyft, etc.) qui souhaite décider des données utilisateur à collecter. Les questions et le processus de réflexion suivants constituent un excellent modèle à suivre pour les équipes de marketing et de développement. À la fin de cet exercice, les deux équipes devraient avoir une bonne compréhension des événements et attributs personnalisés qu'il est judicieux de collecter pour atteindre leur objectif.

**Question de cas n° 1 : Quel est l'objectif ?**

L'objectif est simple : il s'agit de permettre aux utilisateurs de héler les taxis via l'application.

**Question n° 2 : Quelles sont les étapes intermédiaires pour atteindre cet objectif à partir de l'installation de l'application ?**

1. Ils ont besoin que les utilisateurs commencent le processus d'enregistrement et remplissent leurs informations personnelles.
2. Les utilisateurs doivent compléter et vérifier le processus d'enregistrement en saisissant un code dans l'application qu'ils reçoivent par SMS.
3. Ils doivent essayer de héler un taxi.
4. Pour héler un taxi, il faut être disponible au moment de la recherche.

Ces actions pourraient alors être marquées comme les événements personnalisés suivants :

- Début de l'inscription
- Inscription complète
- Des courses de taxi réussies
- Les appels de taxis infructueux

Après avoir mis en œuvre les événements, vous pouvez maintenant lancer les campagnes suivantes :

1. Envoyer un message aux utilisateurs qui ont commencé l'enregistrement, mais qui ne l'ont pas terminé dans un certain délai.
2. Envoyer des messages de félicitations aux utilisateurs qui ont terminé leur inscription.
3. Envoyer des excuses et des crédits promotionnels aux utilisateurs qui ont eu des appels de taxi infructueux, qui n'ont pas été suivis d'un appel de taxi réussi dans un certain laps de temps.
4. Envoyez des promotions aux utilisateurs les plus performants ayant effectué un grand nombre d'appels de taxis pour les remercier de leur fidélité.
5. Beaucoup, beaucoup plus.

**Question n° 3 : Quelles sont les autres informations que nous pourrions souhaiter connaître sur nos utilisateurs et qui nous permettraient d'améliorer notre message ?**

- S'ils disposent ou non d'un crédit promotionnel ?
- La note moyenne qu'ils attribuent à leurs conducteurs ?
- Des codes promotionnels uniques pour l'utilisateur ?

Ces caractéristiques pourraient alors être étiquetées comme les attributs personnalisés suivants :

- Solde créditeur promotionnel (type décimal)
- Note moyenne du conducteur (type entier)
- Code promo unique (type de chaîne)

L'ajout de ces attributs vous permettra d'envoyer des campagnes à des utilisateurs tels que :

1. Rappeler aux utilisateurs qui n'ont pas utilisé l'application depuis 7 jours et qui ont un crédit promotionnel restant sur leur compte qu'il existe et qu'ils devraient revenir sur l'application et l'utiliser !
2. Envoyer un message aux utilisateurs qui donnent de mauvaises notes aux conducteurs afin d'obtenir un retour direct de la part des clients et de savoir pourquoi ils n'ont pas apprécié leur trajet.
3. Utilisez nos [fonctions de modélisation et de personnalisation des messages][17] pour faire glisser l'attribut de code promotionnel unique dans les messages adressés aux utilisateurs.

## Meilleures pratiques

### Bonnes pratiques générales

#### Utiliser les propriétés des événements

- Nommez un événement personnalisé quelque chose qui décrit l'action d'un utilisateur.
- Utiliser généreusement les propriétés d'événement personnalisées pour représenter les données importantes d'un événement
- Par exemple, plutôt que de capturer un événement personnalisé distinct pour le visionnage de chacun des 50 films différents, il serait plus efficace de capturer le simple visionnage d'un film en tant qu'événement et d'avoir une propriété d'événement qui inclut le nom du film

### Meilleures pratiques de développement

#### Définir des identifiants pour chaque utilisateur

Des identifiants doivent être définis pour chacun de vos utilisateurs. Ils doivent être immuables et accessibles lorsque l'utilisateur ouvre l'application. Nous **vous recommandons vivement de** fournir cet identifiant, car il vous permettra de.. :

- Suivez vos utilisateurs à travers les appareils et les plateformes, améliorant ainsi la qualité de vos données comportementales et démographiques.
- Importez des données sur vos utilisateurs à l'aide de notre [API de données utilisateur][9].
- Ciblez des utilisateurs spécifiques avec notre [API de messagerie][10] pour les messages généraux et transactionnels.

Les identifiants doivent comporter moins de 512 caractères, être privés et difficiles à obtenir (par exemple, ne pas être une simple adresse électronique ou un nom d'utilisateur). Si un tel identifiant n'est pas disponible, Braze attribuera un identifiant unique à vos utilisateurs, mais vous ne disposerez pas des possibilités énumérées pour les identifiants d'utilisateur. Vous devez éviter de définir des identifiants pour les utilisateurs pour lesquels vous ne disposez pas d'un identifiant unique qui leur soit propre. La transmission d'un identifiant d'appareil n'offre aucun avantage par rapport au suivi anonyme automatique de l'utilisateur que Braze propose par défaut. Voici quelques exemples d'identifiants appropriés et inappropriés.

Bonnes options pour les identifiants d'utilisateur :

- Adresse électronique hachée ou nom d'utilisateur unique
- Identifiant unique de la base de données
- Identifiant Facebook

Ils ne doivent pas être utilisés comme identifiants :

- ID de l'appareil
- Numéro aléatoire ou ID de session
- Tout identifiant non unique

#### Donner des noms lisibles aux événements et attributs personnalisés
Imaginez que vous soyez un spécialiste du marketing qui commence à utiliser Braze un an ou deux après sa mise en œuvre, la lecture d'une liste déroulante remplie de noms tels que "usr_no_acct" sans autre contexte peut être intimidante. En donnant à vos événements et à vos attributs des noms identifiables et lisibles, vous faciliterez la tâche de tous les utilisateurs de votre plateforme. Les meilleures pratiques suivantes sont à prendre en considération :

- Ne commencez pas un événement personnalisé par un caractère numérique. La liste déroulante est triée par ordre alphabétique et le fait de commencer par un caractère numérique rend plus difficile la segmentation par le filtre de votre choix.
- Dans la mesure du possible, n'utilisez pas d'abréviations obscures ou de jargon technique.
  - Exemple : `usr_ctry` peut convenir comme nom de variable pour le pays d'un utilisateur dans un morceau de code, mais l'attribut personnalisé doit être envoyé à Braze en tant que `user_country`, au moins pour donner un peu de clarté à un spécialiste du marketing qui utilise le tableau de bord par la suite.

#### Ne consigner les attributs que lorsqu'ils sont modifiés
Chaque attribut transmis à Braze est considéré comme un point de données, même si l'attribut transmis contient la même valeur que celle enregistrée précédemment. Le fait de n'enregistrer les données que lorsqu'elles changent permet d'éviter l'utilisation redondante de points de données et offre une expérience plus fluide en évitant les appels d'API inutiles.

#### Éviter de générer des noms d'événements par programme
Si vous créez constamment de nouveaux noms d'événements, il vous sera impossible de segmenter vos utilisateurs de manière significative. Il est généralement préférable de capturer des événements génériques ("Regarder une vidéo" ou "Lire un article") plutôt que des événements très spécifiques tels que ("Regarder Gangnam Style" ou "Lire un article") : Best 10 Lunch Spots in Midtown Manhattan"). Les données spécifiques relatives à l'événement doivent être incluses dans une propriété de l'événement, et non dans le nom de l'événement.

### Limites et contraintes techniques
Il convient de tenir compte des limitations et contraintes suivantes lors de la mise en œuvre d'événements personnalisés :

#### Contraintes de longueur
Tous les événements personnalisés, les noms d'attributs personnalisés (clés) et les valeurs de chaînes d'événements personnalisés de 255 caractères ou plus seront tronqués. Idéalement, ils doivent être aussi courts que possible afin d'améliorer les performances du réseau et de la batterie de votre application. Si possible, limitez-les à 50 caractères.

#### Contraintes de contenu
Le contenu suivant sera découpé par programme à partir de vos attributs et événements. Veillez à ne pas utiliser les éléments suivants :

- Espaces blancs de début et de fin
- Nouvelles lignes
- Tous les chiffres non compris dans les numéros de téléphone
  - Exemple : "(732) 178-1038" sera condensé en "7321781038"
- Les caractères qui ne sont pas des espaces doivent être convertis en espaces.
- $ ne doit pas être utilisé comme préfixe pour des événements personnalisés.
- Toutes les valeurs d'encodage UTF-8 non valides
  -  "Mon domaine \\x80" serait condensé en "Mon domaine"

#### Clés réservées
Les clés suivantes sont réservées et ne peuvent pas être utilisées comme propriétés d'événements personnalisés :

- `time`
- `product_id`
- `quantity`
- `event_name`
- `price`
- `currency`

#### Définitions des valeurs

- Les valeurs entières sont de 64 bits
- Les décimales ont 15 chiffres décimaux par défaut.

### Analyse d'un champ de nom générique

S'il n'existe qu'un seul champ de nom générique pour un utilisateur (par exemple, "JohnDoe"), vous pouvez attribuer ce titre entier à l'attribut First Name (prénom) de votre utilisateur. En outre, vous pouvez essayer d'analyser le nom et le prénom de l'utilisateur en utilisant des espaces, mais cette dernière méthode comporte le risque potentiel de mal nommer certains de vos utilisateurs.

[4]: {{site.baseurl}}/developer_guide/platform_wide/analytics_overview/#purchase-events--revenue-tracking
[8]: {% image_buster /assets/img_archive/custom_event_analytics_example.png %} "custom_event_analytics_example.png"
[9]: {{site.baseurl}}/developer_guide/rest_api/user_data/#user-data
[10]: {{site.baseurl}}/developer_guide/rest_api/messaging/
[11] : http://www.regextester.com/pregsyntax.html
[12] : #integers
[16] : #example-case
[17] : {{site.baseurl}}/user_guide/personalization_and_dynamic_content/personalized_messaging/#personalized-messaging
[18]: {% image_buster /assets/img_archive/customEventProperties.png %} "customEventProperties.png"
