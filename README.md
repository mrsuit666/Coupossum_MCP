<p align="center"><img src="logo.png" alt="Coupossum" width="120" height="120"></p>
<p align="center"><a href="https://lobehub.com/mcp/mrsuit666-coupossum_mcp"><img src="https://lobehub.com/badge/mcp/mrsuit666-coupossum_mcp" alt="MCP Badge"></a></p>

# Coupossum MCP server

Search current coupon codes, deals and products on sale from thousands of online stores, right from your AI assistant. Built on Feedico data. It is not an official Feedico server.

Coupossum helps you find a working discount before you buy. Ask for a store, a product or a kind of deal, and Coupossum returns the current coupon codes and offers, with the code, the end date, the countries it's valid in and a link to the store. It also finds products currently on sale, with the sale price, the regular price and the countries they ship to.

Free, no account, no API key. All tools are read-only.

| | |
|---|---|
| Endpoint | `https://coupossum.com/mcp` |
| Transport | Streamable HTTP (remote) |
| Authentication | None |
| Tools | 4, all read-only |
| Website | [coupossum.com](https://coupossum.com) |
| Setup guide | [coupossum.com/ai](https://coupossum.com/ai) |
| Support | support@coupossum.com |

This repository describes the hosted server; it doesn't contain its source code. There's nothing to install or run.

## Add it to your assistant

- **Claude** (claude.ai or Claude Desktop): Coupossum is listed in [Claude's connector directory](https://claude.ai/directory/coupossum); add it from there. Or by hand: Settings → Connectors → Add custom connector. Name `Coupossum`, URL `https://coupossum.com/mcp`. No OAuth settings are needed.
- **ChatGPT:** turn on developer mode under Settings → Apps & Connectors → Advanced, then create a connector with the URL `https://coupossum.com/mcp` and no authentication.
- **Claude Code:**

  ```bash
  claude mcp add --transport http coupossum https://coupossum.com/mcp
  ```

- **Cursor, VS Code and other MCP clients:** add a remote HTTP server with the URL `https://coupossum.com/mcp`, for example:

  ```json
  {
    "mcpServers": {
      "coupossum": { "url": "https://coupossum.com/mcp" }
    }
  }
  ```

## Tools

| Tool | What it does | Inputs |
|---|---|---|
| `search_coupons` | Finds current coupon codes and deals by keyword, store and country | `query`, `store`, `country` (all optional) |
| `search_stores` | Finds stores by name or domain, with their number of current offers | `query` |
| `get_store_offers` | Lists one store's current codes and deals, soonest ending first | `store`, `country` (optional) |
| `search_products` | Finds products on sale by keyword, maximum price, currency and country | `query`, `max_price`, `currency`, `country` (optional) |

Each call returns at most 10 results. When there are more, the answer says so and links to all of them on coupossum.com.

## Prompts

Ready-made questions you can pick in apps that show MCP prompts:

| Prompt | What it asks | Inputs |
|---|---|---|
| `store_coupons` | "Are there any coupon codes or deals for <store> right now?" | `store`, `country` (optional) |
| `products_on_sale` | "Find <product> on sale under <price>." | `product`, `max_price`, `country` (optional) |

## Example prompts

- "Are there any coupon codes for Nike right now?"
- "Any discounts on shoes? I'm in Germany."
- "Which free shipping deals end this week?"
- "Shoes on sale under 100 USD."
- "Does Coupossum have Adidas?"

## Where the offers come from

Offers come from the stores' affiliate programs, which share their codes and deals with partners like Coupossum. That's why the links are affiliate links: if you buy through one, Coupossum may earn a commission, at no extra cost to you.

- Coupons are refreshed every 2 hours, stores and products daily.
- Expired codes are removed, offers whose links fail a daily check are hidden, and duplicates are shown once.
- The server answers from Coupossum's own database and makes no external calls while answering.

Your assistant shows you the links, and you open them yourself. The server tells assistants never to open, fetch or prefetch them.

## Privacy and limits

- No accounts, cookies or tracking. For statistics, each call logs the tool name, its search terms and the number of results, without IP addresses.
- 30 requests per minute per IP address.
- Full policy: [coupossum.com/privacy](https://coupossum.com/privacy).

## For directory and automated testing

Please don't open the store links in automated tests: automated clicks count as invalid traffic for the stores' affiliate programs. Listing the tools works without any link being opened:

```bash
curl -s https://coupossum.com/mcp \
  -H "content-type: application/json" \
  -H "accept: application/json, text/event-stream" \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/list"}'
```

## Contact

Questions or problems: support@coupossum.com
