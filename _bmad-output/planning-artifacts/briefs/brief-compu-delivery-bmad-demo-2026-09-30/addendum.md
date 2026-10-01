# Addendum: Product Brief for the Bulk Hardware Order System (BMAD demo)

## Mockup divergences from the forged idea

The UI mockups in `figma-designs/` are kept as-is and used as layout reference only. Where they differ from the forged idea (`_bmad-output/forge/bulk-hardware-order-system/forged-idea.md`), **the forged idea wins**. The spec and build steps must not implement these from the mockups.

| Group | Topic | Forged idea (authoritative) | Mockups (do not implement) |
|---|---|---|---|
| Pricing | Wording | "Payable total", not "net" | "Net Total", "Final Net Total", "Net Billed Amount"; "Unit MSRP" instead of "MRP" |
| Pricing | Tax | MRP is tax-inclusive, no tax split, not a tax invoice | `invoice-staff-detail-page.html` is a "COMMERCIAL TAX INVOICE" with 8.25% tax added |
| Pricing | Discount | Flat client %, rounded once on total | Computed per line, with "Tier 1 (15% Discount)" labels |
| Pricing | Tiers | Rejected, no size-based pricing | `create-order-page.html` gives 5% off at 50+ units ("Bulk Volume Tier") |
| Pricing | Shipping | ERP quote, or waived by contract | Always "Waived (Enterprise Tier)", no quote shown |
| Pricing | Pricing snapshot | Frozen at creation | Recomputed live from hard-coded JavaScript |
| Orders | Editing | No edits | "View / Edit Order" and "Update Order Line Items" in INTAKE and PROCESSING |
| Orders | Cancel | Client only, INTAKE or BACKORDERED, not a request | Also in PROCESSING. Labeled "Cancel Request" |
| Orders | States | Includes BACKORDERED. Final state is FINAL_DELIVERY | No BACKORDERED. Stepper says "Delivered" |
| Orders | Intake validation | Quantity 1–10,000, 1–100 lines, no duplicate SKU, active contract, ERP-down refusal | Quantity min 1 only, no other checks |
| Orders | History | Paginated, all states, newest first, timeline per order | No pagination or timeline. History shows only Delivered and Cancelled orders, with active orders shown separately |
| Roles | Invoice Employee | Read-only, delivered orders only, no "billed" flag | Sees Shipped orders, records payment (which flips the order to Delivered), edits billing notes, has Unbilled, Draft, Sent, and Paid statuses |
| Roles | Client scoping | Per selected client | Enterprise pages are hard-wired to a single client, "Acme Corp". Orders in one localStorage list |

The persona dropdown (a "Role:" selector, no login) is the one point where the mockups agree with the forged idea.

## Parked: mockup-only features (out of v1)

These features are not in the forged idea; they are candidates for a later change-request demo:

- **Warehouse:** serial asset tagging with a ship gate, carrier choice (FedEx, DHL, UPS, Internal Fleet), tracking and waybill generation, facility selector and SLA, per-order manifest.
- **Invoicing:** invoice numbers, due dates, Net 30 terms, EIN and AP contact, payment recording, historical ledger, Print/PDF, staff billing notes.
- **Other:** contract and MSA badges, draft order state, toast notifications (the cancel confirmation modal is harmless UI and may be reused).

## Mockup data notes

- Orders in the mockups span clients Acme, Apex, Starlight, Nexus, BrightWave, Titan, and Meridian. A seeded client list for the demo can start from these.
