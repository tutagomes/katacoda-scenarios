## Parabéns!

Você criou sua primeira imagem Docker do zero — da aplicação ao container rodando!

### Resumo dos comandos aprendidos

**Build:**

```bash
docker build -t <nome>:<tag> .     # Constrói uma imagem a partir do Dockerfile na pasta atual
docker build -t <nome>:<tag> -f <caminho/Dockerfile> .   # Especifica um Dockerfile diferente
docker images                       # Lista todas as imagens disponíveis localmente
docker rmi <nome>:<tag>             # Remove uma imagem
```

**Execução:**

```bash
docker run -d --name <nome> -p 9090:8080 <imagem>:<tag>          # Roda a imagem como container
docker run -d --name <nome> -p 9090:8080 -v ${PWD}:/caminho <imagem>  # Com volume montado
docker logs <nome>                  # Exibe os logs do container
docker logs -f <nome>               # Logs em tempo real
```

**Inspeção:**

```bash
docker history <imagem>:<tag>       # Mostra as camadas da imagem e seus tamanhos
docker history --no-trunc <imagem>  # Camadas com comandos completos (sem truncar)
docker inspect <imagem>:<tag>       # JSON completo com toda a configuração da imagem
docker inspect --format='...' <img> # Filtra campos específicos do inspect
docker image prune -a               # Remove todas as imagens não utilizadas
```

**Referência rápida das instruções do Dockerfile:**

```dockerfile
FROM <imagem>:<tag>                    # Imagem base
WORKDIR /caminho                       # Diretório de trabalho
COPY <origem> <destino>                # Copia arquivos da máquina para a imagem
RUN <comando>                          # Executa comando durante o build
EXPOSE <porta>                         # Documenta a porta que a aplicação usa
ENV VARIAVEL=valor                     # Define variável de ambiente
LABEL chave="valor"                    # Adiciona metadados à imagem
CMD ["arg"]                            # Argumentos padrão (substituível no docker run)
ENTRYPOINT ["executavel"]              # Executável fixo do container
HEALTHCHECK --interval=30s CMD <cmd>   # Verifica saúde da aplicação periodicamente
```

**CMD vs ENTRYPOINT:**

```dockerfile
# Executável fixo + argumento padrão substituível
ENTRYPOINT ["dotnet"]
CMD ["MinhaApi.dll"]
# docker run imagem OutroApp.dll  → executa: dotnet OutroApp.dll
```

### O que você construiu

```
package.json + server.js + Dockerfile
         ↓ docker build
    imagem: minha-app:1.0
         ↓ docker run
    container rodando na porta 9090
```

### Próximos passos sugeridos

- **Docker: Redes** — faça containers conversarem entre si
- **Docker: Volumes e Persistência** — salve dados de bancos de dados entre reinicializações
- **Docker Compose** — orquestre múltiplos containers com um único arquivo
