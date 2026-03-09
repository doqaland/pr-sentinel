# Sample PR Sentinel Output — Feature PR

> This is an example of what PR Sentinel posts as a PR comment.
> PR: "Add email verification step to registration flow"

---

## 🔍 PR Sentinel — QA Analysis

**AC Coverage** — Partially. The diff implements the verification email send and token generation.
The token expiry and re-send flow mentioned in AC #3 are not visible in this diff.

**Risk Level:** 🟡 MEDIUM

---

### ✅ What this change does (QA perspective)
After a user registers, they now receive a verification email before their account is activated.
Unverified accounts are blocked from logging in and shown a prompt to check their email.
A verification token is stored server-side and validated when the user clicks the link.

---

### ⚠️ Potential Bugs & Functional Gaps
- Token validation does not appear to handle expired tokens — the error response is a 500, not a user-friendly message
- No handling found for the case where a user registers twice with the same email before verifying
- The redirect after successful verification hardcodes `/dashboard` — may break for users with custom entry points

---

### 🧪 Suggested Test Scenarios
- **Happy path**: Given a new user registers, when they click the verification link, then their account is activated and they are redirected to /dashboard
- **Expired token**: Given a user clicks a verification link older than 24h, when the token is validated, then they see a clear expiry message with an option to resend
- **Double registration**: Given a user registers twice with the same email before verifying, when the second registration completes, then only one account exists and the token is refreshed
- **Invalid token**: Given a user tampers with the token URL, when they submit it, then they receive a 400 error with a clear message — not a 500

---

### 🔄 Regression Risk
- Login flow — unverified account blocking is new behavior; any existing test for login will need updating
- Password reset flow — if it shares the token generation utility, expiry logic may be affected
- Admin user creation — if admins bypass registration, verify the bypass still works

---

### ❓ Open Questions for the Developer
1. What is the token expiry duration, and is there a resend limit?
2. Does the verification requirement apply to OAuth/SSO registrations or only email/password?
3. Is the `/dashboard` redirect hardcoded intentionally or should it respect the original destination?
