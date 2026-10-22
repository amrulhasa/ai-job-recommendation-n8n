# Setup Guide

## Prerequisites

- An n8n instance
- An OpenRouter API credential
- A Serper API key
- A Gmail account connected through n8n OAuth2

## Import

1. Download/import `workflow/n8n-job-recommendation-sanitized.json` into n8n.
2. Open the **OpenRouter Chat Model** and **OpenRouter Chat Model1** nodes and select your OpenRouter credential.
3. Open the **HTTP Request** node.
4. Set **Authentication** to **Generic Credential Type**.
5. Set **Generic Auth Type** to **Header Auth**.
6. Create/select a Header Auth credential with:
   - Header name: `X-API-KEY`
   - Value: your Serper API key
7. Keep `Content-Type: application/json` as the request header.
8. Open the Gmail node and select your Gmail OAuth2 credential.

## Verify

Run a test form submission and confirm that:

- the first AI chain returns structured job searches;
- the Serper HTTP Request returns search results;
- the second AI chain returns recommendations;
- the Gmail node sends the final email.

## Security checklist

Before publishing a repository:

- [ ] No Serper API key in the JSON
- [ ] No OpenRouter API key in the JSON
- [ ] No Gmail OAuth token in the JSON
- [ ] No passwords or private tokens in README/docs
- [ ] Use n8n Credentials for secrets

If a secret has ever been exposed publicly, revoke/rotate it before continuing.
