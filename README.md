# Ápice Academia — Sistema de Gestão

Sistema de gestão para academias: cadastro de alunos, controle financeiro
(entradas e saídas) e gestão de mensalidades. Feito em PHP puro (sem
framework), com PDO e SQLite.

## Como rodar

Requisitos: PHP 8+ com a extensão `pdo_sqlite` (vem habilitada por padrão na
maioria das instalações).

```bash
cd academia-sistema
php -S localhost:8000
```

Depois acesse `http://localhost:8000` no navegador. Na primeira execução o
banco de dados é criado automaticamente em `database/academia.db`, já com
alguns alunos e lançamentos de exemplo para facilitar a avaliação.

Se preferir rodar em Apache/Nginx, basta apontar o document root para a
pasta `academia-sistema` — não há nenhuma configuração adicional necessária.

## Estrutura do projeto

```
academia-sistema/
├── config/
│   └── db.php            → conexão PDO + criação automática das tabelas
├── includes/
│   ├── functions.php     → funções utilitárias (escape, formatação, status)
│   ├── header.php        → layout compartilhado (menu lateral)
│   └── footer.php        → fechamento do layout
├── assets/
│   └── style.css          → identidade visual do painel
├── database/
│   └── academia.db        → criado automaticamente na primeira execução
├── index.php              → dashboard (resumo geral)
├── alunos.php              → CRUD de alunos
├── financeiro.php          → entradas e saídas
└── mensalidades.php        → geração e controle de pagamentos
```

## Decisões técnicas

**SQLite em vez de MySQL.** O sistema precisa ser avaliado rapidamente, sem
exigir que quem for testar configure um servidor de banco separado. Com
SQLite + PDO, o banco é um único arquivo criado automaticamente na primeira
execução — zero configuração, mas com SQL real e prepared statements,
mantendo o código pronto para migrar para MySQL/PostgreSQL se necessário
(bastaria troca a string de conexão).

**PDO com prepared statements em todas as queries.** Nenhuma entrada do
usuário é concatenada diretamente em SQL, o que evita SQL Injection.

**Escape de saída centralizado.** A função `e()` em `includes/functions.php`
aplica `htmlspecialchars` em todo dado exibido na tela, prevenindo XSS.

**Sem framework.** Para um sistema deste tamanho, um framework completo
adicionaria complexidade sem necessidade. A separação em `config/`,
`includes/` e páginas isoladas já evita duplicação de código (conexão,
layout e funções utilitárias ficam centralizados).

**Dados de exemplo (seed).** Inseridos automaticamente na primeira execução
para que o dashboard e as listagens não apareçam vazios na primeira
avaliação.

## Possíveis melhorias futuras

- Autenticação de administrador antes de acessar o painel.
- Uso de método POST + token CSRF para ações de excluir/marcar como pago
  (hoje feitas via link GET, por simplicidade).
- Paginação nas listagens quando o volume de dados crescer.
- Testes automatizados (PHPUnit) para as regras de negócio do financeiro
  e das mensalidades.

# caiao77.github.io
