# Forged idea: bulk hardware order system (demo)

Enterprise clients place bulk hardware orders; system prices them from ERP reference data, tracks lifecycle, lets clients view history and cancel.

## Locked
- **Pricing:** payable total = (Σ ERP MRP × qty, tax-inclusive) × (1 − client flat discount %), rounded once on the total, + shipping unless contract waives it. Called "payable total", not "net". No tax split; this is an order confirmation, not a tax invoice.
- **ERP mock (read-only):** supplies catalog/MRP, client flat discount, active-contract flag, per-order shipping quote, shipping-waiver flag. This system owns the order record and lifecycle. Mock has a "simulate unavailable" switch.
- **Snapshot:** MRP per line, discount %, shipping, payable total frozen at order creation; ERP not re-queried for pricing.
- **Intake:** qty whole number 1–10,000 per line; 1–100 lines; SKUs exist in ERP; no duplicate SKU; client has active contract. Any failure, including ERP down (retryable error), refuses the order and creates nothing. "Bulk" is descriptive, no size-based pricing.
- **One order = one destination = one shipment.** No partial shipments.
- **States:** INTAKE → PROCESSING → SHIPPED → FINAL_DELIVERY; PROCESSING ⇄ BACKORDERED (whole order waits); CANCELLED.
- **Cancel:** client only in INTAKE or BACKORDERED; check and write atomic against staff transitions. No reason captured.
- **Personas (dropdown, no auth):** Enterprise User (pick client from seeded list; place/view/cancel for that client only; sees own discount only), Warehouse Operator (all clients; forward transitions and BACKORDERED only; no cancel, no edits), Invoice Employee (read-only, delivered orders with snapshot).
- **History:** paginated list of the client's orders in all states, newest first, no filters; per-order append-only timestamped status timeline (transition, time, persona).

## Rejected
- Tiers, per-SKU rates, rebates: flat % only.
- Contracts authored here: they come from the ERP.
- Shipping computed here or flat-fee config: ERP quote only.
- Multi-address orders, partial shipments: one shipment per order.
- CONFIRMED and REJECTED states; cancel in PROCESSING; cancel as a request: too complex for demo.
- Per-user roles inside a client; real auth; "billed" flag for Invoice Employee.

## Assumed, not confirmed
- Shipping quote frozen at intake.
- Dropdown client scoping is simulated identity, not a security boundary.
- No inventory tracking; Warehouse Operator sets BACKORDERED manually.
- Real ERP response shape unknown; mock should be no friendlier than the real one (effective dates, inconsistent rate formats).

## Out of scope
- Build vs using the ERP's own client portal (scoped out, not refuted).
