# Preuve concept - digitalstate
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
<pre>docker run --name db_orocrm -v /tmp/db_oro:/var/lib/mysql \
-e MYSQL_ROOT_PASSWORD=pass \
-e MYSQL_DATABASE=oro_crm \
-e MYSQL_PASSWORD=orocrm \
-d mysql:5.5</pre>
 
####Créer un container docker orobap avec la commande suivante:
 <pre>docker run \
 --name orocrm \
 -p 80:80 \
 --link db_orocrm:dborocrm \
 -d olidac/orocrm:1.8.2</pre>
 
####Pour terminer l'installation, on doit éxécuter cette commande à l'intérieur du container:
<pre>docker exec -it orocrm bash</pre>
<pre>/var/www/orocrm# app/console oro:install</pre>

Avec la dernière commande, oro installera
  * Les migrations des bases données
  * Les définitions de workflow
  * Les définitions des processus
  * Les définitions des déclencheurs
  * et quelques autres données

Il est également possiblement de passer les arguments d'installation directement avec la commande console:
<pre>/var/www/orocrm# app/console oro:install --organization-name=vdm --user-name=admin --user-email=admin@admin.com --user-firstname=admin --user-lastname=admin --user-password=admin --application-url=http://localhost/oro --drop-database --timeout=3600 --sample-data=n --env=dev</pre>
  
On vous demandera par la suite de configurer quelques paramètres, on aurait pu également passer ces paramètres dans la commande "oro install" initiale

voir aussi [https://github.com/advancingu/OroCRMDocker](https://github.com/advancingu/OroCRMDocker)

####Pour créé la stack de orocommerce
Éxécuter la commande suivante:
<pre>docker-compose -f <(curl https://raw.githubusercontent.com/fprieur/orocommerce/master/compose/autoinstall/docker-compose.yml) up -d</pre>
* attendre quelques minutes pour que l'application web soit disponible<br><br>
l'application sera ensuite disponible à l'url http://($HOSTNAME):3080 <br><br>
pour se connecter en tant qu'admin: <br>
http://($HOSTNAME):3080/admin<br>
u johndoe@example.com<br>
p admin1111<br>

le dépot est un fork du dépot https://github.com/djocker/orocommerce
