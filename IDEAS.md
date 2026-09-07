# Website ideas

This is the reading file for the website: a single place to capture ideas, improvements, promotions, and anything else worth trying.

Add new notes at the **top** of a section. Leave unfinished thoughts in. A rough line is better than a lost idea.

When you have something to save:

1. Open this file
2. Drop the idea under the right heading (or under **Inbox** if you are not sure)
3. Commit and push

---

## Priority order

**When you resume work, follow this list in order.**

1. **Owner email after a purchase** — After a user buys anything, send me an email. Subject: a short summary of that user. Body: did they get the email, could they download the `.zip` or `.pdf`, does the email have an attachment, all of that in one place.

   Build prompt (copy this when you start work): [Priority 1 — detailed prompt](#priority-1--detailed-prompt-copy-this)

2. **Sign-off interview section** — Add a new interview section on the site called Sign-off, with around 100 questions.

   Full notes: [New pages and features](#new-pages-and-features)

---

## About the website

Fill this in so every idea has context.

- **Name:**
- **URL:**
- **One-line purpose:** What this site is for, in one sentence
- **Who it is for:**
- **What a visitor should do:** Sign up, buy, read, contact, something else
- **Current status:** Idea / draft / live

### What exists today

Pages, features, and content already on the site:

-

### What is not on the site yet

Known gaps, pages you still want, or things you keep meaning to add:

- **Priority 1:** Owner purchase-status email after a sale (see **Priority order**)
- **Priority 2:** Sign-off interview section with around 100 questions (see **Priority order**)

---

## Inbox

Dump anything here first. Sort it into a section later if you want.

<!-- Newest on top -->

- 

---

## Betterment

Improvements to what already exists: clearer copy, faster pages, easier navigation, fewer steps, fewer bugs.

<!-- Newest on top -->

- 

---

## New pages and features

Things the site does not do yet.

<!-- Newest on top -->

- **Priority 2 — 2026-09-06 — Sign-off interview section (~100 questions)**

  **Do this second when work resumes.**

  Add one new interview section on the website: **Sign-off**. Put around 100 questions in that section.

- **Priority 1 — 2026-09-06 — Owner email after a purchase, with a user summary and delivery status**

  **Do this first when work resumes.**

  Once a user buys anything from the website, I also get a mail. The subject should be a short summary of that user (who they are / what they bought).

  The mail body should tell me:

  - Did the user get the email? (was their purchase / download email delivered)
  - Was the user able to download the `.zip` file or the `.pdf`?
  - Does the email contain an attachment?
  - All of the information above in one place, so I can see the full picture without hunting

  Detailed prompt: [Priority 1 — detailed prompt](#priority-1--detailed-prompt-copy-this)

---

## Priority 1 — detailed prompt (copy this)

Use this as the full brief when you resume work. Do this before priority 2. Cover the flow end to end and check every status below. Do not ship until the owner mail answers every question in one place.

```
You are implementing PRIORITY 1 on my website. Do this first. Do not work on the Sign-off interview section (priority 2) until this is done.

GOAL
After a customer buys anything on the website, two things must happen:

1. The customer receives a purchase / delivery email for what they bought, including the digital files they paid for (.zip and/or .pdf), as an attachment and/or a download link.
2. I (the site owner) receive a separate status email about that same purchase. I should not have to open logs, the admin panel, or the customer's inbox to know what happened.

Owner email — subject
The subject must be a short summary of the user and the purchase, readable in a mailbox list without opening the mail. Include enough to identify the person and the order at a glance, for example:
- customer name (or email if no name)
- order id
- product name
- date/time of purchase
Keep it one line. Example shape:
[Purchase] Jane Doe · Order 1842 · STA Interview Pack · 2026-09-07 16:20 UTC

Owner email — body (all of this in ONE email, every time)
I must be able to scan the body and answer these questions without hunting:

A. Who bought, and what?
   - Customer name
   - Customer email
   - Order id
   - Product(s) bought
   - Amount paid and currency
   - Payment status (paid / failed / refunded / pending)
   - Purchase date and time (with timezone)
   - Billing or checkout details that identify the user if name is missing

B. Did the user GET the email?
   Report delivery of the CUSTOMER's purchase/download email, not mine.
   For that customer email, state clearly:
   - Was it sent? yes / no, and timestamp
   - To which address
   - Provider message id if we have one
   - Delivery status: queued / sent / delivered / bounced / delayed / failed / unknown
   - Bounce or failure reason if it failed
   - Opened? yes / no / unknown, and timestamp if yes
   If we cannot know a field, write "unknown" and why (for example "provider does not report opens"). Never leave the field off the mail.

C. Does the customer email CONTAIN an attachment?
   - Attachment present? yes / no
   - File names
   - File types (.zip, .pdf, other)
   - File sizes
   - If there is no attachment and we used a download link instead, say that clearly
   - If attachment was expected but missing, say FAILED and why

D. Was the user able to DOWNLOAD the .zip and/or the .pdf?
   Treat .zip and .pdf as separate checks. For each expected file:
   - Expected? yes / no
   - How they get it: attachment, link, or both
   - Download (or equivalent access) succeeded? yes / no / not yet / unknown
   - Timestamp of first successful download, if any
   - Number of download attempts
   - Link expired, broken, or unauthorized? yes / no
   - Error if download failed
   If the product only has one file type, still show both rows and mark the other "not applicable".

E. One-line health summary at the top of the body
   A single status I can trust, such as:
   - ALL OK — customer email delivered, attachment present, zip downloaded, pdf downloaded
   - WAITING — email delivered, files not downloaded yet
   - PROBLEM — email bounced / attachment missing / download failed
   Put this line first in the body, then the details.

WHEN THIS RUNS
Trigger only after a successful paid purchase (payment captured). Do not send me a "success" owner mail for abandoned carts or failed payments.
If payment fails, I still want a PROBLEM owner mail only if we already tried to send the customer files; otherwise skip or send a short "payment failed, no files sent" note. Be consistent and document the rule in the implementation.

END-TO-END FLOW YOU MUST BUILD AND VERIFY
1. Customer completes checkout and payment succeeds.
2. Order is stored with customer identity, product, amount, and timestamps.
3. System prepares the correct .zip and/or .pdf for that product.
4. Customer email is composed:
   - correct recipient
   - useful subject
   - body they can understand
   - attachment(s) if that is how we deliver
   - working download link(s) if that is how we deliver
   - files match the product they bought
5. Customer email is sent through the real mail path (not a fake log line).
6. Delivery status is recorded (sent, delivered, bounced, failed).
7. Attachment presence is recorded from what was actually sent, not from what we intended.
8. Each download of .zip and .pdf is recorded (success, fail, time).
9. Owner email is composed with the subject summary and the full status body above.
10. Owner email is sent to my address.
11. If status changes later (for example they download an hour after purchase), send me an update owner email OR a single follow-up when the first download happens. Do not leave me with only the "not yet" snapshot. State which approach you implemented.

CHECK EVERYTHING — ACCEPTANCE
A purchase is not done until you can demonstrate all of the following with a real test order:

Happy path
- [ ] Pay for a product that includes both .zip and .pdf
- [ ] Customer receives the email
- [ ] That email has the expected attachment(s), or a working download link if that is the design, and the mail body says which
- [ ] Opening the attachment / using the link actually yields the correct .zip and .pdf (not empty, not wrong product, not expired)
- [ ] Downloading .zip is recorded
- [ ] Downloading .pdf is recorded
- [ ] I receive the owner email
- [ ] Subject identifies the user and the order without opening it
- [ ] Body answers: did they get the email? attachment present? zip downloaded? pdf downloaded?
- [ ] Health line at the top is ALL OK after both files are downloaded

Customer email problems
- [ ] Wrong or bouncing customer address → owner mail says they did NOT get the email, with the bounce reason, health line PROBLEM
- [ ] Mail provider outage / send failure → owner mail says not sent, with error, health line PROBLEM
- [ ] Email sent but attachment missing when it should be there → owner mail says attachment NO, health line PROBLEM
- [ ] Download link 404, expired, or unauthorized → owner mail says download failed for that file, health line PROBLEM
- [ ] Customer never downloads → first owner mail says "not yet" / WAITING, not ALL OK

File and product problems
- [ ] Product that is zip-only: pdf row is "not applicable"; zip still checked
- [ ] Product that is pdf-only: zip row is "not applicable"; pdf still checked
- [ ] Wrong file attached (other product's zip/pdf) is a FAIL
- [ ] Empty or corrupt file is a FAIL
- [ ] Large attachment that the provider rejects is reported on the owner mail

Order and identity problems
- [ ] Guest checkout still produces a usable subject (email if no name)
- [ ] Duplicate submit / double webhook does not send me duplicate "success" mails for the same order unless status actually changed
- [ ] Refund or failed capture does not look like a successful delivery on my mail
- [ ] Two different customers in a row: each owner mail is about the right person, right order, right files

Owner-mail problems
- [ ] Owner mail itself must actually arrive at my inbox (test it, not only log it)
- [ ] If owner mail fails to send, log it loudly; still keep the customer delivery working
- [ ] No secrets in the owner mail (no full card numbers, no raw payment tokens, no download URLs that would let a forwarded mail steal the files unless that is already how the product works)

WHAT "CHECK" MEANS
Do not mark this complete from code reading alone. Run the real path:
- real checkout (or the closest staging checkout)
- real outbound email
- open the customer message and confirm attachment / link
- download the .zip
- download the .pdf
- open my owner message and confirm subject + every status field
Record in the PR or notes: screenshots or copied headers/subjects, order id, timestamps, and pass/fail for each checkbox above.

OUT OF SCOPE FOR THIS TASK
- Sign-off interview section and the ~100 questions (priority 2)
- Unrelated redesign, promotions, or new product pages unless they are required to send the files

IF SOMETHING IS UNKNOWN
Build the mail so unknown is visible ("unknown — reason"), not silent. Prefer honest WAITING / PROBLEM over a fake ALL OK.
```

---

## Promotions and growth

Launches, campaigns, social posts, email, SEO, partnerships, ads, referrals.

<!-- Newest on top -->

- 

---

## Design and writing

Look, layout, tone, headlines, photos, branding.

<!-- Newest on top -->

- 

---

## Questions

Decisions you have not made yet. Write the question; add an answer when you have one.

<!-- Newest on top -->

- 

---

## Log

Dated notes so you can see how thinking changed.

### 2026-09-07

- Wrote a detailed end-to-end prompt for **priority 1** (owner email after purchase): subject summary, customer delivery, attachment, zip/pdf download, health line, and a full check list.

### 2026-09-06

- Added **priority 2**: a Sign-off interview section on the website with around 100 questions. Do this after priority 1.
- Marked the owner purchase-status email as **priority 1**: when work resumes, do this first.
- Added feature: after a purchase, send me an email whose subject summarizes the user, and whose body reports whether they got the email, whether they could download the `.zip` or `.pdf`, and whether that email had an attachment.
- Created this file as the place to push website ideas.
