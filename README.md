> **许可变更**：自本次更新起，本项目许可由 MIT 变更为**「小河狸工作室非转售许可 1.0」**——
> 个人使用与**企业内部使用免费**（含内部办公、财务与会计处理、内部系统集成）；
> 对外销售、集成进收费产品或服务、托管 / SaaS / 代运营、以及**向第三方交付成果或提供服务，须事先取得书面授权**。
> 许可全文见 [LICENSE](./LICENSE)。商业授权：https://littlebeaver.top

![License](https://img.shields.io/github/license/LittleBeaverStudio/KingdeeDataAnalyzer?label=license)
# 小河狸发票助手 Skill


这个仓库用于 OpenClaw / WorkBuddy / Codex 类智能体读取本机“小河狸发票助手”的发票台账数据。

使用前请先打开“小河狸发票助手”。Skill 默认会自动扫描 `127.0.0.1:8876-8895`，识别实际运行端口，不需要每台电脑固定填写地址。也可以通过 `INVOICE_ASSISTANT_BASE_URL` 手动指定地址。

示例：

```bash
python scripts/invoice_assistant_client.py companies
python scripts/invoice_assistant_client.py summary --company-id 1 --start 2026-01-01 --end 2026-06-30
python scripts/invoice_assistant_client.py invoices --company-id 1 --keyword 作废
python scripts/invoice_assistant_client.py attachments --company-id 1
python scripts/invoice_assistant_client.py open-attachment --attachment-id 123
python scripts/invoice_assistant_client.py rankings --company-id 1 --limit 10
```

当前版本主要用于只读提取数据，不支持通过智能体导入、修改或删除发票数据。`open-attachment` 只会在本机调用系统默认程序打开已归档的 PDF/OFD/XML 文件，不会把文件内容上传到智能体或云端。
