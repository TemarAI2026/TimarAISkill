# Status Semantics

## Purpose

This page explains how current public payment and payout statuses should be interpreted by external integrators and AI coding assistants.

## Payment status semantics

Current payment query returns normalized string statuses on the wire, including:

- `PENDING`
- `SUCCESS`
- `CANCEL`
- `RISK`

These are the statuses downstream systems should consume from the public response.

## Payout status semantics

Current payout query returns a raw `int` status value on the wire.

Examples of current mapped meanings:

- `0`: `PENDING_BUSINESS_APPROVAL`
- `1`, `2`, `3`, `6`, `7`, `8`, `9`: pending-family states
- `4`: `CANCEL`
- `5`: `SUCCESS`

## Consumption rules

- do not assume payment and payout expose status in the same wire format
- persist the raw status value or raw status string returned by the platform
- map payout `int` values through the current published table
- handle unknown future values safely with logging and investigation

## Common mistakes

- treating payout `status` as if it were already a normalized string
- collapsing all non-success states into one generic failure bucket
- discarding the raw returned status when normalizing locally
- assuming status semantics are identical between payment and payout

## Exact source reference

- [`../../skill/references/crypto/statuses.en.md`](../../skill/references/crypto/statuses.en.md)
