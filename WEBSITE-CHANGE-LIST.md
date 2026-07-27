# Website & Referrer Community — Change List

Captured 2026-07-27 from Bryan. Working list — tick items as they ship.

## Main website

### Design / structure
- [ ] Telehealth Preview full-screen window needs adjusting (decided: adjust the window, not a video)
- [ ] Sleeker mobile menu design
- [x] ~~"For Referring Doctors" menu item~~ — **decided: no, keep relying on printed cards + personal connections**
- [x] ~~Cleveland live now~~ — **decided: still "coming soon."** Renamed to "Ramsey Surgical Clinic, Cleveland" on the booking form, visible but not selectable.

### Copy changes
- [ ] **Why choose us:** "We welcome clear communication and questions as it supports informed decisions, and collaboration with your other health care providers and partnership throughout your surgical journey."
- [ ] **How do I get started:** remove the email (it isn't listed); change "contact us" to a hyperlink on *online form*
- [ ] **Three Simple Steps** (welcome area), step 3 becomes: "Call or connect with us for an appointment and get an email or SMS confirmation with any details you may need."
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
- [ ] Patient forms page → pay invoice button page: "back to website" links not yet checked.
- [x] Every "Book Online" / HealthEngine link and the "click here to book" link across index.html and welcome.html now point to the real flow instead of a placeholder HealthEngine plugin URL.

### Contact section — decided: moved to Supabase
- [x] Simplified to First & Last name, Email, Phone, Question (dropped the procedure-type dropdown).
- [x] **Share Your Schedule** added: Mon–Fri with a free-text time field beside each, note "Help us coordinate a time we can connect."
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

## Referrer community page (refer.html)

- [ ] A5 printable form — fine-tune, link it, and make the preview viewable on mobile
- [ ] Build a tutorial walking through the referral portal's steps and features, and how it connects to the clinic

### Copy — Availability Notices
Get notified when new appointment availability opens, including any new consulting locations — so your patients can be seen promptly. Or when Dr Raymond is on leave.

### Copy — how referrals work
Referrals may be sent through your personal referral portal — from our app or computer logon. If you set up more than one location or practitioner you can choose them from the drop down. This will fill out your practice and provider number details automatically. Please include your patient's relevant history, imaging, and your clinical question or concern. Dr Anna gets access to your notes directly through her app.

### Copy — Your portal
**Built for colleagues & community.**
We built a dedicated space just for the practitioners who refer to us. Your own login, your own dashboard — with a direct line to Dr Anna & staff; to help invite and support more intentional connection.

**Hear from Dr Anna**
A vision to include updates, new availability, and service announcements which come to you through your portal — not a newsletter. Connect through Coviu to join advanced video calls for complex cases with tools, charts, & scans — right from your dashboard.

**Your own dashboard**
See the referrals you've recently sent, and direct history with the clinic — personalised to you.

**Make it yours.**
Your portal isn't a generic tool — it's a space built around you. Choose your accent colour, add your practice logo alongside ours, and set how you'd like to be greeted.

### Copy — Join the referrer community
Create your account and get your own portal — a personal space to stay connected with Dr Anna, send referrals digitally, and track your patients' care. With our free app — your office can use a phone or any tablet for easy filing and sending of your referrals — or they may even be finished and sent from a laptop or desktop. These reach our dedicated secure inbox, and can be seen by Dr Anna herself through her app.

---

## Decisions (answered 2026-07-27)
1. Telehealth preview → adjust the window (not a video). Still to build.
2. "For Referring Doctors" menu item → no, keep it private.
3. Cleveland → still "coming soon." Named and ready, not selectable.
4. Contact form → moved to Supabase. Done.
5. Detailed booking page (wisdomsurgery.me) → yes, switched. Done, and it was silently broken before this — see Booking flow above.
