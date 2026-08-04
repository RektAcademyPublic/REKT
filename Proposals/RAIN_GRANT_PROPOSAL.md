# Grant Proposal: Rekt Markets 

**Applicant:** Rekt Academy
**Amount Requested:** $50,000 USD
**Category:** Ecosystem Integration & Prediction Market Development

---

## 1. TL;DR
Rekt Academy trains Web3 security researchers and white-hats. We are applying for a $50,000 grant from the Rain Foundation to build **"Rekt Markets"** using the OpenClaw SDK. 

Our goal is straightforward: let security researchers, auditors, and AI agents bet on live cybersecurity events. We are bringing prediction markets to Web3 infosec.

## 2. The Problem
While white-hat bug bounties (like Immunefi) do an excellent job incentivizing direct vulnerability discovery, the broader landscape of Web3 security remains completely reactive. Hacks happen, post-mortems drop, and people move on. There is currently no financial incentive for the wider community of analysts to aggregate threat intelligence, predict systemic risks, or track the outcomes of ongoing exploits in real-time. 

## 3. What we're building: Rekt Markets
We are deploying custom prediction markets focused entirely on cybersecurity. 

Take the recent $89M Coldcard firmware exploit. Instead of just debating it on Crypto Twitter, researchers can use Rekt Markets to trade positions on live outcomes:
*   **Apprehension:** *"Will the Coldcard hacker (bc1qq85...) be identified or arrested before Q4?"*
*   **Vulnerabilities:** *"Will a critical (Severity 9+) bug be found in [Protocol X's] upcoming audit?"*
*   **Attack Vectors:** *"What will be the root cause for the next bridge hack? (Options: Private Key leak, Logic Bug, Oracle Manipulation)"*
*   **On-chain tracking:** *"Will the attacker move funds through Tornado Cash within 30 days?"*

## 4. Technical Implementation
We are integrating Rain’s decentralized infrastructure directly into our platform:

*   **OpenClaw SDK:** We will wire the SDK into our existing React frontend, allowing users to create, fund, and trade positions without leaving Rekt Academy.
*   **Olympus AI Oracles:** We'll configure Rain's Olympus AI to monitor Etherscan for specific fund movements, or scan CVE databases and public law enforcement feeds to autonomously resolve our security markets. 
*   **Dispute Resolution:** Ambiguous outcomes (e.g., a "white-hat" rescue vs. a malicious drain) will fallback to Rain's built-in dispute mechanism for human oracle resolution.
*   **Arbitrum L2:** Operating on Arbitrum ensures low fees and fast execution for our users while driving transactions on the network.

## 5. Ecosystem Value & Business Model
Security researchers are highly analytical and follow live incidents closely. Giving them a platform to hedge their analysis drives immediate, high-quality trading volume to the Rain protocol.

As the frontend operator, Rekt Academy will capture the **0.5% protocol revenue share** generated from trading volume. This creates a sustainable revenue loop for our education platform while consistently driving liquidity and users into Rain's ecosystem.

## 6. Milestones & Funding Request

**Total Ask:** $50,000

**Milestone 1: SDK Integration ($15,000)**
*   Integrate OpenClaw SDK into the Rekt Academy frontend.
*   Build out the market creation and trading UI.

**Milestone 2: Oracle Configuration & Testnet ($20,000)**
*   Configure Olympus AI parameters specifically for infosec events (Etherscan API hooks, CVE feeds).
*   Deploy the first batch of Rekt Markets on the Arbitrum testnet.

**Milestone 3: Mainnet Launch & Liquidity Seeding ($15,000)**
*   Mainnet deployment.
*   Seed initial liquidity for the first 3-5 high-profile security markets.
*   Run an onboarding campaign to our existing user base to bootstrap initial trading volume.
