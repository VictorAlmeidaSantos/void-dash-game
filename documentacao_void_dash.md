# Documentação do Sistema: Void Dash - Anomalia

## Contexto do Sistema

### 1. Visão Geral da Aplicação
O sistema "Void Dash - Anomalia" é um jogo web 2D que combina as mecânicas de Endless Runner (corrida infinita) com elementos de Action Rogue-lite. O jogador controla uma entidade que avança continuamente por um ambiente hostil gerado de forma procedural, devendo desviar de obstáculos e derrotar inimigos. O núcleo do sistema baseia-se na sobrevivência contínua, interrompida por ciclos de progressão (aumento de poder) e finalizada por um confronto pré-determinado contra um chefe.

### 2. Premissas Tecnológicas e de Arquitetura (Base para RNF)
* **Abordagem "Vibe Code":** O sistema deve ser gerado integralmente por Inteligência Artificial a partir de prompts lógicos, com foco em extrema estabilidade e ausência de bugs de colisão ou física.
* **Stack Tecnológico:** Desenvolvimento puramente Client-Side em um único arquivo (`index.html`), utilizando HTML5 Canvas para renderização gráfica geométrica e JavaScript (Vanilla) para toda a lógica, manipulação de estado e física.
* **Independência de Assets:** Proibição do uso de imagens, spritesheets ou áudios externos. Toda a representação visual (jogador, inimigos, cenário) será renderizada via código através de formas geométricas com esquemas de cores em neon.
* **Hospedagem e Execução:** O sistema deve ser capaz de rodar localmente e ser servido através de ferramentas de linha de comando (como opencode cli), compatível com qualquer navegador web moderno.

### 3. Regras de Negócio e Dinâmica de Jogo
O core loop (ciclo principal) do jogo divide-se em três estados distintos:

**3.1. Estado de Sobrevivência (Corrida Infinita):**
* O personagem principal permanece estático no eixo X (lado esquerdo da tela) e só pode se mover no eixo Y (pulo e queda com gravidade simulada).
* O cenário avança da direita para a esquerda em uma velocidade que aumenta gradativamente.
* Inimigos e obstáculos são instanciados proceduralmente fora da tela (à direita) e se movem em direção ao jogador.
* O jogador acumula pontuação com base no tempo de sobrevivência e na destruição de entidades inimigas.

**3.2. Controles e Sistema de Combate:**
* **Ação de Pulo:** Permite ao jogador evitar fendas no chão e inimigos rasteiros. Possui física de gravidade associada.
* **Ação de Ataque (Pode ser Segurado):** Dispara um feixe ou projétil à frente. O comando suporta retenção (Charge) para alterar a propriedade do ataque dependendo das habilidades adquiridas.
* **Ação de Rebater (Deflect):** Cria um campo de repulsão temporário ao redor do jogador, capaz de refletir projéteis inimigos e rechaçar as esferas do Chefe. No ar, pode ser ativado para um ataque de mergulho (Ground Slam), dependendo das habilidades.

**3.3. Entidades e Obstáculos:**
* **Fenda Espacial (Obstáculo de Solo):** Buracos no terreno. Cair neles resulta em Game Over (a menos que mitigado por habilidade).
* **Mina Temporal (Obstáculo Aéreo):** Entidade flutuante invulnerável aos ataques básicos que pulsa constantemente. Colisão é fatal.
* **Drone do Vazio (Inimigo Base):** Movimenta-se pelo solo, atira projéteis e salta caso o jogador esteja no ar. Colisão é fatal.
* **Inimigo Laser:** Flutua pelo cenário rastreando o eixo Y do jogador e emite um feixe de laser contínuo após um aviso visual.

### 4. Sistema de Progressão e Habilidades (Elementos Rogue-lite)
A cada 30 segundos (ou ao atingir marcos de 600 pontos), o fluxo do tempo no jogo é congelado. Uma interface modal apresenta ao jogador uma escolha de progressão.
* **Sorteio de Habilidades:** O sistema sorteia até 3 opções aleatórias, baseadas em pesos de raridade (Comum, Rara, Épica, Lendária).
* **Evolução por Níveis:** Cada habilidade atinge o Nível 3. Após maximizada, sai da piscina de sorteios.

**Catálogo de Habilidades Implementadas:**
1. **Véu do Vazio (Comum - Foco em Defesa):**
   * Nível 1: Escudo que absorve 1 hit. Recarrega a cada 10s.
   * Nível 2: Recarga cai para 8s. Ao ser quebrado, emite um pulso que destrói projéteis em área.
   * Nível 3: Recarga cai para 6s. O escudo reflete o primeiro projétil inimigo.
2. **Botas Gravitacionais (Comum - Foco em Mobilidade):**
   * Nível 1: Desbloqueia o Pulo Duplo.
   * Nível 2: Desbloqueia o Pulo Triplo.
   * Nível 3: Adiciona *hitbox* de dano ao pulo; inimigos que encostam no jogador durante a trajetória ascendente são destruídos.
3. **Eco de Matéria (Rara/Lendária - Foco em Projéteis e Dano em Massa):**
   * Nível 1: O ataque converte-se em um projétil de energia que viaja pela tela.
   * Nível 2 (Lendária): Segurar o ataque carrega um projétil maior e mais rápido com dano perfurante.
   * Nível 3: Carga Dupla. Segurar o botão até o limite dispara um feixe Laser Massivo que atinge todos os inimigos da tela (uso limitado com recarga de punição).
4. **Distorção de Tempo (Épica - Foco em Sobrevivência e Controle de Grupo):**
   * Nível 1: Usar o comando de Rebater no ar ativa um mergulho veloz (Ground Slam). Ao tocar o chão, cria uma onda de choque que destrói ameaças.
   * Nível 2: Aumenta o raio do mergulho e deixa uma "Fenda Dimensional" no chão por 2 segundos que destrói inimigos terrestres.
   * Nível 3: Ativa a passiva de Retrocesso. Ao receber dano letal, o tempo retrocede 3 segundos, colocando o jogador em local seguro e deduzindo levemente a pontuação (acionado 1 vez por partida).

### 5. Condições de Clímax e Encerramento (A Arena do Boss)
A geração infinita possui uma condição exata de transição.
* **O Gatilho:** Ao atingir 3.000 pontos (ou sobreviver 150 segundos), o jogo entra no modo Arena.
* **O Chefe ("Arquiteto do Vazio"):** Entidade geométrica (HP: 7) com ciclos de *fade-in/fade-out* (teleporte randômico na área direita da tela).
* **Padrões de Ataque do Chefe:**
  * Feixes de Laser (Dois tipos): Laser estático em alturas aleatórias e Laser guiado (rastreia o jogador durante o carregamento).
  * Esferas de Energia: Projéteis constantes disparados em direção ao jogador a cada ~2.5 segundos.
* **Condição de Vitória:** O jogador deve usar o ataque básico ou a ação de Rebater (Deflect) para rechaçar as Esferas de Energia de volta ao chefe, subtraindo seu HP até 0.

---

## Estruturas de Dados JSON (Persona, Requisitos Funcionais e Não Funcionais)

### Persona JSON
```json
{
  "name": "Kleber",
  "age": "25",
  "Who": {
    "W1_profession": "Analista de suporte técnico em uma empresa de tecnologia.",
    "W2_education_level": "Ensino superior em andamento na área de tecnologia da informação.",
    "W3_self_description": "Gosta de jogos desafiadores que exigem reflexos rápidos, tomada de decisão constante e sensação clara de progressão. Valoriza sistemas previsíveis, justos e que recompensem habilidade e aprendizado.",
    "W4_fears_concerns_frustrations_and_why": "Fica frustrado quando jogos apresentam colisões inconsistentes, controles imprecisos, dificuldade artificial, mortes consideradas injustas ou sistemas de progressão pouco claros, pois isso reduz a sensação de domínio, aprendizado e mérito pessoal."
  },
  "Context": {
    "C1_routine_tasks_using_web_mobile_desktop_applications": "Utiliza diariamente aplicações web, desktop e mobile para trabalho, comunicação, entretenimento e jogos. Costuma acessar jogos diretamente pelo navegador, espera carregamento rápido, compatibilidade com diferentes dispositivos e interfaces intuitivas que permitam iniciar uma sessão sem configurações complexas."
  },
  "Experience_with_Technology": {
    "E1_liked_parts_of_applications_and_why": "Prefere aplicações com resposta rápida aos comandos, feedback visual imediato, interfaces organizadas e sistemas que permitam compreender facilmente consequências das ações. Em jogos, aprecia indicadores visuais claros de progresso, pontuação e evolução de habilidades.",
    "E2_disliked_parts_of_applications_and_why": "Não gosta de menus excessivamente complexos, informações escondidas, tutoriais longos e obrigatórios, lentidão de desempenho, falhas de colisão, travamentos ou mecânicas difíceis de compreender, pois interrompem a experiência e reduzem a confiança no sistema.",
    "E3_devices_used": "Notebook pessoal, computador desktop para jogos e smartphone Android.",
    "E4_how_learns_new_applications": "Aprende explorando funcionalidades diretamente, observando feedback visual da interface e realizando experimentação prática antes de buscar documentação externa.",
    "E5_prefers_step_by_step_or_shortcuts": "Prefere começar com orientações simples e rapidamente migrar para atalhos e domínio próprio da aplicação.",
    "E6_information_processing_style_visual_text_audio": "Predominantemente visual, utilizando elementos gráficos, indicadores de estado, animações e organização espacial para compreender informações rapidamente.",
    "E7_virtual_social_behavior_interactive_or_reserved": "Interativo, costuma participar de comunidades online relacionadas a jogos, compartilhar experiências e discutir estratégias."
  },
  "Problems": {
    "P1_routine_problems_related_to_application_context": "Frequentemente encontra jogos de navegador com desempenho inconsistente, colisões imprecisas, excesso de elementos visuais que dificultam a leitura da ação, progressão repetitiva ou dificuldade em compreender quais melhorias impactam efetivamente a partida.",
    "P2_features_the_new_application_should_have_to_help": "Deve apresentar controles responsivos, colisões confiáveis, feedback visual claro para ataques e obstáculos, exibição objetiva das habilidades disponíveis e seus níveis, progressão compreensível durante a partida, desempenho estável em navegadores modernos e reinício rápido após derrota."
  },
  "Existing_Solutions": {
    "S1_existing_technologies_and_how_they_help": "Utiliza jogos endless runner e rogue-lite disponíveis em navegadores e plataformas digitais, que oferecem partidas rápidas, progressão baseada em habilidade e desafios crescentes sem exigir instalação complexa.",
    "S2_positive_or_essential_characteristics": "Controles precisos, desempenho estável, feedback visual imediato, curva de aprendizado clara, mecânicas de carregamento (charge) e repulsão (deflect), leitura fácil de obstáculos e inimigos, além de objetivos bem definidos como pontuação e enfrentamento de chefes.",
    "S3_negative_or_dispensable_characteristics": "Elementos visuais excessivamente poluídos, sistemas de progressão confusos, tutoriais extensos, dependência de recursos externos para funcionar, excesso de telas intermediárias e mecânicas que punem o jogador sem fornecer informações claras."
  }
}

{
  "functional_requirements": [
    {
      "requirement_id": "FR1",
      "description": "O sistema deve permitir que o jogador execute a ação de pulo por meio de um único comando de entrada durante a partida.",
      "requirement_type": "Must",
      "priority": "High",
      "derived_from": "P2_controls_responsivos, S2_controles_precisos"
    },
    {
      "requirement_id": "FR2",
      "description": "O sistema deve permitir que o jogador execute a ação de ataque por meio de um comando de entrada, suportando a retenção do botão (charge) para ataques mais fortes.",
      "requirement_type": "Must",
      "priority": "High",
      "derived_from": "P2_controles_responsivos, S2_controles_precisos"
    },
    {
      "requirement_id": "FR2_5",
      "description": "O sistema deve permitir que o jogador execute a ação de rebater (deflect) por meio de um comando de entrada para refletir projéteis e esferas do chefe.",
      "requirement_type": "Must",
      "priority": "High",
      "derived_from": "P2_controles_responsivos, S2_controles_precisos"
    },
    {
      "requirement_id": "FR3",
      "description": "O sistema deve exibir a pontuação atual do jogador durante toda a sessão de jogo.",
      "requirement_type": "Must",
      "priority": "High",
      "derived_from": "E1_indicadores_visuais_claros_de_progresso, S2_objetivos_bem_definidos"
    },
    {
      "requirement_id": "FR4",
      "description": "O sistema deve aumentar a pontuação com base no tempo de sobrevivência do jogador.",
      "requirement_type": "Must",
      "priority": "High",
      "derived_from": "S2_objetivos_bem_definidos"
    },
    {
      "requirement_id": "FR5",
      "description": "O sistema deve conceder pontos adicionais ao destruir inimigos.",
      "requirement_type": "Must",
      "priority": "High",
      "derived_from": "E1_feedback_visual_imediato, S2_progressao_significativa"
    },
    {
      "requirement_id": "FR6",
      "description": "O sistema deve exibir feedback visual quando um inimigo for destruído pelo ataque do jogador.",
      "requirement_type": "Must",
      "priority": "High",
      "derived_from": "E1_feedback_visual_imediato, P2_feedback_visual_claro"
    },
    {
      "requirement_id": "FR7",
      "description": "O sistema deve exibir feedback visual quando ocorrer colisão fatal com obstáculo ou inimigo.",
      "requirement_type": "Must",
      "priority": "High",
      "derived_from": "W4_mortes_injustas, E1_compreender_consequencias_das_acoes"
    },
    {
      "requirement_id": "FR8",
      "description": "O sistema deve apresentar uma tela de Game Over após uma colisão fatal válida.",
      "requirement_type": "Must",
      "priority": "High",
      "derived_from": "W4_sistemas_previsiveis_e_justos"
    },
    {
      "requirement_id": "FR9",
      "description": "O sistema deve disponibilizar um botão de reinício imediato após o Game Over.",
      "requirement_type": "Must",
      "priority": "High",
      "derived_from": "P2_reinicio_rapido_apos_derrota"
    },
    {
      "requirement_id": "FR10",
      "description": "O sistema deve exibir uma tela de vitória ao derrotar o chefe final.",
      "requirement_type": "Must",
      "priority": "High",
      "derived_from": "S2_objetivos_bem_definidos"
    },
    {
      "requirement_id": "FR11",
      "description": "O sistema deve exibir a pontuação final ao término da partida.",
      "requirement_type": "Must",
      "priority": "Medium",
      "derived_from": "E1_indicadores_visuais_claros_de_progresso"
    },
    {
      "requirement_id": "FR12",
      "description": "O sistema deve apresentar visualmente as opções de habilidade durante cada evento de progressão.",
      "requirement_type": "Must",
      "priority": "High",
      "derived_from": "P2_exibicao_objetiva_das_habilidades"
    },
    {
      "requirement_id": "FR13",
      "description": "O sistema deve exibir o nome da habilidade em cada opção apresentada ao jogador.",
      "requirement_type": "Must",
      "priority": "High",
      "derived_from": "P2_exibicao_objetiva_das_habilidades"
    },
    {
      "requirement_id": "FR14",
      "description": "O sistema deve exibir o nível atual de cada habilidade já adquirida.",
      "requirement_type": "Must",
      "priority": "High",
      "derived_from": "P2_exibicao_objetiva_das_habilidades_e_seus_niveis"
    },
    {
      "requirement_id": "FR15",
      "description": "O sistema deve atualizar visualmente o nível de uma habilidade após sua evolução.",
      "requirement_type": "Must",
      "priority": "High",
      "derived_from": "E1_feedback_visual_imediato"
    },
    {
      "requirement_id": "FR16",
      "description": "O sistema deve remover habilidades de nível máximo das futuras opções de sorteio.",
      "requirement_type": "Must",
      "priority": "High",
      "derived_from": "W3_sistemas_previsiveis_e_justos"
    },
    {
      "requirement_id": "FR17",
      "description": "O sistema deve exibir visualmente quando uma habilidade produzir um projétil ou feixe aprimorado.",
      "requirement_type": "Should",
      "priority": "Medium",
      "derived_from": "E6_processamento_visual"
    },
    {
      "requirement_id": "FR18",
      "description": "O sistema deve exibir visualmente a barra de recarga (charge) do ataque ao longo da ação do jogador.",
      "requirement_type": "Should",
      "priority": "Medium",
      "derived_from": "E6_processamento_visual"
    },
    {
      "requirement_id": "FR19",
      "description": "O sistema deve apresentar aviso visual intermitente da chegada do chefe final.",
      "requirement_type": "Must",
      "priority": "High",
      "derived_from": "E1_feedback_visual_imediato, S2_objetivos_bem_definidos"
    },
    {
      "requirement_id": "FR20",
      "description": "O sistema deve apresentar visualmente o HP numérico (ex: 7/7) e a barra de vida do chefe.",
      "requirement_type": "Should",
      "priority": "Medium",
      "derived_from": "E1_compreender_consequencias_das_acoes, S2_objetivos_bem_definidos"
    },
    {
      "requirement_id": "FR21",
      "description": "O sistema deve apresentar uma interface inicial simples que permita iniciar a partida sem necessidade de configuração prévia.",
      "requirement_type": "Must",
      "priority": "High",
      "derived_from": "C1_interfaces_intuitivas_sem_configuracoes_complexas"
    },
    {
      "requirement_id": "FR22",
      "description": "O sistema deve permitir que o jogador compreenda os comandos básicos diretamente pela interface inicial.",
      "requirement_type": "Should",
      "priority": "Medium",
      "derived_from": "E4_aprende_explorando, E5_orientacoes_simples"
    },
    {
      "requirement_id": "FR23",
      "description": "O sistema deve apresentar visualmente os obstáculos e inimigos de forma distinguível entre si.",
      "requirement_type": "Must",
      "priority": "High",
      "derived_from": "S2_leitura_facil_de_obstaculos_e_inimigos"
    },
    {
      "requirement_id": "FR24",
      "description": "O sistema deve apresentar visualmente os efeitos e as descrições das escolhas de progressão para facilitar a tomada de decisão.",
      "requirement_type": "Should",
      "priority": "Medium",
      "derived_from": "P1_dificuldade_em_compreender_melhorias"
    }
  ],
  "non_functional_requirements": [
    {
      "requirement_id": "NFR1",
      "description": "O tempo entre o comando do jogador e a resposta visual correspondente não deve exceder 100 ms em 95% das interações.",
      "requirement_type": "Must",
      "priority": "High",
      "derived_from": "E1_resposta_rapida_aos_comandos"
    },
    {
      "requirement_id": "NFR2",
      "description": "A taxa de atualização da renderização deve permanecer igual ou superior a 60 FPS em pelo menos 95% do tempo de jogo.",
      "requirement_type": "Must",
      "priority": "High",
      "derived_from": "P1_desempenho_inconsistente, E2_lentidao_de_desempenho"
    },
    {
      "requirement_id": "NFR3",
      "description": "A taxa de detecção incorreta de colisões deve ser inferior a 0,1% dos eventos registrados durante testes.",
      "requirement_type": "Must",
      "priority": "High",
      "derived_from": "W4_colisoes_inconsistentes, P1_colisoes_imprecisas"
    },
    {
      "requirement_id": "NFR4",
      "description": "A taxa de encerramentos inesperados da aplicação deve ser inferior a 0,01% das sessões executadas.",
      "requirement_type": "Must",
      "priority": "High",
      "derived_from": "E2_travamentos"
    },
    {
      "requirement_id": "NFR5",
      "description": "A aplicação deve iniciar uma nova sessão em no máximo 3 segundos após o acionamento do botão de reinício.",
      "requirement_type": "Must",
      "priority": "High",
      "derived_from": "P2_reinicio_rapido_apos_derrota"
    },
    {
      "requirement_id": "NFR6",
      "description": "A tela inicial deve permitir iniciar uma partida em no máximo dois cliques ou ações equivalentes.",
      "requirement_type": "Must",
      "priority": "High",
      "derived_from": "C1_interfaces_intuitivas"
    },
    {
      "requirement_id": "NFR7",
      "description": "100% das habilidades apresentadas ao jogador devem exibir identificação textual de nome, nível e raridade com cores adequadas.",
      "requirement_type": "Must",
      "priority": "High",
      "derived_from": "P2_exibicao_objetiva_das_habilidades"
    },
    {
      "requirement_id": "NFR8",
      "description": "100% dos eventos de progressão devem apresentar feedback visual perceptível em até 500 ms após a escolha.",
      "requirement_type": "Must",
      "priority": "High",
      "derived_from": "E1_feedback_visual_imediato"
    },
    {
      "requirement_id": "NFR9",
      "description": "Os elementos visuais críticos para sobrevivência devem permanecer totalmente visíveis dentro da área jogável em 100% do tempo.",
      "requirement_type": "Must",
      "priority": "High",
      "derived_from": "S2_leitura_facil_de_obstaculos_e_inimigos"
    },
    {
      "requirement_id": "NFR10",
      "description": "A aplicação deve ser compatível com navegadores modernos baseados em Chromium, Firefox e WebKit.",
      "requirement_type": "Must",
      "priority": "High",
      "derived_from": "C1_compatibilidade_com_diferentes_dispositivos"
    },
    {
      "requirement_id": "NFR11",
      "description": "A aplicação deve executar corretamente em resoluções de desktop e smartphone sem perda de funcionalidades (suportando toques na tela dividida).",
      "requirement_type": "Must",
      "priority": "High",
      "derived_from": "E3_notebook_desktop_smartphone_android"
    },
    {
      "requirement_id": "NFR12",
      "description": "O carregamento inicial da aplicação deve ocorrer em até 2 segundos em condições normais de execução local.",
      "requirement_type": "Must",
      "priority": "High",
      "derived_from": "C1_carregamento_rapido"
    },
    {
      "requirement_id": "NFR13",
      "description": "A interface deve limitar elementos simultaneamente exibidos a apenas aqueles necessários para a tomada de decisão imediata do jogador.",
      "requirement_type": "Should",
      "priority": "Medium",
      "derived_from": "P1_excesso_de_elementos_visuais"
    },
    {
      "requirement_id": "NFR14",
      "description": "100% dos obstáculos, inimigos e projéteis devem possuir representação visual distinta.",
      "requirement_type": "Must",
      "priority": "High",
      "derived_from": "S2_leitura_facil_de_obstaculos_e_inimigos"
    },
    {
      "requirement_id": "NFR15",
      "description": "A aplicação deve exigir no máximo 1 minuto para que um novo usuário compreenda as ações básicas disponíveis por observação da interface.",
      "requirement_type": "Should",
      "priority": "Medium",
      "derived_from": "E4_aprende_explorando, E5_orientacoes_simples"
    }
  ]
}