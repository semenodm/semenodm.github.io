---
layout: page
title: Free DynamoDB bill review
permalink: /reviews/
---

I'm offering a small number of free DynamoDB bill reviews. You send an anonymized cost breakdown, and I send back a written read of where the money goes and what is worth fixing first.

## What you get

- A breakdown of your DynamoDB spend by driver: base-table storage, index storage, replica copies, writes, replicated writes, and reads.
- The two or three changes most likely to pay back, with a rough one-time cost, monthly saving, and payback period for each.
- Notes on risk: which fixes are safe to try first and which need a staged rollout.

It's a written review, usually within a week. There's no sales call and no obligation.

## Who qualifies

Teams spending **more than $2K per month on DynamoDB**. Below that, the fixes rarely pay back the engineering time.

## What to send

- A **Cost Explorer breakdown of DynamoDB by usage type** for the last 1–3 months (CSV export or a screenshot is fine).
- **Approximate table sizes**: item count, average item size, number of global secondary indexes, and number of replica Regions for the largest tables.
- Optional: how old most of your data is and how often old items still change.

Rename tables and remove account IDs before sharing. **Never send credentials, access keys, or account access.** I don't need them and won't accept them.

## How to request one

[Open a bill review request on GitHub Discussions](https://github.com/semenodm/semenodm.github.io/discussions/new?category=q-a&title=Bill%20review%20request). Say roughly how much you spend on DynamoDB per month, and I'll reply there with how to share the details. If you'd rather not post publicly, mention that in the request and we'll find another way to exchange files.

For background on the method, read [how to archive cold DynamoDB data without losing a single item](/2026/10/13/dynamodb-cold-data-archival.html).
