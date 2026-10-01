# AirSignal email templates

Transactional HTML email templates live in `packages/email` and are rendered with [`@react-email/components`](https://react.email) + Resend.

## Templates

| Template | File | Used for |
|---|---|---|
| Signup verification | `packages/email/src/templates/signup-verification.tsx` | Email signup OTP + magic activation link |
| Password reset | `packages/email/src/templates/password-reset.tsx` | Forgot-password OTP (**code only**, no button) |
| Org invitation | `packages/email/src/templates/org-invitation.tsx` | Team invite with Accept CTA button |

## Variables — signup verification

| Variable | Type | Description |
|---|---|---|
| `{firstName}` | string | Recipient first name from register-intent |
| `{email}` | string | Normalized email being verified |
| `{code}` | string | 6-digit OTP (shown large in the email) |
| `{verificationUrl}` | string | Magic link: `${WEB_URL}/auth/verify?token=…` |
| `{expiresIn}` | string | Human TTL label (e.g. `15 minutes`) |

## Variables — password reset

| Variable | Type | Description |
|---|---|---|
| `{firstName}` | string | Account display name (fallback `there`) |
| `{email}` | string | Normalized account email |
| `{code}` | string | 6-digit OTP (shown large; no CTA button) |
| `{expiresIn}` | string | Human TTL label (e.g. `15 minutes`) |

## From addresses (by purpose)

Do **not** use a single global from. Each product area has its own env key:

| Purpose | Env | Default |
|---|---|---|
| Auth / signup verification | `RESEND_FROM_AUTH` | `AirSignal <noreply-auth@airsignal.dev>` |
| Billing / payments | `RESEND_FROM_BILLING` | `AirSignal <noreply-billing@airsignal.dev>` |

Pick the address in `MailService.fromAddress('auth' | 'billing')`. Domain must be verified in Resend.

## Preview / develop

```bash
pnpm --filter @airsignal/email run build
```

Edit a template under `packages/email/src/templates/`, rebuild, then trigger the flow (or call `renderSignupVerificationEmail` from a small script).

Optional: add [react-email](https://react.email/docs/introduction) preview with:

```bash
pnpm dlx react-email dev --dir packages/email/src/templates
```

## Adding a new template

1. Create `packages/email/src/templates/<name>.tsx` using `@react-email/components`.
2. Export a typed props interface and a render helper from `packages/email/src/index.ts`.
3. Send via `MailService` in `apps/api` using Resend (`RESEND_API_KEY`).
4. Document variables in this README.
5. Keep public JSON / API field names **camelCase**; template props stay camelCase too.
