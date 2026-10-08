# Malaysia Company Registry & Court Cases MCP

Connect your AI assistant to Malaysian company and court data through [The Company Atlas](https://thecompanyatlas.com) and the Model Context Protocol.

- About 1.7 million companies, limited liability partnerships and Labuan entities, searchable by name, SSM registration number, old registration number, entity type and registration year
- About 26,000 Malaysian court cases, from the Magistrates' Court to the Federal Court, with parties, roles, judges and case keywords
- Company parties in court cases are linked to their registration number, so a company profile lists the cases it was involved in

Coverage of the company register is thin: status and address are recorded for about 20,000 entities, mostly in Labuan. For the rest, expect the name, numbers, entity type and registration year.

The MCP connection is provided by The Company Atlas. This repository does not run a separate server.

## Connect

```text
https://thecompanyatlas.com/mcp/malaysia
```

Add this URL as a custom MCP connector in Claude, Cursor, or another MCP-compatible client. Sign in with Google when prompted.

- Transport: Streamable HTTP
- Auth: Google OAuth
- Docs: [thecompanyatlas.com/tools/malaysia-companies-court-cases](https://thecompanyatlas.com/tools/malaysia-companies-court-cases)

## Bahasa Melayu

MCP ini membolehkan pembantu AI mencari maklumat syarikat Malaysia dan kes mahkamah.

- Cari syarikat, PLT dan entiti Labuan mengikut nama, nombor pendaftaran SSM, nombor pendaftaran lama, jenis entiti dan tahun pendaftaran
- Lihat kes mahkamah yang melibatkan sesebuah syarikat, termasuk peranannya (Plaintif, Defendan, Perayu, Responden)
- Cari kes mahkamah mengikut nombor kes, mahkamah, hakim, kata kunci, pihak dan tahun keputusan

## Tools

### `Malaysia_search_companies`

Search Malaysian companies. Filters are combined with AND logic; include at least one.

| Parameter | Type | Description |
|---|---|---|
| `q` | string, optional | Registered name substring, 3–120 characters |
| `entityType` | enum, optional | `Company`, `Limited Liability Partnerships`, `Labuan Company`, `Labuan Limited Partnership`, `Labuan Limited Liability Partnership`, `Labuan Islamic Limited Liability Partnership`, `Foundation`, `Islamic Foundation`, `Redomiciled Foundation` |
| `active` | boolean, optional | Filter by active flag |
| `registeredFrom` / `registeredTo` | integer, optional | Registration year range |
| `page` / `limit` | integer, optional | Pagination, `limit` up to 50 (default 20) |

Returns `{ data, total, page, limit }`. `id` is `MY` + the 12-digit SSM number (or the Labuan number); `id2` is the old registration number.

### `Malaysia_get_company`

Profile by registration number: the 12-digit SSM number (e.g. `199001009294`), the old number (e.g. `200866-W`) or a Labuan number (e.g. `LL00002`).

| Parameter | Type | Description |
|---|---|---|
| `registrationNo` | string | SSM, old or Labuan registration number |

Adds address and former names where recorded, and `courtCases`: the number of cases naming the company as a party and the 20 most recent with its role.

### `Malaysia_list_cases`

Search Malaysian court cases, newest decision first.

| Parameter | Type | Description |
|---|---|---|
| `caseNo` | string, optional | Case number substring |
| `courtCategory` | enum, optional | `Majistret`, `Sesyen`, `Tinggi`, `Rayuan`, `Persekutuan` |
| `court` / `judge` / `keyword` | string, optional | Substring filters |
| `partiesContains` | string, optional | Party name substring |
| `companyId` | string, optional | Linked company id, e.g. `MY199001009294` |
| `year` | integer, optional | Year of decision |
| `page` / `limit` | integer, optional | Pagination, `limit` up to 100 (default 20) |

### `Malaysia_get_case`

One case by id with every party (name, role, linked company id) and the documents in the appeal record.

## Example prompts

- *"Court cases involving Tenaga Nasional Berhad, 199001009294."*
- *"Labuan companies registered since 2020."*
- *"Court of Appeal cases from 2025 where a bank is a party."*

## Setup — Claude

1. Go to [claude.ai/customize/connectors](https://claude.ai/customize/connectors)
2. Click **Add custom connector**
3. Name it **Malaysia Companies & Courts** and paste `https://thecompanyatlas.com/mcp/malaysia`
4. Save and sign in with Google when prompted
