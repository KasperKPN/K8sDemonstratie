# demo-omgeving

Bestemd voor de demo-omgeving voor Kubernetes presentaties

Voor het gebruik van deze omgeving is er toegang nodig tot AWS EKS.
Deze omgeving is uitsluitend bestemd voor ICT Consultants van de organisatie KPN ICT Consulting. (Bij een naamswijziging blijft het recht voor dezelfde organisatie bestaan.)


De omgeving kan momenteel (December 2019) uitgerold worden door gebruik te maken van eksctl. Zorg ervoor dat je verbinding kan maken vanuit je locale omgeving met AWS.
Daarnaast is een lokale instalatie en configuratie van kubectl nodig. 

Creëer het k8s cluster bij Amazon met het volgende commando in de Linux CLI of Windows Powershell:

#de commando's in dit document kunnen in de linuxshell of powershell uitgevoerd worden.

#Zet het cluster op#
eksctl create cluster -f C:\Users\dijk595\Desktop\eks\k8sdemocluster.yaml

#schalen laten zien met nginx pods#
kubectl apply -f 'https://raw.githubusercontent.com/KasperKPN/K8sDemonstratie/master/resources/01.nginx_simple.yaml'

kubectl get deployments nginx-simpel-depl

kubectl apply -f 'https://raw.githubusercontent.com/KasperKPN/K8sDemonstratie/master/resources/02.nginx_simpleV2.yaml'

kubectl describe deployments nginx-simpel-depl
kubectl rollout undo deployments/nginx-simpel-depl


kubectl get deployments

kubectl scale deployment nginx-simpel-depl --replicas=3

kubectl scale deployment nginx-simpel-depl --replicas=1

kubectl autoscale deployment nginx-simpel-depl --min=2 --max=6


#commando op pod laten zien#

kubectl get pods

kubectl exec 
-- hostname

#dashboard & accounts

kubectl apply -f https://raw.githubusercontent.com/KasperKPN/K8sDemonstratie/master/resources/dashboard/aio/deploy/recommended.yaml


#Admin rol rechten geven voor consultants#

kubectl apply -f https://raw.githubusercontent.com/KasperKPN/K8sDemonstratie/master/dashboard_toegang_adminrole/01_dashboard_serviceaccount.yaml
kubectl apply -f https://raw.githubusercontent.com/KasperKPN/K8sDemonstratie/master/dashboard_toegang_adminrole/02_dashboard_adminrol.yaml

#Pod-lees-role aanmaken voor sales#

kubectl apply -f https://raw.githubusercontent.com/KasperKPN/K8sDemonstratie/master/dashboard_toegang_Pod-reader/01_pod-read-role-dashboard.yaml
kubectl apply -f https://raw.githubusercontent.com/KasperKPN/K8sDemonstratie/master/dashboard_toegang_Pod-reader/02_read-serviceaccount-dashboard.yaml
kubectl apply -f https://raw.githubusercontent.com/KasperKPN/K8sDemonstratie/master/dashboard_toegang_Pod-reader/03_pod-read-rolebind-dashboard.yaml


kubectl -n kubernetes-dashboard describe secret $(kubectl -n kubernetes-dashboard get secret | grep admin-user | awk '{print $1}')

#De Commands voor MS Powershell om de omgeving te starten.  Bij Linux open je een nieuwe terminal, en voer het proxy commando uit. Start de URL handmatig in je browser.
start powershell {kubectl proxy --port=8080 --address='0.0.0.0'}
start-process "chrome.exe" "http://localhost:8080/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/#/login"

#persistant volumes
kubectl apply -k https://raw.githubusercontent.com/KasperKPN/K8sDemonstratie/master/resources/wordpress

kubectl get service wordpress


kubectl apply -f https://raw.githubusercontent.com/KasperKPN/K8sDemonstratie/master/resources/gastboek/01_redis-mas-serv.yaml
kubectl apply -f https://raw.githubusercontent.com/KasperKPN/K8sDemonstratie/master/resources/gastboek/02.redis-mas-dep.yaml
kubectl apply -f https://raw.githubusercontent.com/KasperKPN/K8sDemonstratie/master/resources/gastboek/03.redis-slave-serv.yaml
kubectl apply -f https://raw.githubusercontent.com/KasperKPN/K8sDemonstratie/master/resources/gastboek/04.redis-slave-dep.yaml
kubectl apply -f https://raw.githubusercontent.com/KasperKPN/K8sDemonstratie/master/resources/gastboek/05.redis-front-serv.yaml
kubectl apply -f https://raw.githubusercontent.com/KasperKPN/K8sDemonstratie/master/resources/gastboek/06.redis-front-dep.yaml




kubectl get service frontend

kubectl get all

kubectl apply -f  https://raw.githubusercontent.com/KasperKPN/K8sDemonstratie/master/resources/microservices-demo/deploy/kubernetes/namespace_sock-shop.yaml
kubectl apply -f  https://raw.githubusercontent.com/KasperKPN/K8sDemonstratie/master/resources/microservices-demo/deploy/kubernetes/complete-demo.yaml --validate=false

eksctl delete cluster --name=k8sdemocluster --region=eu-central-1
