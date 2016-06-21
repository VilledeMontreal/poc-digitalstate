# Preuve d concept - digitalstate
Ceci est un dépôt pour la preuve de concept avec digital state

##Utilisation
Voici les étapes à suivre pour initialiser la stack digitale state:

####Créer un container docker postgresql avec les variables d'environnements suivantes:
<pre>docker run --name db_camunda -v /tmp:/var/lib/postgresql -e POSTGRES_USER=camunda -e POSTGRES_PASSWORD=camunda -e POSTGRES_DB=process-engine -d postgres</pre>

####Créér un container docker containant l'application camunda avec la commande suivante
<pre>docker run -d --name camunda -p 8080:8080 --link db_camunda:db_camunda -e DB_DRIVER=org.postgresql.Driver -e DB_URL=jdbc:postgresql://db_camunda:5432/process-engine -e DB_USERNAME=camunda -e DB_PASSWORD=camunda camunda/camunda-bpm-platform:latest</pre>

