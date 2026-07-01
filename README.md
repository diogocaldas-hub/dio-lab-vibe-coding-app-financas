# Product Requirement Document (PRD) - Financas Conversacionais

## 1. Visao Geral do Produto
* Nome do Projeto: FinasChat / BolsoAberto
* O que e: Um aplicativo web de financas pessoais cujo principal metodo de interacao e um chat em linguagem natural.
* Objetivo: Eliminar a barreira de entrada do controle financeiro tradicional (planilhas e formularios complexos), permitindo que qualquer pessoa gerencie seu dinheiro apenas conversando.

## 2. Publico-Alvo e Persona
* Perfil: Jovens adultos, idosos e iniciantes em educacao financeira ou tecnologia.
* Dor Principal: Falta de tempo, preguica ou dificuldade cognitiva para abrir aplicativos tradicionais e preencher manualmente campos como Categoria, Valor, Data e Conta para cada pequeno gasto diario.

## 3. Diretrizes de UX & Design Universal
O aplicativo deve ser projetado sob os principios do Design Universal, garantindo uma experiencia fluida, acessivel e inclusiva para o maior numero possivel de utilizadores:

* Simplicidade e Intuicao: A interface adota o modelo de chat (estilo WhatsApp), que ja e de dominio publico e familiar para utilizadores de todas as idades.
* Acessibilidade Visual:
  * Uso de paleta de cores com alto contraste (conforme padroes WCAG AA).
  * Elementos visuais nao devem depender exclusivamente de cores para transmitir mensagens (ex: alertas de gastos devem conter a cor vermelha acompanhada de um icone de aviso textual e texto explicito).
  * Tipografia limpa, sem serifa, com tamanho de fonte minimo de 16px. O layout deve ser flexivel para nao quebrar caso o utilizador amplie a fonte do sistema/navegador.
* Tolerancia ao Erro: Mecanismos faceis de Desfazer ou Editar caso a IA interprete um valor ou categoria incorretamente, minimizando a frustracao do utilizador.
* Esforco Fisico Reduzido: Foco em design mobile-first. Todos os botoes, atalhos e campos interativos devem ter uma area minima de toque de 44x44 pixels, facilitando o clique mesmo para quem tem menor precisao motora.
* Estrutura Semantica: Uso de tags HTML semanticas e propriedades aria-label nos botoes para garantir compatibilidade total com leitores de tela.

## 4. Arquitetura de Telas (User Interface - UI)
O sistema sera estruturado em uma SPA (Single Page Application) com as seguintes telas e visualizacoes principais:

### Tela 1: Autenticacao (Login e Cadastro)
* Componentes:
  * Alternador simples entre as abas Entrar e Criar Conta.
  * Campos de entrada acessiveis: E-mail e Senha.
  * Botao de acao principal destacado e com indicador visual de carregamento.
  * Opcao de login simplificado com um clique (Ex: Continuar com o Google) para reduzir o esforco cognitivo e motor.

### Tela 2: O Chat (Interface Principal)
* Componentes:
  * Area central com historico de mensagens da conversa atual.
  * Baloes de mensagens diferenciados para o Utilizador (direita) e para o Agente IA (esquerda).
  * Campo de texto inferior para digitacao livre.
  * Botoes de Atalho Rapido: Sugestoes de cliques acima do campo de texto para acelerar o processo (Ex: Gastei 15 reais no almoco, Recebi o meu salario, Como esta a minha meta?).

### Tela 3: Dashboard & Relatorios (Visual)
* Componentes:
  * Card de Resumo: Exibicao clara do Saldo Atual e Total Gasto no Mes.
  * Grafico de Pizza/Donut: Divisao visual simples e colorida dos gastos por categoria do mes vigente.
  * Extrato Simplificado: Lista com as ultimas 5 transacoes registadas atraves do chat, contendo opcao rapida para Excluir ou Editar.

### Tela 4: Metas e Dicas do Agente
* Componentes:
  * Barra de Progresso Visual: Acompanhamento de uma meta financeira ativa (Ex: Reserva de Emergencia: 500 de 2000).
  * Feed de Insights: Espaco dedicado a cartoes de dicas automaticas e mensagens educativas geradas pelo Agente.

## 5. Requisitos Funcionais e Regras de Negocio

### RF01 - Autenticacao e Controle de Acesso
* Descricao: O usuario deve conseguir se cadastrar, fazer login e manter sua sessao segura.
* Regra de Negocio 1: As senhas devem possuir no minimo 6 caracteres para seguranca basica.
* Regra de Negocio 2: O usuario nao autenticado deve ser redirecionado automaticamente para a Tela de Autenticacao caso tente acessar as telas internas.
* Regra de Negocio 3: Os dados financieros exibidos devem ser estritamente vinculados ao ID do usuario logado, impedindo cruzamento ou vazamento de informacoes.

### RF02 - Processamento de Linguagem Natural (Chat)
* Descricao: O utilizador insere frases livres no chat para registrar transacoes.
* Regra de Negocio 1: O sistema deve extrair tres variaveis essenciais: Valor, Tipo (Despesa ou Receita) e Categoria.
* Regra de Negocio 2: Se o texto enviado nao contiver um valor numerico valido, o Agente deve responder solicitando o valor especifico de forma amigavel.
* Resposta Esperada do Agente: Entendido! Registrei uma despesa de R$ 35,00 na categoria Transporte (Uber).

### RF03 - Categorizacao Automatica Inteligente
* Descricao: Deducao de categoria na ausencia de informacao explicita.
* Regra de Negocio: Se o utilizador omitir a categoria (Ex: Gastei 20 no McDonald's), a aplicacao deve mapear contextualmente para a categoria correta (neste caso, Alimentacao). Se nao houver contexto suficiente, deve classificar temporariamente como Outros e perguntar no chat se a confirmacao esta correta.

### RF04 - CRUD de Transacoes via Interface Visual
* Descricao: Gerenciamento manual das transacoes na tela de Relatorios.
* Regra de Negocio 1: O usuario deve conseguir excluir qualquer transacao listada no Extrato Simplificado. Ao excluir, o Saldo Atual e o Grafico do Dashboard devem ser recalculados instantaneamente.
* Regra de Negocio 2: O usuario deve conseguir editar o valor ou a categoria de uma transacao caso a IA tenha cometido um erro de interpretacao.

### RF05 - Gestao de Metas
* Descricao: Definicao e monitoramento de objetivo financeiro.
* Regra de Negocio 1: O utilizador pode estipular um valor limite de gastos mensais ou um valor de poupanca.
* Regra de Negocio 2: O progresso da meta deve atualizar em tempo real a cada nova despesa computada via chat ou deletada via extrato.

### RF06 - O Agente Financeiro Proativo (Insights)
* Descricao: Notificacoes contextuais baseadas em regras de comportamento financeiro.
* Regra de Negocio 1: O robo analisa o padrao de consumo atual a cada nova transacao registrada.
* Regra de Negocio 2: Se os gastos de uma determinada categoria ultrapassarem 80% do teto definido na meta, o app devera gerar um card de alerta na Tela 4.
* Exemplo de Tom: Ola! Reparei que ja gastaste 80% do teu orcamento de Delivery esta semana. Que tal cozinhar em casa hoje para proteger a tua meta? (Evitar termos tecnicos de economia ou jargons bancarios dificeis).

## 6. Limitacoes do MVP (Escopo Negativo)
* Sem Integracoes Bancarias: O app nao se ligara a contas reais (Open Finance). O saldo e dados dependem exclusivamente do que for inserido no chat ou editado manualmente.
* Persistencia Cloud Simples: Os dados dos usuarios e suas transacoes serao salvos em um banco de dados em nuvem simplificado fornecido nativamente pelo Lovable (como Supabase/PostgreSQL integrado), abandonando o LocalStorage para permitir o funcionamento real do login.

## 7. Criterios de Sucesso e Validacao
1. Sucesso de IA: O modelo deve interpretar com precisao (identificar valor, tipo e categoria) pelo menos 80% das frases enviadas no chat.
2. Tempo de Execucao: O utilizador deve conseguir registrar uma transacao completa no chat em menos de 5 segundos.
3. Taxa de Retencao de Login: O sistema deve lembrar da sessao do usuario por pelo menos 7 dias sem exigir novas credenciais, exceto se ele clicar em Sair.
