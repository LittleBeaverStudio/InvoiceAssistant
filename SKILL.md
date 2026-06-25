---
name: littlebeaver-invoice-assistant
description: Read and analyze local Little Beaver Invoice Assistant data. Use when a user asks an agent to connect to 小河狸发票助手, query invoice ledgers, invoice items, companies, customer/supplier/product rankings, monthly invoice trends, or local tax invoice summaries through the app's localhost Skill API.
---

# 小河狸发票助手 Skill

Use this skill to read invoice data from the user's local 小河狸发票助手 desktop app. The app must be running on the same computer as the agent. Data is read through the local API only and is not uploaded by this skill.

## Connection

1. Prefer the helper script at `scripts/invoice_assistant_client.py`.
2. If the user provides an address, pass it as `--base-url http://127.0.0.1:<port>`.
3. If no address is provided, auto-discover the running app by scanning `http://127.0.0.1:8876` through `http://127.0.0.1:8895` and checking `/api/metadata`.
4. If discovery fails, ask the user to open 小河狸发票助手 first, then retry.

Optional environment variables:

- `INVOICE_ASSISTANT_BASE_URL`: fixed app address, such as `http://127.0.0.1:8876`.
- `INVOICE_ASSISTANT_PORTS`: comma-separated ports or ranges, such as `8876-8895,9000`.

## Common Reads

Use these commands from the skill folder:

```bash
python scripts/invoice_assistant_client.py info
python scripts/invoice_assistant_client.py companies
python scripts/invoice_assistant_client.py summary --company-id 1 --start 2026-01-01 --end 2026-06-30
python scripts/invoice_assistant_client.py invoices --company-id 1 --keyword 作废 --page-size 100
python scripts/invoice_assistant_client.py items --company-id 1 --keyword 咨询服务
python scripts/invoice_assistant_client.py rankings --company-id 1 --limit 10
```

For natural-language analysis, first fetch the smallest useful dataset:

- Company selection: run `companies`.
- Overall amounts and trends: run `summary`.
- Customer, supplier, and product ranking: run `rankings`.
- Detailed invoice checks: run `invoices` with `keyword`, `start`, `end`, `direction`, and pagination.
- Product/service analysis: run `items`.

## API Semantics

The local Skill API is read-only:

- `/api/skill/health`
- `/api/skill/info`
- `/api/skill/companies`
- `/api/skill/summary`
- `/api/skill/invoices`
- `/api/skill/items`
- `/api/skill/rankings`

Supported filters vary by endpoint:

- `company_id`: may be repeated.
- `start` and `end`: `yyyy-mm-dd`.
- `direction`: `开具` or `取得`.
- `keyword`: fuzzy search for invoice and item detail endpoints.
- `page` and `page_size`: detail pagination.
- `limit`: ranking size.

The summary and ranking endpoints exclude invoices whose status contains `作废`. Detail endpoints keep all statuses so the agent can inspect voided or abnormal invoices when asked.

## Analysis Guidance

When answering finance questions, state the data scope: selected company, date range, and whether voided invoices were excluded. Treat tax differences as invoice-ledger calculations, not final tax payable or filing results.

For questions such as "哪些客户开票下降明显", use the summary/ranking data first. If a precise decline calculation is needed, pull invoice details by month and compare recent periods.

