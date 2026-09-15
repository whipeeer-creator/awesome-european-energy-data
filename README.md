# Awesome European Energy Data [![Awesome](https://awesome.re/badge-flat.svg)](https://awesome.re)

> Where electricity market data actually comes from in Europe — the interface,
> what it costs you to get in, and what breaks.

Every country publishes its power market data somewhere. Finding *which*
somewhere, and whether it is a real API or a spreadsheet behind a login, is the
part nobody writes down. This list is that part.

Entries say what the source holds, how you get at it, and the traps — not just
a name and a link.

## Contents

- [Pan-European](#pan-european)
- [Austria](#austria) · [Belgium](#belgium) · [Czechia](#czechia) ·
  [France](#france) · [Germany](#germany) · [Hungary](#hungary) ·
  [Italy](#italy) · [Netherlands](#netherlands) · [Nordics](#nordics) ·
  [Poland](#poland) · [Slovenia](#slovenia) · [Spain & Portugal](#spain--portugal) ·
  [United Kingdom](#united-kingdom)
- [Ready-made datasets](#ready-made-datasets)
- [Libraries](#libraries)
- [Reading](#reading)
- [Contributing](#contributing)

---

## Pan-European

- **[ENTSO-E Transparency Platform](https://transparency.entsoe.eu)** — the one
  source that covers every member country: day-ahead prices, load, generation
  by type, cross-border flows, balancing, outages. REST API at
  `web-api.tp.entsoe.eu/api`, XML responses.
  *Access:* free, but the token is not self-service — register, then e-mail
  `transparency@entsoe.eu` with the subject "Restful API access". Takes days.
  *Traps:* resolution varies per document and changed to 15 minutes across 2025;
  errors arrive as XML with HTTP 200; forecasts are versioned, so what the API
  returns today is **not** what was published back then — using it as a model
  feature is look-ahead bias.

- **[EEX Transparency](https://www.eex-transparency.com)** — REMIT publications:
  generation unit outages, installed capacity, actual output per unit. The place
  to look when a price spike needs explaining.
  *Access:* free, web and FTP.
  *Traps:* messages are versioned and revised after the fact; an outage you see
  today may not have been public when the market moved.

- **[Nord Pool](https://www.nordpoolgroup.com/en/Market-data1/)** — the Nordic
  and Baltic exchange, plus its own view of coupled continental zones.
  *Access:* free web tables; the API is commercial.

- **[JAO](https://www.jao.eu)** — cross-border capacity allocation for Core and
  the other regions. Auction results, ATC, flow-based domains.
  *Access:* free, public API.
  *Traps:* flow-based data is large and the schema assumes you know the market
  coupling model.

## Austria

- **[APG Markt­transparenz](https://markttransparenz.apg.at)** — the TSO's data
  portal: load, generation, balancing, congestion and redispatch.
  *Access:* free, CSV export per view.
  *Traps:* Austria and Germany were one bidding zone until 30 September 2018.
  Any series crossing that date needs both EIC codes.
- **[EXAA](https://www.exaa.at)** — the Austrian exchange, which runs an
  intraday auction distinct from the main day-ahead coupling.

## Belgium

- **[Elia Open Data](https://opendata.elia.be)** — one of the better TSO
  portals in Europe: load, wind and solar forecast vs actual, imbalance prices
  and volumes, at 15-minute resolution.
  *Access:* free, Opendatasoft API, JSON and CSV, no key for normal use.

## Czechia

- **[ČEPS](https://www.ceps.cz/en/all-data)** — system imbalance per quarter
  hour *and per minute*, activated balancing energy, generation, load,
  cross-border flows.
  *Access:* free SOAP service at `www.ceps.cz/_layouts/CepsData.asmx`. No key.
  *Traps:* SOAP in 2026, and the WSDL is the only documentation of the method
  parameters. Imbalance data appears roughly 5 minutes after the quarter hour.
- **[OTE](https://www.ote-cr.cz/en)** — the market operator: day-ahead and
  intraday results, imbalance price, yearly market reports.
  *Access:* free XLSX downloads on predictable URLs.
  *Traps:* the imbalance price is first an **estimate** and is overwritten;
  the final value is published the following day. Using the final value as a
  real-time feature is a leak.

## France

- **[RTE Data Portal](https://data.rte-france.com)** — consumption, generation
  by type, forecasts, balancing, interconnections.
  *Access:* free, but OAuth2 — register an application, then exchange
  credentials at `digital.iservices.rte-france.com/token/oauth/`.
  *Traps:* per-API rate limits are separate and low; the sandbox and production
  endpoints return different data.
- **[éCO2mix](https://www.rte-france.com/eco2mix)** — the public-facing view of
  the same data, with downloadable history.

## Germany

- **[SMARD](https://www.smard.de)** — the regulator's portal: generation by
  source, consumption, wholesale prices, cross-border flows, back to 2015.
  *Access:* free, and there is an undocumented but stable JSON endpoint under
  `smard.de/app/chart_data/` that the site itself uses.
- **[Netztransparenz](https://www.netztransparenz.de)** — the four TSOs jointly:
  reBAP (the imbalance price), EEG data, balancing volumes.
  *Access:* free downloads; an API exists but needs registration.
- **[Regelleistung.net](https://www.regelleistung.net)** — balancing capacity
  and energy auctions: aFRR, mFRR, FCR, tender results and merit order lists.
  *Access:* free REST API.
  *Traps:* the API moved paths more than once; old integrations break silently.

## Hungary

- **[MAVIR](https://www.mavir.hu/web/mavir-en/data-of-the-hungarian-electricity-system)** —
  load, generation, balancing.
  *Access:* free web tables and downloads; no real API, so ENTSO-E is usually
  the better route.
- **[HUPX](https://hupx.hu)** — the exchange, day-ahead and intraday results.

## Italy

- **[Terna Transparency](https://www.terna.it/en/electric-system/transparency-report)** —
  load, generation, balancing, at zonal level.
  *Access:* free, REST API at `api.terna.it`, OAuth2 client credentials.
- **[GME](https://www.mercatoelettrico.org/en/)** — the market operator: MGP
  results, zonal prices, the PUN.
  *Access:* free XML and XLSX downloads; the statistics section needs a
  (free) login.
  *Traps:* Italy is **six zones**, not one, and Calabria split off from South in
  2021. The PUN is a consumption-weighted average across zones — a reference
  index, not a price anything is settled at. Generators are paid their own
  zone's price, which on the islands differs a lot.

## Netherlands

- **[TenneT export data](https://www.tennet.org/english/operational_management/export_data.aspx)** —
  imbalance prices and volumes at a 1-minute resolution, which is unusual and
  genuinely useful.
  *Access:* free CSV export.
- **[NED.nl](https://ned.nl)** — generation and consumption per energy carrier.
  *Access:* free API with a key.

## Nordics

- **[Nord Pool](https://www.nordpoolgroup.com/en/Market-data1/)** — see above.
- National TSOs, each with an open-data portal:
  **[Statnett](https://www.statnett.no/en/for-stakeholders-in-the-power-industry/data-from-the-power-system/)** (NO) ·
  **[Svenska kraftnät](https://www.svk.se/en/national-grid/operations-and-electricity-markets/)** (SE) ·
  **[Fingrid Open Data](https://data.fingrid.fi/en/)** (FI) ·
  **[Energinet Energi Data Service](https://www.energidataservice.dk)** (DK).
  *Fingrid and Energinet are the two best open-data APIs in Europe* — real REST,
  documented, free keys, minute-level series.
  *Traps:* there is no single "Nordic price". Norway has five zones, Sweden
  four, Denmark two — and DK1 is synchronous with continental Europe while DK2
  is synchronous with the Nordic system. Averaging them hides spreads of several
  hundred percent.

## Poland

- **[PSE raporty API](https://api.raporty.pse.pl/)** — the TSO's new REST API:
  load, generation, imbalance, prices. Replaced the old CSV reports.
  *Access:* free, no key, OData-style filtering.
  *Traps:* the 2024 market reform changed settlement to 15 minutes and renamed
  several series; pre- and post-reform data do not concatenate cleanly.
- **[TGE](https://tge.pl/en)** — the exchange, day-ahead and intraday.

## Slovenia

- **[ELES](https://www.eles.si/en)** — the TSO: load, generation, balancing.
- **[BSP SouthPool](https://www.bsp-southpool.com)** — the regional exchange
  covering SI and neighbours.
  *Access:* free web tables; historical series are thin, so ENTSO-E is usually
  the practical source.

## Spain & Portugal

- **[ESIOS](https://www.esios.ree.es/en)** — the Spanish TSO's data system and
  one of the most complete in Europe: hundreds of indicators, from prices to
  every balancing product.
  *Access:* free REST API at `api.esios.ree.es`, token by e-mail request.
  *Traps:* indicators are addressed by numeric id (`/indicators/600` is the
  day-ahead price); the id list is the real documentation.
- **[OMIE](https://www.omie.es/en)** — the Iberian market operator, day-ahead
  and intraday results for both ES and PT.
  *Access:* free file downloads on predictable URLs.
- **[REN](https://www.ren.pt)** — the Portuguese TSO.
  *Traps:* Spain and Portugal are separate bidding zones coupled as MIBEL —
  usually the same price, sometimes not.

## United Kingdom

- **[Elexon Insights / BMRS](https://bmrs.elexon.co.uk)** — settlement data for
  the GB market: imbalance prices, bid-offer acceptances, generation by fuel.
  *Access:* free REST API at `data.elexon.co.uk/bmrs/api/v1/`, no key needed
  since the 2023 rebuild.
  *Traps:* GB settles in **half-hour periods numbered 1–48**, or 46/50 on clock
  change days. Code that assumes 48 will break twice a year.
- **[NESO Data Portal](https://www.neso.energy/data-portal)** — the system
  operator: demand and wind forecasts, carbon intensity, balancing costs.
  *Access:* free CKAN API.

---

## Ready-made datasets

For when you want the numbers, not the integration:

- **[european-power-prices](https://github.com/whipeeer-creator/european-power-prices)** —
  day-ahead prices for 12 zones since 2022, daily CSV, updated automatically.
- **[eic-codes](https://github.com/whipeeer-creator/eic-codes)** — the bidding
  zone codes every API above expects, as CSV and JSON.
- **[Open Power System Data](https://open-power-system-data.org)** — curated
  research datasets for Europe. Excellent, but no longer updated past 2020.
- **[Energy-Charts](https://www.energy-charts.info)** — Fraunhofer ISE, public
  API, strong on generation mix and long history.

## Libraries

- **[entsoe-py](https://github.com/EnergieID/entsoe-py)** — the standard Python
  wrapper for ENTSO-E, returns pandas objects. Start here for analysis.
- **[entsoe-quickstart](https://github.com/whipeeer-creator/entsoe-quickstart)** —
  single file, no dependencies, for scheduled jobs where pandas is overkill.
- **[entsoe-client](https://github.com/FHof/entsoe-client)** — alternative
  Python client.
- **[epftoolbox](https://github.com/jeslago/epftoolbox)** — benchmarks and
  models for electricity price forecasting.

## Reading

- **[ENTSO-E Transparency manual](https://transparency.entsoe.eu/content/static_content/Static%20content/knowledge%20base/knowledge%20base.html)** —
  what each document type actually contains.
- **[ACER market monitoring reports](https://www.acer.europa.eu)** — how the
  markets behaved, annually, with the definitions spelled out.
- Country-by-country guides on
  [progrunners.com](https://progrunners.com/entso-e-api/) — the longer form of
  several entries above, written while integrating them.

## Contributing

Corrections especially welcome: an endpoint that moved, a registration step
that changed, a trap that cost you an afternoon. Open an issue or a PR.

What belongs here: a source of European electricity market data, with a note on
how you get access and what surprises you. What does not: commercial data
vendors without a free tier, and links with no description.

## Licence

[CC0](LICENSE) — public domain.
