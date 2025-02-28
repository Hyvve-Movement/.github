# Hyvve

**Unlock Data for AI. Contribute and Earn with Hyvve** 🚀

---

## Overview

**Hyvve** is a token-incentivized data marketplace that connects AI researchers, companies, and everyday data contributors. On Hyvve, you can buy AI-ready data or sell your own for token rewards, all on a secure, decentralized platform. 

Hyvve was built for the **Mammothon Hackathon** to foster the creation of AI-ready datasets and ensure contributors are fairly compensated for their valuable data. Whether you're an AI developer in need of data or a user willing to contribute, we provides the infrastructure for seamless data sharing and incentivization.

---

## Key Features

Hyvve offers a comprehensive set of features for both data contributors and campaign creators:

### For Contributors:

- **Text & Image Verification with AI & Vision Model** 🤖📸: Automated checks on text and image submissions using advanced AI algorithms and Optical Character Recognition (OCR) to ensure authenticity and accuracy. Once data is successfully verified, an **on-chain verification attestation** is created by our secure verifier, ensuring that the data is tamper-proof and auditable, providing an immutable proof of authenticity.

- **Reputation System** 🌟: Build your reputation as a contributor by collecting badges, earning higher quality scores, and unlocking higher-paying campaigns based on your submission history.
- **Earn Tokens** 💰: Receive rewards in MOVE tokens for contributing data that meets the campaign’s quality criteria.
  
### For Campaign Creators:
- **Multimodal Data Support** 📝🖼️: Creators can request multiple types of data (e.g., text, images, and more) within the same platform, enabling richer and more comprehensive datasets for AI training.
- **Automated Onchain Monthly Subscription** 🔄💳: Campaign creators can set up recurring payments for premium services, ensuring access to advanced features like advanced analytics, image-based campaigns, and more.
- **Campaign Creator & Reputation System** 🏆: Establish credibility as a campaign creator by maintaining a verified history of fair payouts and clear data requirements. This encourages more contributors to participate in your campaigns.
- **Advanced Campaign Dashboard & Analytics (Premium Users)** 📊📈: Access a powerful campaign management dashboard that offers real-time analytics on the progress of your campaign, contributor activity, and overall data quality.
- **Bulk Data Export** 📦: Once your campaign is complete, easily export your verified dataset in bulk. You can bulk decrypt the data using your generated campaign private key, ensuring the data is ready for AI model training without additional processing.
  
### Shared Features:

- **Hybrid Encryption (RSA + AES)** 🔐: All data is secured with a combination of RSA (for public-key encryption) and AES (for symmetric encryption), ensuring that your data is protected end-to-end.
- **Leaderboard** 🏅: Track your performance and see where you stand in the community with the **Contributor Leaderboard**. The leaderboard ranks contributors based on the quality and quantity of their submissions, allowing you to compete and earn recognition for your contributions.
- **Decentralized Storage (IPFS)** 🌐: Encrypted Data is stored on IPFS, so your data is highly available and resilient with no single point of failure. It's also encrypted, so your data is safe
- **Intuitive UI** 🖥️: Enjoy a clean, user-friendly interface designed for both seasoned AI engineers and first-time data contributors. The platform is easy to navigate, making it simple to manage campaigns, submit data, and track progress.

---

## Monetization

Hyvve offers several revenue streams to maintain and grow the platform:

- **Campaign Fees** 💸: Campaign creators pay a fee (in stablecoins or platform tokens) to launch their data requests, providing a consistent revenue stream for the platform.  
- **Marketplace Cut** 🛍️: The platform takes a small percentage of the fees paid to contributors, ensuring a sustainable business model while maintaining fairness.  
- **Premium Features** 💎: Premium users can access advanced campaign analytics, custom insights, and priority support through a monthly subscription. Features include enhanced analytics, support for image-based campaigns, and additional insights into campaign performance.  
- **Token Incentives** 🔥: The platform is designed around the concept of token rewards for data contributors, creating an ecosystem where tokens can be traded, staked, or used within the platform.

---

## Getting Started

This codebase has four main parts - Each repository has a very detailed Readme, enabling you to understand, test and interact with our project

- **Frontend (Next.js)**: [Hyvve Frontend](https://github.com/Hyvve-Movement/hyvve-frontend)
- **Backend (Python/FastAPI)**: [Hyvve Backend](https://github.com/Hyvve-Movement/hyvve-backend) 
- **Smart Contracts (Move Modules)**: [Hyvve Contracts](https://github.com/Hyvve-Movement/hyvve-contracts)

---

## Technical Challenges (and How We Solved Them + [Code Implementation](#))

- ### Non-deterministic Nature of LLMs and OCR/Vision Models 

    **Challenge:**  
    Our AI verification system—powered by GPT-4o—produced varying scores for identical submitted datasets, a challenge inherent to non-deterministic AI models that undermined the fairness and reliability of our data validation process.
    
    **Our Approach:** ([Code Implementation](https://github.com/Hyvve-Movement/hyvve-backend/blob/main/app/ai_verification/services.py))
    
    - **File Processing:**  
      - Compute a unique SHA256 hash for each submission.  
      - Check against a Redis cache to avoid redundant processing.
    
    - **Content Handling:**  
      - Identify whether the file is an image or a document.  
      - For images, encode them into a multimodal prompt.  
      - For text documents, extract content using libraries the PyPDF2 & python-docx.
    
    - **Structured Evaluation:**  
      - Construct a structured prompt from the processed content.  
      - Feed the prompt to GPT-4o to generate a raw quality score.
    
    - **Normalization for Fairness:**  
      - Normalize the raw score with a random fairness factor.  
      - Ensure the final score remains unbiased and is capped at 100.

- ### Recurring Subscriptions Not Native to Blockchains

    **Challenge:**
    
    Blockchains, like the Movement Bardock testnet inherently lack support for recurring payments, complicating the implementation of subscription-based services such as automated on-chain monthly payments for Hyvve Premium.
    
    **Our Approach:**
    
    - **Delegated Payment System:**  
      - We built a delegated payment capability that allows users to pre-fund their subscription renewals.  
      - Funds are withdrawn and held in a dedicated delegated payment store, ensuring that renewal fees are readily available when due.
    
    - **Automated Renewal Processing:**  
      - The `process_due_renewals` function in our Move contract scans for subscriptions that are active, set to auto-renew, and whose subscription period has expired.
      - For each due renewal, it checks if the subscriber has sufficient delegated funds:
        - If yes, it deducts the subscription fee, updates the subscription period, and emits an "auto_renewed" event.
        - If not, the subscription is deactivated and flagged with a renewal failure.
      
    - **Off-Chain Automation:**  
      - To ensure timely processing, our Celery off-chain automation service calls `process_due_renewals` every 12 hours.
      - This hybrid solution leverages both on-chain logic for security and off-chain scheduling for efficiency, ensuring recurring subscriptions are handled seamlessly.



---

## Contributing

Hyvve is open-source, and we welcome contributions from developers and data scientists who want to be part of the platform’s growth. Whether you’re interested in helping with smart contract development, AI model optimization, or building integrations, your input is invaluable.

---



      +----------------+              +-------------------+                 +-------------------+
      |   Contributor  |  ----> (Data) --->  AI Researcher   |  ----> (Data/Insights) --->  Company      |
      +----------------+              +-------------------+                 +-------------------+
                |                             |                                      |
                |                             |                                      |
                |  (Tokens/Rewards)           |  (Tokens/Rewards)                    |
                v                             v                                      v
      +-----------------------+   <-->   +------------------------+    <-->   +-------------------+
      |  Movement & Celestia  |  (Verifies Data) <-->  Blockchain Network  <-->   |  Secure Verifier    |
      |  (Data Availability & |            (Stores & Manages Data)             |   (Tamper-Proof Data) |
      |  Smart Contracts)     |                                                      +-------------------+
      +-----------------------+
                ^
                |
        (Data Verification)
                |
       +-------------------+
       | AI Verification    |
       |  (OCR, Authenticity)|
       +-------------------+
                |
     (On-Chain Attestation)
                |
      +----------------------+
      | Verified Data Attested|
      | on Blockchain         |
      +----------------------+

