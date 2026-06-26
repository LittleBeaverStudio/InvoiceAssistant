# 小河狸发票助手 Skill

License: MIT

这个仓库用于 OpenClaw / WorkBuddy / Codex 类智能体读取本机“小河狸发票助手”的发票台账数据。

使用前请先打开“小河狸发票助手”。Skill 默认会自动扫描 `127.0.0.1:8876-8895`，识别实际运行端口，不需要每台电脑固定填写地址。也可以通过 `INVOICE_ASSISTANT_BASE_URL` 手动指定地址。

示例：

```bash
python scripts/invoice_assistant_client.py companies
python scripts/invoice_assistant_client.py summary --company-id 1 --start 2026-01-01 --end 2026-06-30
python scripts/invoice_assistant_client.py invoices --company-id 1 --keyword 作废
python scripts/invoice_assistant_client.py rankings --company-id 1 --limit 10
```

当前版本只读提取数据，不支持通过智能体导入、修改或删除发票数据。
