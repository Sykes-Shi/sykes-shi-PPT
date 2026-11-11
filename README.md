# sykes-shi-PPT

一个面向 Codex 的个人风格 PPT 制作技能，帮助制作、修改和检查汇报演示文稿。它把参考稿选择、页面表达、可编辑对象制作和成品检查串成一套按需执行的流程。默认交付可编辑的 PPTX；具体风格和素材由使用者提供。

许可：[CC BY-NC 4.0](LICENSE) · 署名、非商业性使用

## 能做什么

- 按指定参考稿制作新演示，或在现有 PPTX 上局部修改，延续其版式、字体和页面元素。
- 根据沟通目的组织标题、结论、数据、图表和图示；需要精确修改的文字、表格和图表优先保留为原生对象。
- 在适合的场景中使用生成图片或混合页面；若接受整页图片，也可采用图片式呈现。
- 对页面渲染结果进行逐页检查，并按实际需要核对动画、备注、隐藏页和交互。
- 根据任务选择其他工具辅助处理，不要求每份演示采用同一种制作路径。

## 安装

需要已安装并支持技能的 Codex 环境，以及 [Git](https://git-scm.com/downloads)。以下按 [Codex 官方技能目录说明](https://learn.chatgpt.com/docs/build-skills#where-codex-loads-local-skills)安装到当前用户的 `~/.agents/skills`，供个人在不同项目中使用。

```powershell
$skillsDir = Join-Path $env:USERPROFILE '.agents\skills'
New-Item -ItemType Directory -Force -Path $skillsDir | Out-Null
git clone https://github.com/Sykes-Shi/sykes-shi-PPT.git (Join-Path $skillsDir 'sykes-shi-ppt')
Test-Path -LiteralPath (Join-Path $skillsDir 'sykes-shi-ppt\SKILL.md')
```

macOS 或 Linux：

```bash
skills_dir="$HOME/.agents/skills"
mkdir -p "$skills_dir"
git clone https://github.com/Sykes-Shi/sykes-shi-PPT.git "$skills_dir/sykes-shi-ppt"
test -f "$skills_dir/sykes-shi-ppt/SKILL.md" && echo 'SKILL.md found'
```

安装后检查命令应分别显示 `True` 或 `SKILL.md found`。打开一个新的 Codex 会话，输入 `$sykes-shi-ppt` 调用；若当前会话找不到技能，重新打开 Codex 并核对上面的技能目录和 `SKILL.md` 是否位于 `sykes-shi-ppt` 的**直接下一级**。如果目标文件夹已存在，先检查是否已有安装和未保存的修改，避免覆盖。

此前通过 Git 克隆安装过的用户，可在确认安装目录没有需要保留的未提交修改后，运行 `git -C "技能安装目录的实际路径" pull --ff-only` 更新，再开启新会话。手动下载或其他方式安装的副本，应先备份个人修改，再按实际安装方式更新。早期版本说明使用过 `.codex/skills`；如果在那里安装后找不到技能，可将完整项目重新克隆到上面的 `.agents/skills` 目录。请先检查已有副本，避免不必要的重复安装或覆盖修改。

**技能文件是制作指引，不包含 PPTX 制作和渲染程序。**实际能力取决于 Codex 当前的运行环境。请使用能访问本地文件的工作环境；若使用云端环境或外部服务，先确认它能访问哪些文件，以及是否获准发送素材。

安装后建议先在 Codex 中复制以下自检任务，使用独立的本地输出目录：

```text
$sykes-shi-ppt 请检查当前环境是否能访问本地文件、创建可编辑 PPTX 并渲染页面。
请用虚构内容在【云同步目录之外的测试输出路径】制作一页 PPTX，
验证文字能否编辑并尝试渲染成图片或 PDF；分别报告实际完成的检查和文件路径。
如果缺少工具，请列出缺失项和可选安装方案，不要上传我的素材。
```

没有现成工具时，可让 Codex 在云同步目录外的独立环境配置 [python-pptx](https://python-pptx.readthedocs.io/) 以创建或修改常见 PPTX，并使用 [LibreOffice](https://www.libreoffice.org/download/download-libreoffice/) 等工具渲染。具体安装、自检和常见问题见 [环境准备与排查](docs/setup-and-troubleshooting.md)。复杂动画、交互或原稿母版的保真能力需按实际工具与任务验证。

## 从零开始

1. **准备素材**：选几份有权使用的 PPTX 模板、历史优秀页面或图片，放在仓库和云同步工作目录之外的个人素材库。只有 PDF 或截图时也可作为视觉参考。
2. **整理索引**：可让 Codex 根据你指定的素材，为每份文件记录用途、适合的页面类型和页码，并生成本地预览；源文件保留不变。参见 [素材库选择、导入与管理](docs/asset-library.md)。
3. **发起任务**：在新的 Codex 会话中调用 `$sykes-shi-ppt`，提供本次事实材料、素材库或指定参考稿，以及用途、页数和交付要求。参见 [可复制的任务提示词](docs/prompt-examples.md)。
4. **检查成品**：默认要求文字、图表等关键内容可编辑，逐页渲染检查；涉及动画、交互或指定软件时按实际要求测试。

素材库可以随时补充，也可以每次只提供一份参考文件。本项目没有自动导入器或后台扫描功能：使用个人素材时，需要在任务中提供文件、目录或索引位置，让 Codex 按需读取。

## 使用

在 Codex 中明确调用 `$sykes-shi-ppt`，并说明资料、用途、目标受众、参考稿和交付形式。例如：

```text
$sykes-shi-ppt 根据我提供的项目材料，制作一套面向管理层的进展汇报。
请沿用我提供的 PPT 的视觉风格，图表和文字保持可编辑。
```

```text
$sykes-shi-ppt 在这份 PPTX 的副本中修改第 3 页，更新图表数据和说明，保留其余页面、备注和现有动画。
```

```text
$sykes-shi-ppt 参考我提供的模板制作 5 页方案汇报，可以用生成图片增强视觉效果，关键数据和文字请保持可编辑。
```

只需给出与任务有关的信息；已有参考稿、交付要求或约定可以直接复用。具体执行逻辑见 [技能入口](SKILL.md) 和 [工作流程](references/workflow.md)。

提示词可根据任务增删：明确区分**事实材料**（提供本次内容）与**视觉参考**（提供模板或版式），指定唯一模板时直接说明。局部修改现稿时，指出需要改动的页面和须保留的动画、备注或交互；只需审阅时注明不修改文件。

需要了解页面选择、同风格制作、讲稿逻辑和成品检查时，可查看 [页面设计与制作细节](docs/design-and-production.md)。

## 工作流程

1. 明确用途和交付要求：判断是已有稿件编辑、参考稿风格制作、新视觉探索还是只读审阅。
2. 选择参考材料：以本次指定的参考稿为先；没有指定时，使用者可以提供自己的本地模板和素材。
3. 制作页面：保持选定视觉体系的一致性，按页面目的选择合适的文字、图表、原生对象和图片。
4. 按需协作：局部编辑、原生数据图表或图片页等不同任务使用适合的能力，辅助工具不接管整个流程。
5. 核对交付：渲染并检查最终页面；需要动画、交互或特定办公软件兼容性时，针对实际要求另做检查。

## 本地素材库

模板、参考演示、预览图、标识、图片及其他素材体积较大，本仓库不上传素材库。使用者可在本地自行保存有权使用的 PPTX 模板和资源，在提问时提供文件或路径；已有稿件直接作为参考也可以。请按资料的授权和保密要求使用，不要把内部素材自动上传到外部服务。

素材缺失时，技能仍可组织内容并基于当前可用资源制作，但要复现特定版式、标识或素材，需要提供对应参考文件。本仓库不包含这些资源，也不提供其下载地址。

一般按以下顺序选择：本次指定的唯一参考 → 要修改的现稿 → 用户指定素材库中适合任务的参考 → 在已有要求下设计通用版式。事实材料并不自动成为视觉模板；素材库中的历史数字和结论也不自动成为本次事实。完整示例、目录结构和本地索引见 [素材库指南](docs/asset-library.md)。

## 来源与整合方式

本项目是围绕个人使用习惯整理的技能，不是下面项目的官方发行版或直接 fork。感谢以下项目：

| 项目 | 本技能如何使用 |
| --- | --- |
| [PPT Master](https://github.com/hugohe3/ppt-master) | 按需借助其演示文稿工具处理原生对象编辑、图片重建和文件检查等任务；安装与调用独立进行。集成时参考了 [v6.7.3](https://github.com/hugohe3/ppt-master/releases/tag/v6.7.3)，具体功能与命令以实际安装版本为准。 |
| [codex-ppt-skill](https://github.com/ningzimu/codex-ppt-skill) | 借鉴风格锚点、代表页、素材角色和图片式演示的组织方法；用户需要其完整图片式工作流时，可以单独安装和调用。本技能默认仍按交付要求选择可编辑或图片式形式。参考快照为 [`941dcaf`](https://github.com/ningzimu/codex-ppt-skill/tree/941dcafe2f23ea4ae61de99e9b22fb0ef3d9e06c)。 |

这两个项目均采用 MIT 许可，各自的代码、版权声明及许可证以其上游仓库为准。本仓库不内置上述上游项目；使用上游代码时请遵循对应许可。

## 可选依赖

- **PPTX 制作与渲染能力**：使用当前环境中可用的演示文稿工具，具体依赖取决于目标文件与制作方式。
- **PPT Master**：需要其特定原生编辑、图片重建或文件检查能力时单独安装；普通任务无需强制安装。
- **codex-ppt-skill**：需要其完整图片式演示流程时单独安装；需按该项目说明配置图像生成能力。
- **[Lieflat Charts](https://github.com/larashero3-dotcom/lieflat-charts)**：可选的图表设计参考，按其许可单独使用；该项目采用 PolyForm Noncommercial License 1.0.0，商业使用需要另行取得许可。也可使用其他合适的图表方案。

可选工具不会因为列在这里而自动安装或启用。安装和使用前请查看对应项目的依赖、许可与当前文档。

## 许可与素材权利

Copyright © 2026 Sykes-Shi

除另有说明的第三方内容外，本仓库中由 Sykes-Shi 有权授权的技能说明和文档采用 [Creative Commons 署名—非商业性使用 4.0 国际许可协议（CC BY-NC 4.0）](https://creativecommons.org/licenses/by-nc/4.0/deed.zh-hans)。完整协议见 [LICENSE](LICENSE)。

- 可以在非商业用途下分享和改编获授权内容。分享时须署名、提供许可链接、注明是否修改，并保留适用的版权和许可声明。
- 需要商业使用时，请先通过本仓库的 [GitHub Issues](https://github.com/Sykes-Shi/sykes-shi-PPT/issues) 联系作者，另行取得授权。非商业用途的判断以许可协议及具体使用情况为准。
- 本许可适用于上述仓库内容；未上传的素材库不在授权范围内。第三方项目、字体、商标和参考素材分别遵循其权利人规定及对应许可。

本项目允许公开获取和非商业使用；由于限制商业用途，它不属于开放源代码促进会（OSI）定义的开源许可证项目。
