# SprintPulse GTM Content Agent

A low-code, multi-agent go-to-market content workflow built with n8n, Google Sheets, and Groq-hosted language models.

## Project Overview

SprintPulse GTM Content Agent converts a natural-language campaign request into a coordinated content package containing:

- One LinkedIn post
- One promotional email
- One short blog
- Exactly three advertisement variations

The workflow separates strategy, writing, review, and revision into specialized agents. All successful results remain pending human review, and no content is published automatically.

## Demo

- Workflow demonstration: [Demo Video](https://www.loom.com/share/86fce9b0118c49e596a06c977f1e3b12)
- Project documentation: [Project Documentation](documentation/sprintpulse-gtm-agent-project-documentation.pdf)
- Evaluation workbook: [Evaluation Workbook](evaluation/sprintpulse-gtm-agent-evaluation.xlsx)
- Workflow Export: [n8n Workflow Export](workflows/01-sprintPulse-gtm-agent.json)
- Product Catalog: [Synthetic Product Catalog](data/sprintpulse-product-catalog.xlsx)

## Technology Stack

- n8n Cloud for workflow orchestration and internal chat
- Google Sheets for the synthetic product catalog
- Groq API for hosted language-model access
- `openai/gpt-oss-20b` for strategy, writing, revision, and formatting repair
- `openai/gpt-oss-120b` for independent review
- GitHub for version control and submission

## Agent Roles

### Strategy Agent

Creates the target audience, pain points, value proposition, proof points, messaging angles, tone guidance, and campaign hooks using verified product information.

### Writer Agent

Generates the LinkedIn post, promotional email, short blog, and three advertisement variations.

### Reviewer Agent

Independently evaluates the original draft for grounding, completeness, and tone. It returns either `PASS` or `REVISE`.

### Revision Agent

Applies Reviewer feedback once. The workflow does not permit an unlimited revision loop.

## Workflow Summary

1. Receive a campaign request through n8n chat.
2. Normalize the product name, tone, and optional URL.
3. Validate the input.
4. Read the synthetic product catalog from Google Sheets.
5. Find the requested product using case-insensitive matching.
6. Generate a grounded campaign strategy.
7. Generate the four required content formats.
8. Validate the Writer output against a structured schema.
9. Review the original draft independently.
10. Follow the `PASS` route or perform one revision.
11. Return the campaign with `pending_human_review` status.
12. Publish nothing automatically.

## Synthetic Dataset

The catalog contains three fictional products:

- SprintPulse AI
- LaunchPilot
- SupportSense AI

No real customer, company, employee, or confidential data is used.

## Evaluation

The workflow was tested with 10 scenarios covering:

- Three supported products
- Explicit and default tones
- Missing product information
- Unknown products
- Optional URLs
- Case-insensitive product names
- Alternate request wording
- Minimal valid input

### Evaluation Results

- Baseline: 7 of 10 tests passed
- Targeted retests: 3 of 3 passed
- Latest functional coverage: 10 of 10 scenarios
- Final output: pending human review
- Automatic publishing: disabled

Automated Reviewer scores apply to the original draft. Revised campaigns are not automatically scored a second time and therefore still require human verification.

## Repository Structure

```text
sprintpulse-gtm-agent/
├── README.md
├── data/
│   └── sprintpulse-product-catalog.xlsx
├── documentation/
│   └── sprintpulse-gtm-agent-project-documentation.pdf
├── evaluation/
│   └── sprintpulse-gtm-agent-evaluation.xlsx
└── workflows/
    └── 01-sprintPulse-gtm-agent.json
```

## Setup Instructions

1. Download or clone this repository.
2. Import `workflows/01-sprintPulse-gtm-agent.json` into n8n.
3. Upload `data/sprintpulse-product-catalog.xlsx` to Google Sheets.
4. Configure a Google Sheets credential in n8n.
5. Select the uploaded catalog in the Read Product Catalog node.
6. Configure a Groq API credential for the model nodes.
7. Confirm that the required models are available.
8. Keep public chat access disabled unless a controlled deployment is required.
9. Test the workflow using n8n’s internal chat.

### Example Request

```text
Create a professional campaign for SprintPulse AI
```

## Privacy and Security

The exported workflow is sanitized. It does not include:

- API keys
- Passwords
- Personal email addresses
- Original credential identifiers
- Private Google Sheet document IDs

After importing the workflow, users must configure their own credentials and product-catalog reference.

## Guardrails

- Use only verified product-catalog facts.
- Do not invent features, evidence, integrations, or performance results.
- Do not claim that optional URLs were researched.
- Generate every required content format.
- Permit no more than one automated revision.
- Keep final content pending human review.
- Never publish automatically.
- Route invalid or malformed outputs to controlled error responses.

## Known Limitations

- The prototype supports only three fictional products.
- Model-provider limits may cause delays or intermittent failures.
- Structured-output repair increases latency and token usage.
- Revised content is not automatically reviewed a second time.
- Optional URLs are captured but not researched.
- Final content requires human approval.

## Disclaimer

This project is an educational prototype created for the Gen Academy agent-building assignment. It is not a production marketing or publishing system.