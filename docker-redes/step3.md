## Isolamento entre redes

Uma das propriedades mais importantes das redes Docker é o **isolamento**: containers em redes diferentes não conseguem se comunicar entre si, mesmo que estejam no mesmo host. Isso é fundamental para segurança em ambientes com múltiplos projetos ou camadas de serviço.

### Criando duas redes separadas

`docker network create rede-front`{{execute}}

`docker network create rede-back`{{execute}}

### Subindo containers em redes diferentes

`docker run -d --name front --network rede-front nginx`{{execute}}

`docker run -d --name back --network rede-back nginx`{{execute}}

### Confirmando o isolamento

O container `front` na `rede-front` **não deve conseguir** alcançar o container `back` na `rede-back`:

`docker run --rm --network rede-front alpine wget -q -T 3 -O- http://back 2>&1 || echo "Falhou: redes diferentes não se enxergam"`{{execute}}

Como esperado: o DNS interno não resolve nomes de containers que estão em outra rede. O container `back` simplesmente não existe para quem está na `rede-front`.

### Criando uma ponte entre as redes

Em alguns casos, um container precisa fazer parte de duas redes — por exemplo, um servidor de API que precisa falar tanto com o frontend quanto com o banco de dados:

`docker run -d --name api-gateway --network rede-front nginx`{{execute}}

`docker network connect rede-back api-gateway`{{execute}}

Agora o `api-gateway` tem um pé em cada rede. Vamos confirmar:

`docker exec api-gateway wget -q -O- http://front > /dev/null && echo "Alcançou front: OK"`{{execute}}

`docker exec api-gateway wget -q -O- http://back > /dev/null && echo "Alcançou back: OK"`{{execute}}

Enquanto isso, `front` continua sem alcançar `back` diretamente:

`docker run --rm --network rede-front alpine wget -q -T 3 -O- http://back 2>&1 || echo "front → back: ainda bloqueado"`{{execute}}

### Um padrão comum de arquitetura

Esse isolamento permite um padrão seguro muito usado:

```
[browser]
    ↓
[rede-front]
    ↓
[container: frontend] ←→ [container: api-gateway]
                                    ↓
                            [rede-back]
                                    ↓
                          [container: banco-de-dados]
```

O banco de dados fica em uma rede privada inacessível diretamente do frontend — apenas a API pode acessá-lo.

### Limpando

`docker rm -f front back api-gateway`{{execute}}

`docker network rm rede-front rede-back`{{execute}}
