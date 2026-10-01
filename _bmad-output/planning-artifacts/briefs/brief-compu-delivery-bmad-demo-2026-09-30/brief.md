---
title: "Product Brief: Bulk Hardware Order System (BMAD demo)"
status: draft
created: 2026-09-30
updated: 2026-09-30
---

# Product Brief: Bulk Hardware Order System (BMAD demo)

## Executive Summary

This is a clone-and-run demo repository that takes one feature, a bulk hardware order system for enterprise clients, through the BMAD method from a rough idea to working React and Java code. Its reader is a software professional who wants to learn spec-driven development. The order system is the worked example. The deliverable is the trail of artifacts and the README that explains it.

Spec-driven development is easy to describe and hard to picture. This repo shows the whole path on one feature, with every artifact left in place.

## The Problem

`[ASSUMPTION]` Professionals curious about spec-driven development meet it as a slogan or a short demo. They can't see the intermediate artifacts, which step made each one, or how an early decision (say, "flat discount, no tiers") ends up enforced in code. Without seeing them, they can't try the approach on their own work.

## The Solution

A single repository containing:

- **The artifact trail.** The forged idea (the pressure-tested starting concept), this brief, spec, tickets, and code, each produced by a named BMAD step and kept in the repo. `[ASSUMPTION]` The path roughly follows forge → brief → spec → tickets → build. The README holds the detail.
- **A README** that explains how the app was built and maps each step to its artifact.
- **A working app.** A React web UI and a Java API implementing the order system.

## What Makes This Different

It isn't a novel product, and it makes no market claim. Its value is completeness: one small feature, every step, and no hidden hand-edits. It also keeps a real conflict in view. The UI mockups in `figma-designs/` disagree with the forged idea on pricing, tiers, editing, cancellation, and invoicing. The forged idea wins, and the divergences are recorded in [addendum.md](addendum.md). A learner sees what spec authority means when a picture and a spec disagree.

## Who This Serves

**Primary: the learner.** A software professional of intermediate skill who wants to see BMAD in action and then reproduce the path on their own idea.

**Secondary: the system's simulated users** (persona dropdown, no authentication; a simulated identity, not a security boundary):

- **Enterprise User.** Picks a client from a seeded list, then places, views, and cancels that client's bulk orders. Sees only that client's discount.
- **Warehouse Operator.** Sees all clients' orders and moves them forward through the lifecycle, including marking and clearing BACKORDERED. Cannot cancel or edit.
- **Invoice Employee.** Reads delivered orders with their frozen pricing snapshot. Read-only.

## Success Criteria

Success is about the learner, not the order system's users:

1. A learner can name which BMAD step produced which artifact.
2. A learner can reproduce the path on their own idea.
3. `[ASSUMPTION]` The repo runs end to end: the React UI and Java API start from a clone, and the specified behavior works.

There are no adoption or usage metrics for this demo.

## Scope

### In v1, locked in the forged idea

`_bmad-output/forge/bulk-hardware-order-system/forged-idea.md` is the source of truth. It overrides the mockups wherever they differ, and the mockups are a UI layout reference only.

- **Pricing.** Payable total = tax-inclusive enterprise resource planning (ERP) MRP × quantity, less the client's flat discount (rounded once on the total), plus ERP shipping unless the contract waives it. Label it "payable total", not "net". Frozen at order creation.
- **ERP mock.** Read-only: catalog and MRP, client discount, active-contract flag, shipping quote, waiver flag, plus a "simulate unavailable" switch.
- **Intake rules.** Whole-number quantity 1–10,000 per line, 1–100 lines, no duplicate SKU, every SKU exists in the ERP, and the client has an active contract. Any failure, including an ERP outage, refuses the order and creates nothing.
- **Lifecycle.** INTAKE → PROCESSING → SHIPPED → FINAL_DELIVERY, PROCESSING ⇄ BACKORDERED, and CANCELLED. One order is one destination and one shipment. There is no inventory tracking: the Warehouse Operator sets BACKORDERED manually.
- **Cancel.** Only the Enterprise User can cancel, and only in INTAKE or BACKORDERED. The cancel check and write are atomic against Warehouse Operator transitions.
- **History.** A paginated list of the client's orders in all states, newest first, plus an append-only status timeline per order.

### In v1, assumed

- **Stack.** `[ASSUMPTION]` React web UI and Java API. Framework, database, and structure are left to the architecture step.
- **README.** `[ASSUMPTION]` It is a deliverable in scope, since the success criteria depend on it.
- **Catalog picker.** `[ASSUMPTION]` A simple SKU-or-name picker over the ERP mock's catalog, as a UI convenience.

### Out of v1

Asset tagging, carrier and tracking, invoice numbers, ledger, payment recording and tax invoices, volume tiers, per-SKU rates, order editing, cancel in PROCESSING, multi-address orders and partial shipments, real authentication, and contracts authored in this system.

## Open Questions and Unknowns

- The real ERP response shape is unknown. The mock should be no friendlier than the real response, so it should include effective dates and inconsistent rate formats.
- Shipping quote frozen at intake is assumed, not confirmed.
- Whether to build this at all versus using the ERP's own client portal is outside this demo's scope, not ruled out.

## Vision

If it works, learners will run the same path on their own ideas (Success Criterion 2). Parked features such as tiers, invoicing, and asset tagging could later serve as worked change requests for a follow-up demo, `[ASSUMPTION]` using `bmad-correct-course`.
