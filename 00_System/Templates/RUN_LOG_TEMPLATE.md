# Run Log Template

```markdown
# Run Log: RUN-XXXX-XXXX

- **run_id**: RUN-XXXX-XXXX
- **agent**: [Agent 名称 / 角色类型]
- **input**: [输入的具体文件路径、数据集或参数配置]
- **action**: [执行的操作步骤与转换逻辑概要]
- **files_created**:
  - [相对路径或绝对路径]
- **files_modified**:
  - [修改的文件列表，无则填 None]
- **issues**: [涉及或记录在 ISSUES.md 中的问题编号，无则填 None]
- **result**: success | partial_success | failed
- **next_action**: [建议的后续步骤或交接给人类的动作]
```
