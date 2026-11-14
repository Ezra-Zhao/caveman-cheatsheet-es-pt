# 第一次上手演练 | First-run drill

从零到跑通，按顺序来（命令均出自上游 README，2026-11-14 口径）：

1. 先看诚实数字卡再装：你的计费方式若是按次计费（如 Copilot 按请求数），或你的会话以短问答为主，先别装——规则本身约吃 1,160 个输入 token，短会话净亏。
2. 装：`npx @caveman-ai/cli@latest`（Node.js 22.13 以上）。装过旧版务必带 `@latest`。介意统计的话装完先关：`caveman telemetry off`。
3. 验状态：`caveman status`，确认六件套开关与它覆盖到的 agent 列表无误；暂时用不上的先关，如 `caveman off browse`。
4. 只开 Say less 跑一个真实任务：在 agent 里打 `/caveman`，拿你手头一个真实的排障或改代码任务跑完，对照你平时的输出长度与账单感受一下；再用 `/ultracave` 跑同类任务对比。觉得太短就退回 `/caveman` 档，别硬扛。
5. 会话总被日志撑爆的，再验 Read less：拿一次测试运行的完整输出走一遍，对比它压缩前后的体积（原仓口径单文件可压 98% 上下，但整场会话大约是三成，别拿单文件数字当预期）。
6. 想试 Auto 的：`caveman login`（免费账号），先读一遍 README 的 Privacy 一节再决定——登录并选用 Auto 后，你的提问及附带上下文会发给 Caveman Cloud 做路由。不想用随时 `caveman off routing`。
7. 退出与卸载：agent 里说 `stop caveman` 退出语气模式；只装了 skill 的用 `npx skills remove JuliusBrussee/caveman -g` 移除。
