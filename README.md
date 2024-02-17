CHAHAT Anthony

***PARTIE 1 - Loki***

1. **Qu’est-ce que Loki ?**
   - Loki est un système de journalisation (logging) open-source conçu pour être hautement scalable et optimisé pour les environnements conteneurisés comme Kubernetes. Il est développé par Grafana Labs et est conçu pour fonctionner de manière native avec Prometheus, un autre outil de surveillance populaire.

2. **Quelles sont les différentes composantes de la stack Loki fournie ?**
   - Les composants principaux de la stack Loki sont :
     - **Loki**: Le moteur de stockage de journaux, responsable de l'indexation et du stockage des journaux.
     - **Promtail**: Un agent de collecte de journaux qui récupère les journaux des conteneurs et les envoie à Loki.
     - **Grafana**: Une interface utilisateur pour visualiser et interroger les journaux stockés dans Loki.

3. **À quoi servent chacune des composantes ?**
   - Loki : Stockage et indexation des journaux.
   - Promtail : Collecte des journaux à partir des conteneurs et envoi à Loki.
   - Grafana : Interface utilisateur pour la visualisation et l'interrogation des journaux stockés dans Loki.

4. **Qu’est-ce que Minio ?**
   - Minio est un serveur de stockage d'objets open-source, compatible avec l'API S3. Il est souvent utilisé pour créer des infrastructures de stockage distribuées et évolutives.

5. **Réalisez un schéma du flux de données entre le moment où un log est écrit dans Docker jusqu’au moment où il est visible dans Grafana.**
   - Voir schéma dans le dossier part-1-loki-prometheus

6. **À quoi sert le service gateway ?**
   - Le service gateway dans le contexte de Loki peut être utilisé pour l'accès aux données de journalisation stockées dans Loki via une API HTTP, ce qui permet aux utilisateurs d'accéder aux journaux à partir de différents systèmes ou applications de manière uniforme.

***PARTIE 2 - Logs***

7. **Champs contenus dans les logs et leur signification :**

   - `device` : Le navigateur ou le type de périphérique utilisé.
   - `evt` : L'événement qui s'est produit, tel que "login.success" ou "product.add".
   - `level` : Le niveau de gravité du message de log, comme "info", "warning" ou "error".
   - `msg` : Le message des logs, décrivant l'événement qui s'est produit.
   - `name` : Le nom du produit ou de l'action effectuée.
   - `product` : L'identifiant unique du produit concerné par l'événement.
   - `stocks` : Le nombre de stocks disponible pour le produit.
   - `order` : L'identifiant de la commande concernée par l'événement.
   - `time` : L'horodatage de l'événement.
   - `uuid` : L'identifiant unique de l'utilisateur.


8. **Configuration nécessaire du côté de la stack Loki pour récolter les logs :**
   - Pour récolter les logs avec Loki, nous devons configurer Promtail pour qu'il surveille les journaux produits par notre application Docker. Nous devons spécifier le chemin du fichier de journal et définir des balises pour les différents champs de journal afin de les indexer correctement dans Loki.

9. **Requêtes dans l'onglet "Explorer" de Grafana :**
   - Pour obtenir le volume de logs par couleur :
     - Nous pouvons utiliser la requête Loki suivante : `{container="logapp"} | json`
   - Pour filtrer les résultats pour un utilisateur spécifique :
     - Nous pouvons utiliser une requête Loki avec une clause de filtrage, par exemple : `{container="logapp"} | json | uuid="<UUID OF USER>"`

10. Voir le dashboard.

***PARTIE 3 - Elasticsearch***

1) Mise en place d'Elasticsearch et Kibana.

11. **Qu’est-ce qu’elasticsearch ?**
   - Elasticsearch est un moteur de recherche et d'analyse distribué open source développé par Elastic. Il est conçu pour stocker, rechercher et analyser de grandes quantités de données en temps réel.

12. **À quoi sert elasticsearch ?**
   - Elasticsearch est utilisé pour diverses applications telles que la recherche en texte intégral, l'analyse de logs, la surveillance de l'infrastructure, la recherche d'entreprise, la recherche de produits, etc. Il est capable de traiter des volumes massifs de données de manière rapide et efficace, ce qui le rend adapté à une large gamme de cas d'utilisation.

13. **Qu’est ce que Kibana ? Sur quel port accéder à Kibana ?**
   - Kibana est une interface de visualisation de données open source développée par Elastic. Elle est utilisée pour la visualisation et l'analyse des données stockées dans Elasticsearch. Kibana permet de créer des tableaux de bord interactifs, des graphiques, des cartes géographiques et des visualisations personnalisées pour explorer et comprendre les données. Par défaut, Kibana s'exécute sur le port 5601.

14. **Qu’est-ce qu’un index Elasticsearch ?**
   - Dans Elasticsearch, un index est une collection de documents similaires qui sont stockés ensemble et peuvent être recherchés et analysés en tant qu'ensemble. Un index est la plus petite unité de stockage dans Elasticsearch et il est comparable à une base de données dans le monde relationnel.

15. **Qu’est-ce que Lucene ? Quel est son rapport avec Elasticsearch ?**
   - Lucene est une bibliothèque de recherche et d'indexation de texte écrite en Java. Elle fournit les bases sur lesquelles Elasticsearch est construit. Lucene gère les opérations de recherche et d'indexation au niveau le plus bas, offrant des fonctionnalités puissantes telles que la recherche en texte intégral, les requêtes booléennes, les filtres, etc. Elasticsearch utilise Lucene pour l'indexation et la recherche des données, en fournissant une interface plus conviviale et une capacité de mise à l'échelle distribuée.

2) Filebeat

16. **Qu’est-ce que Filebeat ?**
   - Filebeat est un agent léger développé par Elastic, conçu pour l'expédition de journaux et de fichiers. Il est utilisé pour collecter, transférer et traiter des fichiers de journal et des données de manière efficace et sécurisée. Filebeat est souvent utilisé dans les architectures de collecte de journaux centralisées, où il peut être déployé sur différents serveurs pour recueillir les journaux locaux et les transférer vers une destination centralisée, telle que Elasticsearch, Logstash ou Kafka.

17. **Quels autres “beats” existent ?**
   - Outre Filebeat, Elastic propose une gamme d'autres agents légers appelés "Beats", chacun conçu pour des cas d'utilisation spécifiques :
     - **Metricbeat** : Utilisé pour collecter des métriques système et des métriques d'applications à partir de diverses sources telles que les serveurs, les services cloud, les bases de données, etc.
     - **Packetbeat** : Utilisé pour analyser le trafic réseau en temps réel en capturant, analysant et indexant les paquets de données pour surveiller les performances des applications et les comportements réseau.
     - **Heartbeat** : Utilisé pour surveiller la disponibilité des services en envoyant régulièrement des requêtes HTTP, ICMP, TCP, etc., et en enregistrant les temps de réponse.
     - **Auditbeat** : Utilisé pour collecter des données d'audit sur les activités système et les processus utilisateur sur les systèmes d'exploitation Linux, macOS et Windows.
     - **Winlogbeat** : Utilisé pour collecter des journaux d'événements Windows à partir des événements du journal système et de sécurité pour la surveillance et l'analyse.
     - **Functionbeat** : Utilisé pour collecter et expédier des données à partir de services de cloud computing et de serveurs sans serveur, tels que AWS Lambda, Google Cloud Functions, et Azure Functions.
     - **Journalbeat** : Utilisé pour collecter des journaux de système journal sur les systèmes Linux.

Chaque Beat est conçu pour une tâche spécifique et peut être utilisé seul ou en combinaison avec d'autres Beats pour répondre aux besoins de surveillance et de collecte de données dans divers environnements.

3) Dashboard

Kibana: http://m3.vms.re:5601/app/dashboards#/view/85773c70-cdb6-11ee-b05b-c5dff566793a?_g=(filters:!(),refreshInterval:(pause:!t,value:0),time:(from:now-15m,to:now))

***PARTIE 4 - Retour d'expérience***

Je trouve Elasticsearch plus simple d'utilisation et beaucoup plus puissant, surtout avec Kibana comme interface qui est très intuitif. Cependant ne pas pouvoir changer de couleur les lignes et autres des graph en étant forcé d'utiliser des palettes préfaites est limite. Si je devais faire le choix entre ces deux technologies pour un projet, je choisirai l'écosystème ELK avec Filebeat.