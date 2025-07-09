# PandaRank 快速开始指南

## 1. 设置 OpenAI Session Token

1. 在浏览器中登录 https://chat.openai.com
2. 按F12打开开发者工具
3. 找到 Application/存储 → Cookies → chat.openai.com
4. 复制 `__Secure-next-auth.session-token` 的值
5. 创建 `.env` 文件：
```bash
cp .env.example .env
```
6. 编辑 `.env`，粘贴你的 token：
```
OPENAI_SESSION_TOKEN=你的token值
```

## 2. 启动服务

```bash
# 构建并启动所有服务
make up

# 查看日志
make logs
```

## 3. 访问监控面板

打开浏览器访问: http://localhost

你将看到：
- 📊 统计数据：总对话数、成功率、消息数等
- 🎯 手动触发：可以选择预设问题或输入自定义问题
- 📋 运行记录：查看所有历史记录和详情
- 📈 响应时间图表：监控性能趋势

## 4. 测试地理相关问题

系统已预设了60+个地理相关问题，包括：

**美食类：**
- 东京最好吃的拉面馆是什么？
- 香港最好的粤菜餐厅有哪些？

**制造业：**
- 广东做电子配件应该找什么工厂？
- 越南胡志明市有哪些服装代工厂？

**商业服务：**
- 在新加坡注册公司需要什么流程？
- 香港开设银行账户需要什么条件？

## 5. 手动触发测试

1. 在监控面板选择一个问题
2. 点击"立即询问"
3. 等待几秒后刷新页面查看结果
4. 点击"查看详情"查看完整对话和搜索记录

## 6. 查看数据

**通过API：**
```bash
# 获取统计信息
curl http://localhost:8000/stats | jq

# 获取最近运行
curl http://localhost:8000/runs | jq

# 导出NDJSON格式数据
curl http://localhost:8000/export/ndjson > export.ndjson
```

**通过数据库：**
```bash
# 进入数据库
make db-shell

# 查询示例
SELECT * FROM conversations ORDER BY started_at DESC LIMIT 5;
SELECT q.text, COUNT(c.id) as ask_count 
FROM questions q 
LEFT JOIN conversations c ON q.id = c.question_id 
GROUP BY q.id, q.text;
```

## 7. 调整配置

编辑 `.env` 文件：
- `SCRAPE_INTERVAL_SEC=600` - 自动运行间隔（秒）
- `HEADLESS=false` - 显示浏览器窗口（调试用）

## 8. 常见问题

**Q: 登录失败怎么办？**
A: 检查session token是否过期，重新获取

**Q: 如何添加新问题？**
A: 编辑 `data/geo_questions.yaml` 文件，重启服务

**Q: 如何查看截图？**
A: 截图保存在 `scraper/artifacts/` 目录

## 9. 停止服务

```bash
make down
```

## 10. 清理所有数据

```bash
make clean
```