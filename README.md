# writing-style
Think in any logic, write in my style.

## How to use

### 方式一：直接使用预设规则

```bash
mkdir -p ~/.claude/rules && curl -o ~/.claude/rules/artical-writing-style.md https://raw.githubusercontent.com/sepinetam/writing-style/main/.claude/rules/artical-writing-style.md
```

### 方式二：从已有文章中提取风格（推荐）

1. 克隆项目
```bash
git clone git@github.com:sepinetam/writing-style.git
cd writing-style
```

2. 在 Claude Code 中打开项目
```bash
claude
```

3. 把你的参考论文放到 `examples/` 目录下（支持 PDF、DOCX、TXT、MD）

4. 运行 skill
```
/learn-writing-style 相关文件在examples
```

5. 按提示回答风格偏好问题，最终生成 `.claude/rules/artical-writing-style.md`

6. (Optional) 全局应用
```bash
mkdir -p ~/.claude/rules && cp .claude/rules/artical-writing-style.md ~/.claude/rules/artical-writing-style.md
```

### 预设规则的适用范围

- **适用**：经济学实证研究论文、学术报告、研究提案
- **不适用**：非学术写作（邮件、博客）、纯数学证明、教学讲义

### 规则核心特征

- 句长25-30词，段落70-90词
- 被动语态适度（5-7/千词），方法部分被动、结果部分主动
- 第一人称策略性使用（引言避免，方法允许，结果避免，讨论允许）
- 引用密度中等（4-6/千字），引用后必须给出自己的解释
- 数据结果精确报告，每个统计量后紧跟解释
