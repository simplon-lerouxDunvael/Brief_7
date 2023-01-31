---
marp: true
theme: gaia
markdown.marp.enableHtml: true
paginate: true
---
<!-- backgroundImage: "linear-gradient(to bottom, #ffb7c5, #DCD0FF)" -->

<!--
_color: black
-->

# Présentation Brief 6
#### Mise à jour du service applicatif Voting App via Azure DevOps Pipeline
Dunvael 

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![width:700px height:300px]()


<!-- paginate: false -->

---
# SOMMAIRE  
1 - Projet
2 - Topologie
3 - Déploiement via le pipeline Azure DevOps
4 - Outils, logiciels et ressources
5 - 


3 - Nombre de scripts
4 - Plan projet prévu VS plan projet exécuté
6 - Compréhension des outils et des logiciels
7 - Difficultés rencontrées
8 - Solutions trouvées
9 - DAT et Executive Summary

Ressources utilisées


<!-- paginate: true -->
<!--
_color: black
-->

---
## 1 - Ressources utilisées  
* Documentation Microsoft Azure
* Documentation Kubernetes
* Portail Azure pour l'interface graphique
* Cloudshell du portail Azure
* Moteur de recherche Google
* Visual Studio Code
* Github
* *L'aide précieuse de Luna et Quentin*

<!--
_color: black
-->

---
## 2 - Topologie

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![width:500px height:500px](https://user-images.githubusercontent.com/108001918/196372669-f0cf2f50-dd24-4ff1-88b6-ed350f2249f7.png)

<!--
_color: black
-->

---
## 3 - Nombre de scripts

Un script principal et un script de suppression des ressources présents dans les deux parties.

Plusieurs scripts pour la part 2 afin de pouvoir tester et m'assurer que chaque manifest fonctionne.
Permet de  définir et d'enchaîner les étapes importantes du brief 6, mais aussi de faciliter la lisibilité et la modification des scripts.
* Part 1 : un script principal
* Part 2 : deux scripts Ingress, un script Issuer, un script principal, un script Certif, un script auto-scaling, le graph loadtester
  
<!--
_backgroundColor: black
_color: black
-->

--- 
## 4 - Plan projet prévu VS plan projet exécuté 
|  | *Plan projet prévu* | *Plan projet exécuté* |
|-----------|:----------:|:------------:|
| GR|&#9745;|&#9745;|
| Clés SSH |&#9745;|&#9745;|
| Images container |&#9745; |&#9745;|
| Load Balancer |&#9745;|&#9745;|
| ClusterIP |&#9745;|&#9745;|
| Secret Kubernetes |&#9745;|&#9745;|
| Storage account |&#9745;|&#9745;| 

<!--
_color: black
-->

--- 
|  | *Plan projet prévu* | *Plan projet exécuté* |
|-----------|:----------:|:------------:|
| Persistent Volume |&#9745;|&#9745;|
| Persistent Volume Claim |&#9745;|&#9745;|
| Ingress |&#9745;|&#9745;|
| Nginx |&#9745;|&#9745;|
| Cert-manager |&#9745;|&#9745;|
| Certificat TLS |&#9745;|&#9745;|
| DNS |&#9745;|&#9745;|
| Load tester |&#9745;|&#9745;|
<!--
_color: black
-->

--- 

|  | *Plan projet prévu* | *Plan projet exécuté* |
|-----------|:----------:|:------------:|
| Executive summary |&#9745;|&#9745;|
| DAT |&#9745;|&#9745;|
| Explications fonctionnement Kubernetes |&#9745;|&#9745;|
| Méthodologie Scrum |&#9745;|&#9745;|

<!--
_color: black
-->

---
## 5 - Tâches supplentées

Certaines tâches de la partie 2 du Brief 6 ont été supplentées afin de palier le manque de temps et permettre de réaliser les bonus.
* App Gateway avec Agic-on
* Certbot

<!--
_color: black
-->

---
## 6 - Compréhension des outils et des logiciels
***Kubernetes*** = 
* Plateforme de déploiement et de gestion d’infrastructures résilientes, auto-réparatrices et à haute disponibilité.
Possibilité d’augmenter ou de réduire le nombre de ressources (haute modulabilité).
* Création de pools de nodes dans un cluster. Chaque node ayant son Kubelet agissant comme proxy pour les pods présents en leur sein.
* Le DNS intégré permet aux services de communiquer avec les pods. Ceux-ci communiquent également entre eux grâce aux Kubelets et à leurs services attribués.

<!--
_color: black
-->

---

***Ingress*** = 
* L'Ingress (ou entrée réseau), intégré à Kubernetes, expose les routes HTTP et HTTPS de l'extérieur du cluster à des services au sein du cluster. 
* Le routage du trafic est contrôlé par des règles définies sur la ressource Ingress.
* Un Ingress peut être configuré pour donner aux services des URLs HTTPS accessibles de l'extérieur, un load balancer et un DNS. Un contrôleur d'Ingress est responsable de l'exécution de l'Ingress.

<!--
_color: black
-->

---
## 6 - Difficultés  rencontrées
J'ai rencontré plusieurs difficultés :

* Difficultés avec le certificat TLS et de la connexion au nom de domaine en https
* Gestion du pipeline avec le script en Bash

<!--
_color: black
-->

---
## 7 - Solutions trouvées 
Afin de palier aux difficultés, j'ai cherché des solutions, me suis remise en question et ai adopté différents comportements :

* Communications avec les autres membres de la formation
* Des temps de pause (câlins et bisous au chaton par exemple ou pause chocolat)

<!--
_color: black
-->

---
## 8 - DAT et Executive Summary

[Document d'Architecture Technique](https://github.com/simplon-lerouxDunvael/Brief_7/blob/main/Docs/DAT.md)

[Executive summary](https://github.com/simplon-lerouxDunvael/Brief_7/blob/main/Docs/Executive_summary.docx)

<!--
_color: black
-->

---
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![Merci_-_Noir](https://user-images.githubusercontent.com/108001918/196387576-cfcdcdda-7a6b-4021-ab84-3a06ebc95ab6.png)

<!--
_color: black
-->