# Storyboard Video Previs Skill

把故事、主题、脚本、MV 概念或广告创意，转成适合视频生成前期使用的电影分镜故事板。

这个 skill 的目标不是简单总结剧情，而是帮助你快速看清最终视频的构图、节奏、镜头语言、动作连续性和情绪走向。它会把一段文字拆成面板序列，并生成可继续用于图像模型或视频预演的故事板提示词。

## 能做什么

1. 从一段故事生成 8、12、16 或 24 面板故事板
2. 为每个面板设计故事节拍、视觉动作、景别、角度、镜头运动和连续性提示
3. 输出可用于 GPT Image 或其他图像模型的整合提示词
4. 适配动作、舞蹈、体育、恐怖、对话、广告、MV 和实验影像
5. 支持粗糙动态漫画草图、黑白预演分镜、电影手绘分镜等风格
6. 支持中文规划，并在需要时补充英文图像生成提示词

## 适合什么时候用

当你有一个视频想法，但还不确定画面是否成立时，可以先用这个 skill 生成故事板。

典型输入包括：

1. 一个短故事
2. 一段广告创意
3. 一个音乐视频概念
4. 一个动作或舞蹈场景
5. 一个图像生成提示词草稿
6. 一个需要扩展成分镜的角色或世界观设定

## 输出内容

默认输出包含四个部分：

1. 故事板创意简报
2. 逐面板分镜表
3. 整合图像生成提示词
4. 负面提示词和质量检查清单

如果你要继续做图生视频，也可以要求它增加每个面板的镜头运动、角色运动、转场和时长建议。

## 安装

把仓库克隆到 Codex skills 目录：

```bash
git clone git@github.com:gainubi/storyboard-video-skill.git ~/.codex/skills/storyboard-video-previs
```

如果你已经有同名目录，可以先换一个目录名，或者备份后再覆盖。

## 使用示例

```text
用 $storyboard-video-previs 把下面这个故事做成 16:9 的 16 面板视频故事板。

一个年轻跑者在暴雨中的城市天台上追逐一束会逃跑的光，最后发现光来自未来的自己。
```

也可以指定风格：

```text
用 $storyboard-video-previs 做一个 12 面板黑白粗糙手绘分镜，风格接近早期动画预演，重点表现镜头运动和情绪递进。
```

## 文件结构

```text
storyboard-video-previs/
├── SKILL.md
├── agents/
│   └── openai.yaml
└── references/
    ├── storyboard-core.md
    ├── reference-image-system.md
    ├── scene-geography.md
    ├── continuity-system.md
    ├── annotation-system.md
    ├── storyboard-validator.md
    ├── procedural-montage.md
    ├── character-motion-dna.md
    ├── action-choreography.md
    ├── shot-language.md
    └── storyboard-formats.md
```

`SKILL.md` 是主工作流。

`references/storyboard-core.md` 提供生产目标解析、Mode Router、Cinematic Function、Director Plan、模式保护和输出校验规则。

`references/reference-image-system.md` 提供参考图角色、控制边界、优先级和冲突校验规则。

`references/scene-geography.md` 提供空间锚点、Camera Axis、运动路径和 Active Wide Reset 规则。

`references/continuity-system.md` 提供分级 Continuity State、Panel In/Out、状态传播和连续性校验规则。

`references/annotation-system.md` 提供 SIMPLE、PRO、CLEAN、LEGACY_BLUE 注释 Profile 及颜色语义校验规则。

`references/storyboard-validator.md` 提供统一的 BLOCKER、WARNING、NOTE、PASS 严重级别和 Validation Notes 输出格式。

`references/procedural-montage.md` 提供状态推进型过程的模式路由、六问判断器、道具/步骤因果、节奏曲线和嵌入 Pattern 规则。

`references/character-motion-dna.md` 提供角色动作语言、重心、节奏、攻防和禁止动作规则。

`references/action-choreography.md` 提供 Action 的 Cause → Effect、动作阶段、Combat Geography、升级和结尾校验规则。

`references/shot-language.md` 提供景别、角度、镜头运动、构图和注释系统。

`references/storyboard-formats.md` 提供面板数量、故事节奏和不同视频类型的结构模板。

## 设计原则

1. 先理解故事引擎，再设计面板
2. 每个面板都必须有新的动作或情绪功能
3. 镜头语言服务于节奏，不为了炫技而堆术语
4. 关键道具、角色方向、空间关系和光线方向要保持连续
5. 最后一格必须有清晰、压倒性、可记住的画面
