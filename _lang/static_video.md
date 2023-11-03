---
page_order: 0.5
nav_title: Video Test Page
layout: featured_video
video_id: XY5uXoKIvFY
video_source: youtube
noindex: true
---

# Collecte de données utilisateur

Avant de terminer votre implémentation Braze, assurez-vous d'avoir une conversation entre votre équipe marketing et votre équipe de développement concernant vos objectifs marketing. Lorsque vous décidez ce que vous voulez suivre, et comment vous voulez le suivre avec Braze, il est utile de considérer ces objectifs et de travailler à l'envers à partir de là. Référencez notre cas d'une \[Application Taxi/Covoiturage]\[16] à la fin de ce guide pour un exemple de ce processus.

Ce guide de bonnes pratiques vous aidera à comprendre exactement ce que Braze considère comme un événement personnalisé par rapport à un attribut personnalisé.

## Données collectées automatiquement

Les événements et attributs suivants sont capturés et mis à jour automatiquement par le SDK Braze dans le cadre des points de données `Session Start` et `Session End`, ou par le backend Braze. Vous n'avez pas besoin de les enregistrer séparément en tant qu'événements personnalisés ou attributs personnalisés.

#### Informations d'utilisation
- Première application utilisée (Date)
- Dernière application utilisée (Date)
- Nombre total de sessions (entier)
- Nombre d'éléments de rétroaction (entier)
- Nombre de sessions au cours des Y derniers jours (nombre entier et date)
- Courriel disponible (booléen)
- Fil d'actualités Voir le nombre (entier)

#### Reciblage de campagne
- Dernier message reçu (date)
- Dernière campagne emailing reçue (date)
- Dernière campagne push reçue (Date)
- Dernier fil d'actualité consulté (Date)
- Carte cliquée (entier)
- Message reçu de la campagne
  - Ce filtre vous permet de cibler les utilisateurs en fonction du fait qu'ils n'ont (pas) reçu de campagne précédente.
- Message reçu de Campagne avec Tag
  - Ce filtre vous permet de cibler les utilisateurs en fonction du fait qu'ils n'ont (pas) reçu de campagne ayant actuellement un tag.
- Retarget Campaign
  - Ce filtre vous permet de cibler les utilisateurs selon qu'ils ont ou non ouvert, ou cliqué sur un e-mail spécifique, push ou slideup dans le passé

#### Informations sur l'appareil
- Emplacement disponible (booléen)
- Localisation la plus récente (si la permission de localisation est accordée à votre application)
- Push activé (booléen)
- Device Locale
- Langue (tirée de Device Locale)
- Pays (d'abord tiré de l'adresse IP. Si ce n'est pas disponible, extrait de Device Locale)
- Version de l'application la plus récente
- Modèle d'appareil
- Device OS Version
- Résolution de périphérique
- Appareil Porteur sans fil
- Fuseau horaire de l'appareil
- Device Identifier
- Désinstallé (Date et Boolean)

## Événements personnalisés

Les événements personnalisés sont des actions prises par vos utilisateurs ; ils sont les mieux adaptés pour suivre les interactions utilisateur à forte valeur ajoutée avec votre application. L'enregistrement d'un événement personnalisé peut déclencher un nombre quelconque de campagnes de suivi avec des délais configurables, et active les filtres de segmentation suivants autour de la récence et de la fréquence de cet événement :

| Options de segmentation | Filtre déroulant | Options de saisie |
| ---------------------| --------------- | ------------- |
| Vérifiez si l'événement personnalisé s'est produit **plus de X fois** | **PLUS DE** | **INTEGER** |
| Vérifiez si l'événement personnalisé s'est produit **moins de X fois** | **MOINS DE** | **INTEGER** |
| Vérifier si l'événement personnalisé s'est produit **exactement X fois** | **EXACTEMENT** | **INTEGER** |
| Vérifier si le dernier événement personnalisé s'est produit **après X date** | **APRÈS** | **DATE** |
| Vérifier si le dernier événement personnalisé s'est produit **avant X date** | **AVANT** | **DATE** |
| Vérifier si le dernier événement personnalisé s'est produit il y a **plus de X jours** | **PLUS DE** | **NOMBRE DE JOURS** (Positif) Entier) |
| Vérifier si le dernier événement personnalisé s'est produit il y a **moins de X jours** | **MOINS DE** | **NOMBRE DE JOURS** (Positif) Entier) |
| Vérifiez si l'événement personnalisé s'est produit **plus de X (Max = 50) nombre de fois** | **PLUS DE** | au cours des derniers **jours Y (Y = 1,3,7,14,21,30)** |
| Vérifiez si l'événement personnalisé s'est produit **moins de X (Max = 50) nombre de fois** | **MOINS DE** | au cours des derniers **jours Y (Y = 1,3,7,14,21,30)** |
| Vérifier si l'événement personnalisé s'est produit **exactement X (Max = 50) nombre de fois** | **EXACTEMENT** | au cours des derniers **jours Y (Y = 1,3,7,14,21,30)** |
{: .reset-td-br-1 .reset-td-br-2 .reset-td-br-3}

Braze note le nombre de fois que ces événements se sont produits ainsi que la dernière fois qu'ils ont été effectués par chaque utilisateur pour la segmentation. Sur la page d'analyse des événements personnalisés, vous pouvez visualiser globalement la fréquence à laquelle chaque événement personnalisé se produit, ainsi que par segment dans le temps pour une analyse plus détaillée. Ceci est particulièrement utile pour voir comment vos campagnes ont affecté l'activité d'événements personnalisés en regardant les lignes grises superposées Braze sur les séries chronologiques pour indiquer la dernière fois qu'une campagne a été envoyée.

\![custom_event_analytics_example.png]\[8]

>  \[Incrémentation des attributs personnalisés]\[10] peut être utilisé pour garder un compteur sur une action utilisateur similaire à un événement personnalisé. Cependant, vous ne pourrez pas afficher les données d'attribut personnalisées dans une série chronologique. Les actions des utilisateurs qui n'ont pas besoin d'être analysées dans des séries chronologiques doivent être enregistrées via cette méthode.

### Stockage d'événements personnalisé

Toutes les données du profil utilisateur (événements personnalisés, attribut personnalisé, données personnalisées) sont stockées tant que ces profils sont actifs. Les propriétés des événements sont stockées pendant soixante (60) jours pour la segmentation.

### Propriétés d'événement personnalisées

Avec des propriétés d'événement personnalisées, Braze vous permet de définir des propriétés sur des événements et des achats personnalisés. Ces propriétés peuvent être utilisées pour qualifier davantage les conditions de déclenchement, augmenter la personnalisation dans la messagerie et générer des analyses plus sophistiquées grâce à l'exportation de données brutes. Les valeurs des propriétés peuvent être des objets string, numeric, boolean ou Date. Cependant, les valeurs des propriétés ne peuvent pas être des objets de tableau.

Par exemple, si une application de commerce électronique voulait envoyer un message à un utilisateur lorsqu'il abandonne son panier, elle pourrait en outre améliorer son public cible et permettre une personnalisation accrue de la campagne en ajoutant une propriété événementielle personnalisée de la « valeur panier » des paniers des utilisateurs.

\![customEventProperties.png]\[18]

Les propriétés d'événement personnalisées peuvent également être utilisées pour la personnalisation dans le modèle de messagerie. Toute campagne utilisant \[Action-Based Delivery]\[19] avec un événement déclencheur peut utiliser les propriétés d'événement personnalisées de cet événement pour la personnalisation de la messagerie. Si une application de jeu voulait envoyer un message aux utilisateurs ayant terminé un niveau, elle pouvait personnaliser davantage le message avec une propriété pendant le temps nécessaire aux utilisateurs pour terminer ce niveau. Dans cet exemple, le message est personnalisé pour trois segments différents en utilisant \[logique conditionnelle]\[18].  La propriété d'événement personnalisée appelée ``time_spent``, peut être incluse dans le message en appelant ``{% raw %} {{event_properties.${time_spent}}} {% endraw %}``.

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

Les propriétés d'événement personnalisées sont conçues pour vous aider à personnaliser votre messagerie ou à créer des campagnes de diffusion granulaires basées sur l'action. Si vous souhaitez créer des segments basés sur la récence et la fréquence des propriétés d'événements, contactez votre gestionnaire de succès client, car cela peut entraîner des coûts de données supplémentaires.

## Attributs personnalisés
Les attributs personnalisés sont les meilleurs pour stocker des attributs sur vos utilisateurs, ou des informations sur les actions de faible valeur au sein de votre application. Vous devez garder à l'esprit que nous ne stockons pas d'informations de séries chronologiques pour les attributs personnalisés, donc vous n'obtiendrez aucun graphique basé sur eux comme l'exemple précédent pour les événements personnalisés.

### Stockage des attributs personnalisés

Toutes les données du profil utilisateur (événements personnalisés, attribut personnalisé, données personnalisées) sont stockées tant que ces profils sont actifs. Les propriétés des événements sont stockées pendant soixante (60) jours pour la segmentation.

### Types de données d'attribut personnalisés
Les attributs personnalisés sont des outils extraordinairement flexibles qui permettent un excellent ciblage. Les types de données suivants peuvent être stockés en tant qu'attributs personnalisés:

#### Chaînes (Caractères Alphanumériques)
Les attributs de chaîne sont utiles pour stocker l'entrée utilisateur, comme une marque préférée, un numéro de téléphone ou une dernière chaîne de recherche dans votre application. Les attributs Strings peuvent contenir jusqu'à 255 caractères.

| Options de segmentation | Filtre déroulant | Options de saisie |
| ---------------------| --------------- | ------------- |
| Vérifier si l'attribut string **correspond exactement** à une chaîne entrée| **EQUALS** | **STRING** |
| Vérifier si l'attribut string **correspond partiellement** à une chaîne entrée **OU** à une expression régulière | **MATCHES REGEX** | **CHAÎNE** **OU** **EXPRESSION RÉGULIÈRE** |
| Vérifier si l'attribut string **ne correspond pas partiellement** à une chaîne entrée **OU** à une expression régulière | **NE CORRESPOND PAS À REGEX*** | **CHAÎNE** **OU** **EXPRESSION RÉGULIÈRE** |
| Vérifier si l'attribut string **ne correspond pas** à une chaîne entrée| **NE VAUT PAS** | **STRING** |
| Vérifier si l'attribut string **existe** sur le profil d'un utilisateur | **N'EST PAS VIERGE** | **S.O.** |
| Vérifier si l'attribut string **n'existe pas** sur le profil d'un utilisateur | **IS BLANK** | **S.O.** |
{: .reset-td-br-1 .reset-td-br-2 .reset-td-br-3}

{% alert important %}
\* Lorsque vous segmentez à l'aide du filtre **NE CORRESPOND PAS REGEX**, il est nécessaire qu'il existe déjà un attribut personnalisé avec une valeur attribuée dans ce profil utilisateur. Braze suggère d'utiliser la logique "OU" pour vérifier si un attribut personnalisé est vide afin de s'assurer que les utilisateurs sont ciblés correctement.
{% endalert %}

{% alert tip %}
Pour en savoir plus sur l'utilisation de notre filtre d'expressions régulières, consultez cette documentation sur les [expressions régulières compatibles Perl (PCRE)](http://www.regextester.com/pregsyntax.html).
<br>
Plus de ressources sur regex:
- [Regex avec Braze]({{site.baseurl}}/user_guide/engagement_tools/segments/regex/)
- [Débogueur et testeur Regex](https://regex101.com/)
- [Tutoriel Regex](https://medium.com/factory-mind/regex-tutorial-a-simple-cheatsheet-by-examples-649dc1c3f285)
{% endalert %}

#### Tableaux
Les attributs de tableau sont bons pour stocker des listes connexes d'informations sur vos utilisateurs. Par exemple, stocker les 100 derniers contenus qu'un utilisateur a regardés dans un tableau permettrait une segmentation spécifique des intérêts.

Les tableaux d'attributs personnalisés sont des ensembles unidimensionnels; les tableaux multidimensionnels ne sont pas pris en charge. **Ajouter un élément à un tableau d'attributs personnalisé ajoute l'élément à la fin du tableau, sauf s'il est déjà présent, auquel cas il est déplacé de sa position actuelle à la fin du tableau.** Par exemple, si un tableau \['hotdog','hotdog','hotdog','pizza'] a été importé, il affichera dans l'attribut array comme \['hotdog', 'pizza'], car nous ne supportons que des valeurs uniques.

Si le tableau contient son nombre maximum d'éléments, le premier élément sera écarté et le nouvel élément ajouté à la fin. Voici un exemple de code montrant le comportement du tableau dans le SDK Web:

```
var abUser = appboy.getUser();
// initialize array for this user, assuming max length of favorite_foods is set to 4.
abUser.setCustomUserAttribute('favorite_foods', ['pizza', 'wings', 'pasta']); // => ['pizza', 'wings', 'pasta']
abUser.addToCustomAttributeArray('favorite_foods', 'fries'); // => ['pizza', 'wings', 'pasta', 'fries']
abUser.addToCustomAttributeArray('favorite_foods', 'pizza'); // => ['wings', 'pasta', 'fries', 'pizza']
abUser.addToCustomAttributeArray('favorite_foods', 'ice cream'); // => ['pasta', 'fries', 'pizza', 'ice cream']

```

Le nombre maximal d'éléments dans les tableaux d'attributs personnalisés est par défaut de 25. Le maximum pour les tableaux individuels peut être augmenté jusqu'à 100 dans le tableau de bord Braze, sous **Paramètres de données** > **Attributs personnalisés**. Si vous souhaitez que ce maximum soit augmenté, contactez votre responsable du service client. Les tableaux dépassant le nombre maximum d'éléments seront tronqués pour contenir le nombre maximum d'éléments.

| Options de segmentation | Filtre déroulant | Options de saisie |
| ---------------------| --------------- | ------------- |
| Vérifiez si l'attribut array **comprend une valeur qui correspond exactement** à une valeur entrée| **INCLUS VALUE** | **STRING** |
| Vérifiez si l'attribut array **n'inclut pas une valeur correspondant exactement** à une valeur entrée| **N'INCLUT PAS LA VALEUR** | **STRING** |
| Vérifiez si l'attribut array **contient une valeur qui correspond partiellement** à une valeur entrée **OU** Expression régulière | **MATCHES REGEX** | **CHAÎNE** **OU** **EXPRESSION RÉGULIÈRE** |
| Vérifier si l'attribut array **a une valeur** | **A UNE VALEUR** | **S.O.** |
| Vérifier si l'attribut array **est vide** | **EST VIDE** | **S.O.** |
{: .reset-td-br-1 .reset-td-br-2 .reset-td-br-3}

>  Nous utilisons \[expressions régulières compatibles Perl (PCRE)]\[11].

#### Dates
Les attributs Date sont utiles pour stocker la dernière fois qu'une action spécifique a été effectuée, afin que vous puissiez proposer une messagerie de réengagement spécifique au contenu à vos utilisateurs.

>  La dernière date à laquelle un événement personnalisé ou un événement d'achat s'est produit est automatiquement enregistrée et ne doit pas être enregistrée en double via un attribut de date personnalisé.

Les filtres de dates utilisant des dates relatives (p. ex., il y a plus de 1 jour, moins de 2 jours) mesurent 1 jour comme 24 heures. Toute campagne que vous exécutez en utilisant ces filtres inclura tous les utilisateurs par tranches de 24 heures. Par exemple, la dernière application utilisée il y a plus de 1 jour capturera tous les utilisateurs qui "ont utilisé l'application pour la dernière fois plus de 24 heures" à partir de l'heure exacte à laquelle la campagne est lancée. Il en sera de même pour les campagnes définies avec des plages de dates plus longues – donc cinq jours à compter de l’activation signifieront les 120 heures précédentes.

| Options de segmentation | Filtre déroulant | Options de saisie |
| ---------------------| --------------- | ------------- |
| Vérifiez si l'attribut date **est antérieur à** une **date sélectionnée**| **AVANT** | **CALENDAR DATE SELECTOR** |
| Vérifiez si l'attribut date **est après** une **date sélectionnée**| **APRÈS** | **CALENDAR DATE SELECTOR** |
| Vérifier si l'attribut date est **plus de X nombre** de **jours** | **PLUS DE** | **NOMBRE DE JOURS** |
| Vérifier si l'attribut date est **inférieur à X nombre** de **jours**| **MOINS DE** | **NOMBRE DE JOURS** |
| Vérifiez si l'attribut date est **dans plus de X nombre** de **jours dans le futur** | **DANS PLUS DE** | **NOMBRE DE JOURS À VENIR** |
| Vérifiez si l'attribut date est **inférieur à X nombre** de **jours dans le futur** | **EN MOINS DE** | **NOMBRE DE JOURS À VENIR**  |
| Vérifier si l'attribut date **existe** sur le profil d'un utilisateur | **EXISTE** | **S.O.** |
| Vérifier si l'attribut date **n'existe pas** sur le profil d'un utilisateur | **N'EXISTE PAS** | **S.O.** |
{: .reset-td-br-1 .reset-td-br-2 .reset-td-br-3}

#### Nombres entiers (standard et incrémental) et décimaux (flottants/doubles) {#integers}
Les attributs numériques ont une grande variété de cas d'utilisation. L'incrémentation d'attributs personnalisés entiers est utile pour stocker le nombre de fois qu'une action ou un événement donné s'est produit. Les nombres entiers et décimaux standards ont toutes sortes d'usages, par exemple : (Enregistrement de la taille des chaussures, de la taille, du nombre de fois qu'un utilisateur a consulté une certaine caractéristique du produit ou une catégorie).
>  L'argent dépensé dans l'application ne doit pas être enregistré par cette méthode. Il doit plutôt être enregistré via nos [méthodes d'achat][4].

| Options de segmentation | Filtre déroulant | Options de saisie |
| ---------------------| --------------- | ------------- |
| Vérifiez si l'attribut numérique **est plus qu'**une **valeur entière ou décimale**| **PLUS DE** | **ENTIER** ou **DÉCIMAL** |
| Vérifiez si l'attribut numérique **est inférieur à** une **valeur entière ou décimale**| **MOINS DE** | **ENTIER** ou **DÉCIMAL** |
| Vérifiez si l'attribut numérique **est exactement** une **valeur entière ou décimale**| **EXACTEMENT** | **ENTIER** ou **DÉCIMAL** |
| Vérifiez si l'attribut numérique **n'égale pas** une **valeur entière ou décimale**| **NE VAUT PAS** | **ENTIER** ou **DÉCIMAL** |
| Vérifier si l'attribut numérique **existe** sur le profil d'un utilisateur | **EXISTE** | **S.O.** |
| Vérifier si l'attribut numérique **n'existe pas** sur le profil d'un utilisateur | **N'EXISTE PAS** | **S.O.** |
{: .reset-td-br-1 .reset-td-br-2 .reset-td-br-3}

#### Booléens (Vrai/Faux)
Les attributs booléens sont utiles pour stocker des statuts d'abonnement et d'autres données binaires simples sur vos utilisateurs. Les options de saisie que nous fournissons vous permettent de trouver les utilisateurs qui ont explicitement une variable définie à une valeur true/false en plus de ceux qui n'ont pas encore enregistré d'enregistrement de cet attribut.

| Options de segmentation | Filtre déroulant | Options de saisie |
| ---------------------| --------------- | ------------- |
| Vérifier si la valeur booléenne **est** | **IS**  | **VRAI**, **FAUX**, **VRAI OU NON**, ou **FAUX OU NON** |
| Vérifier si la valeur booléenne **existe** sur le profil d'un utilisateur | **EXISTE**  | **S.O.** |
| Vérifier si la valeur booléenne **n'existe pas** sur le profil d'un utilisateur | **N'EXISTE PAS**  | **S.O.** |
{: .reset-td-br-1 .reset-td-br-2 .reset-td-br-3}

## Événements d'achat / Suivi des revenus

L'utilisation de nos méthodes d'achat pour enregistrer les achats intégrés établit la valeur à vie (VLT) pour chaque profil d'utilisateur individuel. Ces données sont visibles dans notre page de revenus en séries chronologiques.

| Options de segmentation | Filtre déroulant | Options de saisie |
| ---------------------| --------------- | ------------- |
| Vérifiez si le nombre total de dollars dépensés **est supérieur à** une **valeur entière ou décimale**| **SUPÉRIEUR À** | **ENTIER** ou **DÉCIMAL** |
| Vérifiez si le nombre total de dollars dépensés **est inférieur à** une **valeur entière ou décimale**| **MOINS DE** | **ENTIER** ou **DÉCIMAL** |
| Vérifiez si le nombre total de dollars dépensés **est exactement** une **valeur entière ou décimale**| **EXACTEMENT** | **ENTIER** ou **DÉCIMAL** |
| Vérifier si le dernier achat a eu lieu **après X date** | **APRÈS** | **DATE** |
| Vérifier si le dernier achat a eu lieu **avant X date** | **AVANT** | **DATE** |
| Vérifier si le dernier achat a eu lieu il y a **plus de X jours** | **PLUS DE** | **DATE** |
| Vérifier si le dernier achat a eu lieu il y a **moins de X jours** | **MOINS DE** | **DATE** |
| Vérifier si l'achat a eu lieu **plus de X (Max = 50) nombre de fois** | **PLUS DE** | au cours des derniers **jours Y (Y = 1,3,7,14,21,30)** |
| Vérifier si l'achat a eu lieu **moins de X (Max = 50) nombre de fois** | **MOINS DE** | au cours des derniers **jours Y (Y = 1,3,7,14,21,30)** |
| Vérifier si l'achat a eu lieu **exactement X (Max = 50) nombre de fois** | **EXACTEMENT** | au cours des derniers **jours Y (Y = 1,3,7,14,21,30)** |
{: .reset-td-br-1 .reset-td-br-2 .reset-td-br-3}

>  Si vous souhaitez segmenter sur le nombre de fois qu'un achat spécifique a eu lieu, vous devez également enregistrer cet achat individuellement comme un \[attribut personnalisé croissant]\[12].

## {#example-case} de cas d'utilisation des applications de taxi/partage
Pour ce cas d'exemple, considérons une application de Taxi/Covoiturage (comme Hailo, Lyft, etc.) qui veut décider quelles données utilisateur collecter. Les questions suivantes et le processus de brainstorming sont un excellent modèle à suivre pour les équipes de marketing et de développement. À la fin de cet exercice, les deux équipes devraient avoir une solide compréhension des événements et des attributs personnalisés qu'il est judicieux de recueillir pour aider à atteindre leur objectif.

**Question no 1 : Quel est le but ?**

Leur objectif est simple dans la mesure où ils veulent que les utilisateurs hèlent des courses de taxi via leur application.

**Question no 2 : Quelles sont les étapes intermédiaires pour atteindre cet objectif depuis l'installation de l'application ?**

1. Ils ont besoin que les utilisateurs commencent le processus d'inscription et remplissent leurs informations personnelles.
2. Ils ont besoin que les utilisateurs terminent et vérifient le processus d'enregistrement en entrant un code dans l'application qu'ils reçoivent par SMS.
3. Ils doivent essayer de héler un taxi.
4. Pour héler un taxi, ils doivent être disponibles lorsqu'ils fouillent.

Ces actions pourraient alors être marquées comme les événements personnalisés suivants:

- Début des inscriptions
- Enregistrement terminé
- Grêles de taxi réussies
- Hails de taxi infructueux

Après avoir implémenté les événements, vous pouvez maintenant exécuter les campagnes suivantes:

1. Utilisateurs qui ont commencé l'inscription, mais qui ne l'ont pas terminée dans un certain délai.
2. Envoyez des messages de félicitations aux utilisateurs qui terminent l'inscription.
3. Envoyez des excuses et des crédits promotionnels aux utilisateurs qui ont eu des grêles de taxi infructueuses, qui n'ont pas été suivies d'une grêle de taxi réussie dans un certain laps de temps.
4. Envoyez des promotions aux utilisateurs puissants avec beaucoup de succès Taxi Hails pour les remercier de leur fidélité.
5. Beaucoup, beaucoup plus.

**Question no 3 : Quelles autres informations aimerions-nous connaître sur nos utilisateurs qui éclaireront notre messagerie?**

- Qu'ils aient ou non un crédit promotionnel?
- La note moyenne qu'ils donnent à leurs conducteurs ?
- Codes promotionnels uniques pour l'utilisateur?

Ces caractéristiques pourraient alors être marquées comme les attributs personnalisés suivants:

- Solde de crédit promotionnel (type décimal)
- Note moyenne du conducteur (type entier)
- Code promotionnel unique (type de chaîne)

L'ajout de ces attributs vous permettrait d'envoyer des campagnes aux utilisateurs comme:

1. Rappeler aux utilisateurs qui n'ont pas utilisé l'application depuis 7 jours qui ont un crédit promotionnel restant sur leur compte qu'il est là et qu'ils doivent revenir sur l'application et l'utiliser!
2. Messagerie aux utilisateurs qui donnent de faibles notes aux conducteurs pour obtenir des commentaires directs des clients pour voir pourquoi ils n'ont pas apprécié leurs trajets.
3. Utilisez notre \[modèle de message et fonctionnalités de personnalisation]\[17] pour glisser l'attribut unique de code de promotion dans la messagerie destinée aux utilisateurs.

## Meilleures pratiques

### Pratiques exemplaires générales

#### Utiliser les propriétés des événements

- Nommer un événement personnalisé quelque chose qui décrit une action qu'un utilisateur effectue
- Utilisez généreusement les propriétés d'événement personnalisées pour représenter des données importantes sur un événement
- Par exemple, plutôt que de capturer un événement personnalisé distinct pour regarder chacun de 50 films différents, il serait plus efficace de capturer simplement regarder un film comme un événement et d'avoir une propriété d'événement qui inclut le nom du film

### Meilleures pratiques de développement

#### Définir des identifiants pour chaque utilisateur

Les identifiants doivent être définis pour chacun de vos utilisateurs. Ceux-ci doivent être immuables et accessibles lorsqu'un utilisateur ouvre l'application. Nous vous **recommandons fortement** de fournir cet identifiant car il vous permettra de:

- Suivez vos utilisateurs sur différents appareils et plateformes, améliorant ainsi la qualité de vos données comportementales et démographiques.
- Importez des données sur vos utilisateurs en utilisant notre \[user data API]\[9].
- Ciblez des utilisateurs spécifiques avec notre \[API de messagerie]\[10] pour les messages généraux et transactionnels.

Les identifiants d'utilisateur doivent contenir moins de 512 caractères et doivent être privés et difficiles à obtenir (p. ex., pas une adresse électronique ou un nom d'utilisateur ordinaire). Si un tel identifiant n'est pas disponible, Braze attribuera un identifiant unique à vos utilisateurs, mais vous n'aurez pas les capacités répertoriées pour les identifiants d'utilisateur. Vous devez éviter de définir des identifiants d'utilisateur pour les utilisateurs pour lesquels il vous manque un identifiant unique qui leur est lié en tant qu'individu. Passer un identifiant d'appareil n'offre aucun avantage par rapport au suivi anonyme automatique des offres Braze par défaut. Voici quelques exemples d'identifiants d'utilisateur appropriés et inadaptés.

Bonnes options pour les identifiants:

- Adresse e-mail hachée ou nom d'utilisateur unique
- Identifiant unique de base de données
- Facebook ID

Ceux-ci ne doivent pas être utilisés comme identifiants:

- Device ID
- Numéro aléatoire ou ID de session
- Tout identifiant non unique

#### Donnez des noms lisibles aux événements et attributs personnalisés
Imaginez que vous êtes un marketeur qui commence à utiliser Braze un an ou deux après l'implémentation, lire une liste déroulante remplie de noms comme "usr_no_acct" sans autre contexte peut être intimidant. Donner à votre événement et à vos attributs des noms identifiables et lisibles facilitera les choses pour tous les utilisateurs de votre plateforme. Tenez compte des meilleures pratiques suivantes:

- Ne commencez pas un événement personnalisé avec un caractère numérique. La liste déroulante est triée par ordre alphabétique et commencer par un caractère numérique rend plus difficile la segmentation par le filtre de votre choix
- Essayez de ne pas utiliser d'abréviations obscures ou de jargon technique si possible
  - Exemple: `usr_ctry` peut être très bien comme nom de variable pour le pays d'un utilisateur dans un morceau de code, mais l'attribut personnalisé devrait être envoyé à Braze en tant que `user_country` au moins pour donner un peu de clarté à un spécialiste du marketing utilisant le tableau de bord en bas de la ligne.

#### Seuls les attributs log lorsqu'ils changent
Nous comptons chaque attribut passé à Braze comme point de données, même si l'attribut passé contient la même valeur que enregistrée précédemment. Seule la journalisation des données lorsqu'elles changent permet d'éviter une utilisation redondante des points de données et offre une expérience plus fluide en évitant les appels inutiles à l'API.

#### Évitez de générer par programmation des noms d'événements
Si vous créez constamment de nouveaux noms d'événements, il sera impossible de segmenter significativement vos utilisateurs. Vous devez généralement capturer des événements génériques (« Regarder une vidéo » ou « Lire un article ») au lieu d'événements très spécifiques tels que (« Regarder le style Gangnam » ou « Lire l'article : Best 10 Lunch Spots in Midtown Manhattan"). Les données spécifiques sur l'événement doivent être incluses en tant que propriété d'événement, et non en tant que partie du nom de l'événement.

### Limites et contraintes techniques
Soyez attentif aux limitations et contraintes suivantes lorsque vous implémentez des événements personnalisés :

#### Contraintes de longueur
Tous les événements personnalisés, les noms d'attributs personnalisés (clés) et les valeurs de chaînes d'événements personnalisées de 255 caractères ou plus seront tronqués. Idéalement, celles-ci doivent être aussi courtes que possible pour améliorer les performances réseau et batterie de votre application. Limitez-les si possible à 50 caractères.

#### Contraintes de contenu
Le contenu suivant sera découpé par programmation à partir de vos attributs et événements. Prenez soin de ne pas utiliser les éléments suivants:

- Espace blanc de tête et de queue
- Newlines
- Tous les non-chiffres dans les numéros de téléphone
  - Exemple : "(732) 178-1038" sera condensé en "7321781038"
- Les caractères non blancs doivent être convertis en espaces
- $ ne doit pas être utilisé comme préfixe pour des événements personnalisés
- Toute valeur d'encodage UTF-8 invalide
  -  "Mon champ \\x80" serait condensé en "Mon champ"

#### Clés réservées
Les clés suivantes sont réservées et ne peuvent pas être utilisées comme propriétés d'événement personnalisées :

- `time`
- `product_id`
- `quantity`
- `event_name`
- `price`
- `currency`

#### Définitions de valeur

- Les valeurs entières sont 64 bits
- Les décimales ont 15 chiffres décimaux par défaut

### Analyse d'un champ de nom générique

S'il n'existe qu'un seul champ de nom générique pour un utilisateur (par exemple, 'JohnDoe'), vous pouvez affecter ce titre entier à l'attribut Prénom de votre utilisateur. En outre, vous pouvez essayer d'analyser à la fois le nom et le prénom de l'utilisateur en utilisant des espaces, mais cette dernière méthode comporte le risque potentiel de mal nommer certains de vos utilisateurs.

[4]: {{site.baseurl}}/developer_guide/platform_wide/analytics_overview/#purchase-events--revenue-tracking
[8]: {% image_buster /assets/img_archive/custom_event_analytics_example.png %} "custom_event_analytics_example.png"
\[9]: {{site.baseurl}}/developer_guide/rest_api/user_data/#user-data
\[10]: {{site.baseurl}}/developer_guide/rest_api/messaging/
\[11]: http://www.regextester.com/pregsyntax.html
\[12]: #entiers
\[16]: #exemple-cas
\[17]: {{site.baseurl}}/user_guide/personalization_and_dynamic_content/personalized_messaging/#personalized-messaging
[18]: {% image_buster /assets/img_archive/customEventProperties.png %} "customEventProperties.png"
