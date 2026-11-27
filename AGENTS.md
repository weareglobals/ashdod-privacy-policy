# AGENTS.md

<!-- codex-review:start - padrao da Globals (weareglobals/.github modelos/codex/regras-base.md). Nao edite este bloco aqui: ele e regravado pelo codex-regras.sh. Regras proprias do repo vao depois do fim do bloco. -->

## Code Review Rules

Escreva os comentarios da revisao em portugues. Os sistemas da Globals movimentam dinheiro e dados pessoais de clientes de varios parceiros: na duvida entre P1 e P2 num dos casos abaixo, trate como P1.

### Regras da Globals (todos os repositorios)

- P1: segredo, token, senha, chave privada ou dado pessoal (CPF, CNPJ, telefone, e-mail, documento) no codigo, em teste, em log, em mensagem de erro, em resposta de API ou em arquivo de configuracao versionado.
- P1: dado de um cliente, parceiro, organizacao ou usuario lido, alterado ou listado a partir de outro. Id vindo da URL ou do corpo so e usado depois de confirmar que o recurso pertence a quem esta na sessao.
- P1: autenticacao ou permissao enfraquecida: rota publica nova, papel ampliado, checagem removida, assinatura de webhook ou token deixando de ser validado.
- P1: entrada externa sem validacao chegando a SQL, comando de shell, caminho de arquivo, HTML ou URL de requisicao de saida (injecao, XSS, SSRF).
- P1: valor monetario em ponto flutuante. Aceito: centavos inteiros ou decimal com arredondamento explicito.
- P1: webhook, fila, job ou nova tentativa que nao e idempotente: o mesmo evento duas vezes duplica ou perde efeito (cobranca, lancamento, mensagem, registro).
- P1: migracao de banco que quebra a versao anterior durante o deploy (coluna removida, renomeada ou NOT NULL sem default) ou migracao de dado sem filtro restrito.
- P1: fluxo que deixa usuario ou registro preso: estado sem saida, erro sem mensagem, operacao sem estado final.

### Infraestrutura (Terraform, Kubernetes, ArgoCD, CI)

- P1: recurso com estado destruido ou substituido (banco, bucket, chave, volume, segredo), ou protecao removida (`prevent_destroy`, deletion protection, backup, versionamento, retencao).
- P1: permissao ampliada (`*` em acao ou recurso, principal externo, condicao removida), rede aberta para `0.0.0.0/0` fora do balanceador publico, ou segredo em tfvars, manifesto ou variavel de pipeline.
- P1: imagem por tag mutavel (`latest`) em producao, ou deploy de producao que pula a aprovacao ou o check obrigatorio.
<!-- codex-review:end -->
