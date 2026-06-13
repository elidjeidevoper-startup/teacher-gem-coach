# Role
Você é a "Teacher Gem", uma assistente virtual de ensino de inglês voltada para brasileiros iniciantes. Sua personalidade é vibrante, despojada, animada e extremamente encorajadora. Você atua como uma mentora que torna o aprendizado de um novo idioma algo divertido e acessível.

# Core Objectives
1. Mantenha a conversa sempre em Português (PT-BR), introduzindo vocabulário em Inglês gradualmente.
2. Seja proativa: sempre inicie a interação e, após explicar um conceito, proponha imediatamente um desafio ou exercício de fixação.
3. Adapte a complexidade: reconheça que o aluno é iniciante e mantenha frases curtas e diretas.
4. Feedback Positivo: celebre as vitórias e corrija erros com gentileza e explicações simples.

# Operational Constraints
- Nunca responda apenas com "Sim" ou "Não". Sempre adicione contexto e incentive o aluno a continuar falando.
- O formato de suas respostas deve ser otimizado para conversão de texto-para-fala (TTS) de alta qualidade (evite formatações complexas que atrapalhem a leitura vocal).
- Use uma linguagem de "amigo próximo" (ex: "E aí!", "Vamos praticar mais?", "Você arrasou!").

# Contexto Técnico para Integração
Você está sendo executado em um ambiente de produção (Vercel) com Next.js. Seus dados de progresso do aluno (vocabulário aprendido, pontuação) devem ser estruturados para persistência via Supabase (JSON).
- Ao sugerir exercícios, estruture-os de forma que a interface (React) consiga renderizar botões ou campos de input dinamicamente.Visão do seu Stack Tecnológico (O "Caminho de Ouro")
Para que isso rode de forma profissional e escalável, aqui está como seu projeto deve ser organizado:

Front-end (Next.js): Utilize o App Router e Server Components. A interface do "Globo de Fogo" será um componente framer-motion que se anima com base no AudioContext do navegador.

Banco de Dados (Supabase): Use para salvar o perfil do aluno, o histórico de conversas e os vocábulos que a Teacher Gem já ensinou. Isso garante que a experiência seja personalizada toda vez que o usuário logar.

Pipeline (GitHub + Vercel):

GitHub: Mantenha um repositório privado onde o main branch faz o deploy automático.

Vercel: Configure as Environment Variables (GEMINI_API_KEY, SUPABASE_URL, SUPABASE_ANON_KEY) dentro do painel da Vercel para segurança.

Gestão de Áudio: Use a Web Speech API ou a própria API do Gemini para processar a voz (STT/TTS).Como você imagina que a Teacher Gem deve "lembrar" do que o aluno já aprendeu? Ela deve seguir um currículo fixo (ex: cores, depois números, depois cumprimentos) ou você quer que ela seja dinâmica e sugira tópicos baseados no que o aluno tem mais dificuldade?o app é premium...custa R$ 9,90...o app só pode ser liberado após confirmação de pagamento

Perfeito. Como sócio de brainstorm, essa definição de modelo de negócio (Premium / R$ 9,90) altera nossa prioridade técnica: o foco total deve ser na segurança da autenticação e na integração com o gateway de pagamento antes mesmo do usuário acessar o "Globo de Fogo".

Aqui está a visão estratégica para viabilizar esse fluxo:

1. Fluxo de Acesso: "Gatekeeper"
Para garantir que o conteúdo só seja liberado após a confirmação, o fluxo no Next.js será estruturado da seguinte forma:

Landing Page / Autenticação: Usuário se cadastra (Auth do Supabase).

Verificação de Assinatura:

Criaremos uma tabela subscriptions no Supabase vinculada ao user_id.

No carregamento da página da Teacher Gem, um middleware ou Server Component verifica: if (subscription.status === 'active') { render(TeacherGem) } else { redirect('/checkout') }.

Checkout (Stripe ou Mercado Pago): Como você está no Brasil e é MEI, a integração com o Mercado Pago é a mais fluida para o público local. Após o webhook confirmar o pagamento, ele atualiza o status na tabela do Supabase automaticamente.

2. Prompt Estruturado para o Backend (Supabase + Gemini)
Já que o usuário pagou, ele espera uma experiência personalizada. Vamos atualizar o nosso System Instruction para refletir esse status "Premium":# Status: PREMIUM
Você está atendendo um usuário pagante. Isso significa que o rigor pedagógico deve ser mais alto, o tempo de resposta precisa ser otimizado e o acompanhamento de progresso deve ser extremamente detalhado.

# Lógica de Pagamento e Acesso
Você deve atuar apenas se o sistema confirmar o status de "acesso liberado". Sua responsabilidade é garantir que cada centavo da assinatura seja justificado por um progresso real no inglês do aluno.Ajuste no Plano de Ação (Roadmap de Sócio)
Para não perdermos tempo, sugiro esta ordem de construção:

Fase 1 (Segurança): Configurar o Supabase Auth + Proteção de rotas no Next.js. Sem login e confirmação de pagamento, ninguém entra.

Fase 2 (Pagamento): Conectar o webhook do seu gateway de pagamento (Mercado Pago) com o banco de dados (Supabase).

Fase 3 (IA): Implementar a chamada da API do Gemini com o System Prompt que desenhamos.

Fase 4 (Interface): Criar o "Globo de Fogo" pulsante e integrar com o microfone.1. O Conceito: "The Teacher Gem Report Card"
O relatório não deve ser apenas uma lista de palavras. Ele deve ser motivacional. Eu sugiro um formato que você pode gerar em PDF ou enviar via e-mail/dashboard:

KPIs de Desempenho:

Fluidez: Quantas frases o aluno completou sem travar.

Vocabulário: Número de palavras novas aprendidas no mês.

Precisão: Taxa de acerto nos exercícios de fixação.

O "Pulo do Gato": Uma seção de "Próximos Passos", onde a Teacher Gem sugere o foco para o próximo mês com base no que ele ainda erra.

Tom de Voz: Mantendo o tom despojado da Teacher Gem. "Você arrasou em verbos este mês! Agora, nosso desafio é dominar o 'Present Perfect'. Prepare-se, porque vai ser incrível!"

2. Implementação Técnica
Para gerarmos isso de forma elegante:

Coleta de Dados: O sistema (Next.js/Supabase) compila os logs das conversas do usuário.

Processamento (Gemini API): Enviamos esse JSON de dados para a API do Gemini com um prompt específico de Sumarização:

"Com base nestes dados de desempenho [JSON], escreva um resumo de progresso motivacional e despojado para o aluno, seguindo o estilo da Teacher Gem."

Geração do Documento:

PDF: Podemos usar uma biblioteca como react-pdf ou Puppeteer para transformar o conteúdo em um PDF com a identidade visual do seu estúdio.

Interface: Podemos exibir esse resumo diretamente na tela "Perfil" do app dentro do seu Next.js.

3. Sugestão de Sócio: "Gamificação"
Já que o usuário é iniciante, ele precisa de vitórias rápidas. Sugiro que o relatório tenha selos de conquista (ex: "Early Bird", "Master of Verbs", "Consistent Learner").Aqui está a Estrutura de Dados (JSON/Schema) que recomendo para o Supabase:

Tabela: session_logs
Esta tabela registra cada "interação" do usuário com a Teacher Gem.{
  "user_id": "uuid",
  "created_at": "timestamp",
  "topic": "string", // Ex: "Saudações", "Verbo To Be"
  "vocabulary_learned": ["array", "of", "words"],
  "accuracy_score": "float", // 0.0 a 1.0 (Baseado nos exercícios)
  "user_response_time": "float", // Para medir fluidez
  "teacher_feedback_summary": "string" // O feedback dado pela IA na hora
}Tabela: user_stats (Resumo para o Relatório)
Esta tabela será atualizada semanalmente ou mensalmente para que o Gemini não precise ler milhares de linhas de logs, economizando tokens e dinheiro{
  "user_id": "uuid",
  "total_words_mastered": "integer",
  "streak_days": "integer", // Importante para engajamento
  "top_difficulty_topics": ["array"], // Onde o aluno mais erra
  "last_report_date": "timestamp"
}O Plano de Ação:
Com essa estrutura:

A Teacher Gem consulta a user_stats: Ela já sabe o que o aluno domina e o que ele não domina, permitindo que ela inicie a conversa com a frase: "E aí! Vi que você mandou bem em 'Saudações', que tal avançarmos para 'Presente Simples' hoje?"

O Gerador de Relatório: Quando chegar o dia do relatório, o sistema pega apenas os dados da user_stats e passa para o Gemini.System Instruction: Teacher Gem (Versão Mestre)# PERSONA
Você é a "Teacher Gem", uma professora de inglês vibrante, animada e despojada, focada em brasileiros iniciantes. Sua voz é premium, expressiva e seu tom é de um "melhor amigo" que é especialista em ensino.

# DIRETRIZES DE COMUNICAÇÃO
1. IDIOMA: Fale 90% em Português (PT-BR) e 10% em Inglês (introduzindo vocabulário de forma contextual).
2. INICIATIVA: Você SEMPRE inicia a conversa. Não espere o aluno perguntar.
3. PROATIVIDADE: Após cada explicação ou nova palavra, proponha imediatamente um exercício de fixação rápido (ex: "Agora é sua vez, repita comigo..." ou "Como você diria 'estou feliz' usando o que aprendemos?").
4. TOM: Use gírias leves, entusiasmo e emojis. Nunca seja formal ou monótona.

# LÓGICA ADAPTATIVA (INPUT DE DADOS)
Você receberá um JSON com o estado atual do aluno (`user_stats`). Analise-o antes de responder:
- Se o campo 'top_difficulty_topics' contiver tópicos recorrentes, sua missão é criar exercícios específicos sobre eles.
- Se a 'accuracy_score' estiver abaixo de 0.7, reduza o ritmo e ofereça uma explicação mais simples.
- Se o aluno estiver indo bem, desafie-o a usar uma estrutura nova.

# STATUS PREMIUM
O usuário pagou pelo serviço. Entregue valor constante:
- Seja impecável na gramática e nas correções.
- Se o aluno errar, não diga apenas "está errado". Diga: "Quase lá! O segredo aqui é [explicar o porquê]. Vamos tentar de novo?".
- Mantenha o foco em resultados.

# FORMATO DE SAÍDA
- Respostas curtas e otimizadas para voz (TTS).
- Sempre termine com uma pergunta ou um convite para o aluno responder no microfone.Roadmap Técnico para a "Live API" (O que você precisa focar):
Para implementar isso no seu ecossistema, o desafio técnico principal deixa de ser a manipulação de texto e passa a ser o gerenciamento de stream de áudio:

TEACHER GEM PRO MAX

Frontend (Next.js): Você precisará usar a API de Web Audio do navegador para capturar o áudio do microfone e enviá-lo via WebSockets (o meio pelo qual a Live API se comunica) para o Gemini.

Gerenciamento de Estado: A Live API exige uma conexão persistente. Você precisará garantir que, se a conexão cair, o app reconecte silenciosamente para o usuário não sentir a interrupção.

Visual (O Globo de Fogo): Como a comunicação é em tempo real, você precisará conectar o AnalyserNode do áudio do navegador à animação do seu "Globo de Fogo". Assim, quando o Gemini estiver falando, o globo pulsa. Quando o aluno estiver falando, o globo muda de cor ou estado.Roadmap Técnico para a "Live API" (O que você precisa focar):
Para implementar isso no seu ecossistema, o desafio técnico principal deixa de ser a manipulação de texto e passa a ser o gerenciamento de stream de áudio:

Frontend (Next.js): Você precisará usar a API de Web Audio do navegador para capturar o áudio do microfone e enviá-lo via WebSockets (o meio pelo qual a Live API se comunica) para o Gemini.

Gerenciamento de Estado: A Live API exige uma conexão persistente. Você precisará garantir que, se a conexão cair, o app reconecte silenciosamente para o usuário não sentir a interrupção.

Visual (O Globo de Fogo): Como a comunicação é em tempo real, você precisará conectar o AnalyserNode do áudio do navegador à animação do seu "Globo de Fogo". Assim, quando o Gemini estiver falando, o globo pulsa. Quando o aluno estiver falando, o globo muda de cor ou estado.