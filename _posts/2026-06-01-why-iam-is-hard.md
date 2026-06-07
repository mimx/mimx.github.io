---
layout: post
title: "Why IAM Is Hard (And Why That's the Point)"
date: 2026-06-01
tags: [iam, identity, access-management]
excerpt: "Access management sounds simple until you're three levels deep in exception policies and wondering how you got here."
---

Access management sounds simple: give the right people the right access at the right time.
Then you meet your first production system.

## The deceptive simplicity

The idea is clean. A user needs access to a resource. You grant it. Done.

But in practice, "user" becomes hundreds of service accounts, contractors, bots, and shared identities.
"Resource" becomes thousands of applications, databases, S3 buckets, and internal APIs.
"Right time" becomes a governance question nobody can fully answer.

## Why it compounds

Every exception creates precedent. Every workaround becomes a pattern.
After a few years, what started as a clean model is covered in layers of:

- Roles that nobody owns
- Groups with names like `legacy-finance-temp-2019`
- Birthright access that was never revoked
- Approval chains that rubber-stamp because the alternative is friction

## The real problem isn't technical

Most IAM failures aren't failures of technology. They're failures of ownership.
Nobody wants to own the access model. It's unglamorous, thankless, and politically complicated.

The engineer who builds the provisioning pipeline is celebrated.
The person who runs the quarterly access review is not.

## What actually helps

1. **Treat access as code.** Version it. Review it. Diff it.
2. **Make revocation cheap.** If revoking access breaks things, you've already lost.
3. **Automate the boring parts** — especially birthright access and offboarding.
4. **Own the model, not just the tooling.** Tools don't fix culture.

IAM is hard because it sits at the intersection of security, usability, and organizational politics.
That's exactly why it matters.
