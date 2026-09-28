# Meta Ads MCP Server

[![MCP](https://img.shields.io/badge/MCP-remote%20server-blue.svg)](https://modelcontextprotocol.io)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Setup guide](https://img.shields.io/badge/setup%20guide-markifact.com%2Fmeta-ads--mcp-1d6ef5.svg)](https://www.markifact.com/meta-ads-mcp)

> Meta Ads MCP server for Claude, ChatGPT, Codex, Cursor and any MCP client. 85 Meta Ads operations: 40 reads that run instantly and 45 writes that wait for your approval. Hosted, one OAuth login, nothing to install locally.

- **Setup guide:** [markifact.com/meta-ads-mcp](https://www.markifact.com/meta-ads-mcp)
- **Endpoint:** `https://api.markifact.com/mcp/meta-ads`

This is the Meta Ads endpoint of the [Markifact MCP server](https://github.com/markifact/markifact-mcp). It is the full Markifact server with Meta Ads as the entry point, so the same login also reaches every other platform Markifact connects, from Meta and Google Analytics to Sheets and Slack. If you only run Meta Ads, this is the one to install.

## What you can do

- **Reports.** Campaign, ad set and ad performance with placement, device, age, gender, country and hour breakdowns, attribution windows, calculated metrics such as ROAS and frequency, and objective results.
- **Changes.** Create and update campaigns, ad sets and ads, including Advantage+ setups. Single image, video, carousel, catalog and Instagram-post creatives with media upload. Budgets, bids, statuses, schedules, targeting, regional regulation fields.
- **Audiences and assets.** Custom, lookalike and saved audiences, lead forms, catalogs and product sets, media library, ad review feedback, Pages and Instagram accounts linked to the ad account.
- **Diagnostics.** Pixel and Conversions API setup checks, disapproval reasons, ad-level review feedback, and the Meta Ad Library for competitor research.

## Install

Sign up at [markifact.com](https://www.markifact.com?utm_source=github&utm_medium=repo&utm_campaign=meta-ads-mcp) first (free credits included), connect Meta Ads on the [Connections page](https://www.markifact.com/app/connections), then add the server to your client. The [Meta Ads MCP setup guide](https://www.markifact.com/meta-ads-mcp) has the same steps with screenshots.

| Client | Steps |
|---|---|
| **Claude (web and desktop)** | Settings → Connectors → Add custom connector → URL `https://api.markifact.com/mcp/meta-ads` → Connect, then sign in |
| **Claude Code** | `claude mcp add --transport http meta-ads https://api.markifact.com/mcp/meta-ads` or `/plugin install meta-ads@markifact` after `claude plugin marketplace add markifact/markifact-mcp` |
| **ChatGPT and Codex** | Install the [Markifact plugin](https://chatgpt.com/plugins/plugin_asdk_app_69e795096e888191b3908a4dd48a323d) from the Plugin Directory and call it with `@Markifact` |
| **Cursor** | Settings → MCP → Add server → URL `https://api.markifact.com/mcp/meta-ads` (or copy `mcp.json` from this repo into `~/.cursor/mcp.json`) |
| **Codex CLI** | Add to `~/.codex/config.toml`: `[mcp_servers.meta_ads]` then `url = "https://api.markifact.com/mcp/meta-ads"` |
| **Gemini CLI** | `gemini extensions install github.com/markifact/meta-ads-mcp` |
| **Windsurf** | Add the snippet from `mcp.json` to your MCP config |
| **Any MCP client** | Streamable HTTP at `https://api.markifact.com/mcp/meta-ads`, OAuth 2.1 with PKCE, dynamic client registration supported |

Details per client are in [`docs/clients.md`](docs/clients.md). Every client completes the same browser login; the server never asks for API keys or tokens in the chat.

## Prompts that work

**Weekly performance by ad set**

```text
Pull Meta Ads performance by ad set for the last 7 days versus the previous 7. Include spend, impressions, CPM, CTR, results, cost per result and ROAS. Flag anything with cost per result up more than 25%. Do not make changes.
```

**Creative fatigue**

```text
Which ads in the "Prospecting" campaign have frequency above 3 and CTR down more than 20% over the last 14 days? List them with spend and results.
```

**Placement breakdown**

```text
Break down last month's spend and results by placement for the whole account. Which placements have the worst cost per result?
```

**Launch a campaign, paused**

```text
Create a Meta Advantage+ sales campaign for our Black Friday creative. Budget $200 a day, target US, use the uploaded video, and leave it paused so I can review it before it goes live.
```

**Pause with a preview**

```text
List active ad sets with cost per result above $80 over the last 14 days, then pause them. Show me the list and wait for my approval before pausing anything.
```

Name the account, the date range and the metrics, ask for a read before a write, and say whether changes are allowed.

## How writes work

Every write is an approval-gated operation. The client asks for a change, the server prepares it and shows the exact target (account, campaign ID, current value, proposed value), and nothing runs until you confirm. Reads never ask. The server can only reach accounts the authorized Meta Ads login can already see.

## Operations (85)

Generated from the Markifact operation registry.

| Operation | What it does | Type |
|---|---|---|
| `meta_ads_check_catalog_diagnostics` | Checks a Meta catalog for data quality, policy, and feed issues, returning aggregated diagnostic issue groups with severity and affected… | Read |
| `meta_ads_create_ad_creative` | Creates a Meta ad creative from a raw creative payload and returns the new creative_id. | Write, approval required |
| `meta_ads_create_ad_from_creative` | Creates a new Meta ad from an existing ad creative ID. | Write, approval required |
| `meta_ads_create_ad_from_post` | Creates Meta ads from existing Facebook Page posts or Instagram media (no media upload, no copy authoring). | Write, approval required |
| `meta_ads_create_adset` | Create a new Meta Ads ad set with budget, targeting, optimization, and promoted object settings. | Write, approval required |
| `meta_ads_create_campaign` | Create a new campaign in Meta Ads with customizable settings including objective, budget, targeting, and scheduling. | Write, approval required |
| `meta_ads_create_carousel_ad` | Creates new Meta carousel ads with 2-10 cards containing images or videos (bulk mode only). | Write, approval required |
| `meta_ads_create_catalog_ad` | Creates Advantage+ Catalog / dynamic Ads (carousel or single image) via product sets (bulk mode). | Write, approval required |
| `meta_ads_create_experiment` | Create a native Meta A/B split test or ad study using existing campaign/ad-set variants, including instant forms. | Write, approval required |
| `meta_ads_create_flexible_ad` | Creates flexible ads with multiple media (up to 10) and text variations (up to 5 each). | Write, approval required |
| `meta_ads_create_product_set` | Low-level tool that creates a Meta product set inside a catalog from a raw Meta filter passed to Meta as-is. | Write, approval required |
| `meta_ads_create_single_image_ad` | Creates new Meta ads using single image creative assets. | Write, approval required |
| `meta_ads_create_single_video_ad` | Creates new Meta ads using single video creative assets. | Write, approval required |
| `meta_ads_delete_product_set` | Permanently deletes a Meta product set from a catalog. | Write, approval required |
| `meta_ads_duplicate_ad` | Create a copy of an existing Meta ad with optional creative enhancement controls and the ability to move the copy to a different ad set. | Write, approval required |
| `meta_ads_duplicate_adset` | Create a new Meta Ads ad set by duplicating an existing ad set with the option to copy associated ads or move to a different campaign. | Write, approval required |
| `meta_ads_duplicate_campaign` | Create a new Meta Ads campaign by duplicating an existing campaign with the option to copy associated ad sets and ads. | Write, approval required |
| `meta_ads_generate_ad_previews` | Generates preview iframe URLs of Meta advertising campaigns and ad creatives for a selected placement like INSTAGRAM_FEED, FACEBOOK_FEED,… | Read |
| `meta_ads_get_account_details` | Retrieves detailed account information from Meta Ads including status, timezone, currency, business info, spending limits, and settings. | Read |
| `meta_ads_get_account_history` | Retrieves historical account activity and changes for Meta advertising accounts | Read |
| `meta_ads_get_ad` | Fetches one Meta ad by ID with its status, review feedback (disapproval reasons per policy and placement), delivery issues,… | Read |
| `meta_ads_get_ad_creative` | Fetches a Meta ad creative by ad_id or creative_id and returns the raw creative payload. | Read |
| `meta_ads_get_adset_settings` | Retrieve ad set settings and configuration from Meta Ads including budget, targeting, optimization, bid strategy, and all ad set-level… | Read |
| `meta_ads_get_campaign_settings` | Retrieve campaign settings and configuration from Meta Ads including budget, status, objective, bid strategy, and all campaign-level… | Read |
| `meta_ads_get_experiment` | Read a native Meta A/B experiment or ad study, its test cells and available measurement results. | Read |
| `meta_ads_get_media_details` | Gets Meta media details for images and videos using image_hash or video_id. | Read |
| `meta_ads_get_product_details` | Gets full details for a single Meta catalog product by retailer id (SKU) or Meta product id, including review status and rejection reasons. | Read |
| `meta_ads_get_report` | Retrieves Meta Ads (Facebook/Instagram) data with customizable metrics, dimensions, filters, date ranges, sorting options, and built-in… | Read |
| `meta_ads_library` | Search and retrieve ads from Meta's Ad Library to spy on competitors | Read |
| `meta_ads_list_account_block_lists` | Get publisher block lists for Meta Ads account with filtering options for applied lists | Read |
| `meta_ads_list_ads` | Lists ads in a Meta ad account, optionally only the ads of one ad set or campaign, with any status including paused, plus creative… | Read |
| `meta_ads_list_adsets` | List all ad sets in ad account with status and dates | Read |
| `meta_ads_list_campaigns` | List all campaigns in ad account with status and dates | Read |
| `meta_ads_list_catalogs` | Lists Meta catalogs the connected user can access. | Read |
| `meta_ads_list_experiments` | List native Meta A/B tests, split tests and ad studies owned by a business portfolio. | Read |
| `meta_ads_list_instagram_accounts` | Lists the Instagram accounts linked to the Facebook Pages on the Meta Ads connection, with the Pages each one is linked to. | Read |
| `meta_ads_list_instagram_media` | Lists the posts of an Instagram account on the Meta Ads connection (Reels, feed posts, stories) with permalink, shortcode and media id,… | Read |
| `meta_ads_list_media` | Lists the Meta ad account's media library (videos and images) with optional name/filename search applied by Meta at the source. | Read |
| `meta_ads_list_pages` | Lists the Facebook Pages available on the Meta Ads connection, with their IDs. | Read |
| `meta_ads_list_product_sets` | Lists product sets inside a Meta catalog with their filters and product counts. | Read |
| `meta_ads_list_products` | Lists or searches products inside a Meta catalog with an optional raw Meta filter. | Read |
| `meta_ads_list_report_fields` | Search and browse available Meta Ads reporting metrics and dimensions before running reports. | Read |
| `meta_ads_rename_ad` | Renames Meta Ads ads to update their display names | Write, approval required |
| `meta_ads_rename_adset` | Renames Meta Ads ad sets to update their display names | Write, approval required |
| `meta_ads_rename_campaign` | Renames Meta Ads campaigns to update their display names | Write, approval required |
| `meta_ads_replace_ad_creative` | Attaches an existing creative (by creative_id) to an existing ad (by ad_id), replacing whatever creative was attached before. | Write, approval required |
| `meta_ads_search_targeting` | Search for Meta Ads targeting options: geo locations (countries, cities, regions, zips), locales (languages), interests, education… | Read |
| `meta_ads_select_accounts` | Select Meta Ads accounts once and reuse them across multiple meta_ads_get_report nodes,no repeated account selection. | Read |
| `meta_ads_update_ad` | Fallback Meta Ads ad patch tool for advanced or unsupported cases at the ad level, such as renaming an ad, changing… | Write, approval required |
| `meta_ads_update_ad_status` | Updates the status of Meta Ads ads (active, paused, or deleted) for ad management | Write, approval required |
| `meta_ads_update_adset` | Fallback Meta Ads ad set patch tool for advanced or unsupported cases. | Write, approval required |
| `meta_ads_update_adset_audiences` | Update audience targeting on a Meta Ads ad set: custom audiences, lookalike audiences, interests, behaviors, education, work, income,… | Write, approval required |
| `meta_ads_update_adset_bid_value` | Updates the bid amount (Bid Cap / Cost Cap) or minimum ROAS of Meta Ads ad sets. | Write, approval required |
| `meta_ads_update_adset_budget` | Updates the budget (daily or lifetime) of Meta Ads ad sets for budget management. | Write, approval required |
| `meta_ads_update_adset_language` | Updates language targeting (locales) for a Meta Ads ad set. | Write, approval required |
| `meta_ads_update_adset_locations` | Includes or excludes location targeting for a Meta Ads ad set. | Write, approval required |
| `meta_ads_update_adset_placements` | Updates the placement targeting of a Meta Ads ad set. | Write, approval required |
| `meta_ads_update_adset_schedule` | Updates the start_time and/or end_time (schedule) of a Meta Ads ad set. | Write, approval required |
| `meta_ads_update_adset_status` | Updates the status of Meta Ads ad sets (active, paused, or deleted) for ad set management | Write, approval required |
| `meta_ads_update_campaign` | Fallback Meta Ads campaign patch tool for advanced or unsupported cases such as spend cap, special ad category, promoted object or per ad… | Write, approval required |
| `meta_ads_update_campaign_bidding_strategy` | Updates the bidding strategy of a Meta Ads campaign. | Write, approval required |
| `meta_ads_update_campaign_budget` | Updates the budget (daily or lifetime) of Meta Ads campaigns for budget management. | Write, approval required |
| `meta_ads_update_campaign_status` | Updates the status of Meta Ads campaigns (active, paused, or deleted) for campaign management | Write, approval required |
| `meta_ads_update_experiment` | Update a native Meta A/B experiment or ad study using Meta's update parameters, such as name, schedule, cells or objectives. | Write, approval required |
| `meta_ads_update_product_set` | Updates a Meta product set's name, raw Meta filter, or both. | Write, approval required |
| `meta_ads_update_products` | Low-level bulk tool that creates, updates, or deletes catalog products by sending raw Meta items_batch requests as-is. | Write, approval required |
| `meta_ads_upload_media` | Uploads a single image or video to a Meta ad account and returns its media reference (image_hash for images, video_id & auto-generated… | Write, approval required |
| `meta_check_capi_health` | Check Conversions API (CAPI) health metrics for a Meta Ads pixel including event match quality, server side tracking, coverage,… | Read |
| `meta_create_custom_audience` | Create a Meta custom audience in an ad account. | Write, approval required |
| `meta_create_custom_conversion` | Creates a Meta custom conversion on an ad account, defined by a rule over pixel events or URLs (e.g. | Write, approval required |
| `meta_create_lead_form` | Creates a Meta Instant Form (lead gen form) on a Facebook page for Lead Ads campaigns, with contact fields, custom qualifying questions,… | Write, approval required |
| `meta_create_lookalike_audience` | Create a Meta lookalike audience from an existing source custom audience. | Write, approval required |
| `meta_get_custom_audience` | Get a single Meta custom or lookalike audience by ID with its full definition: the website or engagement rule, lookalike specification and… | Read |
| `meta_get_lead_form` | Gets the full configuration of a single Meta Instant Form (lead gen form): questions, intro card, thank-you page, privacy policy,… | Read |
| `meta_get_pixel_details` | Get details for a specific Meta Ads tracking pixel including configuration | Read |
| `meta_get_pixel_stats` | Get statistics for a Meta Ads pixel with various aggregation types and optional date filtering | Read |
| `meta_list_account_pixels` | Lists all pixels connected to a Meta Ads account with tracking and configuration details | Read |
| `meta_list_custom_audiences` | List custom audiences and lookalike audiences from a Meta Ads account. | Read |
| `meta_list_custom_conversions` | List all custom conversions configured in a Meta Ads account. | Read |
| `meta_list_lead_forms` | Lists all lead gen forms from a Facebook page | Read |
| `meta_list_promotable_apps` | Lists the apps a Meta Ads account can promote (id, name, platforms, store URLs, app events). | Read |
| `meta_list_saved_audiences` | List saved audiences (reusable targeting presets) from a Meta Ads account, with their full targeting specification, readable targeting… | Read |
| `meta_retrieve_leads` | Retrieves lead form submissions from Meta Ads (Facebook and Instagram) lead generation campaigns | Read |
| `meta_update_custom_audience` | Updates a Meta custom audience (rename, description, website rule, retention window, opt-out link, customer file source, lookalike spec,… | Write, approval required |
| `meta_update_lead_form_status` | Updates the status of a Meta Instant Form (lead gen form): ACTIVE, ARCHIVED, DELETED, or DRAFT. | Write, approval required |

The server exposes ten MCP tools. `find_operations` searches this list, `get_operation_inputs` returns the exact input schema, `run_operation` runs reads and `run_write_operation` runs approval-gated writes. The remaining tools handle connections, files and media uploads. Architecture notes are in the [main repo](https://github.com/markifact/markifact-mcp/blob/main/docs/architecture.md).

## Related

- [Meta Ads MCP setup guide](https://www.markifact.com/meta-ads-mcp)
- [Official Meta Ads MCP vs alternatives](https://www.markifact.com/blog/official-meta-ads-mcp-vs-alternatives)
- [Connect Meta Ads to Claude](https://www.markifact.com/blog/how-to-connect-meta-ads-to-claude)
- [Connect Meta Ads to ChatGPT](https://www.markifact.com/blog/how-to-connect-meta-ads-to-chatgpt)
- [Meta Ads MCP in Claude Code](https://www.markifact.com/blog/meta-ads-mcp-claude-code)
- [Meta Ads MCP in Codex](https://www.markifact.com/blog/meta-ads-mcp-codex)

## FAQ

**Do I need a Meta developer app or system user token?**

No. Markifact is an approved Meta tech provider and the server runs on Markifact's infrastructure. You authorize the Facebook login that already has access to your ad accounts, and the server can only reach what that login can see.

**Can it upload creatives?**

Yes. Upload images and video through the client, and the server attaches them to new or existing ads. Catalog and Instagram-post creatives are supported too.

**Can it change my campaigns?**

Yes, and every change is previewed and waits for your approval in the chat. Reads run immediately. Nothing is written without a confirmation.

**Which AI clients work?**

Any MCP client with OAuth support: Claude (web, desktop, Code), ChatGPT and Codex, Cursor, Windsurf, Gemini CLI and others. Install steps for each are above.

**What does it cost?**

The server is free to install. It needs a Markifact account, which starts with free credits. Plans are on the [pricing page](https://www.markifact.com/pricing).

## Support and policies

Email [contact@markifact.com](mailto:contact@markifact.com) or open an issue. See [SECURITY.md](SECURITY.md), [PRIVACY.md](PRIVACY.md) and [TERMS.md](TERMS.md). Licensed under MIT.
