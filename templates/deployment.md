# Deployment Runbook

**Audience:** Human director / on-call operator performing the first production deploy of `<your-app>`.
**Scope:** First-time production deployment, ordered. Distinct from `ARCHITECTURE.md` (design intent) and `README.md` (overview / local dev).
**Single source of truth.** If a deploy step exists, it lives here. Do not scatter deploy info across multiple docs.

---

## 1. One-time prerequisites

Complete each of these before the first deploy. Each item should be a verifiable artifact, not a vague "make sure X is set up."

- [ ] **Hosting target provisioned** — VPS, container platform, or PaaS instance reachable over SSH or with deploy credentials.
- [ ] **Database provisioned** — Postgres (or chosen DB) with admin credentials, connection string captured, network reachable from the app host.
- [ ] **DNS records** — A/AAAA or CNAME records pointed at the host; TLS certificates issued (Let's Encrypt, managed cert, or uploaded).
- [ ] **Secrets store populated** — every variable in `.env.example` has a production value committed to the deploy secrets store (GitHub Actions secrets, Vault, SOPS-encrypted file, etc.). Never inline secrets in `docker-compose.yml`.
- [ ] **Third-party apps configured** — OAuth callback URLs, webhook URLs, email-sending domain SPF/DKIM, payment provider live keys, etc. — each verified by manual test where possible.
- [ ] **Backup destination** — DB backup target (S3 bucket, managed snapshot, or rsync target) and credentials in place before any production data exists.

## 2. First deploy

Run these commands in this order from a clean checkout. Each step lists the expected output / success indicator.

```sh
# 1. Build the production image(s)
<exact build command>
# Expected: image tagged <name>:<version>, docker images shows it
```

```sh
# 2. Run database migrations
<exact migration command>
# Expected: "X migrations applied" with no errors
```

```sh
# 3. Start the stack
<exact start command>
# Expected: container/process status = healthy within N seconds
```

```sh
# 4. Confirm reverse proxy / TLS termination is wired
<exact check>
# Expected: HTTPS responds on the public hostname with the correct certificate
```

## 3. Health verification

After the stack is up, prove it works before declaring deploy complete.

- [ ] `curl https://<host>/health` returns 200 with the expected payload
- [ ] One representative read endpoint returns expected data for an authenticated user
- [ ] One representative write endpoint persists a record and returns 201
- [ ] Logs show no error-level entries since process start
- [ ] Metrics endpoint (or APM dashboard) shows non-zero requests handled

If any check fails, do **not** declare the deploy successful — proceed to rollback.

## 4. Rollback procedure

If verification fails or a regression is detected within the first hour:

```sh
# 1. Roll back the application
<exact rollback command — e.g. redeploy previous image tag>
```

```sh
# 2. Roll back the database migration if (and only if) the new migration was destructive
<exact down-migration command>
# Expected: "X migrations reverted"
```

- [ ] Confirm health checks pass on the previous version
- [ ] Notify any downstream consumers (status page, Slack channel) if the rollback is user-visible
- [ ] File a postmortem ticket with the failure mode captured before context evaporates

## 5. Post-deploy smoke checklist (for the human director)

Run through this list manually within 24 hours of first deploy:

- [ ] Can a brand-new user complete the sign-up flow end-to-end?
- [ ] Can an existing user log in, perform the primary workflow, and log out?
- [ ] Are emails (transactional, password reset) actually delivering?
- [ ] Are webhooks / outbound integrations firing and being received?
- [ ] Are scheduled jobs running on schedule (check logs at the next interval)?
- [ ] Is the backup job running and producing a valid restore artifact?
- [ ] Does the monitoring/alerting pipeline page on a deliberately injected error?

When every box is checked, the deploy is done. If any box stays unchecked for more than 48 hours, treat it as an open production bug.
