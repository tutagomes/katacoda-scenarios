Ok, agora com todos os testes executados, como faço para ter uma visão mais granular do que pode estar acontecendo com minha aplicação?

E é aí que entra o Telescope!
https://[[HOST_SUBDOMAIN]]-8080-[[KATACODA_HOST]].environments.katacoda.com/telescope

Ele permite visualizar cada request, cada query e cada job executado, armazenando informações vitais para um bom debug de o que pode estar acontecendo por debaixo dos panos.

E claro, também é possível monitorar o uso de CPU e memória com a ferramenta HTOP:
`htop`{{execute}}
