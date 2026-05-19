# 原始材料恢复

在起草报告之前，当实验材料或本地约束文件必须恢复时使用这些模式。但下面内容主要基于 Windows 环境，若在 Linux 等环境，请理解指令内涵，编写正确读取指令，而不是死板照抄。

## 规则

- 在起草报告之前解决可读性问题。
- 将乱码提取视为不可用于内容决策的证据。
- 当被阻塞的本地工具是准确恢复原始材料所必需时，请求升级而非继续使用低质量替代方案。
- 在决定最终报告中可以声称什么之前，尽可能恢复原始文本、表格内容和图形标签。

## 经验证的本地命令模板

- 首选本地 Python 调用模板：
  `py -3.10 -c "<python_code>"`

- UTF-8 文本读取模板：
  `py -3.10 -c "import pathlib; print(pathlib.Path(<path>).read_text(encoding='utf-8'))"`

- 已安装 PDF 库检查模板：
  `py -3.10 -c "import importlib.util; mods=<module_list>; print({m: bool(importlib.util.find_spec(m)) for m in mods})"`

- `pypdf` 提取模板：
  `py -3.10 -c "from pathlib import Path; from pypdf import PdfReader; p=Path(<pdf_path>); r=PdfReader(str(p)); out=[]; [out.append(f'===== PAGE {i} =====\\n'+(page.extract_text() or '')) for i,page in enumerate(r.pages,1)]; Path(<output_path>).write_text('\\n\\n'.join(out), encoding='utf-8'); print(<done_message>)"`

- PowerShell UTF-8 文本显示模板：
  `[Console]::OutputEncoding=[System.Text.Encoding]::UTF8; Get-Content -Encoding UTF8 <path>`
