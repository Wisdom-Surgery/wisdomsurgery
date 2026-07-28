# Website & Referrer Community — Change List

Captured 2026-07-27 from Bryan. Working list — tick items as they ship.

## Main website

### Design / structure
- [x] Telehealth Preview full-screen window fixed — root cause was a `transform`/`backdrop-filter` containing-block bug, not a sizing issue. Full-screen toggle now fills the real viewport on desktop and mobile.
- [x] Mobile menu redesigned as a full-screen overlay — "Your Journey" collapses into a closed-by-default accordion, Book Online/Pay Invoice pinned to a sticky bottom bar.
- [x] ~~"For Referring Doctors" menu item~~ — **decided: no, keep relying on printed cards + personal connections**
- [x] ~~Cleveland live now~~ — **decided: still "coming soon."** Renamed to "Ramsey Surgical Clinic, Cleveland" on the booking form, visible but not selectable.

### Copy changes
- [x] **Why choose us:** "We welcome clear communication and questions as it supports informed decisions, and collaboration with your other health care providers and partnership throughout your surgical journey."
- [x] **How do I get started:** email removed; hyperlink moved from "contact us" to *online form*, pointing to wisdomsurgery.me.
- [x] **Three Simple Steps** (welcome area), step 3 now reads: "Call or connect with us for an appointment / Get an email or SMS confirmation with any details you may need."
- [x] Switched to the detailed booking page (wisdomsurgery.me/index.html) as the primary flow — see below.

### Booking flow — decided: switched to wisdomsurgery.me/index.html
- [x] **Real functional fix:** the form was building its full payload, logging it to the browser console, and showing a fake success message. Nothing was ever sent anywhere — no patient's booking request ever reached the practice. Now writes to Supabase (`appointment_requests`), uploads the referral file to a private bucket, and emails the practice via a new `notify-appointment-request` edge function. Verified end-to-end with real REST calls.
- [x] Page one already has no postcode field — nothing to remove.
- [x] "Preferred days" was already positioned under "Preferred time of day" — already correct.
- [x] Notes placeholder rewritten to Bryan's copy.
- [x] Location 3 renamed "Ramsey Surgical Clinic, Cleveland" — marked coming-soon per the decision above.
- [x] Medicare number → **Doctor's Provider Number**.
- [x] Referral letter upload copy rewritten to Bryan's copy.
- [x] Full AU private health fund list (29 funds) + cover-type dropdown added, matching the short intake form.
- [x] Patient forms page → pay invoice button page: "back to website" links fixed. `payment.html` lives on wisdomsurgery.me, so its relative `index.html` links were sending patients to the booking flow instead of the real main site — repointed to `https://wisdomsurgery.clinic`.
- [x] Every "Book Online" / HealthEngine link and the "click here to book" link across index.html and welcome.html now point to the real flow instead of a placeholder HealthEngine plugin URL.
- [x] Booking wizard no longer scrolls to page top on each step — now holds position at the form.

### Contact section — decided: moved to Supabase
- [x] Simplified further per later direction: Name, Email, Phone only — Question, Share Your Schedule, and file upload all removed.
- [x] Formspree retired entirely. This removed the only thing emailing the practice on submit, so a `notify-contact-submission` edge function was added and deployed — without it, moving to Supabase would have made every enquiry go silent. Verified end-to-end with real REST calls.

## Policies

### Telehealth policy (new)
Before your first Telehealth consultation, we ask for your consent to the Telehealth call. We ask for AI consent, which we use to help us concentrate clinical notes and important planning information.

### Financial policy — Surgical Fee section (replace)
The surgeon's fees reflect Dr Anna Raymond's specialist surgical services, and (if applicable) those of her surgical assistant. There are 2 types of item numbers: 3-digit (Dental/ADA) and 5-digit (Medicare/MBS).

Please be aware that only 5-digit Medicare item numbers attract a Medicare rebate. You may be eligible for Private Health Fund rebates for this procedure and we strongly advise you to check with your Health Fund: please confirm the 3-digit item numbers with your Dental/Extras insurer, and the 5-digit item numbers with your Hospital insurer, and make a note of the available rebates.

Payment of the surgeon's fee is required before surgery. Your booking can only be secured once payment has been received. Occasionally, an operating list may become fully booked before payment is made. If this occurs, we will offer you the next available surgical date and, if you wish, place you on a cancellation waitlist for your preferred date. After your procedure, a receipt will be provided for you to submit to your Private Health Insurer, to facilitate reimbursement of any eligible rebates.

Quotations are valid for a period of six (6) months from the date issued. Please do not hesitate to contact Wisdom Surgery Clinic if you have questions about your Fee Estimate. Thank you for trusting us with your care. Trust is our highest compliment, and we are honoured to be your partner in specialist surgical care.

**Other Possible Costs**
Please note that other possible costs can include pharmacy, pathology and radiology items, which are billed separately by each provider.

### Financial policy — Consultation section (append)
In some circumstances, your consultation may be bulk billed. From 1 July 2026, Medicare requires all patients receiving a bulk-billed service to complete a Medicare Assignment of Benefit (AOB) form, either before or after their appointment.

Where applicable, we will send you a secure electronic link to complete this form. If you prefer not to sign the AOB form, we will ask you to pay for your consultation on the day of your appointment, after which you can claim your eligible Medicare rebate directly from Medicare.

### Public System Alternative (new)
For some procedures, treatment may also be available through the public hospital system at no out-of-pocket cost, although waiting times may apply. If you would like information about public system options for your condition, Dr Raymond or our team can discuss this with you during your consultation. Please note, Dr Raymond does not operate through the public system, so onward referral of care would be required.

### Overdue Accounts (new)
Accounts unpaid after the due date may incur late payment charges. Outstanding accounts referred to a debt collection agency may attract additional recovery costs, including debt collection and legal fees, which will be added to the outstanding balance.

We encourage you to contact us promptly if you have any difficulty meeting payment obligations. We value your health and that includes financial and mental well being we may help support. Please contact us about payment plans if you need assistance.

### Cancellation — Repeated Cancellations (new)
Where a patient repeatedly cancels or reschedules appointments, we may, at our discretion, request full pre-payment of fees before a further appointment is scheduled.

### Aftercare policy — After your procedure (new)
Written post-operative care instructions will be provided to you before you leave following any surgical procedure, or messaged to you directly. Please read these carefully — they include information about pain management, wound care, diet, activity restrictions, and what to expect during your recovery.

---

## Referrer community page (refer.html) — done

- [x] A5 printable form — real source PDF (`forms/Wisdom-Surgery-Referral-Pad-A5.pdf`) embedded as an inline preview, with Download and "Open & print" actions. Prints pixel-accurate since it's the actual PDF, not an HTML recreation. Responsive height for mobile.
- [x] Tutorial section ("Using your portal") — 5-step walkthrough from login to Dr Anna receiving the referral.
- [x] Availability Notices card, "How referrals work" copy, new "Your Portal" section (4 subsections), and "Join the referrer community" registration copy all applied as drafted below.

---

## Decisions (answered 2026-07-27)
1. Telehealth preview → adjust the window (not a video). Still to build.
2. "For Referring Doctors" menu item → no, keep it private.
3. Cleveland → still "coming soon." Named and ready, not selectable.
4. Contact form → moved to Supabase. Done.
5. Detailed booking page (wisdomsurgery.me) → yes, switched. Done, and it was silently broken before this — see Booking flow above.
