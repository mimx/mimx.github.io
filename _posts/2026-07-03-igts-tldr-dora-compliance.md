---
layout: post
title: "TL;DR: IGTS & The Road to DORA Compliance"
date: 2026-07-03
categories: [technology]
wide: true
tags: [igts, iga, dora, sod, compliance, identity-governance, tagging-standard]
excerpt: "Every IGA programme eventually hits a brick wall. IGTS is how you break through it — and how to extend it for DORA."
---

<style>
  .igts-hero {
    margin: 2rem 0 3rem 0;
    border-radius: 8px;
    overflow: hidden;
  }

  .igts-hero-bands {
    display: flex;
    height: 6px;
    background: linear-gradient(to right, #3b82f6 0%, #3b82f6 14.28%, #6366f1 14.28%, #6366f1 28.56%, #f59e0b 28.56%, #f59e0b 42.84%, #ef4444 42.84%, #ef4444 57.12%, #10b981 57.12%, #10b981 71.4%, #14b8a6 71.4%, #14b8a6 85.68%, #06b6d4 85.68%, #06b6d4 100%);
  }

  .igts-byline {
    font-size: 0.9rem;
    color: #999;
    margin: 1.5rem 0 1rem 0;
    font-style: italic;
  }

  .igts-pullquote {
    border-left: 4px solid #3b82f6;
    padding: 1.5rem;
    margin: 2rem 0;
    background: rgba(59, 130, 246, 0.05);
    border-radius: 4px;
    font-size: 1.1rem;
    font-weight: 500;
    color: #e0e0e0;
  }

  .igts-section {
    margin: 3rem 0;
  }

  .igts-section h2 {
    font-size: 1.6rem;
    margin-bottom: 1.5rem;
    color: #fff;
  }

  .igts-section p {
    line-height: 1.8;
    margin-bottom: 1rem;
    color: #ccc;
  }

  .igts-chaos-diagram {
    margin: 2rem 0;
    padding: 1.5rem;
    background: #1a1a1a;
    border: 1px solid #333;
    border-radius: 4px;
    font-family: 'JetBrains Mono', monospace;
    font-size: 0.9rem;
    overflow-x: auto;
  }

  .igts-chaos-flow {
    display: flex;
    align-items: center;
    justify-content: space-between;
    gap: 1rem;
    flex-wrap: wrap;
    margin: 1rem 0;
  }

  .igts-chaos-box {
    flex: 1;
    min-width: 120px;
    padding: 1rem;
    background: #222;
    border: 1px solid #444;
    border-radius: 4px;
    text-align: center;
    font-size: 0.85rem;
  }

  .igts-chaos-box.problem {
    border-color: #ef4444;
    color: #fca5a5;
  }

  .igts-chaos-box.warning {
    border-color: #f59e0b;
    color: #fcd34d;
  }

  .igts-chaos-arrow {
    color: #666;
    font-size: 1.2rem;
  }

  .igts-tag-widget {
    margin: 2rem 0;
    padding: 1.5rem;
    background: #1a1a1a;
    border: 1px solid #333;
    border-radius: 8px;
  }

  .igts-tag-pills {
    display: flex;
    gap: 0.5rem;
    flex-wrap: wrap;
    margin-bottom: 1.5rem;
    align-items: center;
  }

  .igts-tag-prefix {
    font-family: 'JetBrains Mono', monospace;
    font-size: 0.85rem;
    color: #666;
    padding: 0.5rem 0.25rem;
  }

  .igts-tag-pill {
    padding: 0.5rem 1rem;
    border-radius: 20px;
    font-size: 0.9rem;
    font-weight: 600;
    cursor: pointer;
    border: 2px solid transparent;
    transition: all 0.2s ease;
    font-family: 'JetBrains Mono', monospace;
  }

  .igts-tag-pill:hover {
    transform: scale(1.05);
  }

  .igts-tag-pill.active {
    transform: scale(1.1);
    box-shadow: 0 0 12px rgba(255,255,255,0.2);
    outline: 2px solid rgba(255,255,255,0.4);
    outline-offset: 2px;
  }

  .igts-tag-pill-blue   { background: rgba(59,130,246,0.2);  color: #60a5fa; border-color: #3b82f6; }
  .igts-tag-pill-indigo { background: rgba(99,102,241,0.2);  color: #818cf8; border-color: #6366f1; }
  .igts-tag-pill-amber  { background: rgba(245,158,11,0.2);  color: #fcd34d; border-color: #f59e0b; }
  .igts-tag-pill-red    { background: rgba(239,68,68,0.2);   color: #fca5a5; border-color: #ef4444; }
  .igts-tag-pill-emerald{ background: rgba(16,185,129,0.2);  color: #6ee7b7; border-color: #10b981; }
  .igts-tag-pill-teal   { background: rgba(20,184,166,0.2);  color: #5eead4; border-color: #14b8a6; }
  .igts-tag-pill-cyan   { background: rgba(6,182,212,0.2);   color: #67e8f9; border-color: #06b6d4; }

  .igts-tag-detail {
    margin-top: 1rem;
    padding: 1.25rem 1.5rem;
    background: #222;
    border-left: 3px solid #3b82f6;
    border-radius: 4px;
    display: none;
  }

  .igts-tag-detail.active {
    display: block;
    animation: igtsSlideIn 0.25s ease;
  }

  @keyframes igtsSlideIn {
    from { opacity: 0; transform: translateY(-8px); }
    to   { opacity: 1; transform: translateY(0); }
  }

  .igts-tag-detail h4 {
    margin: 0 0 0.4rem 0;
    color: #fff;
    font-size: 1rem;
  }

  .igts-tag-detail p {
    margin: 0.4rem 0 0.8rem 0;
    color: #bbb;
    font-size: 0.9rem;
    line-height: 1.6;
  }

  .igts-tag-values {
    display: flex;
    gap: 0.5rem;
    flex-wrap: wrap;
    margin-top: 0.75rem;
  }

  .igts-tag-value {
    padding: 0.3rem 0.75rem;
    background: #333;
    border: 1px solid #555;
    border-radius: 12px;
    font-size: 0.8rem;
    font-family: 'JetBrains Mono', monospace;
    color: #bbb;
  }

  .igts-tag-value.current {
    color: #000;
    font-weight: 700;
  }

  .igts-comparison {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 1.5rem;
    margin: 2rem 0;
  }

  .igts-comparison-box {
    padding: 1.5rem;
    background: #1a1a1a;
    border: 1px solid #333;
    border-radius: 8px;
  }

  .igts-comparison-box h4 {
    margin: 0 0 1rem 0;
    color: #fff;
    font-size: 1.1rem;
  }

  .igts-comparison-before { border-top: 3px solid #ef4444; }
  .igts-comparison-after  { border-top: 3px solid #10b981; }

  .igts-comparison-list {
    list-style: none;
    padding: 0;
    margin: 0;
  }

  .igts-comparison-list li {
    padding: 0.4rem 0;
    color: #bbb;
    font-size: 0.9rem;
  }

  .igts-comparison-list li::before {
    content: "• ";
    margin-right: 0.4rem;
  }

  .igts-comparison-before li::before { color: #ef4444; }
  .igts-comparison-after  li::before { color: #10b981; }

  .igts-dora-cards {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
    gap: 1.25rem;
    margin: 2rem 0;
  }

  .igts-dora-card {
    padding: 1.5rem;
    background: #1a1a1a;
    border: 1px solid #333;
    border-radius: 8px;
    transition: border-color 0.2s, box-shadow 0.2s;
  }

  .igts-dora-card:hover {
    border-color: #555;
    box-shadow: 0 4px 16px rgba(0,0,0,0.4);
  }

  .igts-dora-num {
    display: inline-block;
    width: 2rem;
    height: 2rem;
    line-height: 2rem;
    text-align: center;
    border-radius: 50%;
    background: #2a2a2a;
    border: 1px solid #555;
    color: #60a5fa;
    font-weight: 700;
    font-size: 0.95rem;
    margin-bottom: 0.75rem;
  }

  .igts-dora-card h4 {
    margin: 0 0 0.75rem 0;
    color: #fff;
    font-size: 1rem;
  }

  .igts-dora-card p {
    margin: 0;
    color: #bbb;
    font-size: 0.9rem;
    line-height: 1.6;
  }

  .igts-dora-ext {
    margin-top: 1rem;
    padding: 0.6rem 0.9rem;
    background: #222;
    border-left: 3px solid #6366f1;
    border-radius: 4px;
    font-family: 'JetBrains Mono', monospace;
    font-size: 0.82rem;
    color: #818cf8;
    letter-spacing: 0.02em;
  }

  .igts-questions { margin: 2rem 0; }

  .igts-question {
    margin: 1.25rem 0;
    padding: 1.25rem 1.5rem;
    background: #1a1a1a;
    border-left: 4px solid #6366f1;
    border-radius: 4px;
    color: #ccc;
    font-style: italic;
    line-height: 1.7;
  }

  .igts-question-mark {
    margin-right: 0.5rem;
    color: #818cf8;
    font-weight: 700;
    font-style: normal;
  }

  .igts-footer-links {
    margin: 3rem 0 1rem 0;
    padding-top: 2rem;
    border-top: 1px solid #333;
    display: flex;
    flex-direction: column;
    gap: 0.75rem;
  }

  .igts-footer-links a {
    color: #60a5fa;
    text-decoration: none;
    font-size: 0.95rem;
    transition: color 0.2s;
  }

  .igts-footer-links a:hover { color: #93c5fd; text-decoration: underline; }

  @media (max-width: 640px) {
    .igts-comparison { grid-template-columns: 1fr; }
    .igts-tag-pill   { font-size: 0.78rem; padding: 0.4rem 0.75rem; }
  }
</style>

<div class="igts-hero"><div class="igts-hero-bands"></div></div>

<div class="igts-byline">IGTS Working Group &nbsp;·&nbsp; July 3, 2026 &nbsp;·&nbsp; ~8 min read</div>

<div class="igts-pullquote">
  <em>"Every IGA programme eventually hits a brick wall. IGTS is how you break through it."</em>
</div>

<div class="igts-section">
  <h2>The Problem: The Mapping Nightmare</h2>
  <p>
    Identity Governance and Administration (IGA) has a scaling problem. We build beautiful certification campaigns, robust workflows, and complex Segregation of Duties (SoD) matrices. But underneath the shiny interface, the engine relies on a fundamental, flawed assumption: that we can accurately map millions of granular, disparate technical entitlements back to business functions.
  </p>
  <p>
    When a new API role is created in AWS, or a new permission set is deployed in Salesforce, the IGA platform doesn't know what it means. It just sees strings: <code>IAM-Role-7824</code> or <code>Del_Cust_Rec</code>. The burden of translation falls entirely on the IGA administrator — or increasingly, an error-prone LLM inference engine — to manually classify these entitlements against the SoD matrix.
  </p>
  <p>
    The result? Rule bloat. A typical enterprise IGA deployment requires tens of thousands of custom SoD mapping rules. It is slow, brittle, and provides dangerously low confidence during audits.
  </p>

  <div class="igts-chaos-diagram">
    <div style="color:#999; font-size:0.8rem; margin-bottom:1rem;">$ cat /var/iga/entitlements.log | head -5</div>
    <div style="color:#fca5a5; margin-bottom:1.5rem;">
      IAM-Role-7824 &nbsp;·&nbsp; Del_Cust_Rec &nbsp;·&nbsp; A_FIN_INV_APPROVER &nbsp;·&nbsp; PROD-DB-ADMIN-RO &nbsp;·&nbsp; GRP_FINANCE_AP_003
    </div>
    <div class="igts-chaos-flow">
      <div class="igts-chaos-box">Raw Entitlements<br><small style="color:#777">(cryptic strings)</small></div>
      <div class="igts-chaos-arrow">→</div>
      <div class="igts-chaos-box problem">Manual Rules<br><small>10,000+ mappings</small></div>
      <div class="igts-chaos-arrow">→</div>
      <div class="igts-chaos-box warning">IGA Engine<br><small>slow · brittle</small></div>
    </div>
    <div style="color:#ef4444; font-size:0.85rem; margin-top:1.25rem;">⚠ Low audit confidence &nbsp;·&nbsp; Rule bloat &nbsp;·&nbsp; Brittleness on every schema change</div>
  </div>
</div>

<div class="igts-section">
  <h2>The Core Blueprint: The IGTS Tag</h2>
  <p>
    The Identity Governance Tagging Standard (IGTS) flips the paradigm. Instead of the IGA engine trying to interpret raw entitlements after they are created, the source system embeds a standardised, machine-readable tag directly into the identity object at the moment of creation.
  </p>
  <p>
    Click each segment below to explore the tag anatomy:
  </p>

  <div class="igts-tag-widget">
    <div class="igts-tag-pills">
      <span class="igts-tag-prefix">igts/v1/</span>
      <div class="igts-tag-pill igts-tag-pill-blue"    data-idx="0">finance</div>
      <div class="igts-tag-pill igts-tag-pill-indigo"  data-idx="1">accounts-payable</div>
      <div class="igts-tag-pill igts-tag-pill-amber"   data-idx="2">critical</div>
      <div class="igts-tag-pill igts-tag-pill-red"     data-idx="3">inscope</div>
      <div class="igts-tag-pill igts-tag-pill-emerald" data-idx="4">post</div>
      <div class="igts-tag-pill igts-tag-pill-teal"    data-idx="5">financial</div>
      <div class="igts-tag-pill igts-tag-pill-cyan"    data-idx="6">permanent</div>
    </div>

    <div class="igts-tag-detail" data-idx="0" style="border-color:#3b82f6">
      <h4>Domain</h4>
      <p>The high-level business area that owns this entitlement. Drives top-level SoD scoping and certification routing.</p>
      <div class="igts-tag-values">
        <span class="igts-tag-value current" style="background:#3b82f6">finance</span>
        <span class="igts-tag-value">hr</span>
        <span class="igts-tag-value">tech</span>
        <span class="igts-tag-value">legal</span>
        <span class="igts-tag-value">sales</span>
        <span class="igts-tag-value">ops</span>
      </div>
    </div>

    <div class="igts-tag-detail" data-idx="1" style="border-color:#6366f1">
      <h4>Process</h4>
      <p>The specific business process within the domain. SoD rules are evaluated at this level — process + function = conflict detection.</p>
      <div class="igts-tag-values">
        <span class="igts-tag-value current" style="background:#6366f1; color:#fff">accounts-payable</span>
        <span class="igts-tag-value">vendor-master</span>
        <span class="igts-tag-value">general-ledger</span>
        <span class="igts-tag-value">payroll</span>
        <span class="igts-tag-value">deployment</span>
      </div>
    </div>

    <div class="igts-tag-detail" data-idx="2" style="border-color:#f59e0b">
      <h4>Sensitivity</h4>
      <p>Risk level of the system or data touched by this entitlement. Drives certification frequency, approver tier, and monitoring intensity.</p>
      <div class="igts-tag-values">
        <span class="igts-tag-value current" style="background:#f59e0b; color:#000">critical</span>
        <span class="igts-tag-value">high</span>
        <span class="igts-tag-value">medium</span>
        <span class="igts-tag-value">low</span>
      </div>
    </div>

    <div class="igts-tag-detail" data-idx="3" style="border-color:#ef4444">
      <h4>SOX Scope</h4>
      <p>Whether this entitlement is in scope for Sarbanes-Oxley (or equivalent regulatory compliance). Determines audit evidence requirements.</p>
      <div class="igts-tag-values">
        <span class="igts-tag-value current" style="background:#ef4444; color:#fff">inscope</span>
        <span class="igts-tag-value">outscope</span>
      </div>
    </div>

    <div class="igts-tag-detail" data-idx="4" style="border-color:#10b981">
      <h4>Function</h4>
      <p>The action being performed. This is the SoD engine's primary conflict axis — <code>post</code> vs <code>approve</code> in the same process = violation.</p>
      <div class="igts-tag-values">
        <span class="igts-tag-value current" style="background:#10b981; color:#000">post</span>
        <span class="igts-tag-value">approve</span>
        <span class="igts-tag-value">read</span>
        <span class="igts-tag-value">create</span>
        <span class="igts-tag-value">delete</span>
        <span class="igts-tag-value">configure</span>
      </div>
    </div>

    <div class="igts-tag-detail" data-idx="5" style="border-color:#14b8a6">
      <h4>Data Class</h4>
      <p>The type of data accessed. Feeds data governance, privacy controls, and DORA article obligations around data confidentiality.</p>
      <div class="igts-tag-values">
        <span class="igts-tag-value current" style="background:#14b8a6; color:#000">financial</span>
        <span class="igts-tag-value">pii</span>
        <span class="igts-tag-value">internal</span>
        <span class="igts-tag-value">public</span>
        <span class="igts-tag-value">operational</span>
      </div>
    </div>

    <div class="igts-tag-detail" data-idx="6" style="border-color:#06b6d4">
      <h4>Lifecycle</h4>
      <p>Duration of the access grant. Temporary and emergency access require tighter review cycles and automatic expiry enforcement.</p>
      <div class="igts-tag-values">
        <span class="igts-tag-value current" style="background:#06b6d4; color:#000">permanent</span>
        <span class="igts-tag-value">temporary</span>
        <span class="igts-tag-value">emergency</span>
      </div>
    </div>
  </div>

  <p style="margin-top:1.5rem">
    By adopting this standard, the source of truth is embedded within the entitlement itself. The IGA engine no longer needs to guess or rely on manual mappings. Classification becomes deterministic.
  </p>
</div>

<div class="igts-section">
  <h2>Why This Unlocks SoD</h2>
  <p>
    With IGTS tags flowing into the IGA platform, the SoD evaluation engine transforms from a complex web of custom rules into a stable, highly efficient conflict matrix based strictly on <strong>domain</strong>, <strong>process</strong>, and <strong>function</strong>.
  </p>
  <p>
    If a user holds <code>finance/accounts-payable/.../post/...</code> and requests <code>finance/accounts-payable/.../approve/...</code> — the engine flags it instantly. No custom rules. No manual lookup. Just tag comparison.
  </p>

  <div class="igts-comparison">
    <div class="igts-comparison-box igts-comparison-before">
      <h4>Before IGTS</h4>
      <ul class="igts-comparison-list">
        <li>Tens of thousands of SoD rules</li>
        <li>Manual entitlement classification</li>
        <li>Rule logic owned by individuals</li>
        <li>Brittle on system/schema changes</li>
        <li>Low audit confidence</li>
        <li>LLM inference as last resort</li>
      </ul>
    </div>
    <div class="igts-comparison-box igts-comparison-after">
      <h4>After IGTS</h4>
      <ul class="igts-comparison-list">
        <li>Clean domain × function conflict matrix</li>
        <li>Tags embedded at source creation</li>
        <li>Logic owned by the standard</li>
        <li>Resilient to system changes</li>
        <li>High audit confidence</li>
        <li>LLM validates tags, not guesses</li>
      </ul>
    </div>
  </div>
</div>

<div class="igts-section">
  <h2>DORA: Five Gaps to Bridge</h2>
  <p>
    The Digital Operational Resilience Act (DORA) introduces strict new requirements for financial entities in the EU. Standard IGA practices, optimised for SOX compliance, fall short in five key areas. Here is how IGTS can be extended to bridge each gap.
  </p>

  <div class="igts-dora-cards">
    <div class="igts-dora-card">
      <div class="igts-dora-num">1</div>
      <h4>Resilience Dimension (RTO/RPO)</h4>
      <p>DORA prioritises business continuity. A system might be out of SOX scope but still control critical banking infrastructure — it must be tagged for resilience tier so IGA can enforce stricter review cycles.</p>
      <div class="igts-dora-ext">;rto=tier1</div>
    </div>

    <div class="igts-dora-card">
      <div class="igts-dora-num">2</div>
      <h4>Third-Party & Vendor Concentration Risk</h4>
      <p>DORA Pillar IV demands strict management of ICT third-party risk. If an identity grants admin rights to a Critical Third-Party Provider (CTPP) or external MSP, that dependency must be explicit in the tag.</p>
      <div class="igts-dora-ext">;vendor=azure;ctpp=yes</div>
    </div>

    <div class="igts-dora-card">
      <div class="igts-dora-num">3</div>
      <h4>Elevating 'Read' Permissions</h4>
      <p>In standard SoD, <code>function=read</code> is low risk. Under DORA, read-only access to network topologies, IAM policies, or backup configs is a critical reconnaissance asset. The IGA engine must treat <code>read</code> on <code>sensitivity=critical</code> with elevated monitoring — a policy change, not a tag extension.</p>
    </div>

    <div class="igts-dora-card">
      <div class="igts-dora-num">4</div>
      <h4>Non-Human Identities (NHIs)</h4>
      <p>DORA covers service accounts, CI/CD pipelines, and API integrations. The base <code>lifecycle</code> segment is human-centric. NHIs need explicit typing so IGA can apply appropriate governance, rotation policies, and audit trails.</p>
      <div class="igts-dora-ext">;identity-type=service-account</div>
    </div>

    <div class="igts-dora-card">
      <div class="igts-dora-num">5</div>
      <h4>Tag Integrity & Configuration Drift</h4>
      <p>IGTS tags reside in mutable fields — AD descriptions, Kubernetes annotations, database notes. A malicious actor could alter a tag to evade compliance detection. The IGA platform must cross-reference tag changes against SIEM and audit logs. Trust but verify.</p>
    </div>
  </div>
</div>

<div class="igts-section">
  <h2>Community Questions</h2>
  <p>IGTS is early-stage. These questions are worth debating with your team and across the IGA community. If you have an answer, an experience, or a challenge — <a href="https://github.com/mimx/mimx.github.io/issues/new?labels=igts,discussion&title=IGTS+Discussion%3A+">open a GitHub issue</a> and let's build the body of knowledge together.</p>

  <div class="igts-questions">
    <div class="igts-question">
      <span class="igts-question-mark">Q</span> Does your IGA platform support custom metadata fields for DORA extensions like <code>;rto=tier1</code> or <code>;vendor=azure;ctpp=yes</code>? How do you store them today?
      <div style="margin-top:0.75rem"><a href="https://github.com/mimx/mimx.github.io/issues/new?labels=igts,discussion&title=IGTS+Q%3A+DORA+metadata+fields&body=**Question%3A**+Does+your+IGA+platform+support+custom+metadata+fields+for+DORA+extensions%3F%0A%0A**My+experience%3A**%0A" style="font-size:0.82rem; color:#818cf8; font-style:normal">→ Share your answer on GitHub</a></div>
    </div>
    <div class="igts-question">
      <span class="igts-question-mark">Q</span> How do you handle tag integrity when entitlements live in mutable AD Description fields or freeform spreadsheet notes? What's your drift detection approach?
      <div style="margin-top:0.75rem"><a href="https://github.com/mimx/mimx.github.io/issues/new?labels=igts,discussion&title=IGTS+Q%3A+Tag+integrity+%26+drift&body=**Question%3A**+How+do+you+handle+tag+integrity+in+mutable+fields%3F%0A%0A**My+approach%3A**%0A" style="font-size:0.82rem; color:#818cf8; font-style:normal">→ Share your answer on GitHub</a></div>
    </div>
    <div class="igts-question">
      <span class="igts-question-mark">Q</span> What's your approach to classifying NHIs — service accounts, CI/CD tokens, API keys — in the IGTS taxonomy? Same segments, or a parallel namespace?
      <div style="margin-top:0.75rem"><a href="https://github.com/mimx/mimx.github.io/issues/new?labels=igts,discussion&title=IGTS+Q%3A+Non-human+identity+classification&body=**Question%3A**+How+do+you+classify+NHIs+in+the+IGTS+taxonomy%3F%0A%0A**My+approach%3A**%0A" style="font-size:0.82rem; color:#818cf8; font-style:normal">→ Share your answer on GitHub</a></div>
    </div>
    <div class="igts-question">
      <span class="igts-question-mark">Q</span> Are your auditors asking for IGTS-style structured evidence yet? What does that conversation look like?
      <div style="margin-top:0.75rem"><a href="https://github.com/mimx/mimx.github.io/issues/new?labels=igts,discussion&title=IGTS+Q%3A+Auditor+evidence+requests&body=**Question%3A**+Are+auditors+asking+for+IGTS-style+evidence%3F%0A%0A**My+experience%3A**%0A" style="font-size:0.82rem; color:#818cf8; font-style:normal">→ Share your answer on GitHub</a></div>
    </div>
  </div>
</div>

<div class="igts-footer-links">
  <a href="https://mimx.org/posts/iga-compliance-sod-deep-dive/">Read the full SoD deep dive →</a>
  <a href="https://mimx.org/posts/igts-identity-governance-tagging-standard/">Read the full IGTS specification →</a>
</div>

<script>
(function () {
  var pills   = document.querySelectorAll('.igts-tag-pill');
  var details = document.querySelectorAll('.igts-tag-detail');

  function activate(idx) {
    pills.forEach(function (p) { p.classList.remove('active'); });
    details.forEach(function (d) { d.classList.remove('active'); });
    var pill   = document.querySelector('.igts-tag-pill[data-idx="' + idx + '"]');
    var detail = document.querySelector('.igts-tag-detail[data-idx="' + idx + '"]');
    if (pill)   pill.classList.add('active');
    if (detail) detail.classList.add('active');
  }

  pills.forEach(function (pill) {
    pill.addEventListener('click', function () {
      activate(this.getAttribute('data-idx'));
    });
  });

  // Start with first segment active
  activate('0');
})();
</script>
