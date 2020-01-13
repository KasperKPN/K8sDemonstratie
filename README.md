# demo-omgeving

Bestemd voor de demo-omgeving voor Kubernetes presentaties

Voor het gebruik van deze omgeving is er toegang nodig tot AWS EKS.
Deze omgeving is uitsluitend bestemd voor ICT Consultants van de organisatie KPN ICT Consulting. (Bij een naamswijziging blijft het recht voor dezelfde organisatie bestaan.)


De omgeving kan momenteel (December 2019) uitgerold worden door gebruik te maken van eksctl. Zorg ervoor dat je verbinding kan maken vanuit je locale omgeving met AWS.
Daarnaast is een lokale instalatie en configuratie van kubectl nodig. 

Download of clone de gehele en nieuwste demo-omgeving van gitlab alvorens een demonstratie-omgeving te deployen.
Creëer het k8s cluster bij Amazon met het volgende commando in de Linux CLI of Windows Powershell:
eksctl create cluster -f %localpath%\demo-omgeving\k8sdemocluster.yaml

De volgende commando's kunnen in volgorde worden uitgevoerd in linuxshell of powershell:

#Zet het cluster op#
eksctl create cluster -f https://github.com/KasperKPN/K8sDemonstratiek8sdemocluster.yaml

#Simpele web applicatie uitrollen#
kubectl apply -f 'https://github.com/KasperKPN/K8sDemonstratie\resources\01 nginx_simple.yaml'
kubectl get deployments nginx-simpel-depl

#Update doorvoeren en terugdraaien#
kubectl apply -f 'https://github.com/KasperKPN/K8sDemonstratie\resources\02 nginx_simpleV2.yaml'
kubectl describe deployments nginx-simpel-depl
kubectl rollout undo deployments/nginx-simpel-depl
kubectl get deployments

#Schalen laten zien met nginx pods#
kubectl scale deployment nginx-simpel-depl --replicas=3
kubectl scale deployment nginx-simpel-depl --replicas=1
kubectl autoscale deployment nginx-simpel-depl --min=2 --max=6

#Commando op pod laten zien#
kubectl get pods
kubectl exec
-- hostname

#dashboard & accounts
kubectl apply -f https://github.com/KasperKPN/K8sDemonstratie\resources\dashboard\aio\deploy\recommended.yaml
kubectl apply -k https://github.com/KasperKPN/K8sDemonstratie\dashboard_toegang_adminrole
kubectl apply -k %localpath%\dashboard_toegang_Pod-reader

kubectl -n kubernetes-dashboard describe secret $(kubectl -n kubernetes-dashboard get secret | grep admin-user | awk '{print $1}')
start powershell {kubectl proxy --port=8080 --address='0.0.0.0'}
http://localhost:8080/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/#/login

#persistant volumes
kubectl apply -k https://github.com/KasperKPN/K8sDemonstratie\resources\wordpress
kubectl get service wordpress

kubectl apply -k https://github.com/KasperKPN/K8sDemonstratie\resources\gastboek
kubectl get service frontend
kubectl get all
kubectl apply -f  https://github.com/KasperKPN/K8sDemonstratie\resources\microservices-demo\deploy\kubernetes\namespace_sock-shop.yaml
kubectl apply -f  https://github.com/KasperKPN/K8sDemonstratie\resources\microservices-demo\deploy\kubernetes\complete-demo.yaml --validate=false

#volledige cluster verwijderen#
eksctl delete cluster --name=k8sdemocluster --region=eu-central-1
