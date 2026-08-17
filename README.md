# Cyber Bernard — AI Email Risk Detector

AI-powered email risk detector built on [Dify](https://dify.ai). Uses RAG over a cybersecurity knowledge base to flag phishing, spoofing, and social-engineering attempts with confidence-scored risk reports.

## Overview

Cyber Bernard is a generative AI cybersecurity assistant that helps users evaluate emails, messages, and links for signs of spam, scams, and social-engineering attacks. Rather than relying on keyword-based filtering, it analyzes context, tone, structure, and intent — the same way a trained analyst would read a suspicious message — to catch polished, AI-generated scams that traditional filters miss.

The assistant was built and tested on 100+ simulated and real-world email samples covering phishing, spoofing, and social-engineering attacks.

## How It Works

- **Retrieval-Augmented Generation (RAG):** Queries are grounded against a curated cybersecurity knowledge base using hybrid retrieval (vector + keyword search, weighted 70/30) to reduce hallucinations and keep responses factually anchored.
- **Model:** GPT-4o-mini, run in Dify's agent-chat mode with function-calling enabled.
- **Structured risk analysis:** Every message is broken down into:
  1. **Summary** – what the message appears to be
  2. **Risk Assessment** – Low / Medium / High, with a confidence percentage and justification
  3. **Key Warning Signs** – suspicious tone, urgency, mismatched links, unusual requests, etc.
  4. **Recommended Actions** – concrete, safe next steps (e.g., verify through official channels, don't click, report to IT)
  5. **Education** – a short explanation of the attack type and how to spot similar attempts in the future

## Design Principles

- **Never claims certainty** — always frames output as a best-effort assessment and encourages independent verification.
- **Adapts to AI-generated scams** — doesn't rely on spelling/grammar errors as a signal, since modern phishing is often polished.
- **Refuses to help with malicious content** — will not write or improve phishing/scam text, and stays scoped strictly to analyzing user-submitted messages and links.
- **Privacy-conscious** — treats any sensitive info shared by the user as confidential within its reasoning.

## Tech Stack

| Component | Details |
|---|---|
| Platform | Dify (agent-chat app) |
| Model | GPT-4o-mini (OpenAI, via Dify plugin) |
| Retrieval | RAG — hybrid vector + keyword search over a cybersecurity knowledge base |
| Retrieval config | Top-K: 4, vector weight 0.7 / keyword weight 0.3, embedding model: `text-embedding-3-large` |

## Try It Yourself

The full app configuration is exported as a Dify DSL file: [`Cyber Bernard.yml`](./Cyber%20Bernard.yml)

To import it into your own Dify instance:
1. Go to [cloud.dify.ai](https://cloud.dify.ai) (or your self-hosted instance) → **Studio**
2. Click **Import DSL file** (or **Create from DSL**)
3. Upload `Cyber Bernard.yml`
4. Connect your own OpenAI provider credentials and knowledge base dataset, since these are not included in the export for security reasons

## Example Use Case

> **User:** "I got an email saying my Microsoft account will be suspended in 24 hours unless I verify my password at micros0ft-support.com"
>
> **Cyber Bernard:** Flags the mismatched domain, urgency-based pressure tactic, and credential-harvesting request; returns a High-risk assessment with a confidence score, explains the phishing pattern, and recommends verifying directly through the official Microsoft site instead of clicking the link.

---

Built as part of an independent cybersecurity + generative AI project exploring how LLMs and RAG can augment (not replace) traditional email security tooling.
