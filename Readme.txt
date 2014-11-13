rem si machine de l'école
env.bat

rem On crée un dossier pour le projet

vagrant plugin install vagrant-proxyconf
vagrant box -v add dduportal/boot2docker
cd C:\ <Chemin du dossier>

vagrant init dduportal/boot2docker
rem lance la VM (machine virtuelle)

rem Ici, ajouter le fichier vagrant présent sur le dépôt GitHub

vagrant up
rem entre dans la VM
vagrant ssh

rem Téléchargement/Lancement du container postgresql
docker pull jamesbrink/postgresql
docker run -p 5432:5432 --name servpostgresql -d jamesbrink/postgresql

rem Téléchargement/Lancement du container postgresql
docker pull kartoza/geoserver
docker run -p 8080:8080 --name geoserver -d kartoza/geoserver

docker ps
rem Ici, noter l'ID du conteneur psql
docker inspect <ContainerID>
rem Ici, noter l'IP du conteneur psql

docker run --link servpostgresql:pg -ti jamesbrink/postgresql /bin/bash
psql -U postgres -p 5432 -h <ContainerIP>

rem Ici, on passe en langage sql
rem On ajoute une base de données dab
CREATE DATABASE dab;
\c dab
rem On peut créer ici de nouvelles tables
rem On quitte les bases de données
\q


rem Ouvrez un navigateur internet, Allez sur :
http://127.0.0.1:8080/geoserver/web/
rem Nom: admin
rem mot de passe: geoserver

rem Aller dans Entrepôt -> Ajouter un entrepôt -> Postgis DATABASE
rem remplir Titre = (ce que vous voulez), Host = <AdresseIP> (du serveur postgres), Port = 5432, database = dab, schema = postgres,
rem user = postgres, passwd = postgres
