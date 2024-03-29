# Journal des modifications de FreshRSS

[Voir les changements plus récents (en anglais)](./CHANGELOG.md)

## 2014-02-19 FreshRSS 0.7.1

* Mise à jour des flux plus rapide grâce à une meilleure utilisation du cache
	* Utilisation d’une signature MD5 du contenu intéressant pour les flux n’implémentant pas les requêtes conditionnelles
* Modification des raccourcis
	* "s" partage directement si un seul moyen de partage
	* Moyens de partage accessibles par "1", "2", "3", etc.
	* Premier article : Home ; Dernier article : End
	* Ajout du déplacement au sein des catégories / flux (via modificateurs shift et alt)
* UI
	* Séparation des descriptions des raccourcis par groupes
	* Revue rapide de la page de connexion
	* Amélioration de l’affichage des notifications sur mobile
* Revue du système de rafraîchissement des flux
	* Meilleure gestion de la file de flux à rafraîchir en JSON
	* Rafraîchissement uniquement pour les flux non rafraîchis récemment
	* Possibilité donnée aux anonymes de rafraîchir les flux
* SimplePie
	* Mise à jour de la lib
	* Corrige fuite de mémoire
	* Meilleure tolérance aux flux invalides
* Corrections divers
	* Ne déplie plus l’article lors du clic sur l’icône lien externe
	* Ne boucle plus à la fin de la navigation dans les articles
	* Suppression du champ category.color inutile
	* Corrige bug redirection infinie (Persona)
	* Amélioration vérification de la requête POST
	* Ajout d’un verrou lorsqu’une action mark_read ou mark_favorite est en cours


## 2014-01-29 FreshRSS 0.7

* Nouveau mode multi-utilisateur
	* L’utilisateur par défaut (administrateur) peut créer et supprimer d’autres utilisateurs
	* Nécessite un contrôle d’accès, soit :
		* par le nouveau mode de connexion par formulaire (nom d’utilisateur + mot de passe)
			* relativement sûr même sans HTTPS (le mot de passe n’est pas transmis en clair)
			* requiert JavaScript et PHP 5.3+
		* par HTTP (par exemple sous Apache en créant un fichier ./p/i/.htaccess et .htpasswd)
			* le nom d’utilisateur HTTP doit correspondre au nom d’utilisateur FreshRSS
		* par Mozilla Persona, en renseignant l’adresse courriel des utilisateurs
* Installateur supportant les mises à jour :
	* Depuis une v0.6, placer application.ini et Configuration.array.php dans le nouveau répertoire “./data/”
		(voir réorganisation ci-dessous)
	* Pour les versions suivantes, juste garder le répertoire “./data/”
* Rafraîchissement automatique du nombre d’articles non lus toutes les deux minutes (utilise le cache HTTP à bon escient)
	* Permet aussi de conserver la session valide, surtout dans le cas de Persona
* Nouvelle page de statistiques (nombres d’articles par jour / catégorie)
* Importation OPML instantanée et plus tolérante
* Nouvelle gestion des favicons avec téléchargement en parallèle
* Nouvelles options
	* Réorganisation des options
	* Gestion des utilisateurs
	* Améliorations partage vers Shaarli, Poche, Diaspora*, Facebook, Twitter, Google+, courriel
		* Raccourci ‘s’ par défaut
	* Permet la suppression de tous les articles d’un flux
	* Option pour marquer les articles comme lus dès la réception
	* Permet de configurer plus finement le nombre d’articles minimum à conserver par flux
	* Permet de modifier la description et l’adresse d’un flux RSS ainsi que le site Web associé
	* Nouveau raccourci pour ouvrir/fermer un article (‘c’ par défaut)
	* Boutons pour effacer les logs et pour purger les vieux articles
	* Nouveaux filtres d’affichage : seulement les articles favoris, et seulement les articles lus
* SQL :
	* Nouveau moteur de recherche, aussi accessible depuis la vue mobile
		* Mots clefs de recherche “intitle:”, “inurl:”, “author:”
	* Les articles sont triés selon la date de leur ajout dans FreshRSS plutôt que la date déclarée (souvent erronée)
		* Permet de marquer tout comme lu sans affecter les nouveaux articles arrivés en cours de lecture
		* Permet une pagination efficace
	* Refactorisation
		* Les tables sont préfixées avec le nom d’utilisateur afin de permettre le mode multi-utilisateurs
		* Amélioration des performances
		* Tolère un beaucoup plus grand nombre d’articles
		* Compression des données côté MySQL plutôt que côté PHP
		* Incompatible avec la version 0.6 (nécessite une mise à jour grâce à l’installateur)
	* Affichage de la taille de la base de données dans FreshRSS
	* Correction problème de marquage de tous les favoris comme lus
* HTML5 :
	* Support des balises HTML5 audio, video, et éléments associés
		* Utilisation de preload="none", et réécriture correcte des adresses, aussi en HTTPS
	* Protection HTML5 des iframe (sandbox="allow-scripts allow-same-origin")
	* Filtrage des object et embed
	* Chargement différé HTML5 (postpone="") pour iframe et video
	* Chargement différé JavaScript pour iframe
* CSS :
	* Nouveau thème sombre
		* Chargement plus robuste des thèmes
	* Meilleur support des longs titres d’articles sur des écrans étroits
	* Meilleure accessibilité
		* FreshRSS fonctionne aussi en mode dégradé sans images (alternatives Unicode) et/ou sans CSS
	* Diverses améliorations
* PHP :
	* Encore plus tolérant pour les flux comportant des erreurs
	* Mise à jour automatique de l’URL du flux (en base de données) lorsque SimplePie découvre qu’elle a changé
	* Meilleure gestion des caractères spéciaux dans différents cas
	* Compatibilité PHP 5.5+ avec OPcache
	* Amélioration des performances
	* Chargement automatique des classes
	* Alternative dans le cas d’absence de librairie JSON
	* Pour le développement, le cache HTTP peut être désactivé en créant un fichier “./data/no-cache.txt”
* Réorganisation des fichiers et répertoires, en particulier :
	* Tous les fichiers utilisateur sont dans “./data/” (y compris “cache”, “favicons”, et “log”)
	* Déplacement de “./app/configuration/application.ini” vers “./data/config.php”
		* Meilleure sécurité et compatibilité
	* Déplacement de “./public/data/Configuration.array.php” vers “./data/*_user.php”
	* Déplacement de “./public/” vers “./p/”
		* Déplacement de “./public/index.php” vers “./p/i/index.php” (voir cookie ci-dessous)
	* Déplacement de “./actualize_script.php” vers “./app/actualize_script.php” (pour une meilleure sécurité)
		* Pensez à mettre à jour votre Cron !
* Divers :
	* Nouvelle politique de cookie de session (témoin de connexion)
		* Utilise un nom poli “FreshRSS” (évite des problèmes avec certains filtres)
		* Se limite au répertoire “./FreshRSS/p/i/” pour de meilleures performances HTTP
			* Les images, CSS, scripts sont servis sans cookie
		* Utilise “HttpOnly” pour plus de sécurité
	* Nouvel “agent utilisateur” exposé lors du téléchargement des flux, par exemple :
		* `FreshRSS/0.7 (Linux; http://freshrss.org) SimplePie/1.3.1`
	* Script d’actualisation avec plus de messages
		* Sur la sortie standard, ainsi que dans le log système (syslog)
	* Affichage du numéro de version dans “À propos”


## 2013-11-21 FreshRSS 0.6.1

* Corrige bug chargement du JavaScript
* Affiche un message d’erreur plus explicite si fichier de configuration inaccessible


## 2013-11-17 FreshRSS 0.6

* Nettoyage du code JavaScript + optimisations
* Utilisation d’adresses relatives
* Amélioration des performances coté client
* Mise à jour automatique du nombre d’articles non lus
* Corrections traductions
* Mise en cache de FreshRSS
* Amélioration des retours utilisateur lorsque la configuration n’est pas bonne
* Actualisation des flux après une importation OPML
* Meilleure prise en charge des flux RSS invalides
* Amélioration de la vue globale
* Possibilité de personnaliser les icônes de lecture
* Suppression de champs lors de l’installation (base_url et sel)
* Correction de bugs divers


## 2013-10-15 FreshRSS 0.5.1

* Correction du bug des catégories disparues
* Correction traduction i18n/fr et i18n/en
* Suppression de certains appels à la feuille de style fallback.css


## 2013-10-12 FreshRSS 0.5.0

* Possibilité d’interdire la lecture anonyme
* Option pour garder l’historique d’un flux
* Lors d’un clic sur “Marquer tous les articles comme lus”, FreshRSS peut désormais sauter à la prochaine catégorie / prochain flux avec des articles non lus.
* Ajout d’un token pour accéder aux flux RSS générés par FreshRSS sans nécessiter de connexion
* Possibilité de partager vers Facebook, Twitter et Google+
* Possibilité de changer de thème
* Le menu de navigation (article précédent / suivant / haut de page) a été ajouté à la vue non mobile
* La police OpenSans est désormais appliquée
* Amélioration de la page de configuration
* Une meilleure sortie pour l’imprimante
* Quelques retouches du design par défaut
* Les vidéos ne dépassent plus du cadre de l’écran
* Nouveau logo
* Possibilité d’ajouter un préfixe aux tables lors de l’installation
* Ajout d’un champ en base de données keep_history à la table feed
* Si possible, création automatique de la base de données si elle n’existe pas lors de l’installation
* L’utilisation d’UTF-8 est forcée
* Le marquage automatique au défilement de la page a été amélioré
* La vue globale a été énormément améliorée et est beaucoup plus utile
* Amélioration des requêtes SQL
* Amélioration du JavaScript
* Correction bugs divers


## 2013-07-02 FreshRSS 0.4.0

* Correction bug et ajout notification lors de la phase d’installation
* Affichage d’erreur si fichier OPML invalide
* Les tags sont maintenant cliquables pour filtrer dessus
* Amélioration vue mobile (boutons plus gros et ajout d’une barre de navigation)
* Possibilité d’ajouter directement un flux dans une catégorie dès son ajout
* Affichage des flux en erreur (injoignable par exemple) en rouge pour les différencier
* Possibilité de changer les noms des flux
* Ajout d’une option (désactivable donc) pour charger les images en lazyload permettant de ne pas charger toutes les images d’un coup
* Le framework Minz est maintenant directement inclus dans l’archive (plus besoin de passer par ./build.sh)
* Amélioration des performances pour la récupération des flux tronqués
* Possibilité d’importer des flux sans catégorie lors de l’import OPML
* Suppression de “l’API” (qui était de toute façon très basique) et de la fonctionnalité de “notes”
* Amélioration de la recherche (garde en mémoire si l’on a sélectionné une catégorie) par exemple
* Modification apparence des balises hr et pre
* Meilleure vérification des champs de formulaire
* Remise en place du mode “endless” (permettant de simplement charger les articles qui suivent plutôt que de charger une nouvelle page)
* Ajout d’une page de visualisation des logs
* Ajout d’une option pour optimiser la BDD (diminue sa taille)
* Ajout des vues lecture et globale (assez basique)
* Les vidéos YouTube ne débordent plus du cadre sur les petits écrans
* Ajout d’une option pour marquer les articles comme lus lors du défilement (et suppression de celle au chargement de la page)


## 2013-05-05 FreshRSS 0.3.0

* Fallback pour les icônes SVG (utilisation de PNG à la place)
* Fallback pour les propriétés CSS3 (utilisation de préfixes)
* Affichage des tags associés aux articles
* Internationalisation de l’application (gestion des langues anglaise et française)
* Gestion des flux protégés par authentification HTTP
* Mise en cache des favicons
* Création d’un logo *temporaire*
* Affichage des vidéos dans les articles
* Gestion de la recherche et filtre par tags pleinement fonctionnels
* Création d’un vrai script CRON permettant de mettre tous les flux à jour
* Correction bugs divers


## 2013-04-17 FreshRSS 0.2.0

* Création d’un installateur
* Actualisation des flux en Ajax
* Partage par mail et Shaarli ajouté
* Export par flux RSS
* Possibilité de vider une catégorie
* Possibilité de sélectionner les catégories en vue mobile
* Les flux peuvent être sortis du flux principal (système de priorité)
* Amélioration ajout / import / export des flux
* Amélioration actualisation (meilleure gestion des erreurs)
* Améliorations du CSS
* Changements dans la base de données
* MàJ de la librairie SimplePie
* Flux sans auteurs gérés normalement
* Correction bugs divers


## 2013-04-08 FreshRSS 0.1.0

* “Première” version
