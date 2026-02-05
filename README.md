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

## Exercice 3

Le but de cet exercice est d'accéder depuis votre machine au site nginx. 

Pour ce faire gardez les pods et services de l'exercice (sauf le curl)

Créez le service en vous aidant de la documentation suivante :
https://kubernetes.io/docs/concepts/services-networking/ingress/

Attention, nous sommes sous openshift, de fait l'ingress à pour vocation de créer une route de type Edge. De fait, aidez vous de la documentation suivante pour créer la route de type Edge. https://docs.redhat.com/en/documentation/openshift_container_platform/4.21/html/ingress_and_load_balancing/routes#nw-ingress-edge-route-default-certificate_creating-advanced-routes

Votre site devra avoir l'adresse dns :

votresite.apps.openshift.kakor.ovh

## Exercice 4
En vous aidant de la documentation de Kubernetes, créez un deployment qui va créer 2 pod ibmcom/curl:3.6 avec le label app: curl (attention au command)
De même, faites via un statefulset un pod qui contiendra l’image nginx avec le label app: nginx
Mettez à jour l’image du deployment avec le tag 4.0.0
Regardez ce qui se passe
Mettez à jour l’image avec le tag inexistant 7.4.2
Regardez ce qui se passe

## Exercice 5

Sur le pod liveness-probe précédent ajoutez une nouvelle section readiness et observez le comportement via un watch

Créez un pod nginx qui vérifiera que le port 80 est bien actif
On va donc demander à ce que le readinessProbe soit sur un initial delay de 5 second et une periodSeconds de 3

Pour la liveness ces arguments vont êtres sur 120 et 10 
Rentrez dans le pod nginx via Kubectl exec –it nginx /bin/bash

Modifiez le fichier default.conf via la commande cat EOF 

Via la commande /etc/init.d/nginx reload recharger la conf nginx. Sortez de conteneur et regardez ce qui se passe
