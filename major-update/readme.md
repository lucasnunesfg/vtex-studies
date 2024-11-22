# Release New Major VTEX IO

## Tutorial de quando for preciso lançar uma nova versão da loja!

##  URL da documentação da VTEX

Para mais informações, consulte a 
[documentação oficial da VTEX](https://developers.vtex.com/docs/guides/vtex-io-documentation-migrating-cms-settings-after-major-update).

### Agora o passo a passo!

### Na maioria das vezes vai ser pedido uma nova versão quando for instalar um app aqui: 
     "peerDependencies": {
        "vtex.google-customer-reviews": "1.x"
    },

    - Dito isso o processo e comandos para publicar uma nova versão é parecido com o ** release patch stable **

    ** STEPS **

    1 - Feito as alterações
        -  git add .
        -  git commit -m "commit"
        -  git push
    
    obs: como vou saber que preciso lançar uma nova versão? 
        - Ao rodar o comando "vtex release patch stable" vai gerar um aviso/erro solicitando um update da major

    obs**: Ao rodar o vtex release patch stable o git push já é rodado tbm
  
    2 - Comando VTEX update Major

      - vtex release major stable
    
    obs: Quando fizer o update da major, a loja vai alterar o primeiro número da versão, exemplo:
        versão antiga: 1.0.36
        nova versão: 2.0.0
    
        - vtex deploy --force

    3 - Mudando para ambiente de produção

        -  vtex use prod --production
        -  vtex install

    4 - O passo mais importante (pode perder todo o conteúdo da loja)

        - Pode rodar o vtex browse e entrar no admin do ambiente prod e entrar em: ** configurações da loja -> GraphQl IDE **
        - Ou você pode rodar o comando -> ** vtex browse admin/graphql-ide ** que entra automaticamente
        - Dentro de GraphQl IDE, escolher a versão mais recente do app: ** vtex.pages-graphql@2.x. ** (pesquisar pages que aparece)
    
        -> Dito isso, vamos rodar o comando para pegar a versão antiga e lançar na nova
            - o comando para fazer isso é esse (está na doc da vtex)
                 mutation{
                    updateThemeIds(from:"{appVendor}.{appName}@{oldMajor}.x", to:"{appVendor}.{appName}@{newMajor}.x")
                }

        obs: Pode tirar os colchetes e o appVendor e appName estão no manifest.json da loja, ** copiar exatamente igual**
            ->  "vendor": "",
                "name": "",
        
            -  O oldMajor depende da última versão da loja, exemplo: se estava na 1.0.36 você pode colocar 1.x, o "x" pega a versão mais atualizada da versão 1.0.0 da loja, e a new major você põe a nova versão, no exemplo 2.x. Na próxima versão dessa loja por exemplo, a oldMajor pode ser 2.x e a newMajor 3.x.

            obs: Um ponto importante, a versão nova precisa estar exatamente igual à última versão da loja, então caso não esteja, algo está errado e precisa verificar os passos anteriores. (a oldMajor e newMajor) pode ser o problema.

            - Substituiu, na IDE tem um play, basta clicar e esperar a resposta:
              - > resposta esperada: 
                        {
                            "data": {
                                "updateThemeIds": true
                            }
                        }
                - Apos isso entrar na url prod--loja... e verificar se tudo está ok igual a loja que está em produção/ar, se estiver tudo ok, ir para o próximo passo.
    
    5 - vtex promote 
        - vai gerar um link e por precaução verificar novamente e caso esteja tudo certo, (y).


## new major concluído



