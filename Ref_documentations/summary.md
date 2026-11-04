# OMS — Executive Summary

**Classification:** Confidential — For Recipients and Authorized Parties Only  
**Project:** Baitulmaal Orphan Management System  
**Version:** 1.0  
**Date:** 11.04.2026  
**Status:** Issued for Review

## 1. Purpose

OMS is a module of the Baitulmaal PMO Platform for registering, verifying, sponsoring, supporting, funding, following up, and reporting on orphan beneficiaries.

Every approved orphan receives a unique, system-generated identifier such as:

`ORP-EG-2026-000142`

All sponsorships, activities, disbursements, visits, and progress reports are linked to that identifier.

## 2. Problem

Orphan operations span central management, country offices, field offices, partners, finance, compliance, and Donor Care. Data is collected through separate forms, verified manually, matched to sponsors by hand, and reported through disconnected spreadsheets. This creates duplicate records, untracked funds, missed renewals, late reports, and a risk of sharing sensitive beneficiary data with donors.

## 3. Solution

OMS creates one controlled orphan record and a set of structured, event-driven workflows.

**Partner → Verification → Orphan Profile → Sponsorship → Disbursement → Progress Report → Sponsor**

An orphan record moves through clear statuses:

- Draft → Submitted → Under Review → Verified → Approved → Registered
- Available for Matching → Matched → Active → Expiring Soon → Renewed / Terminated

## 4. Initial Scope

- Partner compliance and approval.
- Orphan intake form and bulk Excel import with duplicate detection.
- Verification with configurable approval authority.
- Orphan Profile and unique Orphan ID.
- Automated sponsor matching and audited manual override.
- Renewal and termination monitoring coordinated with Donor Care.
- General support activities with targets and beneficiary selection.
- Fund allocation, transfer, delivery, and proof of delivery.
- Quarterly financial reports with two-stage approval.
- Field visits and annual progress reports (July cycle).
- Privacy review and email delivery of approved reports.
- Notifications, audit log, and reports & analytics.

## 5. Target Outcome

A development team should be able to use this documentation to estimate, design, implement, test, and deploy the initial release without relying on undocumented assumptions, once the pending decisions in `Risk-assumption.md` are confirmed.

## 6. Indicative Delivery

Target initial release: approximately 20 weeks from kickoff, subject to confirmation of pending business decisions and PMO platform dependencies.
