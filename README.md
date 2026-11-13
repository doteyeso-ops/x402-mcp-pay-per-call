# Pay-per-call tools for agents: what an x402 endpoint actually is

A 402 with a price and an address. No signup, no API key, no invoice - and a client that knows what to do with it.

An agent that needs a tool should not have to negotiate an account. x402 makes the price part of the HTTP exchange: you ask for the resource, the server answers 402 with what it costs and where to pay, you pay, you retry, you get the data. The idea is elegant and the hard part is everything around it - discovery, price sanity, and whether the agent's owner ever sees the bill.

## The numbers that decide it

- **402 is the contract** — A bare call returns 402 with a body naming the price, the asset and the payTo address per chain. A client that cannot parse that body cannot use the endpoint, however good the data is.
- **Settlement is on-chain, not in the response** — The 402 tells you where to pay; the proof of payment is a transaction. Any endpoint that unlocks on a header alone without verifying settlement is giving the data away.
- **Price granularity is the product decision** — A cent per call is invisible; a dollar per call needs a human. Endpoints that price near zero get called by loops, not customers - which is why on-chain volume is a poor proxy for revenue.
- **MCP is the discovery layer** — An MCP server advertises tools an agent can call; x402 prices them. Together they replace the API key, not the invoice.
- **Self-payment looks like traction** — A treasury that funds test wallets which then call its own endpoints generates on-chain volume and zero revenue. Check the counterparty before calling anything a sale.

## How to do it, in order

1. **Make the challenge machine-readable.** Name the resource, the price, the asset and the chain. If a client has to scrape prose to find the address, it will not pay.
2. **Verify settlement before serving.** Check the transaction, the amount, the asset and the destination. A 200 without verification is a free endpoint dressed as a paid one.
3. **Price per call against a real cost.** If the unit price is below the cost of verifying the payment, you have built a donation box.
4. **Publish one working example.** A curl that returns 402 and a second curl that pays is worth more than any landing page.
5. **Instrument the funnel.** Challenges served, payments attempted, payments settled, unique payers. The last one is the only number that means anything.

## Questions

**Do agents actually pay for tools?**

Some do, in small amounts. Treat revenue as unproven until you can name a payer that is not your own wallet.

**Which is better, x402 or an API key?**

Keys suit humans with accounts; x402 suits agents with a wallet and no procurement. They are different customers, not competing designs.

**How do I test an endpoint without a wallet?**

Call it and read the 402. If the challenge names price, asset and destination correctly, the contract is working before any money moves.

## If you would rather not do this by hand

Vibes-Coded exposes agent tools over x402 and MCP - search, page-to-markdown, JSON repair, shared memory - with a free tier and a working 402 challenge you can inspect before paying anything.

→ **Vibes-Coded** — https://vibes-coded.com?utm_source=github&utm_medium=repo&utm_campaign=x402-mcp-pay-per-call

Written by the person who runs Vibes-Coded. On-chain revenue to date is under a dollar, and this page says so rather than implying traction.

---

Vibes-Coded · agent tools over x402 · vibes-coded.com
