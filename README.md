# HedgeTools

**Options hedging and execution utilities for Interactive Brokers.**

HedgeTools is a compact Python toolkit built around Interactive Brokers for monitoring option legs, constructing multi-leg option combinations, and submitting limit orders with incremental price adjustment.

It started as a personal trading utility rather than a general-purpose trading framework. The focus is a practical execution problem: **get a multi-leg option order into the market, monitor execution, and gradually improve the limit price when the order does not fill.**

## What it does

- **1–4 leg option combinations** with configurable strikes, expirations, actions, ratios, and quantities
- **Interactive Brokers integration** using both `ib_insync` and the official `ibapi` interface
- **Contract resolution** before market-data requests and order submission
- **Market-data preview** with bid / ask / last prices for individual legs
- **Estimated combination pricing** from individual leg quotes
- **Incremental limit-price adjustment** toward a configured final price
- **Partial-fill tracking** with filled / remaining quantities and average fill price
- **Manual confirmation** immediately before order submission
- **Voice notifications** through `pyttsx3`
- **IB BAG contracts** for multi-leg combination orders

The repository contains concrete 1-, 2-, 3-, and 4-leg examples, while the underlying utilities support configurable combinations.

## Execution workflow

```text
Define option legs → resolve contracts → read market data
                  → estimate combination price
                  → human confirmation → submit limit order
                  → monitor fills → adjust price within final limit
```

The price adjustment is explicitly bounded by a configured final price; the utility does not blindly chase the market.

## Project structure

```text
HedgeTools/
├── IBOptionTool.py
├── IBOptionToolOffical.py
├── IBPriceOffical.py
├── IBOption1Leg.py
├── IBOption1Leg_CALL.py
├── IBOption1Leg_PUT.py
├── IBOption2Leg.py
├── IBOption3Leg.py
├── IBOption4Leg.py
└── aiTips/
```

The numbered scripts demonstrate different option-leg configurations. The `IBOption*` modules contain reusable order-management and market-data logic.

## Requirements

- Python 3
- Interactive Brokers TWS or IB Gateway
- An IBKR account with the required market-data and trading permissions
- `ib_insync`
- `ibapi`
- `pyttsx3` for optional voice notifications

```bash
python -m venv .venv
source .venv/bin/activate
# Windows: .venv\\Scripts\\activate
pip install ib_insync ibapi pyttsx3
```

Configure TWS / IB Gateway to accept API connections before running an example.

## Running an example

```bash
python IBOption2Leg.py
```

Before running, review the example's order parameters:

```text
spread_symbol
leg*_expiry
leg*_strike
leg*_right
leg*_action
combo_quantity
combo_init_price
combo_price_final
combo_price_step
```

These values define the actual order.

## Safety

This repository contains **live-order capable trading code**.

Review the contract, quantity, limit prices, account, and TWS/IB Gateway connection before confirming an order. Start with paper trading and understand the IB API behavior before connecting to a live account.

The incremental pricing logic is an execution utility, not a trading strategy or a recommendation to trade any particular option.

## Relationship to my quant work

This repository is intentionally small. It demonstrates the **execution layer** of my options tooling: contract resolution, market-data checks, multi-leg construction, bounded price adjustment, and order-state monitoring.

My separate quantitative trading platform is a larger system covering market-data ingestion, time-series storage, strategy signals, human review, automated execution, and position management. I plan to publish that project separately.

## License

No license has currently been added to this repository.