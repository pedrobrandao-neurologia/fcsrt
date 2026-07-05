# FCSRT — Teste de Recordação Seletiva Livre e com Pista (versão digital, por voz)

Aplicação web autocontida (um único arquivo `index.html`) que operacionaliza um
paradigma de memória derivado do **FCSRT-IR** (*Free and Cued Selective
Reminding Test — with Immediate Recall*; Grober & Buschke). O objetivo é
distinguir déficits de **armazenamento/consolidação** (perfil amnéstico do tipo
hipocampal) de déficits de **recuperação** (perfil frontossubcortical),
controlando a codificação e coordenando explicitamente a pista de codificação
com a pista de recuperação.

A evocação é captada por **voz** (Web Speech API), com *fallback* automático para
**digitação**. Todos os dados permanecem no dispositivo — nada é enviado a
servidores — e são exportados em **JSON** e **CSV** ao final.

> **Aviso.** Ferramenta de pesquisa/apoio. As saídas **não são diagnósticas** e
> não substituem avaliação clínica. Requer dados normativos locais para
> interpretação.

---

## 1. Fundamentação do paradigma

O FCSRT distingue-se dos demais testes de aprendizado de lista de palavras por
duas propriedades:

1. **Codificação controlada.** Antes da memorização, garante-se que cada item foi
   efetivamente processado de forma semântica. O item só é considerado
   *codificado* quando o participante o identifica corretamente a partir da sua
   categoria (recordação imediata com pista).
2. **Coordenação codificação ↔ recuperação.** A **mesma** categoria semântica que
   serviu de pista na codificação é usada como pista na recuperação. Isso torna a
   pista *ótima* e isola o que resta quando a codificação já foi assegurada: se o
   item foi codificado mas não é recuperado nem com a pista ideal, o problema é de
   **armazenamento**, não de recuperação.

Por essa razão o FCSRT é recomendado pelo *International Working Group* (Dubois et
al.) como marcador da síndrome amnéstica do tipo hipocampal. A versão de
referência é a **FCSRT-IR** internacional, de **16 itens**.

### Estrutura dos estímulos

16 itens, cada um pertencente a uma **categoria semântica única e distinta** (16
categorias diferentes — ao contrário do CVLT/HVLT, em que várias palavras
compartilham uma categoria). A versão verbal é apresentada digitalmente. O banco
incluído é original (não reproduz a lista proprietária de Grober-Buschke) e é
**substituível** por itens normatizados para a população-alvo:

| # | Item | Categoria (pista) | # | Item | Categoria (pista) |
|---|------|-------------------|---|------|-------------------|
| 1 | UVA | FRUTA | 9 | TÊNIS | CALÇADO |
| 2 | MARTELO | FERRAMENTA | 10 | FÍGADO | ÓRGÃO DO CORPO |
| 3 | TARTARUGA | RÉPTIL | 11 | ALFACE | VERDURA |
| 4 | VIOLINO | INSTRUMENTO MUSICAL | 12 | SATURNO | PLANETA |
| 5 | GIRASSOL | FLOR | 13 | PARDAL | AVE |
| 6 | CADEIRA | MÓVEL | 14 | ESMERALDA | PEDRA PRECIOSA |
| 7 | SARDINHA | PEIXE | 15 | ALECRIM | TEMPERO |
| 8 | ENFERMEIRO | PROFISSÃO | 16 | CAMINHÃO | VEÍCULO |

---

## 2. Fases da aplicação

O fluxo é autoguiado e executado inteiramente no dispositivo.

### Fase A — Codificação controlada + Recordação Imediata (IR)
Os itens são apresentados em **cartões** (por padrão 4 itens por cartão). Para
cada cartão:
- **Identificação com pista:** para cada categoria mostrada, o participante
  **toca** no item correspondente entre as opções do cartão (análogo do
  "apontar" do FCSRT). Após 2 erros, o item correto é revelado (aprendizagem com
  erro mínimo). O acerto é seguido da apresentação item↔categoria como
  confirmação de codificação.
- **Verificação de IR:** com os itens ocultos, pede-se — item a item, pela
  categoria — que o participante **diga (ou digite)** qual era o item. Itens não
  recuperados na IR são **reapresentados** e retestados (até `maxIrRounds`
  rodadas, padrão 2). Registra-se se cada item foi codificado e em quantas
  rodadas.

### Fase B — Intervalo preenchido (interferência, ~2 min)
Tarefa distratora para impedir a manutenção do material na memória de trabalho e
introduzir demanda de recuperação de longo prazo. Duas opções:
- **Selecionar o maior número** (4 opções); ou
- **Cálculo aritmético simples** (adição, subtração ou multiplicação).

Registram-se acurácia e tempo de reação médio dos acertos.

### Fase C — Evocação livre
Sem pistas, o participante evoca **todas as palavras que lembra**. Por voz, a
transcrição contínua é comparada aos alvos em tempo real; encerra-se ao dizer
"terminei"/"acabei" ou tocando em **Terminei**. Registram-se itens recuperados,
ordem, latências e **intrusões**.

### Fase D — Evocação seletiva com pista
Apenas para os itens **não** evocados livremente, apresenta-se a **categoria**
(pista de recuperação, idêntica à da codificação) e pede-se o item. O ganho
obtido aqui é a medida-chave de sensibilidade à pista.

Ao final, uma tela de **Resultados** consolida as métricas e habilita a
exportação.

---

## 3. Métricas e interpretação

| Sigla | Métrica | Definição |
|-------|---------|-----------|
| **FR** | Free Recall | nº de itens evocados na fase livre (Fase C) |
| **CR** | Cued Recall | nº de itens adicionais recuperados com pista (Fase D) |
| **TR** | Total Recall | FR + CR |
| **ISC** | Índice de Sensibilidade à Pista | `(TR − FR) / (n − FR)` = proporção do que faltava que a pista resgatou |
| — | Intrusões (livre / com pista) | respostas não pertencentes à lista |
| — | Codificados na IR | itens confirmados na verificação de recordação imediata |

**Leitura orientadora (não diagnóstica):**
- **TR preservado com ganho expressivo pela pista** (ISC alto) → perfil de
  **recuperação** (frontossubcortical): o traço está armazenado, a pista o
  resgata.
- **TR baixo apesar da pista** (ISC baixo), com intrusões → perfil de
  **armazenamento/consolidação** (amnéstico tipo hipocampal): o traço não foi
  consolidado, e nem a pista ótima o recupera.

Interpretar sempre com dados normativos locais.

### Adaptações em relação ao FCSRT-IR clássico
- Evocação por **voz** (com *fallback* de digitação), em vez de resposta oral
  transcrita manualmente pelo aplicador.
- **Uma** evocação após o intervalo preenchido; **não** inclui a evocação tardia
  de 20–30 min nem os três ensaios de recordação da versão longa. Estas escolhas
  estão registradas no campo `paradigm` do JSON exportado.

---

## 4. Arquitetura do código

Tudo em **`index.html`** (HTML + CSS + JavaScript *vanilla*, sem dependências,
sem build, sem rede). Roda abrindo o arquivo no navegador ou servido como página
estática.

### Organização (blocos no `<script>`)
- **Banco de estímulos** — `FCSRT_ITEMS` (item + categoria/pista).
- **Utilitários** — `shuffle`, `chunk`, `randInt`, `mean`, `sd`, `escapeHtml`.
- **Pareamento de respostas** — `normalize` (maiúsculas, remoção de acentos via
  NFD), `levenshtein` (distância de edição), `stemPt` (redução de plurais do
  português) e `matchesTarget`, que combina: igualdade exata → igualdade de
  *stem* → tolerância de Levenshtein dependente do comprimento (0 para itens
  ≤4 letras, 1 para ≤7, 2 para maiores). `parseTranscript` extrai itens
  reconhecidos e intrusões de uma transcrição.
- **Reconhecimento de voz** — objeto `Speech` encapsulando a Web Speech API
  (`SpeechRecognition`/`webkitSpeechRecognition`): modo contínuo, resultados
  intermediários, reinício automático no `onend`, idioma `pt-BR`. `ensureMic`
  testa a permissão de microfone via `getUserMedia`.
- **Fases** — `runEncoding` (A), `runInterference` (B), `runFreeRecall` (C),
  `runCuedRecall` (D), cada uma com variantes **voz** e **digitação**.
- **Resultados e exportação** — `computeSummary`, `renderResults`, `exportJSON`,
  `exportCSV`.
- **Orquestração** — `startSession` encadeia A→B→C→D; `beginFlow` decide
  voz × digitação conforme suporte e permissão de microfone.

### Parâmetros configuráveis (tela inicial → "Parâmetros do teste")
| Campo | Chave | Faixa | Padrão |
|-------|-------|-------|--------|
| Nº de itens | `nItems` | 8–16 | 16 |
| Itens por cartão | `perCard` | 2–4 | 4 |
| Confirmação de codificação (ms) | `confirmMs` | 600–2500 | 1100 |
| Reapresentação por item (ms) | `restudyMs` | 2000–8000 | 4000 |
| Tarefa de interferência | `interferenceType` | maior número / cálculo | maior número |
| Interferência / intervalo (s) | `interferenceDurationMs` | 30–180 | 120 |
| — (fixo) | `fixationMs` | — | 800 |
| — (fixo) | `maxIrRounds` | — | 2 |

Todos os valores passam por *clamp* aos limites acima.

### Exportação de dados
- **JSON** (`fcsrt_<id>_<timestamp>.json`): sessão completa — configuração,
  `targets`, `cards`, `encoding` (IR por item, rodadas, latências, erros de
  identificação), `interference` (ensaios, acurácia, TR médio), `recall`
  (livre/com pista, ordem, latências, transcrições/entradas, intrusões) e
  `summary`.
- **CSV** (`fcsrt_<id>_<timestamp>.csv`): formato **longo**, uma linha por item,
  com BOM UTF-8 e quebras `\r\n` (compatível com Excel e pronto para o R).
  Colunas: `patientId, recallMode, itemIndex, item, category, cardIndex,
  encodedIR, irRounds, freeRecalled, freeRT_ms, cuedRecalled, cuedRT_ms,
  finalStatus, FR_total, CR_total, TR_total, ISC, freeIntrusions,
  cuedIntrusions, interfType, interfAccuracy, interfMeanRT_ms`.

---

## 5. Compatibilidade multiplataforma

| Recurso | Comportamento |
|---------|---------------|
| **Reconhecimento de voz** | Disponível em **Chrome/Edge** (desktop e Android). Onde a Web Speech API ou o microfone não estão disponíveis (ex.: Firefox, alguns iOS/Safari, `file://` sem `mediaDevices`), a aplicação detecta e **degrada automaticamente para digitação**, sem interromper o teste. |
| **Toque e mouse** | Interface por toques grandes (alvos ≥ 74 px), também operável a mouse; `inputMode` registrado na sessão. |
| **Áudio (`beep`)** | `AudioContext` com *fallback* `webkitAudioContext` e `resume()` automático quando suspenso pela política de *autoplay* (Chrome/Safari). |
| **Download dos arquivos** | O elemento `<a download>` é anexado ao DOM antes do clique e removido em seguida, garantindo o *download* também no **Firefox**. |
| **Responsividade** | *Layout* fluido até 720 px, com ajustes específicos ≤ 560 px; respeita `prefers-reduced-motion`. |
| **Privacidade** | 100% no dispositivo; nenhum dado transmitido. |

**Requisitos:** navegador moderno. Para responder **por voz**, use Google Chrome
ou Microsoft Edge (desktop ou Android) e conceda acesso ao microfone. Em outros
navegadores o teste roda normalmente em **modo digitado**.

### Correções recentes
- **Pareamento de plurais do português** (`stemPt`): plurais irregulares
  (`-ão→-ões`, `-l→-is`, `-m→-ns`, `-r/-z/-s→-es`) agora são reconhecidos —
  ex.: *caminhões→CAMINHÃO*, *girassóis→GIRASSOL*, *pardais→PARDAL*,
  *alecrins→ALECRIM* —, sem gerar falsos positivos em intrusões.
- **`AudioContext` suspenso**: `resume()` garante o sinal sonoro sob a política de
  *autoplay*.
- **Download no Firefox**: âncora inserida no DOM antes do clique.

---

## 6. Como executar

1. Baixe/clone o repositório.
2. Abra `index.html` no navegador **ou** sirva a pasta como conteúdo estático
   (ex.: `python3 -m http.server`) — recomendado para permitir o microfone, que
   alguns navegadores restringem em `file://`.
3. Informe um **código** do participante (sem dados pessoais), ajuste os
   parâmetros se necessário e inicie.
4. Ao final, exporte **JSON** e/ou **CSV**.

---

## 7. Limitações

- Ferramenta **não diagnóstica**; exige normas locais.
- O banco de 16 itens é original e deve ser validado/normatizado para a
  população-alvo antes de uso clínico.
- O reconhecimento de voz depende do navegador e da qualidade do áudio; o modo
  digitado é o *fallback* universal.
- Não implementa a evocação tardia de 20–30 min da versão longa do FCSRT.
