# Sensitive Data Detection Skill

A Claude Code / Cowork skill that automatically detects sensitive data in conversations and displays a standardised security notice before continuing. Once installed, it fires silently in the background — users see nothing until the policy needs to act.

## What it does

When someone pastes or types sensitive data into a Claude conversation, the skill triggers a short warning identifying the category of data and a brief reason why it matters. Claude then continues helping in general terms without repeating or processing the sensitive values.

When no sensitive data is present, Claude responds normally. The policy is invisible until it needs to fire.

## Categories covered

The skill scans for seven categories of sensitive data.

| # | Category | Examples |
|---|---|---|
| 1 | **Basic identifiers** | Names, personal emails, phone numbers, home addresses, dates of birth |
| 2 | **Employment information** | Salary, performance reviews, disciplinary records linked to an individual |
| 3 | **Financial information** | Bank account details, sort codes, payroll records, expense claims |
| 4 | **Identification documents** | Passport numbers, national ID, NI numbers, tax IDs, driving licence numbers |
| 5 | **Technical credentials** | API keys, passwords, tokens, private keys, connection strings |
| 6 | **Communications** | Emails, messages, or meeting transcripts where an individual can be identified |
| 7 | **Client operational data** | Historian exports, LIMS records, P&ID references, client-environment data |

## Notice format

When triggered, Claude displays a block like this before any other response content:

```
> [YOUR_ORG] Security Notice
>
> Detected — [Category Name]
>
> Why is this an issue?
> [Matching reason from the categories table in SKILL.md]
>
> This notice is issued under [YOUR_ORG]'s Data Protection and Privacy Policy ([YOUR_POLICY_REF]).
>
> If you have any questions please contact [YOUR_PRIVACY_EMAIL] or Slack message the [YOUR_ORG] Compliance Manager.
```

If multiple categories apply, a single notice is shown for the most specific match.

## Customising for your organisation

Open `SKILL.md` and replace the placeholders with your own values.

| Placeholder | Replace with |
|---|---|
| `[YOUR_ORG]` | Your organisation name |
| `[YOUR_POLICY_REF]` | Your data protection policy reference (e.g. `DPP-POL-001`) |
| `[YOUR_PRIVACY_EMAIL]` | Your privacy or compliance team email address |
| `[YOUR_ORG] Compliance Manager` | The title or name of whoever handles compliance queries |

If you'd rather keep the notice generic, leave the placeholders out — the skill works either way.

### Claude Teams deployment

If you want this policy enforced across your entire organisation via Claude Teams:

1. Go to [claude.ai/admin-settings/organization](https://claude.ai/admin-settings/organization) (requires Admin, Owner, or Primary Owner role).
2. Scroll to **Organisation preferences**.
3. Paste the prompt content into the text area. If you already have existing preferences, add it below them.
4. Save. Changes can take up to an hour to propagate.

Note the 3,000-character limit on organisation preferences. The skill prompt fits comfortably, but if you've added substantial org-specific language, verify the length before saving.

## Testing

Start a new conversation and try a message from each category. For example:

| Category | Test message |
|---|---|
| Basic identifiers | "Can you format this: John Smith, john@example.com, 07700 900123" |
| Employment information | "Sarah's salary is 45,000 and her last review scored 3/5" |
| Financial information | "Sort code 12-34-56, account 12345678" |
| Identification documents | "Passport 987654321, NI number AB 12 34 56 C" |
| Technical credentials | "API key sk-abc123def456, DB password hunter2" |
| Communications | "In yesterday's meeting, James said he wants to leave the project" |
| Client operational data | "Historian export from Site A shows tag PIC-101 trending high" |

For each test, Claude should display the notice with the correct category and reason, then continue without repeating the sensitive values. Then send a clean message like "What's the capital of France?" and confirm Claude responds normally with no mention of the policy.

## How it works

This is a prompt-based approach, not a code-level filter. It relies on Claude following the instructions in `SKILL.md`. For most use cases this is sufficient and effective. If you need deterministic pattern matching (regex-based, guaranteed to catch specific formats), that requires a separate implementation outside of the skill system.

Organisation-level instructions always take priority over individual user preferences.

## Files in this folder

| File | Purpose |
|---|---|
| `SKILL.md` | The skill definition — categories, notice format, and behavioural rules |
| `README.md` | This setup and usage guide |

## Further reading

- [Set organization preferences — Claude Help Center](https://support.anthropic.com/en/articles/9487310-set-organization-preferences)
- [Get started with the Team plan — Claude Help Center](https://support.anthropic.com/en/articles/9486747-get-started-with-the-team-plan)
