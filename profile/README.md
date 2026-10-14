<div align="center">

# Vinkius

### The connectivity layer for AI builders.

#### One Application, unlimited users — each with their own connectors and credentials, isolated and never exposed to Vinkius.

![Vinkius Connect](https://img.shields.io/badge/Vinkius%20Connect-11%2C000%2B%20connectors-blue?style=flat-square)
![AI capabilities](https://img.shields.io/badge/AI%20capabilities-20M%2B-orange?style=flat-square)
[![API](https://img.shields.io/badge/api.api.vinkius.com-11%2C000%2B%20connectors-blue?style=flat-square)](https://api.vinkius.com)
[![Docs](https://img.shields.io/badge/docs.vinkius.com-Connect%20SDK-black?style=flat-square)](https://docs.vinkius.com/connect-sdk)

[Website](https://vinkius.com) · [Docs](https://docs.vinkius.com) · [Cloud](https://cloud.vinkius.com) · [AI Connect](https://vinkius.com/learn/en/connect-sdk)

<img src="https://raw.githubusercontent.com/VinkiusForDevs/.github/main/assets/hero.webp" alt="Autonomous warehouse orchestrated by AI — the Vinkius connectivity platform" width="100%" />

</div>

---

**Vinkius** turns the AI application you're building into a connectivity platform for your own users. One backend API: each of your users gets their own connectors, their own credentials and their own AI capabilities — and your agents call them from any model runtime. Vinkius handles the connectivity, authentication, permissions, protocols and execution behind those capabilities. Your application identifies the user. That division of labor is the product.

Your users need your AI to act in the real world: open the issue, send the message, query the database, file the ticket. Without a connectivity layer, that means building every integration yourself — OAuth flows per provider, encrypted token storage, protocol handling, API churn, security reviews. **Months of plumbing before your product does anything real.** Vinkius removes the integration tax: the connectors are built and maintained, credentials are handled with per-user isolation, and the capabilities your agent calls arrive through one API. You compete on your product. The plumbing is done.

## Why teams build on Vinkius

- **Thousands of connectors from day one** — GitHub, Slack, Notion, databases, internal APIs and more, already maintained and growing. You do not build integrations, you do not store tokens, you do not touch OAuth plumbing.
- **Per-user isolation** — every user connects their own accounts with completely separate connection state, credentials and capabilities. Nothing crosses the boundary.
- **Your real users are never exposed to Vinkius** — the SDK needs only your opaque `external_id`. No email, no name, no profile. Vinkius is invisible infrastructure under your product.
- **Credentials are write-only** — your backend sets them and can verify which fields are configured, but nobody (not your code, not the model, not the dashboard) reads them back.
- **Two planes over one credential** — control plane (`users`, `connectors`, `credentials`, `catalog`) and execution plane (`capabilities()`, `execute()`) share a single `vk_app_sk_*` secret.
- **Bring your own model runtime** — capabilities are framework-neutral JSON Schema objects with OpenAI, Anthropic, Gemini, Vercel AI SDK, LangChain, LlamaIndex and OpenAI-compatible adapters at the model boundary.

## AI Connect SDKs

Official SDKs for the [Vinkius AI Connect platform](https://vinkius.com/learn/en/connect-sdk), one per ecosystem. Same concepts, same two-plane API, same adapters everywhere.

| Language | Repository | Package |
|:---------|:-----------|:--------|
| TypeScript | [`ai-connect-sdk-typescript`](https://github.com/VinkiusForDevs/ai-connect-sdk-typescript) | [`@vinkius/connect`](https://www.npmjs.com/package/@vinkius/connect) (npm) |
| Python | [`ai-connect-sdk-python`](https://github.com/VinkiusForDevs/ai-connect-sdk-python) | PyPI |
| Java | [`ai-connect-sdk-java`](https://github.com/VinkiusForDevs/ai-connect-sdk-java) | Maven Central (`com.vinkius:connect-sdk`) |
| Go | [`ai-connect-sdk-go`](https://github.com/VinkiusForDevs/ai-connect-sdk-go) | Go modules |
| C# / .NET | [`ai-connect-sdk-c-sharp`](https://github.com/VinkiusForDevs/ai-connect-sdk-c-sharp) | NuGet |
| PHP / Laravel | [`ai-connect-sdk-laravel`](https://github.com/VinkiusForDevs/ai-connect-sdk-laravel) | Composer |
| Rush monorepo | [`ai-connect-sdk-rush`](https://github.com/VinkiusForDevs/ai-connect-sdk-rush) | npm (Rush) |

## Quickstart (TypeScript)

```ts
import { Vinkius } from '@vinkius/connect';

const vinkius = new Vinkius({
  appId: process.env.VINKIUS_APP_ID!,
  apiKey: process.env.VINKIUS_APP_KEY!,
});

// give one of YOUR users their own GitHub connection
const user = vinkius.user('alice_123');
const connector = user.connector('github');
await connector.connect();
await connector.credentials.set({ personal_access_token: '***' });

// the executable capabilities available to that user
const capabilities = await user.capabilities({ include: ['github'] });
const created = await capabilities
  .findCapability('Create a new issue in a GitHub repository')
  .execute({ owner: 'acme', repo: 'api', title: 'Broken login flow' });

console.log(created.results?.[0]?.data);
```

> Install: `npm install @vinkius/connect` · other languages: see the repository table above. Requires a free [Vinkius Cloud](https://cloud.vinkius.com) account.

## Core concepts

| Term | Meaning | Example |
|:-----|:--------|:--------|
| **Connector** | An integration available in the catalog | `github`, `slack`, `notion` |
| **Connection** | One connector configured for one application user | Alice's GitHub connection |
| **Capability** | An executable action exposed by a connection | `github__create_issue` |

A connector is not a capability — one connector exposes many capabilities (`list repositories`, `create issue`, `search code`, …). `user.capabilities()` returns the capabilities available to **that user**, as executable, framework-neutral objects. An empty result is normal: treat availability as runtime data.

## Security by design

One Application, many users, zero exposure. Per-user isolation, write-only credentials, opaque external identifiers, scoped tokens, and documented guarantees behind every call — see [Security](https://vinkius.com/learn/en/connect-sdk/security) and [Authentication](https://vinkius.com/learn/en/connect-sdk/authentication).

## Start building

- **Docs**: [vinkius.com/learn/en/connect-sdk](https://vinkius.com/learn/en/connect-sdk)
- **Cloud dashboard**: [cloud.vinkius.com](https://cloud.vinkius.com)
- **API**: [api.vinkius.com](https://api.vinkius.com)
- **Issues**: open an issue in the SDK repository for your language

<div align="center">

**Build AI agents that act for each user.**

[Get started →](https://vinkius.com/learn/en/connect-sdk)

</div>
