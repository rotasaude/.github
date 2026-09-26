# .github — padrões da organização rotasaude

Repositório especial do GitHub: o que está em `.github/` aqui vale como padrão
para **todos os repositórios da org** que não tiverem o próprio. Hoje isso são
os cinco templates de issue.

## A organização

O Rota Saúde faz a triagem de saúde do cidadão em nome da prefeitura e leva o
resultado até o atendimento presencial. Cada cidade tem banco próprio
([ADR 0020](https://github.com/rotasaude/docs/blob/main/adr/0020.md)). Um repo
por app, mais contratos e documentação
([ADR 0002](https://github.com/rotasaude/docs/blob/main/adr/0002.md)):

| Repo | Papel |
|---|---|
| [`api`](https://github.com/rotasaude/api) | Backend único (Rails 8): regra de negócio, bancos, filas e todas as APIs |
| [`wpda`](https://github.com/rotasaude/wpda) | Canal web do cidadão e relatório público por token |
| [`dashboard`](https://github.com/rotasaude/dashboard) | Painel da cidade: operação, protocolos assinados, equipe, atendimento presencial |
| [`admin`](https://github.com/rotasaude/admin) | Console da plataforma: catálogo e provisionamento de cidades, entrada numa cidade por grant |
| [`maintenance`](https://github.com/rotasaude/maintenance) | Acesso técnico a todas as cidades via GraphQL; só development/staging |
| [`contracts`](https://github.com/rotasaude/contracts) | Contratos compartilhados: eventos, schema de protocolo, tipos, design tokens |
| [`docs`](https://github.com/rotasaude/docs) | ADRs, specs, planos, módulos, ciclo de desenvolvimento |
| **`.github`** (este) | Padrões da org: templates de issue |

O README de cada repo descreve o papel dele, como rodar em dev e as suas
armadilhas. Comece pelo [`docs`](https://github.com/rotasaude/docs).

## O que tem aqui

| Arquivo | O que faz |
|---|---|
| [`feature.yml`](.github/ISSUE_TEMPLATE/feature.yml) | **Funcionalidade** (`F-NN.M`, label `type:feature`): passo 1 do [ciclo de desenvolvimento](https://github.com/rotasaude/docs/blob/main/ciclo-desenvolvimento.md). Pede F-ID, módulo, apps tocadas, ADRs, critério de aceite, camadas de teste, out-of-scope, dependências e release-alvo |
| [`adr.yml`](.github/ISSUE_TEMPLATE/adr.yml) | **ADR** (`type:adr`): decidir, não implementar. Fecha quando `adr/NNNN.md` entra no `docs` |
| [`desvio.yml`](.github/ISSUE_TEMPLATE/desvio.yml) | **Desvio** (`type:drift`): divergência entre ADR e código, com evidência, gravidade e eixo de risco |
| [`pulse.yml`](.github/ISSUE_TEMPLATE/pulse.yml) | **Pulse semanal** (`type:pulse`): discussão do relatório semanal de drift; fecha quando os desvios viram issues próprias |
| [`verificacao.yml`](.github/ISSUE_TEMPLATE/verificacao.yml) | **Verificação humana** (`type:verification`): audita uma funcionalidade `Done` e decide se ela vai para `Verified` |

## Como o trabalho é rastreado

- Toda funcionalidade é uma issue com F-ID, espelhando
  [`funcionalidades-mvp.csv`](https://github.com/rotasaude/docs/blob/main/funcionalidades-mvp.csv)
  e os [módulos](https://github.com/rotasaude/docs/blob/main/modulos/README.md).
- As issues entram no **GitHub Project #1** da org. O status anda por
  `Not Started` → `In Progress` → `Done` → `Verified`; só um humano promove
  para `Verified`.
- **Labels** (os mesmos 31 em cada repo de app, `contracts` e `docs`):
  `mod-01`…`mod-14`, `app:api`, `app:admin`, `app:dashboard`, `app:wpda`,
  `app:maintenance`, `app:contracts`, `app:docs`, `type:feature`,
  `type:chore`, `type:hotfix`, `type:adr`, `type:drift`, `type:pulse`,
  `type:verification` e `risco:baixo|medio|alto`.
- Funcionalidade que toca mais de um repo vira **uma issue por repo**, com o
  mesmo F-ID no título e referências cruzadas.
- Referência a ADR vai sempre por **URL completa** do repo `docs`, nunca por
  caminho relativo: código e ADR vivem em repos diferentes.

## Convenções da org

- **Commits** em inglês, no formato Conventional Commits, com o tipo por
  extenso (`feat`, `fix`, `refactor`, `docs`, `test`, `chore`…).
- **Repos públicos.** Nenhum segredo entra em repo: `master.key`, `.env` e
  secrets de deploy ficam fora, e a CI lê secrets do repositório.
- **Contrato, não código:** as apps compartilham o que está em `contracts`,
  nunca código entre repos
  ([ADR 0015](https://github.com/rotasaude/docs/blob/main/adr/0015.md)).
