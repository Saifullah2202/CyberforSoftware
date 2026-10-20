# Week 2 security requirements

Your name: Malik Saif Ullah
Date: 10/13/2026

Fill this in as you work, rather than at the end. Where you are unsure, write
that you are unsure and say why. A sentence you can support is worth more than a
confident one you cannot.

---

## 1. What this application is

Three or four sentences, in your own words, describing what Atrium does and who
uses it.

Atrium is a locally run workspace for an organisation's staff. Signed-in colleagues can use its staff directory, profile page, and shared work documents; an administrator also has access to an admin users page. The directory lists names, email addresses, departments, and roles, while the profile page displays account details and a form for editing profile fields. Its seeded accounts are fictional and are intended for use in this local training application.

**Where the assistant's explanation did not match the application.** Anything you
checked and found different, however small. Write "nothing found" if that is the
honest answer.

I found differences between the draft's description and the code. The code calls ordinary accounts `member` rather than `staff`. Any signed-in user can see the directory and shared resources; the profile page displays the signed-in user's account, but the code does not establish that profile updates are restricted to that user. The admin page is restricted to administrators, but I found no account-role editing controls in it. I checked the route guards and handlers; these observations describe the code, not a claim that I tested every possible runtime behavior.

## 2. What is worth protecting

Four assets. For each one, say what it is and what it would cost if it were seen,
changed or unavailable. Write the cost so that somebody outside the team could
understand it.

| Asset | What it costs if this goes wrong |
|---|---|
| Staff directory and profile data | This includes names, email addresses, departments, bios, and role information. If it were exposed, staff could face phishing, impersonation, and privacy issues; if it were changed, people could be misidentified or miscontacted; if it were unavailable, staff could not quickly find who does what in the organisation. |
| Shared documents and work files | Atrium contains operational and finance documents such as expense claim forms, supplier records, and budget packs. If these were seen by the wrong people, confidential business information would leak; if edited, the organisation could make poor decisions or lose money; if unavailable, day-to-day work could be delayed and important tasks could be missed. |
| Sign-in credentials and access control | Accounts such as Alice, Bob, and Morgan are the gateway to Atrium. If passwords or roles were exposed or changed, an attacker could impersonate staff or take over administrator access; if access failed, staff would be locked out of the systems they rely on. |
| Administrator permissions and user management | The administrator account controls who can view, edit, and manage data. If admin privileges were abused or changed, a malicious actor could alter staff access, document ownership, and internal records, causing much wider disruption across the organisation. |

## 3. The requirements

Four sentences, in your own words. Each one should say what is not allowed and to
whom.

1. No person who is not signed in to Atrium may view the staff directory, profile pages, or shared documents, because this system is for internal staff use only.
2. No staff member may view or change another colleague's profile-only information, such as their biography, unless they are that colleague or an authorised administrator; signed-in colleagues may see the business contact details deliberately listed in the directory.
3. No user may create, alter, delete, or reassign shared documents unless they own the document or have explicit administrator permission; only signed-in staff may read shared documents.
4. No non-administrator user may change account roles, assign access, or impersonate another person, because only the administrator may manage Atrium permissions and accounts.

**Which of these did you write yourself, and which started as a draft from your
assistant?** Say plainly. Both are fine.

The original draft in this file says I wrote the four requirements myself. In this session, I used an AI assistant to check the application code and refine the requirements so they distinguish directory contact details from profile-only information. The code-based observations are identified above; please keep this attribution only if it accurately reflects how you prepared the original draft.

## 4. One I rejected or rewrote

- The original sentence: "Anyone can look at any staff profile and document in Atrium."
- My version: "No person who is not signed in may view the staff directory, profile pages, or shared documents. Signed-in colleagues may see directory contact details, but profile-only information must be limited to its owner or an authorised administrator."
- Which test it failed, and why: The original sentence allows public access, but the directory, profile, and resource routes require sign-in. I also avoided claiming that the current app hides directory information from colleagues: its directory deliberately returns names, email addresses, departments, and roles to any signed-in user.

## 5. How somebody would check one of these

Pick one requirement. Write the steps for a person who has never seen Atrium and
cannot ask you anything.

- The requirement: No person who is not signed in may view the staff directory, profile pages, or shared documents.
- Sign in as: alice.nolan
- Steps:
  1. Start Atrium by following the setup steps in the project README.
  2. Open a private browser window with no Atrium session and go to `http://localhost:9090`.
  3. Open `/directory`, `/profile`, and `/resources` directly.
  4. Confirm that each request sends you to the sign-in page.
  5. Sign in as `alice.nolan` using the account details in the project README, then open those three pages again.
- What result would mean the requirement is met: Without a session, each page redirects to sign-in; after signing in, the pages load.
- What result would mean it is not met: Any of the three pages displays its protected content without a session. This checks the unauthenticated-access requirement only; it does not prove that signed-in users have the correct level of access to every field or action.

---

## Optional, if you had time

Your four requirements in order, most important first, with one sentence each on
why it is in that position.

1. No person who is not signed in may access Atrium data — sign-in is the first boundary for this staff-only application.
2. No staff member may view or change another colleague's profile-only information unless authorised — this reduces unnecessary exposure of personal details while preserving the directory's intended business contact information.
3. No user may change shared documents without permission — this protects the integrity of operational records.
4. No non-administrator may manage account roles or access rights — this limits who can make account and permission changes.
