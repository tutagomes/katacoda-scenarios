Agora, vamos fazer um deploy "de verdade", não apenas um contêiner manual.

Vamos primeiro apagar os deployments e serviços:

`kubectl delete deployment exemplo-laravel`{{execute}}

`kubectl delete services exemplo-laravel`{{execute}}

Vamos criar um arquivo `travel-deployment.yml` com o comando

`touch travel-deployment.yml`{{execute}}

 com o conteúdo:

```yaml
apiVersion: apps/v1 # for versions before 1.9.0 use apps/v1beta2
kind: Deployment
metadata:
  name: travel-deployment
spec:
  selector:
    matchLabels:
      app: travel
  replicas: 2 # tells deployment to run 2 pods matching the template
  template:
    metadata:
      labels:
        app: travel
    spec:
      containers:
      - name: travel-laravel
        image: registry.gitlab.com/arthurgomesfaria/laravelcompleto:master
        ports:
        - containerPort: 80
        lifecycle:
          postStart:
            exec:
              command: ["php", "artisan", "key:generate"]
```

E então executar o comando

`kubectl apply -f travel-deployment.yml` {{execute}}

Vamos ver então o status:

`kubectl get pods`{{execute}}

Deveria então haver 2 réplicas do serviço.

Podemos também alterar as réplicas no arquivo e executar novamente o:

`kubectl apply -f travel-deployment.yml` {{execute}}

E novamente, podemos acessar-los habilitando o port-foward:

`kubectl port-forward deployment/exemplo-laravel 9000:80 --address 0.0.0.0`{{execute}}

E então através da URL:

https://[[HOST_SUBDOMAIN]]-9000-[[KATACODA_HOST]].environments.katacoda.com/api/cafes

https://[[HOST_SUBDOMAIN]]-9000-[[KATACODA_HOST]].environments.katacoda.com