# Preuve d concept - digitalstate
Ceci est un dépôt pour la preuve de concept avec digital state

##Utilisation
Voici les étapes à suivre pour initialiser la stack digitale state:

## Pour camunda:
####Créer un container docker postgresql avec les variables d'environnements suivantes:
<pre>docker run --name db_camunda -v /tmp:/var/lib/postgresql \
-e POSTGRES_USER=camunda \
-e POSTGRES_PASSWORD=camunda \ 
-e POSTGRES_DB=process-engine \
-d postgres</pre>

####Créér un container docker containant l'application camunda avec la commande suivante
<pre>docker run -d --name camunda -p 8080:8080 --link db_camunda:db_camunda \
-e DB_DRIVER=org.postgresql.Driver \
-e DB_URL=jdbc:postgresql://db_camunda:5432/process-engine \
-e DB_USERNAME=camunda \
-e DB_PASSWORD=camunda \
camunda/camunda-bpm-platform:latest</pre>

## Pour orobap:
####Créer un container docker mysql avec les variables d'environnements suivantes:
<pre>docker run --name db_orocrm -v /tmp/oro:/var/lib/mysql \
-e MYSQL_ROOT_PASSWORD=pass \
-e MYSQL_DATABASE=oro_crm \
-e MYSQL_USER=orocrm \
-e MYSQL_PASSWORD=orocrm \ 
-d mysql:5.5</pre>
 
####Créer un container docker orobap avec la commande suivante:
 <pre>docker run \
 --name orocrm \
 -p 80:80 \
 --link db_orocrm:db_orocrm \
 -d olidac/orocrm</pre>
 
####Pour terminer l'installation, on doit éxécuter cette commande à l'intérieur du container:
<pre>docker exec -it orocrm bash</pre>
<pre>orocrm# app/console oro:install</pre>
Avec la dernière commande, oro installera
  * Les migrations des bases données
  * Les définitions de workflow
  * Les définitions des processus
  * Les définitions des déclencheurs
  * et quelques autres données
  
On vous demandera par la suite de configurer quelques paramètres, on aurait pu également passer ces paramètres dans la commande "oro install" initiale
