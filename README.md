# App Geek — inscrições por QR Code e caixa das barracas para eventos acadêmicos

![Next.js](https://img.shields.io/badge/Next.js_15-000000?logo=nextdotjs&logoColor=white)
![React](https://img.shields.io/badge/React_19-20232A?logo=react&logoColor=61DAFB)
![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?logo=typescript&logoColor=white)
![Node.js](https://img.shields.io/badge/Node.js-339933?logo=nodedotjs&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?logo=postgresql&logoColor=white)
![Testes](https://img.shields.io/badge/testes-427_automáticos_+_28_casos-2ea44f)

Sistema web full-stack criado para o **Geek Friday / HallowGeek** da Faculdade Uniguaçu. Ele resolve dois problemas do evento:

1. **Saber exatamente quem passou pelo evento:** a inscrição é feita pelo celular, lendo um QR Code que muda sozinho. Esses registros servem de base para emitir certificados com horas para os alunos.
2. **Controlar as vendas das barracas dos cursos:** cada curso tem o seu caixa (PDV). Ele registra vendas, calcula o troco, abre e fecha turnos e emite um relatório em formato de recibo.

> 🔒 **O código-fonte é privado.** Esta página apresenta o projeto: telas, arquitetura, decisões técnicas, requisitos e testes. Posso mostrar o código em entrevista.

---

## Telas

### Inscrição do participante

| QR Code dinâmico (telão do evento) | Cadastro no celular | QR vencido ou já usado |
|---|---|---|
| ![QR Code dinâmico](docs/img/02-qr-code.png) | ![Cadastro no celular](docs/img/03-cadastro-celular.png) | ![QR Code expirado](docs/img/14-qr-expirado-celular.png) |

O QR é trocado de tempos em tempos. Depois de lido, a pessoa tem 3 minutos para concluir o cadastro, e cada aparelho só cadastra uma vez.

### Painel da organização

![Painel de cadastros](docs/img/01-painel-admin.png)

O painel mostra indicadores em tempo real, gráficos por tipo de participante e por curso, além de busca e filtros. Também permite editar cadastros, exportar CSV, fazer backup, ligar ou desligar as inscrições e ajustar a validade do QR.
*(Dados de demonstração. Os nomes são fictícios.)*

### Módulo Caixa das Barracas

| Escolha do curso | Central da organização |
|---|---|
| ![Escolha do curso](docs/img/04-caixa-escolha-curso.png) | ![Central do caixa](docs/img/05-caixa-central.png) |

**Venda, com comanda, código de cada produto e troco sugerido em notas:**

![Tela de venda](docs/img/06-caixa-vender.png)

**Turno de caixa:** abertura com fundo de troco, relatórios guardados por turno, gráficos de vendas por forma de pagamento e por produto, sangria e suprimento, e fechamento com conferência da gaveta.

![Turno de caixa](docs/img/07-caixa-turno.png)

**Gestão das barracas pela organização:** uma barraca por curso e período, e exclusão de barraca com histórico protegida por nome, senha do master e confirmação.

| Nova barraca (curso e período) | Excluir com histórico |
|---|---|
| ![Nova barraca](docs/img/15-nova-barraca-periodo.png) | ![Excluir com histórico](docs/img/16-excluir-com-historico.png) |

| Recibo do turno (imprimir/PDF) | No celular |
|---|---|
| ![Recibo](docs/img/08-caixa-recibo.png) | ![Caixa no celular](docs/img/09-caixa-celular.png) |

### Segurança e controle de acesso

Área só do administrador *master*, com grupos de usuários e o que cada grupo pode fazer.

| Permissões de um grupo | Barraca própria do usuário |
|---|---|
| ![Permissões do grupo](docs/img/10-seguranca-grupo.png) | ![Barraca própria](docs/img/13-barraca-propria.png) |

**Coordenador de curso:** entra com o próprio login, não vê o painel de cadastros e cai direto nas barracas **do curso dele** (todos os períodos), com o total do curso e "Operar caixa" em cada barraca.

![Coordenador de curso](docs/img/18-coordenador-curso.png)

| Curso coordenado do usuário |
|---|
| ![Curso coordenado](docs/img/19-curso-coordenado.png) |

**Usuários:** busca, filtros, quem está online, última atividade, editar, definir senha nova e excluir.

![Usuários](docs/img/11-seguranca-usuarios.png)

**Preparar o próximo evento:** limpa cadastros e caixa com senha do master, confirmação digitada e backup completo antes.

![Limpar tudo](docs/img/12-limpar-tudo.png)

---

## Funcionalidades

**Inscrições**
- QR Code dinâmico com validade configurável e token temporário assinado.
- Cadastro de universitários (curso e período), professores e visitantes.
- Bloqueio de cadastro duplicado por aparelho.
- Validação do nome do participante (só letras, números e espaços) no servidor.
- Tela de aviso no tema do evento quando o QR está vencido ou já foi usado.
- Painel com gráficos, busca, filtros, edição e exportação CSV.
- Vários administradores, com indicação de quem está online e um administrador *master*.
- Backup e limpeza protegida dos dados ao fim do evento.

**Caixa das Barracas**
- Login próprio para cada curso, com um link exclusivo para cada barraca.
- Cadastro de produtos com categoria, estoque opcional e opção de pausar a venda.
- Venda em dinheiro, Pix, crédito ou débito. O troco aparece com sugestão de notas.
- Turnos de caixa com fundo de troco, sangria e suprimento. No fechamento, o sistema calcula o esperado, recebe o valor contado e mostra a diferença.
- Relatório de cada turno no formato de recibo (imprimir ou PDF) e exportação CSV compatível com o Excel.
- Central da organização com o total do evento, as vendas por forma de pagamento e a situação de cada barraca.
- Uma barraca por curso e período, com link de login próprio. Barracas antigas continuam funcionando até o período ser definido.
- Gestão das barracas pela organização: criar, editar curso, período e usuário, trocar a senha do curso e excluir. Barraca com vendas só sai com "Excluir com histórico", que pede o nome da barraca, a senha do master e a confirmação.
- Controle de estoque opcional por produto.
- Código automático para cada produto (001, 002…), que aparece na venda, no recibo e no CSV.
- Gráficos do turno: vendas por forma de pagamento (rosca) e por produto (barras).

**Segurança e controle de acesso**
- Grupos de usuários (ex.: Portaria, Coordenador, Professor, Aluno/Monitor) com cada funcionalidade ligada ou desligada pelo master.
- Chaves de acesso por grupo: entrar no painel de cadastros e entrar no caixa das barracas.
- Barraca própria: o usuário fica ligado a uma barraca e vende só nela, com o próprio login.
- Coordenador de curso: o usuário fica ligado a um curso e vê e opera todas as barracas desse curso (todos os períodos), sem o painel de cadastros e sem as barracas dos outros cursos.
- Gestão de usuários: online agora, última atividade, editar nome e grupo, senha nova (encerra as sessões), desconectar e excluir.
- "Limpar tudo" para o próximo evento, com inscrições desligadas, senha do master, confirmação digitada e backup completo obrigatório (ou ciência explícita do risco).

## Arquitetura

```mermaid
flowchart LR
  subgraph Navegador
    P[Participante<br/>celular] -->|lê o QR| E[/entrar/]
    A[Organização] --> PA[Painel /admin]
    C[Caixa do curso] --> CX[/caixa/&lt;curso&gt;/]
  end
  E --> API
  PA --> API
  CX --> API
  subgraph Servidor["Next.js (Route Handlers · Node.js)"]
    API[API REST] --> R[Regras de negócio<br/>validação · sessões · rate limit]
  end
  R --> DB[(PostgreSQL)]
```

- **Front e back no mesmo projeto Next.js (App Router).** As páginas são componentes React e a API usa Route Handlers.
- **O servidor é a fonte da verdade.** Preço, total, troco, estoque e saldo da gaveta são recalculados no servidor, com os dados do banco. O valor enviado pela tela nunca é usado.
- **Dinheiro guardado em centavos (inteiros)**, para não ter erro de arredondamento. Venda, baixa de estoque e movimento de caixa são gravados juntos, numa única transação.
- **Migrações SQL versionadas.** Por exemplo, um índice único parcial garante só um turno aberto por barraca.
- **Em produção na nuvem:** hospedado no **Netlify**, com o PostgreSQL gerenciado no **Supabase**, conexão com TLS e certificado validado.
- **Compatível com o banco antigo.** O código detecta se cada migração nova já foi aplicada e, enquanto não foi, funciona como antes. Assim o site pode ser publicado antes de atualizar o banco, sem cair.

## Segurança

- Senhas guardadas com hash **scrypt**. As sessões ficam no banco, com **cookies HTTP-only**.
- **Limite de tentativas de login:** depois de 5 erros em 15 minutos, aquele login é bloqueado por 15 minutos.
- **Conferência da origem** em toda requisição que altera dados, contra CSRF.
- **Isolamento entre barracas:** cada curso só enxerga e altera o próprio caixa, e o servidor confere isso em todas as rotas.
- **Permissões conferidas no servidor:** cada rota da API checa a permissão do grupo. Esconder um botão na tela não basta.
- **Ações críticas só do master:** criar e excluir usuários, backup, limpeza e exclusão de barraca com histórico. Elas pedem a senha do master, com limite de tentativas.
- **Limpeza em uma transação só:** ou apaga tudo, ou nada.
- **CSV seguro:** a exportação neutraliza fórmulas maliciosas (*CSV injection*).
- **Privacidade (LGPD):** o caixa não guarda nenhum dado pessoal.

## Qualidade e testes

- **427 testes automáticos** (Node.js Test Runner), cobrindo regras de negócio, segurança, permissões por grupo e regressões.
- **28 casos de teste do módulo de caixa**, ligados aos requisitos. Os casos CT01 a CT24 rodam por um roteiro automático contra a API, e os casos CT25 a CT28 foram executados no navegador. **Todos foram aprovados.**
- Especificação completa com **20 requisitos funcionais, 12 não funcionais, 10 regras de negócio, 10 casos de uso** e matriz de rastreabilidade: 👉 [docs/requisitos-e-testes.md](docs/requisitos-e-testes.md)
- Checagem de tipos com TypeScript no modo estrito e build de produção conferido antes de cada entrega.
- Migrações testadas num banco vazio (instalação do zero) e num banco igual ao de produção (sem as migrações novas).

## Tecnologias

| Camada | Ferramentas |
|---|---|
| Interface | Next.js 15 (App Router), React 19, TypeScript, CSS |
| API | Next.js Route Handlers (Node.js) |
| Banco de dados | PostgreSQL, `pg`, migrações SQL |
| Autenticação | scrypt, sessões no banco, cookies HTTP-only |
| QR Code | `qrcode` + tokens temporários |
| Testes | Node.js Test Runner, roteiro de testes de API, Playwright |
| Hospedagem | Netlify (aplicação) e Supabase (PostgreSQL gerenciado, TLS) |
| Ambiente | Docker (PostgreSQL), WSL, VS Code |

## Minha participação

- Desenvolvimento do sistema de inscrições em grupo.
- **Módulo Caixa das Barracas:** levantamento de requisitos, modelagem do banco (turnos, vendas, movimentos), API, telas e relatórios.
- **Segurança e controle de acesso:** grupos e permissões verificadas no servidor, gestão de usuários, barraca própria, coordenador de curso e a limpeza para o próximo evento.
- **Melhorias do caixa:** código automático dos produtos, gráficos do turno e reorganização dos relatórios.
- **Integração em equipe:** entrega por *pull request*, juntando as minhas mudanças com as do colega (barracas por curso e período), resolvendo conflitos e corrigindo um erro de sintaxe numa migração.
- **Qualidade:** especificação dos requisitos, casos de teste, roteiro de testes automatizados da API e matriz de rastreabilidade.
- Artigo científico e banner apresentados no **VI Encontro de Pesquisa, Extensão e Inovação** da Uniguaçu.

## Equipe

Projeto acadêmico do curso de **Engenharia de Software — Faculdade Uniguaçu**.

- Gustavo Henrique Magagnin Silva
- Lucas Vergopolan Proner
- Mauricio Speth: publicação no Netlify com Supabase, integração do caixa ao projeto oficial, barracas por curso e período, gestão e exclusão de barracas pelo master, e a tela de entrada do QR

Orientação: Prof. Junior Mateus Peters
