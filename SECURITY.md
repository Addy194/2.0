# Security Policy

Varuna Netra is a Smart India Hackathon prototype and should be deployed with normal web-application security controls.

## Secrets

Never commit API keys, access tokens, passwords, private keys, production `.env` files, or real test credentials. Use environment variables or the deployment platform's secret manager. `backend/.env.example` contains placeholders only.

If a secret is committed accidentally, **rotate or revoke it immediately**. Removing the file in a later commit is not sufficient because the value can remain in Git history.

## Demo accounts

Demo credentials must be non-production accounts with no access to real systems or personal data. Do not publish working administrator credentials in README files, screenshots, test reports, or issue comments.

## Reporting a vulnerability

Please report security issues privately to the repository owner rather than opening a public issue containing exploit details, credentials, or sensitive data.

## Scope note

Varuna Netra provides investigation decision support. Its vessel rankings and jurisdiction references are not legal determinations and should be reviewed by an authorized human analyst.
