Vamos então experimentar um pouco com o rollback:

Vamos alterar o conteúdo do nosso deploy para:

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
        image: registry.gitlab.com/arthurgomesfaria/laravelcompleto:latest
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



Tudo certo, correto? Agora, imagine que houve algum problema com o deploy, é possível reverter para a versão antiga de forma bem simples:

`kubectl rollout history deployment travel-deployment`{{execute}}

`kubectl rollout history deployment travel-deployment --revision=2`{{execute}}

`kubectl rollout undo deployment travel-deployment`{{execute}}

Ou caso queira fazer um rollback para uma versão específica:

`kubectl rollout undo deployment travel-deployment --to-revision=1`{{execute}}



E novamente, podemos acessar-los habilitando o port-foward:

`kubectl port-forward deployment/exemplo-laravel 9000:80 --address 0.0.0.0`{{execute}}



E então através da URL:

https://[[HOST_SUBDOMAIN]]-9000-[[KATACODA_HOST]].environments.katacoda.com/api/cafes

https://[[HOST_SUBDOMAIN]]-9000-[[KATACODA_HOST]].environments.katacoda.com