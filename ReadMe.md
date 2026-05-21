#===================================================
# Déploiement garantie sous docker v4.27.2
#===================================================
- Comment builder les images :
  Dans l'emplacement <Chemin_d'import>/projet-conteneurisation/appscore, executer le fichier docker-compose.yaml par la commande : docker-compose build 
  Dans l'emplacement <Chemin_d'import>/projet-conteneurisation/appscore, executer le fichier Dockerfile-fluentd par la commande : docker build -f Dockerfile-fluentd .

- Comment pousser les images en local :
  Executer les commandes : docker run -d -p 5000:5000 --name registry registry:2
                           docker tag applicants-api:latest localhost:5000/applicants-api:latest
                           docker tag service-api-jobs:latest localhost:5000/service-api-jobs:latest
                           docker tag service-api-identity:latest localhost:5000/service-api-identity:latest
                           docker tag mssql-linux:latest localhost:5000/mssql-linux:latest
                           docker tag web:latest localhost:5000/web:latest
  
- Forcer docker à utiliser le bon contexte WSL : docker context use default

- Comment déployer le projet sur un cluster vierge
  Installer le gestionnaire de paquets kubernetes-helm : 
  
       (Windows) -> choco install kubernetes-helm 
	   
       (Linux)   -> sudo apt-get install curl gpg apt-transport-https --yes
                     curl -fsSL https://packages.buildkite.com/helm-linux/helm-debian/gpgkey | gpg --dearmor | sudo tee /usr/share/keyrings/helm.gpg > /dev/null
                     echo "deb [signed-by=/usr/share/keyrings/helm.gpg] https://packages.buildkite.com/helm-linux/helm-debian/any/ any main" | sudo tee /etc/apt/sources.list.d/helm-stable-debian.list
                     sudo apt-get update
                     sudo apt-get install helm
					 
	     (MacOS)   -> git clone https://github.com/helm/helm.git	   
                     cd helm
                     make
  
  Dans le repertoire <Chemin_d'import>/projet-conteneurisation/, lancer la commande : helm install projetconteneurisation-release helm_projet-conteneurisation/


- Comment accéder aux différentes applications (URL, ports, ingress)
  Si pas déja fait, entrez les dns suivant dans le fichier hosts : webapp.local, prometheus.local, kibana.local (en 127.0.0.1)
  Taper dans un navigateur web l'url : https//webapp.local, https//prometheus.local, https//kibana.local (port 443 implicite, qui sera redirigée par l'ingress vers les differents services)
	
	
- Comment desinstaller le projet : helm uninstall projetconteneurisation-release

- Comment vérifier l’état du cluster (commandes kubectl, dashboards…)
  Voir tous les pods/services/pvcs/ingress crees 
    kubectl get all; kubectl get pvc ; kubectl get ingress

  Voir l'historique de deploiement 
    helm history projetconteneurisation-release 

  Voir l'etat de du deploiement helm 
    helm ls 

  Voir la conf d'un pod 
    kubectl describe pod id_pod

  Voir les logs d'un pod 
    kubectl logs id_pod

Présentation du projet : https://www.canva.com/design/DAG_bMOTrPs/uE189OtI_rhDhktSsuyAzQ/edit
