# 🎈 BalloonShooter 开发计划 — Android 优先，快速出 Demo

> 目标一句话：**2~3 周内在安卓真机上跑起来一个"随节拍打气球 + 涂鸦上墙"的可玩 Demo**，全程内建中 / 英 / 日三语；PC 和手柄作为 Demo 之后的第一阶段扩展。

---

## 1. 现状盘点

| 项 | 状态 |
|----|------|
| 引擎 | Unity 6000.0.77f1 + URP 17.0.4（模板已自带 Mobile / PC 双渲染管线配置 `Assets/Settings/Mobile_RPAsset` / `PC_RPAsset`） |
| 输入 | Input System 1.19 已装，`InputSystem_Actions.inputactions` 为模板默认，未定制 |
| 代码 | **尚无游戏代码**，只有 SampleScene 与模板资源 |
| 设计 | README 已定义完整愿景（节奏射击 + 涂鸦 + 彩蛋 + 多模式），原文案为"手柄优先" |
| 多语言 | 未接入 |

**结论：从零开工，愿景已清晰，需要做的是砍出一个最小可玩范围并按安卓触屏重排优先级。**

---

## 2. 优先级调整

1. **主目标：安卓手机（触屏）Demo** —— 所有设计决策以"手机上单指点按好玩"为准
2. **后续：Windows PC + 手柄** —— 输入层从第一天就用 Input System 的 action map 抽象，手柄届时只是加一个 control scheme，不返工
3. **三语（简体中文 / English / 日本語）从第一天就是硬约束** —— 所有 UI 文案一律走本地化表，禁止硬编码字符串（事后补翻译便宜，事后改架构昂贵）

---

## 3. Demo 范围：做什么 / 不做什么

### ✅ Demo 内（最小可玩闭环）

- 1 个场景（一面主墙的小房间）、1 首歌（2~3 分钟）
- **Beat 气球**（拍点生成、临近甜点窗口发光）
- 触屏点按射击 + **屏幕空间点按辅助**（点击点附近半径内最近的气球算命中）
- 判定：Perfect / Good / Pop（无 Miss、无失败、无惩罚）
- 爆破 → 墙面**涂鸦贴花**（Perfect 更大更艳）+ 粒子 + 分层音效
- 得分 / 连击 HUD，标题页 → 游玩 → 结算页完整流程
- 设置页：语言切换、音量
- 三语 UI（zh-Hans / en / ja），系统语言自动匹配

### ⏳ Demo 后（明确延后，README 愿景不变）

- Hold / Chain / Echo 气球、自适应密度、Slow-Bloom 慢动作
- One-Button 模式、Zen Paint、Daily Wall、Egg Hunt 等模式
- 彩蛋气球全家桶（Demo 可留 1 个金气球当彩头，可选）
- 模板喷漆、墙面快照导出
- **PC + 手柄（后续第一优先，见 §8）**

---

## 4. 里程碑

> 估时按每天可投入开发计；业余时间按比例放宽。每个里程碑结束都在真机装一次 APK。

### M0 — 项目底座（1~2 天）

- 切换 Android 平台；横屏；`Application.targetFrameRate = 60`
- **Scripting Backend 直接用 IL2CPP + ARM64**（新款安卓机不少已是 64 位 only，Mono/ARMv7 包装不上；构建慢就平时在 Editor 迭代、按里程碑上真机）
- Min API Level 24（Android 7.0）起步即可
- 安装包：`com.unity.localization`（会带上 Addressables 依赖，正常）
- 引入字体：Noto Sans SC / Noto Sans JP（SIL OFL，可免费商用）
- 建目录骨架：`Scripts/{Core, Rhythm, Shooting, Paint, UI}`、`Prefabs`、`Audio`、`Materials`、`Settings/Beatmaps`
- 确认 Android 构建走 `Mobile_RPAsset`（阴影关 / MSAA 2x / 无后处理重效果）

### M1 — 核心玩法闭环（3~5 天）★ 最重要

- **BeatClock**：`AudioSettings.dspTime` 调度播放 + `AudioSource.timeSamples` 读取当前拍（采样级精度、不漂移）
- **BeatmapAsset (ScriptableObject)**：Demo 只存 BPM + 首拍 offset + 段落密度，**按拍自动生成气球，不逐首打谱**——这是快速出 Demo 的关键取舍
- BalloonSpawner + 对象池；气球带"甜点窗口临近发光"
- 触屏输入：Input System（EnhancedTouch）点按 → `ScreenPointToRay` + 屏幕空间最近目标辅助（辅助半径约 80~120 px，真机调）
- 判定窗口：Perfect ±70ms / Good ±150ms / 其余任意时刻 Pop 均有效
- 得分、连击、简单 HUD（占位文案也走本地化 key）

**M1 验收：手机上跟着一首歌点气球，Perfect 手感成立。手感不成立就停下调手感，不进 M2。**

### M2 — 涂鸦与打击感（2~3 天）

- URP Decal Projector 贴花池（上限 64~128 张，先进先出回收）；Perfect 用大号溅射贴图
- **真机若贴花压力大 → 退化方案：贴墙 Quad + 半透明溅射材质**（效果九成、成本更低，Demo 完全够用）
- 爆破粒子、音效分层（Perfect 额外加一层音色）、轻微屏幕脉冲
- Perfect 时轻震动（Android Vibrator 短脉冲，可在设置关闭）

### M3 — 三语与 UI（2~3 天）

- Unity Localization：Locale 建 `zh-Hans` / `en` / `ja`；String Table（`UI` 表 + `Gameplay` 表）
- 启动按 `Application.systemLanguage` 自动选择，设置页可手动切换并存入本地 JSON
- TMP 字体方案：主字体 → Noto Sans SC → Noto Sans JP 的 **fallback 链**；Demo 阶段用**动态字体 atlas**（省事），发布前再按实际用字烘焙静态 atlas
- 完成 UI 流程：标题页 / 开始（单曲）/ 暂停 / 结算页 / 设置页，三语各过一遍排版（日文偏长、注意按钮宽度）

### M4 — 真机调优与打包（约 2 天）

- 中端机（骁龙 7 系 / 天玑 8000 档）稳 60fps；用 Profiler 确认贴花与粒子开销
- 音频延迟：Android 上选低延迟音频设置；**Demo 演示用扬声器**（蓝牙耳机延迟明显，校准功能留到后续）
- 打正式 APK，过一遍 §9 验收清单

**合计：约 10~14 个工作日。**

---

## 5. 多语言方案（贯穿全程的规矩）

- **规矩一条：任何面向玩家的字符串必须走 LocalizedString / 本地化表，代码里禁止硬编码文案**
- Key 命名：`menu.play`、`settings.language`、`result.perfect_count` 之类的分层命名
- 英文复数、格式差异用 Localization 的 Smart String 处理
- CJK 要点：
  - 动态 atlas 阶段中日字库不用操心；烘焙静态 atlas 时用文案实际出现的字生成，控制内存
  - 日文文案通常比中英文长 20~40%，UI 布局用自动伸缩，别写死宽度
- 三语文案量在 Demo 阶段很小（约 30~50 条），自己维护一张表即可，不需要外部工具

---

## 6. 关键技术决策（为什么这样最快）

| 决策 | 理由 |
|------|------|
| 不打谱，BPM 自动生成拍点 | 逐首打谱是 Demo 最大的隐形时间坑；自动拍点足以验证"跟着节拍打气球爽不爽" |
| 触屏辅助用屏幕空间最近目标 | 比准星磁吸简单一个量级，且天然适合点按；准星磁吸留给手柄阶段 |
| Input System action map 从第一天抽象 | 手柄 / 鼠标后续只是新增 control scheme + 绑定，玩法代码零改动 |
| IL2CPP + ARM64 一步到位 | 避免 Mono 包在 64 位 only 新机型上装不上的坑 |
| 贴花保留 Quad 退化路线 | URP Decal 在移动端有真实开销，先做池化，压不住就换 Quad，不赌 |
| 无 Miss、无失败从 Demo 起生效 | 这是产品灵魂，也顺便省掉失败流程的全部 UI/逻辑 |

---

## 7. 风险与对策

| 风险 | 对策 |
|------|------|
| URP Decal 移动端性能不足 | 池化上限 + Quad 退化方案（M2 已内置该路线） |
| CJK 字体 atlas 内存 / 包体 | Demo 动态 atlas；发布前按实际用字烘焙静态 atlas |
| 音画同步（尤其蓝牙耳机） | Demo 用扬声器演示；音频偏移校准做成后续设置项 |
| IL2CPP 构建慢拖累迭代 | 日常在 Editor 内迭代（触屏用鼠标模拟），按里程碑上真机 |
| 手感调不好 | M1 设了硬性验收关卡：手感不成立不进入下一里程碑 |

---

## 8. Demo 之后的路线

- **P1 — PC + 手柄**（Demo 后第一件事）：Windows 构建；同一份 `.inputactions` 增加 Gamepad / 键鼠 scheme；准星磁吸 + 右摇杆软吸附换目标；One-Button 模式放在这一阶段做（它本质是手柄/放松玩法）；Perfect 手柄震动
- **P2 — 完整节奏系统**：Hold / Chain / Echo 气球、逐首谱面（此时再打谱）、自适应密度、Slow-Bloom
- **P3 — 模式与彩蛋**：Zen Paint、Daily Wall、Egg Hunt、彩蛋气球全集、模板喷漆、墙面快照导出 PNG
- **P4 — 增强**：自定义音乐导入 + BPM 检测、色盲配色、更多曲目

---

## 9. Demo 验收清单

- [ ] 中端安卓真机稳 60fps，一首歌 2~3 分钟完整可玩
- [ ] 气球随节拍生成，Perfect / Good / Pop 判定准确、音效反馈分层
- [ ] 每次爆破在墙上留下贴花，Perfect 明显更大更艳，一首歌下来墙面像一幅画
- [ ] 全程无失败、无惩罚、无 Miss 文案
- [ ] 标题 → 游玩 → 结算 → 再来一局 流程完整
- [ ] 中 / 英 / 日三语切换即时生效，系统语言自动匹配，CJK 无豆腐块、无布局溢出
- [ ] 所有文案均来自本地化表（抽查代码无硬编码字符串）
