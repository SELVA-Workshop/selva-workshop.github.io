# Workshop Information Update Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Publish the finalized SELVA workshop schedule, complete ordered reviewer list, and confirmed speaker roster.

**Architecture:** Keep the existing single-page static site and edit only its content-bearing HTML. Validate behavior with temporary Python assertions against the rendered source, avoiding new dependencies and permanent test infrastructure for a one-time data update.

**Tech Stack:** HTML5, Python 3 standard library, Git

---

### Task 1: Update the workshop page

**Files:**
- Modify: `index.html:283-430`
- Reference: `../info/ACL-SELVA 2026 Program Committee Status.csv`

- [ ] **Step 1: Run failing schedule and speaker assertions**

Run:

```bash
python3 -c 'from pathlib import Path; h=Path("index.html").read_text(); assert "<section id=\"schedule\">" in h; assert "10:20–10:30" in h; assert "Awarding &amp; Closing" in h; assert "Kurt Keutzer" not in h'
```

Expected: FAIL because the schedule section is commented out, finalized entries are absent, and Kurt Keutzer is still listed.

- [ ] **Step 2: Run failing committee assertions**

Run:

```bash
python3 -c 'from pathlib import Path; h=Path("index.html").read_text(); names=["Abdulhamid Abubakar","Zhongzhu Zhou","Shikhar Shiromani","Khalid Shaikh"]; assert all(n in h for n in names); assert h.index("Abdulhamid Abubakar") < h.index("Zhongzhu Zhou") < h.index("Shikhar Shiromani") < h.index("Khalid Shaikh"); section=h[h.index("<section id=\"committee\">"):h.index("<section id=\"papers\">")]; assert "Weilun Feng" not in section'
```

Expected: FAIL because the CSV-derived committee has not been added.

- [ ] **Step 3: Replace the schedule template**

Uncomment the schedule section, set its title to `Workshop Schedule`, remove the template lead, and use this table body:

```html
<tbody>
  <tr><td>09:00</td><td>Opening</td><td>Welcome remarks</td></tr>
  <tr><td>09:00–09:20</td><td>Ruihao Gong</td><td>Online</td></tr>
  <tr><td>09:20–09:40</td><td>Michele Magno</td><td>Online</td></tr>
  <tr><td>09:40–10:00</td><td>Priya Panda</td><td>Online</td></tr>
  <tr><td>10:00–10:10</td><td>Oral Paper 1</td><td>Video</td></tr>
  <tr><td>10:10–10:20</td><td>Oral Paper 2</td><td>Video</td></tr>
  <tr><td>10:20–10:30</td><td>Break</td><td></td></tr>
  <tr><td>10:30–10:50</td><td>Huanrui Yang</td><td>Video</td></tr>
  <tr><td>10:50–11:10</td><td>Yukang Chen</td><td>Online</td></tr>
  <tr><td>11:10–11:20</td><td>Oral Paper 3</td><td>Video</td></tr>
  <tr><td>11:20–11:30</td><td>Oral Paper 4</td><td>Video</td></tr>
  <tr><td>11:30</td><td>Awarding &amp; Closing</td><td>Best Paper Award</td></tr>
</tbody>
```

- [ ] **Step 4: Update the speaker section**

Use the heading and lead below, and delete the complete Kurt Keutzer `<article>` card while preserving the other five cards:

```html
<h2 class="section-title">Invited Speakers</h2>
<p class="section-lead">
  Invited speakers for the SELVA Workshop.
</p>
```

- [ ] **Step 5: Replace the committee section**

Set the title to `Program Committee (Reviewers)`, the lead to `We thank the following reviewers for their service.`, and render two existing grid columns. Place these 40 assigned reviewers in the first list, preserving order:

```text
Abdulhamid Abubakar; Badal Nyalang; Yash Ganpat Sawant; Tanmoy Debnath; Yuze Lv; Jiakai Wang; Bogdan Bogachov; Yulong Cao; Yisong Xiao; Himanshu Mishra; Toprak Seda Karaosmanoğlu; Aryan Arya; Mingyuan Zhang; Keshav Gupta; Jun Guo; Awais Rauf; Xiaoxia Wu; Jiaming Qu; Ziyan Chen; Diksha Agarwal; Mohan Premchand Bhambhani; Madhu Gopinathan; Michael Cacioli; Yan Wang; Duorui Wang; Tianlin Li; Ruihao Gong; KOTTILINGAM KOTTURSAMY; Ioannis Tzachristas; Jisen Li; Jiachen Sun; Guoxin Fan; Donglin Zhuang; Sávio Salvarino Teles de Oliveira; Md. Shahriar Karim; Xiaokun Liu; Mingqiang Wu; Zizhong Li; Hang Yu; Zhongzhu Zhou
```

Place these 16 unassigned reviewers in the second list, preserving order:

```text
Shikhar Shiromani; Jan Dobrosolski; Tianbo Wang; Shayan Ali Akbar; Shreya Rajpal; Michal Golovanevsky; Ruikui Wang; cliu@cmri.org.au; Xiaowei Zhao; Yuqing Ma; Jingzhi Li; Marshiat Mithe Syed; Nikita Letov; Anzheng Wang; Yaoyao Fiona Zhao; Khalid Shaikh
```

Each semicolon-delimited name becomes one `<li>` element. Do not include Weilun Feng or affiliations.

- [ ] **Step 6: Run content and markup verification**

Run both assertions from Steps 1 and 2 again. Expected: both PASS with exit code 0.

Run:

```bash
python3 -c 'from html.parser import HTMLParser; from pathlib import Path; p=HTMLParser(); p.feed(Path("index.html").read_text()); p.close()'
git diff --check
git diff -- index.html
```

Expected: HTML parser exits 0, `git diff --check` exits 0, and the diff contains only the requested schedule, speaker, and committee edits.

- [ ] **Step 7: Commit the implementation**

```bash
git add index.html
git commit -m "Update workshop schedule and participants"
```
