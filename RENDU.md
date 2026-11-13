# Rendu — Atelier Kubernetes

> Complétez ce fichier au fil de l'atelier, puis faites un commit et un push (voir la section « Rendu » du README).

**Nom / Prénom :**
**Groupe :**

---

## Exercice 1 — Cluster

1. Combien de nœuds affiche `kubectl get nodes` ? Quel est le rôle de chacun ?
   >La commande affiche 3 nœuds : un nœud principal (server), qui dirige le cluster et décide où lancer les applications, et deux nœuds de travail (agents), qui font tourner les applications.
2. Que montre `docker ps` ? Qu'est-ce qu'un « nœud » dans k3d, concrètement ?
   >docker ps montre un conteneur Docker pour chaque nœud, plus un conteneur supplémentaire qui sert de point d’entrée vers le cluster. Un nœud k3d n’est donc pas une vraie machine : c’est simplement un conteneur Docker qui joue le rôle d’une machine du cluster.

## Exercice 2 — Premier déploiement

3. Quels objets Kubernetes ont été créés par `kubectl create deployment` ? (indice : `kubectl get all`)
   >La commande a créé trois objets : un Deployment, un ReplicaSet et un pod. Le Deployment gère le ReplicaSet, qui s’assure à son tour que le pod existe.
4. Vous avez supprimé le pod `web-...` : que s'est-il passé, et pourquoi ?
   >Le pod supprimé a été remplacé aussitôt par un nouveau, avec un nom différent (web-68d995574f-v5j9b est devenu web-68d995574f-28mhd). Le Deployment demande qu’il y ait toujours 1 pod en marche : quand il en manque un, Kubernetes le recrée automatiquement.

## Exercice 3 — Modifier un conteneur en direct

5. Après suppression du pod, votre message « Bienvenue... » a-t-il survécu ? Pourquoi ?
   >Non, le message a disparu : la page est revenue à celle par défaut de nginx. La modification avait été faite directement dans le conteneur. Quand le pod est supprimé, son contenu est perdu, et le nouveau pod repart de l’image d’origine.
6. Pourquoi le `port-forward` a-t-il cessé de fonctionner après la suppression du pod ?
   >Le port-forward était relié à l’ancien pod. Quand ce pod a été supprimé, le tunnel s’est retrouvé coupé et il ne bascule pas tout seul vers le nouveau pod, il faut donc le relancer.

## Exercice 4 — Déclaratif

7. Quelle différence entre `kubectl create` et `kubectl apply` ?
   >kubectl create crée un objet une seule fois, et échoue s’il existe déjà. kubectl apply crée l’objet s’il n’existe pas, ou le met à jour s’il existe.
8. À quoi sert le `selector` d'un Service ? Que se passe-t-il s'il ne correspond à aucun pod ?
   >Le selector indique au Service vers quels pods envoyer les requêtes. S’il ne correspond à aucun pod, le site est inaccessible.

## Exercice 5 — Scaling & mises à jour

9. Sur quels nœuds se trouvent vos 3 réplicas ? Collez la sortie de `kubectl get pods -l app=site -o wide`.
  NAME                    READY   STATUS    RESTARTS   AGE     IP           NODE               NOMINATED NODE   READINESS GATES
site-5cc664db9c-bnl9t   1/1     Running   0          2m15s   10.42.0.6    k3d-lab-agent-1    <none>           <none>
site-5cc664db9c-fg826   1/1     Running   0          2m2s    10.42.1.6    k3d-lab-server-0   <none>           <none>
site-5cc664db9c-lvzm2   1/1     Running   0          2m9s    10.42.2.10   k3d-lab-agent-0    <none>           <none>
10. Avec l'image `nginx:9.99`, quel statut avait le pod ? Quelle commande vous a donné la cause ?
    >Le nouveau pod était en erreur ImagePullBackOff : l’image nginx:9.99 n’existe pas, donc elle ne peut pas être téléchargée. La cause est donnée par kubectl describe pod, dans la partie « Events ».
11. Pendant la panne, le site restait-il accessible ? Pourquoi ?
    >Oui, le site restait accessible. Kubernetes garde les anciens pods en marche tant que les nouveaux ne sont pas prêts.

## Exercice 6 — Portainer & Mario

12. URL publique de votre Mario :
    >https://upgraded-space-succotash-q757744v55pw34qx-8585.app.github.dev/
13. Retrouvez le Deployment `mario` en ligne de commande. Portainer et `kubectl` parlent-ils au même composant ? Lequel ?
    >On retrouve le Deployment avec kubectl get deploy mario. Oui, Portainer et kubectl parlent au même composant : le serveur d’API de Kubernetes (API server). C’est pour cela que ce qui est créé dans Portainer est visible avec kubectl, et inversement.

## Bonus (facultatif)

14. Bonus réalisés et remarques :
    >

---

**Score obtenu avec `./scripts/check.sh` :** ... / ...
