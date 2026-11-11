# 安装后的环境检查与排查

本技能是给 Codex 的制作指引。要实际创建、修改和渲染 PPTX，当前工作环境还需要有相应的演示文稿工具。先使用 [README 中的自检提示词](../README.md#安装)做一页虚构内容的测试，确认文件可创建、文字可编辑，并实际渲染页面。请使用可以访问所选素材和输出目录的运行环境。

## 没有 PPTX 工具时的一种本地选择

如果当前环境没有现成的制作能力，可以配置 Python 与 `python-pptx` 来制作常见的可编辑 PPTX，并安装 LibreOffice 用于渲染。它们独立于本项目，不会随技能自动安装；也可以使用当前环境中已有的其他合适工具。以下是供 Codex 调用的环境准备步骤，**不是每次发起 PPT 任务都要重新安装**。

1. 安装 [Python](https://www.python.org/downloads/) 和 [LibreOffice](https://www.libreoffice.org/download/download-libreoffice/)。安装后重新打开终端，确认 Python 可运行；LibreOffice 可通过系统应用列表或安装位置找到。
2. 在公开仓库、素材库和云同步目录之外选择一个本地工具目录，例如 `D:\PPT工具` 或 `~/ppt-tools`。下面示例会在该目录创建独立 Python 环境，请按实际路径修改。

Windows PowerShell：

```powershell
$pptToolsDir = 'D:\PPT工具' # 替换成可写的本地路径
New-Item -ItemType Directory -Force -Path $pptToolsDir | Out-Null
python -m venv (Join-Path $pptToolsDir 'venv')
$pptPython = Join-Path $pptToolsDir 'venv\Scripts\python.exe'
& $pptPython -m pip install python-pptx
& $pptPython -c "import pptx; print('python-pptx', pptx.__version__)"
```

macOS 或 Linux：

```bash
ppt_tools_dir="$HOME/ppt-tools" # 如需其他位置请修改
mkdir -p "$ppt_tools_dir"
python3 -m venv "$ppt_tools_dir/venv"
"$ppt_tools_dir/venv/bin/python" -m pip install python-pptx
"$ppt_tools_dir/venv/bin/python" -c "import pptx; print('python-pptx', pptx.__version__)"
```

需要 Python 的 `venv` 和 `pip` 可用且能访问软件包来源。部分系统需单独安装相关组件；若命令失败，根据报错和操作系统的 Python 文档处理。Windows 如使用 `py` 启动器，请按本机 Python 安装方式调整，`python` 未被识别时先修复 Python 安装或路径。

3. 在 Codex 中说明虚拟环境解释器路径和 LibreOffice 的实际安装位置，重新运行 README 的单页自检。例如 Windows 指定 `D:\PPT工具\venv\Scripts\python.exe`；macOS 或 Linux 指定 `~/ppt-tools/venv/bin/python`。如果 `soffice` 不在命令搜索路径中，请提供其实际可执行文件路径。

这条路线适合常见可编辑对象和基础渲染；原稿中的复杂母版、动画、备注、交互和目标软件兼容性应按实际任务与工具验证。需要保留复杂对象时，可选择有相应能力的其他工具；不要把生成或 PDF 渲染成功当作动画测试通过。

## 常见问题

- **克隆提示找不到 Git**：先安装 Git，并重新打开终端；用 `git --version` 检查。
- **安装目录已存在**：先检查它是不是已安装的技能及是否有本地修改。Git 克隆的副本可按 README 说明更新；不要直接覆盖。
- **新会话找不到技能**：确认技能文件位于用户目录下的 `.agents/skills/sykes-shi-ppt/SKILL.md`，重新打开 Codex 后再试。若之前按旧版说明装到了 `.codex/skills`，请参照 README 的新版安装位置；已有副本先检查并保留个人修改。
- **提示素材路径不存在或没有权限**：确认路径真实存在、运行环境可访问，并同时提供素材库根目录与索引文件的实际路径。远程环境不能默认访问你电脑上的本地路径。
- **能生成 PPTX 但无法渲染**：检查渲染程序是否安装、可执行文件路径是否正确，再运行单页自检。需要某个办公软件的实际兼容性或动画验证时，应在该软件中检查并说明结果。
- **担心素材外传**：本项目不附带后台导入或上传功能。选择外部服务前，先确认处理地点和资料授权；本地预览、索引和工作文件也应按原始材料的保密要求保存。
