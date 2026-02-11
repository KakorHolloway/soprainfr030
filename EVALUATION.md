# Evaluation 

## Objectifs

Déployez votre application dans Openshift de A à Z

## Rendu :

Le but est de pouvoir reproduire le déploiement. Pour se faire, documentez sous la forme de votre choix (README.md, Word) les étapes pour arriver au déploiement de votre applicatif de la constitution du Dockerfile à son déploiement.

A la fin vous devrez rendre :

- Le document mentionné ci-dessus
- Les fichiers YAML et Dockerfile de votre déploiement
- Une capture d'écran de votre site déployé via votre navigateur

## Consignes

Par groupe de 2 au maximum, choisissez une application consitituée d'un back et d'un front à déployer.

En exemple vous pouvez aussi créer le Dockerfile pour l'une de ces applications (je n'attends pas à ce que vous génériez un dockerfile pour la bdd):
- Bookstack
- GLPI
- Django Oscar


- Créez l'image docker de vos composants applicatifs et mettez la à disposition sur harbor.kakor.ovh/ipi/nomgroupe-back (dans l'idéal elle ne devraient s'executer en root)

https://harbor.kakor.ovh (utilisateur ipi)

- Réfléchissez à l'infrastructure à mettre en place sur Kubernetes à l'aide de l'outil Draw.io (ou du moins avec les symboles officiels de Kube vu en cours) 

Cette infrastructure doit répondre aux exigences suivantes :

- Vous ne devez pas déployer directement les pods mais utiliser les objets adaptés pour éviter une supression de ces derniers
- Si mot de passe il y a, il doit être mis en place dans des secrets
- Avec la documentation et le cours, stockez la donnée persitente à travers des PV créés manuellement sur le serveur NFS 192.168.1.56 /Volume1/public/nfs-share-openshift/project-gp-x
- Créez une route edge pour Openshift à travers l'ingress dédié
- Vérifiez que vos services communiquent entre-eux (le back doit parler au front)

Déployez l'infrastructure. 

