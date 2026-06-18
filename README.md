# Void Dash — Anomalia

![Game](https://img.shields.io/badge/status-conclu%C3%ADdo-brightgreen)
![Plataforma](https://img.shields.io/badge/plataforma-web-blue)
![Stack](https://img.shields.io/badge/stack-HTML5%20Canvas%2FVanilla%20JS-orange)

**Void Dash** é um *endless-runner / rogue-lite* de ação com temática neon e física baseada em gravidade. Desenvolvido em HTML5 Canvas puro (Vanilla JS), o jogo roda inteiramente em um único arquivo — sem dependências, sem assets externos, sem build step.

**Colaboradores:** João Victor de Almeida Santos, Albert Vinicius Pinto Carvalho,
Carlos Henrique Pinto Oliveira

---

## Contexto do Projeto

O sistema combina as mecânicas de **corrida infinita** com elementos **rogue-lite**. O jogador controla uma entidade que avança continuamente por um ambiente hostil gerado de forma procedural, desviando de obstáculos e derrotando inimigos. A cada 30 segundos (ou 600 pontos), o tempo congela e o jogador escolhe entre habilidades aleatórias para evoluir seu personagem. O objetivo final é sobreviver até o confronto com o **Arquiteto do Vazio**, o chefe final.

### Persona Principal: Kleber

Kleber tem 25 anos, é analista de suporte técnico e cursa TI. Gosta de jogos desafiadores que exigem reflexos rápidos e tomada de decisão constante. Valoriza sistemas previsíveis, colisões justas e feedback visual imediato. Fica frustrado com mortes injustas ou colisões inconsistentes — por isso o jogo prioriza **estabilidade de física** e **detecção de colisão confiável** (< 0,1% de erro).

---

## Metodologia Ágil — Épicos e User Stories

### Épico 1: Controles e Movimentação

| # | User Story | FR |
|---|-----------|----|
| US01 | Como jogador, eu quero pular para evitar fendas no chão e inimigos rasteiros. | FR1 |
| US02 | Como jogador, eu quero atacar para destruir inimigos e projéteis. | FR2 |
| US03 | Como jogador, eu quero rebater (deflect) para refletir projéteis e esferas do chefe. | FR2.5 |

### Épico 2: Pontuação e Progressão

| # | User Story | FR |
|---|-----------|----|
| US04 | Como jogador, eu quero ver minha pontuação atual durante a partida. | FR3 |
| US05 | Como jogador, eu quero ganhar pontos por tempo de sobrevivência. | FR4 |
| US06 | Como jogador, eu quero ganhar pontos extras ao destruir inimigos. | FR5 |

### Épico 3: Feedback Visual

| # | User Story | FR |
|---|-----------|----|
| US07 | Como jogador, eu quero ver feedback visual quando destruo um inimigo. | FR6 |
| US08 | Como jogador, eu quero ver feedback visual quando sofro dano fatal. | FR7 |
| US09 | Como jogador, eu quero ver uma tela de Game Over ao morrer. | FR8 |
| US10 | Como jogador, eu quero reiniciar a partida rapidamente após Game Over. | FR9 |
| US11 | Como jogador, eu quero ver uma tela de vitória ao derrotar o chefe. | FR10 |

### Épico 4: Habilidades e Progressão Rogue-lite

| # | User Story | FR |
|---|-----------|----|
| US12 | Como jogador, eu quero escolher entre habilidades durante a partida. | FR12 |
| US13 | Como jogador, eu quero ver o nome e nível de cada habilidade. | FR13, FR14 |
| US14 | Como jogador, eu quero ver o nível da habilidade atualizado após evoluir. | FR15 |
| US15 | Como jogador, eu quero que habilidades maximizadas saiam das opções futuras. | FR16 |

### Épico 5: Chefão

| # | User Story | FR |
|---|-----------|----|
| US16 | Como jogador, eu quero um aviso visual antes do chefe aparecer. | FR19 |

### Épico 6: Interface Inicial

| # | User Story | FR |
|---|-----------|----|
| US17 | Como jogador, eu quero uma tela inicial simples para começar a jogar. | FR21 |
| US18 | Como jogador, eu quero entender os controles básicos pela interface inicial. | FR22 |

---

## Instruções de Execução

### Pré-requisitos

Nenhum. Apenas um navegador web moderno (Chromium, Firefox, WebKit).

### Opção 1 — Abrir diretamente (recomendado)

Abra o arquivo `index.html` no navegador com um duplo clique.

### Opção 2 — Servidor HTTP local

```bash
# Python 3
python3 -m http.server 8080
```

Depois acesse [http://localhost:8080](http://localhost:8080).

```bash
# Node.js (alternativa)
npx serve .
```

### Testar o Chefão Diretamente

Para pular a fase de progressão e ir direto para a luta contra o **Arquiteto do Vazio**, acesse:

```
http://localhost:8080/boss_test.html
```

O arquivo `boss_test.html` é uma versão de teste com um modal que permite escolher habilidades rapidamente. Após selecionar algumas habilidades (ou fechar o modal), o chefe é invocado automaticamente. Use esta página para treinar o deflect e testar padrões do boss sem precisar jogar a partida inteira.

---

## Instruções de Uso

### Controles

| Ação | Teclado | Mobile |
|------|---------|--------|
| **Pular** | `W` / `↑` / `Espaço` | Toque no terço esquerdo da tela |
| **Atacar** | `Z` / `J` | Toque no terço central da tela |
| **Rebater (Deflect)** | `X` | Toque no terço direito da tela |
| **Iniciar / Reiniciar** | `Espaço` / `Enter` | Toque em qualquer lugar |

### Mecânicas

- **Pulo**: Pulos adicionais são desbloqueados com a habilidade *Botas Gravitacionais*.
- **Ataque**: Pode ser segurado para carregar (com a habilidade *Eco de Matéria* Nível 2+). Soltar cedo dispara um projétil básico; segurar até o limite dispara um projétil perfurante ou um laser massivo.
- **Rebater (Deflect)**: Cria um campo de repulsão. No chão, reflete projéteis inimigos. No ar (com *Distorção de Tempo* Nível 1+), ativa um **Ground Slam** — mergulho rápido que causa onda de choque ao atingir o chão.
- **Habilidades**: A cada 30s ou 600 pontos, escolha entre 3 habilidades aleatórias para evoluir seu personagem. Cada habilidade tem 3 níveis.
- **Chefão**: Ao atingir 3.000 pontos ou 150 segundos, o *Arquiteto do Vazio* aparece. Use o deflect ou ataque para rebater as esferas de energia de volta contra ele e reduzir seu HP (7 pontos de vida).

### Dicas

- As **Fendas Espaciais** (buracos roxos) são fatais — pule sobre elas.
- As **Minas Temporais** (octógonos vermelhos) são invulneráveis a ataques básicos — desvie.
- Os **Drones do Vazio** atiram projéteis e saltam quando você está no ar — derrube-os ou use o deflect.
- O **Eco de Matéria Nível 3** (laser massivo) causa dano direto ao chefe.
- A **Distorção de Tempo Nível 3** (retrocesso) salva uma morte por partida — mas o jogador é reposicionado.

---

## Stack Técnico

- **Renderização**: HTML5 Canvas 2D (formas geométricas com efeitos neon `shadowBlur`)
- **Linguagem**: JavaScript Vanilla (sem frameworks, sem classes)
- **Arquitetura**: Arquivo único — HTML + CSS + JS inline
- **Física**: Simulação manual (gravidade, pulo, colisão AABB)
- **Dependências**: Nenhuma

---

## Licença

MIT
