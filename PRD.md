# Semba Fit-Out CRM, Product Requirements Document

Version 1.0, 29 September 2026
Owner Sritesh Naidu, AI Lead, Lemon Sky Edge
Status Concept demo shipped (artifact version 3). This PRD hands the project to Claude Code for the next build stage.

## 1. What this is

A lightweight CRM for the sales side of SEMBA Malaysia Design & Construction Sdn Bhd, a Japanese founded spatial design and fit-out contractor in Kuala Lumpur. It tracks every enquiry from first contact to award, then follows the awarded job through handover to the project team so sales stays on the account until opening day.

It exists for two reasons.

1. To show Semba that Lemon Sky can build tools that understand their business, not generic software.
2. To convince Semba that learning to build with AI from Lemon Sky AI Academy is worth their HRD Corp levy, by showing the kind of tool a trained team can start on themselves and where a connected version needs Lemon Sky's solutions service.

It is a demo built on assumptions. Lemon Sky has almost no information on Semba's current systems. Nothing in it is connected to Semba data. The on screen notices were removed by Sritesh on 4 October 2026, so the presenter must say it.

## 2. Who uses it

Semba's project team of 11 people includes 3 project managers, 1 business manager, 2 quantity surveyors, 2 admin, 2 site coordinators and the managing director. Semba listed no separate sales team. So the CRM's users are the business manager (primary), the managing director (dashboard and reports), the project managers (handover section), and admin (data entry). Design for people with little AI knowledge and no CRM habit.

## 3. The business, in the terms the tool uses

Semba wins fit-out work two ways, and each converts differently.

1. Direct quote. A brand asks for a price, often a repeat client or a Japan HQ referral.
2. Tender. Mall leasing or a consultant invites several contractors. Price sensitive, low conversion.

Design and build was a third route until 4 October 2026. Sritesh removed it because it is a type of work, not a way of winning it. A design and build job comes in as a direct quote or as a tender. Do not add it back as a route. If the type of work is ever needed, it is a separate field.

The client's opening date is the hard deadline, not Semba's proposal date. Between award and construction sit mall management approval and, where fire systems are touched, Bomba approval, typically 2 to 4 weeks together. A slow decision eats that window.

Sources used for the industry model are listed in section 12.

## 4. Scope of the shipped demo (version 3)

Single HTML file, no build step, no backend, no library. All state lives in memory and resets on reload.

### 4.1 Views

1. Dashboard. Five KPI tiles (open pipeline, weighted forecast, overdue follow-ups, quotes gone quiet, awarded this year). Open pipeline by stage bars. Win rate by route (two tiles). Needs attention list, ranked by a score that weights overdue days, quote silence and opening date pressure by deal value. Openings in the next 90 days. Awarded jobs in handover with a four step strip.
2. Pipeline. Kanban with six columns (Enquiry, Site survey, Concept & budget, Proposal sent, Negotiation, Awarded). Drag a card to move it. Every move is logged to the deal history. Card colour stripe shows route (taupe direct quote, navy tender).
3. Deals. Sortable, filterable table of every deal including lost ones. Search on brand, mall, contact, group. Filters on stage, route, sector, owner.
4. Accounts. Removed by Sritesh on 4 October 2026. It grouped deals by client company, but it showed nothing the Pipeline and Deals tabs do not, and searching a group name on the Deals tab gives the same list. The repeat client point is made on the Reports tab (source of enquiries). Do not add it back without him.
5. Follow-ups. Overdue, due this week, and quotes out 14 or more days with no reply.
6. Reports. Source of enquiries, why deals were lost, sector mix of open pipeline, average days from enquiry to decision by route.

### 4.2 Deal drawer

Opens from any deal reference. Shows route and sector, stage progress bar, probability and value, handover strip if awarded, next action with due status, key fields (location, unit size and RM per sq ft, opening date, source, owner, designer, proposal sent date, approvals needed), notes, the assistant panel, a note logger, a stage mover, a handover advancer, and the full history newest first.

### 4.3 Assistant panel

Three buttons per deal. Draft a follow-up, Brief me before the call, What could go wrong. In the Claude artifact it calls the artifact `sample` capability (`window.claude.use("sample")`) with a prompt built from the deal record. Outside the artifact, or if the viewer declines, it falls back to a built-in template generator so the demo never dies on stage. Output is labelled so the reader knows which path produced it.

### 4.4 Other behaviours

The New enquiry button and the Today's follow-ups shortcut were removed by Sritesh on 4 October 2026. The demo shows how enquiries are gathered and presented, adding one is not part of it. Do not add them back without him. Toast confirms every write. Escape closes the drawer. Nav badge shows overdue follow-up count.

## 5. Data model

Every deal is one object. Field list as implemented.

| Field | Type | Notes |
|---|---|---|
| id | number | unique |
| brand | string | outlet brand name |
| group | string | parent company, shown on the Deals list and the deal page, and searchable |
| contact | string | name and role |
| sector | enum | Retail, F&B, Office, Hospitality |
| location | string | mall or building, unit or floor |
| sqft | number | unit size |
| route | enum | Direct quote, Tender |
| source | string | Japan HQ referral, Repeat client, Mall management referral, Website enquiry, Referral, Consultant referral |
| value | number | estimated contract value in RM |
| stage | enum | Enquiry, Site survey, Concept & budget, Proposal sent, Negotiation, Awarded, Lost |
| owner | string | sales owner |
| designer | string or null | assigned designer |
| pm | string | project manager, awarded deals only |
| hand | 0 to 3 | handover step, awarded only. 0 Design, 1 Mall & Bomba submission, 2 Construction, 3 Handover |
| next | string | next action text |
| nextDate | ISO date or null | when the next action is due |
| opening | ISO date or null | client's opening date, the hard deadline |
| sentDate | ISO date or null | when the proposal or tender went out |
| mall | boolean | mall management approval needed |
| bomba | boolean | Bomba approval needed |
| notes | string | free text |
| lostReason | string | lost deals only |
| enq, closed | ISO dates | lost deals only, used for cycle time |
| log | array of [date, text] | history, oldest first |

Stage probabilities, used for the weighted forecast. Enquiry 10%, Site survey 20%, Concept & budget 35%, Proposal sent 50%, Negotiation 70%, Awarded 100%, Lost 0%. These are placeholders. Replace with Semba's real conversion once they supply history.

Win rate history (`HIST`) is a separate hard-coded object of prior year won and lost counts per route, only used for the two win rate tiles. It is invented.

## 6. Sample data rules

All brands, groups, contacts, people and figures are invented. Malls and buildings are real Klang Valley locations. Do not use Semba's real project names from their website (9090, Sunmoulin, SENYA) as deal data. The "Sample data" banner was removed on 4 October 2026 at Sritesh's request. The sample was written for 23 September 2026 (`BASE` in the code). Since 4 October 2026 a date engine moves every sample date forward by the days between `BASE` and the real date, as the Project Management Tool does, so the page always shows today and the story stays the same on demo day. Sample text must not name a calendar date, because the engine cannot move words.

## 7. Visual identity, SEMBA brand palette, set 1 October 2026

Sritesh replaced the two colour identity of 25 September with the full SEMBA brand palette, the same one the Project Management Tool uses. These are the only colours allowed. On 4 October 2026 he asked for the roles to be swapped so the two tools look different. In the CRM the rail is grey, the page ground is navy and taupe is the accent for buttons and main bars.

| Colour | Value | Use |
|---|---|---|
| Navy | #172E58 | page ground (since 4 October 2026), body text and headings, selected nav item, count badge, finished handover steps, secondary bars, tender stripe |
| Grey | #C9CBCA | rail (since 4 October 2026), second level text on the navy page ground |
| Taupe | #B5A08D | primary button, main bars, stage progress, callout rule, direct quote stripe |
| Off white | #EFEFEF | text and icons on navy, never pure white on navy |
| Deep navy | #0F2044 | token defined, not used since the swap |
| Dark taupe | #7D6A57 | primary button hover |
| Pale navy | #E6E9F0 | selected chip, hover row, kanban columns, callouts, assistant output |
| White | #FFFFFF | cards, drawer, table |
| Line | #D9DBDA | borders, dividers, bar tracks, grey pills |
| Softer text | #4A5468 | second level text |
| Softest text | #727B8D | labels, timestamps, hints |
| Green | #1E9E5A on #E6F6EC | good, done, saved |
| Amber | #E39B0A on #FFF4DC, text #9A6600 | needs attention |
| Red | #D93A3A on #FDE8E8 | problem, blocking, late |
| Light blue grey | #8A9BBE | chart extra, token defined, not used |
| Mid navy | #4F6591 | chart extra, token defined, not yet used |

Type. Manrope for everything, loaded from Google Fonts with system fallbacks. EB Garamond was used for page titles and KPI figures until 4 October 2026, when Sritesh asked for one font only. Do not add a second font.

Corners are 10px (`--radius`), one soft shadow, no gradients.

Dark mode is opt in only, by setting `data-theme="dark"` on `<html>`. It no longer follows the system setting. Its values are the same ones the Project Management Tool uses.

The rail shows the SEMBA wordmark (the same drawing the Project Management Tool uses, one `<symbol id="semba-logo">` at the top of `index.html`), then "CRM Tool", then "SEMBA Malaysia". Set by Sritesh on 4 October 2026 to match the Project Management Tool.

## 8. Decisions already made, do not reopen without Sritesh

1. Direct quote and tender win rates are never averaged. Shown separately, always.
2. The client's opening date drives urgency, not internal dates.
3. Approvals (mall management, Bomba) are per deal flags, shown in the drawer and used by the assistant.
4. There is no Accounts view (removed 4 October 2026). Each deal still carries its parent company in `group`.
5. Awarded deals stay visible to sales through the four handover steps.
6. The assistant must always produce something. Template fallback stays even after a live model is wired in.
7. The sample data banner was removed by Sritesh on 4 October 2026, and the rail footer was shortened to "Concept prototype for SEMBA Malaysia. Built by Lemon Sky AI Academy.". Nothing on screen says the data is sample now, the presenter says it.
8. Colours come from the SEMBA brand palette in section 7 and nothing else. Replaced the two colour rule on 1 October 2026.
9. Page ground is navy #172E58, changed from grey by Sritesh on 4 October 2026. Headings that sit straight on the page are off white. Test on a projector before demo day.
10. No dashes and no colons in any copy that reaches Semba. Commas, full stops, parentheses, numbered lists instead.

## 9. Next build stage, what Claude Code should do

The demo is a static page. To become something Semba could actually trial, it needs persistence, a real assistant, and a clean seam between the two. Suggested order.

### Stage A. Persistence, local first

1. Move the `deals` array and `HIST` into `data/deals.json` and `data/history.json`.
2. Add a small Node server (Express or Fastify, no framework beyond that) that serves `index.html` and exposes `GET /api/deals`, `PUT /api/deals/:id`, `POST /api/deals`, `POST /api/deals/:id/log`, `POST /api/deals/:id/stage`.
3. Every write in the page (stage move, note, new enquiry, handover advance) calls the API and re-renders from the response. Keep the optimistic toast.
4. Write to JSON on disk with an atomic rename. SQLite only if concurrent users become real.

### Stage B. Assistant on a real model

1. Add `POST /api/assist` taking `{kind, dealId}`. The server builds the same prompts that live in `PROMPTS` today and calls the Anthropic Messages API with a key from `.env`. Never put a key in the page.
2. Stream the reply to the page. Keep the template fallback for when the server is unreachable or the key is missing, and keep the label that says which path produced the draft.
3. Add a fourth action, "Summarise this account", that reads every deal in the group.

### Stage C. Make it Semba's

1. Replace the sample seed with a CSV import (`data/import.csv`, one row per deal, columns matching section 5).
2. Add a settings file for stage probabilities, owners, designers, PMs and sectors so Semba can edit lists without code.
3. Add a lost reason picker (Price, Timeline, Went quiet, Client postponed, Scope, Other) so the report is categorical rather than free text.
4. Add a proposal date reminder. When a deal moves to Proposal sent, auto set nextDate to sent plus 5 working days.

### Stage D. Optional, only if Semba asks

1. Email chaser sending via Gmail or Microsoft 365. Semba's office suite is still unknown, do not build until it is.
2. WhatsApp deep link to the contact number.
3. Export pipeline to Excel for the MD.

## 10. What not to build

1. No 3D design features. Dropped from the engagement on 17 September 2026.
2. No project management features beyond the handover strip. That lives in the separate SEMBA Sitebook prototype.
3. No user accounts or auth until Semba trials it with more than one person.
4. No promise, in copy or in the tool, that Lemon Sky will build this for Semba. The course teaches them to start it themselves. The solutions service is a separate conversation.

## 11. Open questions for Sritesh

1. Rail wordmark. Answered 4 October 2026, SEMBA wordmark with "CRM Tool" under it, see section 7.
2. Pin the demo date or shift with the clock. Answered 4 October 2026, it shifts with the clock, see section 6.
3. Sales headcount at Semba, still unknown, affects how many owners to seed.
4. Which office suite Semba uses, blocks Stage D item 1.
5. Does Semba's Japan HQ mandate a CRM. If yes, this whole tool is a teaching prop, not a trial candidate. Ask before the proposal goes out.

## 12. Sources behind the industry model

1. Quote and tender pipelines for a commercial fit-out contractor, SME Software Help case study. https://smesoftwarehelp.co.uk/case-studies/commercial-contractor-crm/
2. Construction bid management software features, Followup CRM. https://www.followupcrm.com/blog/construction-bid-management-software
3. Fit Out Construction Guide, Mastt. https://www.mastt.com/guide/fitout
4. Commercial fit-out 2026 guide, Archdesk. https://archdesk.com/blog/commercial-fit-out-2026-guide
5. Retail design and build Malaysia, Keith Ho Design. https://www.keithhodesign.com/latestnews/nid/187713/
6. Retail fit-out timeline in Malaysia, Wingspan International. https://wingspan-international.online/retail-fit-out-timeline-malaysia/
7. SEMBA Malaysia website, palette read live from its stylesheet on 25 September 2026. https://www.semba1008.co.jp/en/malaysia
