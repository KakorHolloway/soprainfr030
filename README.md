# infr030-m1rs

## Connection au cluster Openshift
Pour vous connecter au cluster Openshift mis à disposition :

### Pour trouver les sources rendez-vous dans la release suivante : https://github.com/okd-project/okd/releases/tag/4.19.0-okd-scos.19

### Pour Windows x86

Téléchargez sur votre machine le paquet OKD suivant : 
https://github.com/okd-project/okd/releases/download/4.19.0-okd-scos.19/openshift-client-windows-4.19.0-okd-scos.19.zip

Copiez le fichier oc.exe dans C:\Windows\System32

### Pour Linux x86
Téléchargez sur votre machine le paquet OKD suivant : 
https://github.com/okd-project/okd/releases/download/4.19.0-okd-scos.19/openshift-client-linux-4.19.0-okd-scos.19.tar.gz

Dézippez et copiez le fichier oc dans /urs/bin
```
tar -xvf openshift-client-linux-4.19.0-okd-scos.19.tar.gz
cp oc /usr/bin/
```

### Pour MAC

Télécharger le client suivant 

https://github.com/okd-project/okd/releases/download/4.19.0-okd-scos.19/openshift-install-mac-arm64-4.19.0-okd-scos.19.tar.gz pour ARM

https://github.com/okd-project/okd/releases/download/4.19.0-okd-scos.19/openshift-install-mac-4.19.0-okd-scos.19.tar.gz pour x86

Mettez le fichier oc dans le path.

Ou alors :
```
brew install openshift-cli
```

## Connexion au cluster (pour tous)

Allez sur l'url https://console-openshift-console.apps.openshift.kakor.ovh authentifiez-vous avec l'utilisateur ipi-gp-x (le x étant le numéro de groupe) en choisisant "KeystoneIDP".

Une fois authentifié (attention à ne pas recharger la page même si l'affichage prends du temps), allez en haut à droite de la page et sélectionnez l'option "Copy login Command" et réauthentifiez vous. 

Cliquez sur le lien Display Token et copiez dans votre terminal sur VScode la ligne de commande qui à été donné avec oc. 

Pour vérifier que vous êtes authentifiés, lancez la commande ```oc get pod```

## Exercice 1 : Déploiement de votre premier pod 

A partir de la documentation Kubernetes https://kubernetes.io/docs/concepts/workloads/pods/, créez un pod Nginx qui va utiliser l'image :
harbor.kakor.ovh/public/nginx-rootless

Vérifiez le fonctionnement du conteneur et une fois confirmé, supprimez-le. 

## Exercice 2

Créez deux pod : 

Un pod avec l'image nginx : harbor.kakor.ovh/public/nginx
Un pod avec l'image curl (attention vous allez avoir un status crashloopbackoff lié au fait qu'il n'y a pas de processus actif, ajoutez donc un sleep 3600 au démarrage)

Un service nommé "nginx" relié au pod du même nom à travers le label ```test: nginx```

Pour créer le service ClusterIp , appuyez-vous sur la documentation jointe ici : https://kubernetes.io/fr/docs/concepts/services-networking/service/

Testez avec la commande oc exec, de vous connecter sur le pod curl et vérifiez que le service est bien fonctionnel. 

Attention, les conteneurs étant en root, vous devrez ajouter dans la section spec.containers le contenu suivant :
````
    securityContext:
      allowPrivilegeEscalation: true
````