# Caveman 速查表 | Caveman Cheatsheet | Guía rápida de Caveman

> 一页看懂 Caveman 4.0：让编程 agent 少说话、少读、少跑冤枉路的省 token 工具——回答改成极简电报体，代码、命令、报错逐字不动；4.0 起长成六件套，再加一个独立的终端 agent Caveman Code。本页的命门是「诚实数字卡」：它在什么场合省钱、什么场合倒亏，先看完再装。

- 上游项目：[JuliusBrussee/caveman](https://github.com/JuliusBrussee/caveman)（2026-11-14 实测 119,888 ★ / 6,993 forks，56 个 release，Apache-2.0；官网 caveman.so）
- 平台：macOS / Linux / Windows，一条命令装进你已有的 30 多个编程 agent（Claude Code、Codex、Gemini CLI、OpenCode 等）；需要 Node.js 22.13 以上
- 本页结构：中文主文 → English → Español → Português

---

Apoyo y más: [GitHub Sponsors](https://github.com/sponsors/Ezra-Zhao) · [Gumroad — plantillas y cuadernos que uso](https://ezraflow28.gumroad.com) · [Ko-fi](https://ko-fi.com/ezra777zhao)

## 六件套：先开哪件

4.0 的六件套（原仓称 six rocks）装完默认全开，每件有独立开关（`caveman status` 看状态，`caveman off browse` 这样关单件）。按你的痛点对号入座：

| 你的痛点 | 先开哪件 | 它干什么 |
|---|---|---|
| 只想让回答变短、账单先降一截 | **Say less** | 回答走电报体：先给结论、一句一个意思；代码、命令、路径、报错原样保留 |
| 会话被日志、JSON、测试输出撑爆 | **Read less** | 日志、JSON、CSV、测试输出在模型读之前先压缩，原文件留在磁盘上随时可取回 |
| 不想自己判断每个任务用哪个模型 | **Auto** | 按任务挑模型与出力档位；六件里唯一需要账号的，账号免费 |
| agent 每期会话都重写同一批脚本 | **Blocks** | 把 agent 写过的脚本存进仓库复用，下次直接调用，不再重写重读 |
| 不知道 token 到底花在哪 | **Waste fixes** | 找出浪费点并给修法；新版还能在你暂停时帮你保住提示缓存 |
| 让 agent 读网页时正文太大 | **Browse** | 网页先压缩再交给模型读 |

另有独立产品 **Caveman Code**：一个终端编程 agent，主打同一模型、同样活、账单更小（数字是原厂自测口径，见诚实标注区）。它与六件套分开下载，不在本页展开。

## 一行装法

```bash
npx @caveman-ai/cli@latest
```

一条命令，它自己找你机器上的 agent 和登录态，列出要改什么，你按回车确认就完事。三个坑先说清：

- **必须带 `@latest`**：装过旧版的人更要带——不带的话，你机器上的旧副本优先，新版装了也白装。
- 只想要「回答变短」这一件、不要其余五件：`npx skills add JuliusBrussee/caveman -g`，只装 skill 本体。
- 装完在 agent 里打 `/caveman` 启动（Codex 里是 `$caveman`），说 `stop caveman` 退出；`caveman setup` 可以重开设置画面。

## 三档力度

同一句回答，三档压法（原仓口径，tiktoken o200k 计数）：

| 档位 | 回答 | Token |
|---|---|---:|
| 普通 agent | 一大段解释，约 63 token | 63 |
| `/caveman` | New object ref each render, so React re-renders. Wrap the prop in `useMemo`. | 20 |
| `/ultracave` | Inline object prop, new ref, re-render. `useMemo`. | 14 |
| `/megacave` | 文言文式极短句 | 13 |

规则底子学的是航空维修手册的受控英语（ASD-STE100）：一句最多 20 词、主动语态、一个东西只用一个词。关键约束：`not`、`never`、`no`、`only` 这类否定词永不省略，数字与单位永不改动——省的是废话，不是意思。

## 诚实数字卡（先看这一节再决定装不装）

这是本页跟全网吹捧文的分工所在。原仓自己有 HONEST-NUMBERS 文档专列它输的场合，第三方实测也和原厂数字打架，都摆出来：

- **规则本身要花钱**：这套规则约占 1,160 个输入 token。短问答里省下的输出还不够付这笔入场费，**短会话净亏**。
- **按次计费可能倒亏**：GitHub Copilot 这类按请求次数计费的场景，回答长短不影响价格，短回答省不到钱；你付的是 token 钱（API 计费、订阅内额度）才谈得上省。
- **原厂口径**：十道开发题实测，无指令 6,983 输出 token，「Answer concisely」4,334，`/caveman` 4,119，`/ultracave` 2,693。注意第二行——新模型本来就听得懂「简洁回答」，caveman 是在那个基线之上再压一截，不是凭空省一半。
- **JetBrains 实测**（86 个真实编码任务，配对 A/B）：输出只少 **8.5%**，质量无可测损失。原因不难懂：真实会话的大头是代码和工具调用，不是闲聊，嗓门再小也压不动那部分。
- **Adobe Research 口径**：caveman 式输出让成本降 **1.4 到 2.4 倍**（八个模型、五组数据）；**Elastic 口径**：响应 token 少 **63.6%**、自称零信息损失。三家数字差距大，是因为测的东西不同：纯问答、真实编码会话、搜索场景各测各的。你的场景接近哪家，就信哪家的数。
- **Read less 的实测**（原仓口径）：整场 Claude Code 会话输入 token 少 33.2%（54 次运行、18 题全对）；但单个文件压缩率（98% 上下）和整场会话省的比例是两回事，且 HTML 目前还没有压缩器、那一行实测是负的——原仓把红行留在表里，没藏。
- **Auto 的实测**（原仓口径，跑到一半的中期数）：85 个真实编码任务，Auto 成本约为 Opus 最高档的 0.46 倍、得分基本持平；原仓自己注明样本下 0.5 分的差距分辨不出来，且省的主要来自它在简单任务上自动降档——你本来就用中档的话，省的会少得多。

## 安全场合自动收口

电报体有边界：遇到**安全警告、不可逆操作、以及你已经表现出困惑**这三种场合，它自动切回完整句子，把话说全说清，事情过了再恢复极简。你的提示词它永不改写——Adobe 的研究结论是把用户提示也 caveman 化会让答案变差，所以它只动 agent 自己说的话。

## 命令速查

终端里：

```bash
caveman claude        # 在 caveman 模式下启动一个 agent（codex / gemini / opencode / aider 等同理）
caveman status        # 六件套开关状态、覆盖了哪些 agent、下一步建议
caveman off browse    # 关掉单件（on 同理；可选 output / input / waste-fixes / routing / scripts / browse）
caveman login         # 登录免费账号，开 Auto
caveman learn         # 看你的 token 都花在哪
caveman stats         # 你的 token 使用史
caveman doctor        # 一行一个问题，附修法
```

agent 里：`/caveman`、`/ultracave`、`/megacave` 三档力度；`/caveman-commit` 一行规范提交信息；`/caveman-review` 一行一条审查意见；`/caveman-compress <文件>` 压缩 CLAUDE.md 这类记忆文件（自动留备份）；`/caveman-help` 一屏看全。

## 诚实标注区

- **CLI 默认发使用统计**：命令名、token 计数、随机安装 ID、系统与 IP，不含你的提示、代码与文件路径；介意就先关：`caveman telemetry off` 或设 `DO_NOT_TRACK=1`，关了对 Blocks 也生效。只有 skill 本体（不装 CLI）时不发任何东西。
- **Auto 的数据口径**：登录并选用 Auto 后，你的提问（连同 agent 附带的文件，如 CLAUDE.md、@ 引用的文件）、上一轮提问和上次回答的结尾会发给 Caveman Cloud 做路由；免费档下官方口径是可能留存用于改进路由。不想用就 `caveman off routing`，不登录则什么都不发。
- **Caveman Code 的省钱数字是原厂自测**：「比 Codex 便宜 21%、比 Claude Code 便宜 14%」出自原厂 2026 年 11 月自跑的 Terminal-Bench 口径，未见第三方复测，本页只转述出处、不背书。
- **Blocks 还是预发布**：v0.1.0-rc 阶段、仅 Python、无沙箱；且不是每行都赢——原仓表里 `grep-defs` 一行就输给裸 grep，红行照留。
- **本页数字的时点**：星数与 release 数为 2026-11-14 实测；功能与基准以当日上游 README 与其 HONEST-NUMBERS 文档为准，上游迭代极快，装前扫一眼官方文档。

---

## English (summary)

Caveman (by JuliusBrussee, ~119.9k stars as of 2026-11-14, Apache-2.0) makes your coding agent say less and read less: answers switch to a terse telegram style while code, commands, paths, and error messages stay verbatim. Version 4.0 is six parts in one install (`npx @caveman-ai/cli@latest`, Node.js 22.13+, works with 30+ agents): **Say less** shortens answers (63 → 20 tokens on the canonical example; `/ultracave` 14, `/megacave` 13, tiktoken o200k counts), **Read less** compresses logs/JSON/CSV/test output before the model sees them (upstream measures 33.2% fewer input tokens per full session), **Auto** picks model and effort per task (the only part needing an account — free), **Blocks** saves scripts the agent writes so they get reused instead of rewritten, **Waste fixes** finds where your tokens go, and **Browse** compresses web pages before reading.

The honest part, which is why this page exists: the rules themselves cost ~1,160 input tokens, so short Q&A sessions lose money; per-request billing (GitHub Copilot) saves nothing from shorter answers; JetBrains' A/B test on 86 real coding tasks found only 8.5% fewer output tokens (sessions are mostly code and tool calls, not prose) with no measurable quality loss; Adobe Research reports 1.4–2.4× cost cuts and Elastic 63.6% fewer response tokens in their own setups — believe whichever matches your workload. Safety warnings, irreversible actions, and confused users automatically get full sentences. Your own prompts are never rewritten. The CLI sends usage stats by default (`caveman telemetry off` or `DO_NOT_TRACK=1` stops it); the skill alone sends nothing.

---

## Español (guía rápida — traducción de estudio, pendiente de revisión por un hablante nativo)

Caveman (de JuliusBrussee, ~119,9 mil estrellas en noviembre de 2026, Apache-2.0) hace que tu agente de programación hable menos y lea menos: las respuestas pasan a un estilo telegráfico y breve, mientras el código, los comandos, las rutas y los mensajes de error quedan intactos, carácter por carácter. La versión 4.0 son seis piezas en una sola instalación, más un agente de terminal aparte, Caveman Code.

**Instalación (una línea):**

```bash
npx @caveman-ai/cli@latest
```

Necesitas Node.js 22.13 o superior. El instalador detecta tus agentes (Claude Code, Codex, Gemini CLI, OpenCode y más de 30) y tus inicios de sesión, te muestra qué va a cambiar y confirmas con Enter. **El `@latest` es obligatorio si alguna vez instalaste una versión anterior**: sin él, gana la copia vieja que ya tienes en la máquina. Si solo quieres respuestas cortas y nada más: `npx skills add JuliusBrussee/caveman -g` instala solo la skill. Dentro del agente, `/caveman` activa el modo (`$caveman` en Codex) y `stop caveman` lo apaga.

**Las seis piezas — cuál abrir primero:**

- **Say less** — respuestas cortas. Abre esta si solo quieres bajar la factura de salida.
- **Read less** — comprime registros, JSON, CSV y salidas de tests antes de que el modelo los lea; los originales quedan en tu disco. Abre esta si tus sesiones se llenan de salidas enormes.
- **Auto** — elige modelo y nivel de esfuerzo por tarea. Es la única pieza que pide cuenta, y la cuenta es gratis.
- **Blocks** — guarda los scripts que el agente escribe y los reutiliza en vez de reescribirlos cada sesión.
- **Waste fixes** — localiza dónde se van tus tokens y propone correcciones; también mantiene caliente la caché de prompts cuando haces una pausa.
- **Browse** — comprime las páginas web antes de que el agente las lea.

`caveman status` muestra qué está activo; `caveman off browse` apaga una pieza (sustituye `browse` por la que quieras).

**Los tres niveles, con el ejemplo del propio proyecto (conteo tiktoken o200k):** una respuesta normal de 63 tokens pasa a 20 con `/caveman`, a 14 con `/ultracave` y a 13 con `/megacave`. Las reglas siguen el inglés controlado de los manuales de mantenimiento aeronáutico (ASD-STE100): frases de máximo 20 palabras, voz activa, un solo término por cosa. Las negaciones (`not`, `never`, `no`, `only`) y los números nunca se recortan.

**Los números honestos (léelos antes de instalar):**

- Las reglas cuestan unas 1.160 tokens de entrada. En preguntas y respuestas cortas, lo que ahorras en salida no paga esa entrada: **en sesiones cortas pierdes dinero**.
- Si pagas **por solicitud** (como GitHub Copilot), acortar respuestas no ahorra nada: el precio no depende de la longitud.
- Cifras del proyecto: en diez preguntas de desarrollo, la salida baja de 6.983 tokens sin instrucciones a 4.334 con «Answer concisely», 4.119 con `/caveman` y 2.693 con `/ultracave`. Fíjate en la segunda cifra: los modelos nuevos ya saben ser concisos; Caveman recorta sobre esa base, no sobre cero.
- **JetBrains** probó 86 tareas reales de código en A/B: solo **8,5 % menos tokens de salida**, sin pérdida medible de calidad. Es lógico: una sesión real es sobre todo código y llamadas a herramientas, no prosa.
- **Adobe Research** midió costes entre 1,4 y 2,4 veces menores; **Elastic**, un 63,6 % menos de tokens de respuesta en su escenario. Cada laboratorio mide un escenario distinto: cree la cifra que se parezca a tu trabajo.
- Read less, según el proyecto: 33,2 % menos tokens de entrada por sesión completa (54 ejecuciones, 18 de 18 respuestas correctas). Ojo: comprimir un archivo suelto (98 %) no es lo mismo que la sesión entera, y el HTML aún no tiene compresor — en esa fila el resultado fue negativo y el proyecto lo deja publicado.

**Seguridad y privacidad:** ante avisos de seguridad, acciones irreversibles o un usuario visiblemente confundido, el modo telegráfico se apaga solo y vuelve la frase completa. Tus prompts nunca se reescriben. La CLI envía estadísticas de uso por defecto (comandos, conteos de tokens, un ID aleatorio, sistema e IP; nunca tus prompts ni tu código): se apaga con `caveman telemetry off` o `DO_NOT_TRACK=1`. Auto, solo si inicias sesión y lo eliges, envía tu petición y su contexto a Caveman Cloud para enrutar; en el plan gratis pueden conservarla para mejorar el enrutado.

---

## Português (guia rápido — tradução de estudo, aguardando revisão de um falante nativo)

O Caveman (de JuliusBrussee, ~119,9 mil estrelas em novembro de 2026, Apache-2.0) faz seu agente de programação falar menos e ler menos: as respostas viram um estilo telegráfico e curto, enquanto código, comandos, caminhos e mensagens de erro ficam intactos, caractere por caractere. A versão 4.0 são seis peças numa única instalação, mais um agente de terminal separado, o Caveman Code.

**Instalação (uma linha):**

```bash
npx @caveman-ai/cli@latest
```

Você precisa do Node.js 22.13 ou superior. O instalador detecta seus agentes (Claude Code, Codex, Gemini CLI, OpenCode e mais de 30) e seus logins, mostra o que vai mudar e você confirma com Enter. **O `@latest` é obrigatório se você já instalou alguma versão antiga**: sem ele, a cópia velha que está na máquina vence. Se você só quer respostas curtas e nada mais: `npx skills add JuliusBrussee/caveman -g` instala só a skill. Dentro do agente, `/caveman` liga o modo (`$caveman` no Codex) e `stop caveman` desliga.

**As seis peças — qual ligar primeiro:**

- **Say less** — respostas curtas. Ligue esta se você só quer reduzir a conta de saída.
- **Read less** — comprime logs, JSON, CSV e saídas de testes antes de o modelo ler; os originais ficam no seu disco. Ligue esta se suas sessões enchem de saídas enormes.
- **Auto** — escolhe modelo e nível de esforço por tarefa. É a única peça que pede conta, e a conta é grátis.
- **Blocks** — guarda os scripts que o agente escreve e os reutiliza em vez de reescrevê-los a cada sessão.
- **Waste fixes** — localiza para onde vão seus tokens e propõe correções; também mantém o cache de prompts aquecido quando você faz uma pausa.
- **Browse** — comprime as páginas web antes de o agente ler.

`caveman status` mostra o que está ativo; `caveman off browse` desliga uma peça (troque `browse` pela que quiser).

**Os três níveis, com o exemplo do próprio projeto (contagem tiktoken o200k):** uma resposta normal de 63 tokens cai para 20 com `/caveman`, 14 com `/ultracave` e 13 com `/megacave`. As regras seguem o inglês controlado dos manuais de manutenção aeronáutica (ASD-STE100): frases de no máximo 20 palavras, voz ativa, um só termo por coisa. Negações (`not`, `never`, `no`, `only`) e números nunca são cortados.

**Os números honestos (leia antes de instalar):**

- As regras custam cerca de 1.160 tokens de entrada. Em perguntas e respostas curtas, o que você economiza na saída não paga essa entrada: **em sessões curtas você perde dinheiro**.
- Se você paga **por solicitação** (como o GitHub Copilot), encurtar respostas não economiza nada: o preço não depende do tamanho.
- Números do projeto: em dez perguntas de desenvolvimento, a saída cai de 6.983 tokens sem instrução para 4.334 com «Answer concisely», 4.119 com `/caveman` e 2.693 com `/ultracave`. Repare no segundo número: os modelos novos já sabem ser concisos; o Caveman corta sobre essa base, não sobre zero.
- A **JetBrains** testou 86 tarefas reais de código em A/B: só **8,5 % menos tokens de saída**, sem perda mensurável de qualidade. Faz sentido: uma sessão real é sobretudo código e chamadas de ferramentas, não prosa.
- A **Adobe Research** mediu custos de 1,4 a 2,4 vezes menores; a **Elastic**, 63,6 % menos tokens de resposta no cenário dela. Cada laboratório mede um cenário diferente: acredite no número que se parece com o seu trabalho.
- Read less, segundo o projeto: 33,2 % menos tokens de entrada por sessão completa (54 execuções, 18 de 18 respostas certas). Atenção: comprimir um arquivo solto (98 %) não é o mesmo que a sessão inteira, e o HTML ainda não tem compressor — nessa linha o resultado foi negativo e o projeto mantém isso publicado.

**Segurança e privacidade:** diante de avisos de segurança, ações irreversíveis ou um usuário visivelmente confuso, o modo telegráfico desliga sozinho e a frase completa volta. Seus prompts nunca são reescritos. A CLI envia estatísticas de uso por padrão (comandos, contagens de tokens, um ID aleatório, sistema e IP; nunca seus prompts nem seu código): desliga com `caveman telemetry off` ou `DO_NOT_TRACK=1`. O Auto, só se você fizer login e escolhê-lo, envia seu pedido e o contexto dele à Caveman Cloud para roteamento; no plano grátis eles podem reter esse conteúdo para melhorar o roteamento.

---

## 来源与声明

- 灵感来源 / Source / Fuente / Fonte: [JuliusBrussee/caveman](https://github.com/JuliusBrussee/caveman)（官网 caveman.so；诚实数字出处为上游 docs/HONEST-NUMBERS.md 与 README 内所引 Adobe Research、JetBrains、Elastic 三方链接）
- 本仓是个人学习整理：内容用我自己的话总结自上游公开文档，不搬运原文；基准数字按出处逐项标注原厂口径或第三方口径，命令与参数以 2026-11-14 的上游 README 为准，上游迭代很快，请以官方文档为最终依据。
- 西语、葡语部分为学习译稿，待母语校对。Las secciones en español y portugués son traducciones de estudio pendientes de revisión nativa.
- License: MIT © 2026 Guangyi Zhao
