E como fica o monitoramento?

Podemos utilizar de forma simples a Dashboard do Minikube, habilitando-a através do comando:

`minikube addons enable dashboard`{{execute}}

E então disponibilizamos para o mundo através do deploy do serviço (Só deve ser utilizado no Katacoda):

`kubectl apply -f /opt/kubernetes-dashboard.yaml`{{execute}}

E podemos ver a situação da execução da dashboard:

`kubectl get pods -n kubernetes-dashboard -w`{{execute}}

Finalmente, podemos acessá-la através da url:

https://[[HOST_SUBDOMAIN]]-30000-[[KATACODA_HOST]].environments.katacoda.com

