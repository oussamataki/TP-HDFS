# Intégration de HDFS avec Java

Ce projet propose une démonstration complète de l'interaction entre Java et le système de fichiers distribué Hadoop (HDFS). Il comprend :
- **App** : Lecture d'un fichier stocké sur HDFS.
- **AppWriter** : Écriture de données dans un fichier HDFS.
- **Déploiement avec Docker-Compose** : Installation d'un cluster HDFS avec un NameNode et plusieurs DataNodes.

## Prérequis
Avant de commencer, assurez-vous d'avoir installé les outils suivants :
- Docker & Docker-Compose
- Java Development Kit (JDK) 8+
- Bibliothèques Hadoop (disponibles dans `./jars`)
- Git (optionnel pour cloner le dépôt)

## 1. Mise en place du cluster HDFS

### Récupération du code source
```sh
git clone https://github.com/mrbenboyy/HDFS-Java-App.git
cd HDFS-Java-App
```

### Démarrage du cluster HDFS
```sh
docker-compose up -d
```
Cette commande lance les services suivants :
- **NameNode** : Responsable des métadonnées du système de fichiers.
- **Cinq DataNodes** : Assurent le stockage des blocs de fichiers.
- **ResourceManager** : Supervise l'exécution des tâches YARN.
- **NodeManager** : Gère les applications conteneurisées.

### Vérification du cluster
Assurez-vous que tous les services fonctionnent correctement :
```sh
docker ps
```
L'interface web HDFS est accessible à l'adresse suivante : [http://localhost:9870](http://localhost:9870)

## 2. Gestion des fichiers dans HDFS

### Création de répertoires et de fichiers
```sh
hdfs dfs -mkdir /user/hadoop
hdfs dfs -touchz /exemple.txt
```

### Ajout d'un fichier local dans HDFS
```sh
hdfs dfs -put monfichier.txt /exemple.txt
```

### Liste des fichiers présents
```sh
hdfs dfs -ls /
```

### Lecture du contenu d'un fichier
```sh
hdfs dfs -cat /exemple.txt
```

## 3. Exécution des applications Java

### Compilation du code Java
Assurez-vous que les bibliothèques Hadoop sont bien prises en compte dans le classpath.
```sh
javac -cp "./jars/*" -d . src/org/example/tpHDFS/App.java
javac -cp "./jars/*" -d . src/org/example/tpHDFS/AppWriter.java
```

### Exécution de l'application d'écriture
Cette application crée un fichier (`output.txt`) et y insère des données.
```sh
java -cp "./jars/*:." org.example.tpHDFS.AppWriter
```
Vérification du contenu du fichier :
```sh
hdfs dfs -cat /output.txt
```

### Exécution de l'application de lecture
Affiche le contenu d'un fichier stocké sur HDFS.
```sh
java -cp "./jars/*:." org.example.tpHDFS.App
```

## 4. Arrêt du cluster HDFS
```sh
docker-compose down
```

## 5. Dépannage

### Consulter les logs Hadoop
```sh
docker logs namenode
```

### Vérifier les conteneurs actifs
```sh
docker ps
```

### Redémarrer un service spécifique
```sh
docker restart namenode
```

## Ressources complémentaires
- [Documentation officielle Hadoop](https://hadoop.apache.org/docs/)
- [Commandes HDFS](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/FileSystemShell.html)

Bonne exploration de HDFS avec Java ! 🚀

