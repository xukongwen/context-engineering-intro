# 推送到GitHub

将当前修改推送到GitHub，由AI生成commit信息。

## 执行步骤

1. **检查Git状态**
   - 运行 `git status` 查看当前修改
   - 运行 `git diff` 查看具体更改内容
   - 运行 `git log --oneline -5` 查看最近的提交历史

2. **分析修改内容**
   - 分析所有修改的文件和更改内容
   - 理解更改的目的和影响
   - 检查是否有敏感信息不应提交

3. **生成Commit信息**
   - 根据代码更改内容生成简洁明了的commit信息
   - 使用中文或英文（根据项目历史习惯）
   - 遵循项目的commit信息格式约定
   - 确保信息准确反映更改的本质

4. **添加并提交更改**
   - 使用 `git add .` 添加所有修改的文件
   - 使用生成的commit信息创建提交
   - 在commit信息末尾添加标准的Claude生成标识：
     ```
     🤖 Generated with [Claude Code](https://claude.ai/code)
     
     Co-Authored-By: Claude <noreply@anthropic.com>
     ```

5. **推送到GitHub**
   - 使用 `git push origin main` 推送到主分支
   - 如果当前分支不是main，推送到当前分支
   - 处理任何推送错误或冲突

6. **确认推送成功**
   - 运行 `git status` 确认推送完成
   - 报告推送结果

## 注意事项

- 在推送前确认所有修改都是预期的
- 不要推送包含敏感信息（密钥、密码等）的文件
- 如果有未跟踪的重要文件，询问用户是否需要添加
- 如果推送失败，提供解决方案或建议

## 安全检查

- 扫描代码中的敏感信息
- 检查.gitignore文件以避免推送不必要的文件
- 确认没有临时文件或构建产物被意外包含