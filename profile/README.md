# Hyvve
[![Build Status](https://img.shields.io/badge/build-passing-brightgreen)](https://github.com/your-org/datahive/actions)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](./LICENSE)
[![Version](https://img.shields.io/badge/version-1.0.0-blue)](https://github.com/your-org/datahive/releases)


**Accelerating AI innovation through a community-driven data economy** 🚀


## Overview

**Hyvve** is a decentralized, token-incentivized data marketplace built on the SUI blockchain. It revolutionizes how AI-ready datasets are sourced, verified, and monetized. 

With Hyvve, AI developers can launch campaigns to crowdsource the continuous, diverse, and high-quality data their models need, while contributors around the world earn fair rewards for providing that data. 

The platform integrates Atoma Network’s AI-powered verification workflows to automatically check and score submissions, and uses Walrus, SUI’s decentralized storage layer, to securely store data and model artifacts. 

## Problem Statement
 Modern AI systems face several critical data challenges:
- **Centralized Data & Monopolies**: The most valuable training data is often locked away in silos controlled by tech giants or large institutions. 

- **Stale & insufficient pipelines**: Models drift unless they receive a constant supply of new, real-world data; Real-world conditions change – user behavior shifts, new trends emerge – causing data drift that can degrade model performance over time.

- **Lack of Diversity & Quality**: Bias and poor generalization in AI often trace back to datasets that are not diverse or high-quality enough. Many models are trained on homogeneous or error-prone data.

- **Privacy & Data Sovereignty**: In the age of GDPR and growing user awareness, privacy has become paramount. Users and regulators now demand control and fair value for their data; centralized scraping is no longer acceptable.

- **Synthetic Data & Emerging Trends**: To work around data scarcity and privacy limits, the industry has turned to synthetic data – artificially generated datasets that mimic real data. 
But synthetic data is no silver bullet: its quality depends on real data. Moreover, many AI tasks still require ground-truth real data for validation.

## Solution
- **Open Data Campaigns** – At the core of Hyvve is a campaign-based model. Anyone can post a bounty-backed campaign describing the exact data they need.

- **Token rewards** – When launching a campaign, the requester escrows a bounty in Hyvve tokens as rewards. Contributors who submit data that meets the quality criteria earn tokens from this pool. 

- **Automated Quality Verification (Powered by Atoma)** – Automated checks on data submissions using an advanced multiagent AI workflow on Atoma to dynamically select and combine the most appropriate models to ensure authenticity and accuracy. The AI agents act as campaign verifiers.
- **Privacy-first storage** – All contributions are client-side encrypted and stored on Walrus; only campaign owners hold the decryption keys.

- **Reputation System 🌟**: Build your onchain, tamper proof reputation as a contributor by collecting badges, earning higher quality scores, and unlocking higher-paying campaigns based on your submission history.

- **Bulk Data Export 📦**: Once your campaign is complete, easily export your verified dataset in bulk. You can bulk export the data using your generated campaign RSA private key, ensuring the data is ready for AI model training without additional processing.


---

## Monetization

Hyvve offers several revenue streams to maintain and grow the platform:

- **Campaign Fees** 💸: Campaign creators pay a fee (in MOVE tokens) to launch their data requests, providing a consistent revenue stream for the platform.  
- **Marketplace Cut** 🛍️: The platform takes a small percentage of the fees paid to contributors, ensuring a sustainable business model while maintaining fairness.  
- **Premium Features** 💎: Premium users can access advanced campaign analytics, custom insights, and priority support through a monthly subscription. Features include enhanced analytics, support for image-based campaigns, and additional insights into campaign performance.  
- **Token Incentives** 🔥: The platform is designed around the concept of token rewards for data contributors, creating an ecosystem where tokens can be traded, staked, or used within the platform.

---

## Getting Started

This codebase has three main parts - Each repository has a very detailed Readme, enabling you to understand, test and interact with our project

- **Frontend (Next.js)**: [Hyvve Frontend](https://github.com/Hyvve-Movement/hyvve-frontend)
- **Backend (Python/FastAPI)**: [Hyvve Backend](https://github.com/Hyvve-Movement/hyvve-backend) 
- **Smart Contracts (Move Modules)**: [Hyvve Contracts](https://github.com/Hyvve-Movement/hyvve-contracts)

---

## Major Technical Challenges (and How We Solved Them + [Code Implementation](#))

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
    
    Blockchains, like the Movement Bardock testnet inherently lack support for recurring payments, complicating the implementation of subscription-based services such as **automated on-chain monthly payments** for **Hyvve Premium**.
    
    **Our Approach:** 
    
    - **Delegated Payment System:**  [Code implementation](https://github.com/Hyvve-Movement/hyvve-contracts/blob/master/sources/subscription.move)
      - We built a delegated payment capability that allows users to pre-fund their subscription renewals.  
      - Funds are withdrawn and held in a dedicated delegated payment store, ensuring that renewal fees are readily available when due.
    
    - **Automated Renewal Processing:**  
      - The `process_due_renewals` function in our Move contract scans for subscriptions that are active, set to auto-renew, and whose subscription period has expired.
      - For each due renewal, it checks if the subscriber has sufficient delegated funds:
        - If yes, it deducts the subscription fee, updates the subscription period, and emits an "auto_renewed" event.
        - If not, the subscription is deactivated and flagged with a renewal failure.
      
    - **Off-Chain Automation:** [Code Implementation](https://github.com/Hyvve-Movement/hyvve-backend/blob/5285dc80754478d96fdb92c82700c6dd6a1c7478/app/celery/celery.py#L41)
      - We built an admin-only [api wrapper](https://github.com/Hyvve-Movement/hyvve-frontend/blob/main/pages/api/admin/process-due-subscription.ts) around the `process_due_renewals_` function as a Next.js api route (`process-due-subscriptions`).
      - To ensure timely processing, [our Celery off-chain automation service](https://github.com/Hyvve-Movement/hyvve-backend/blob/116ef4e88eea934145102ad0d50ca575d1c13c32/app/celery/celery.py#L41) calls `process-due-subscriptions` every 12 hours.
      - This hybrid solution leverages both on-chain logic for security and off-chain scheduling for efficiency, ensuring recurring subscriptions are handled seamlessly.

- ### Computational Expense of Real-time Blockchain Analytics

    **Challenge:**  
    Relying solely on the Movement blockchain RPC for each campaign’s analytical computation would have been computationally expensive and could have led to performance bottlenecks.
    
    **Our Solution:** [Code Implementation](https://github.com/Hyvve-Movement/hyvve-backend/tree/main/app/campaigns)
    
    - **Data Mirroring:**  
      We mirrored all successful blockchain transactions to our dedicated PostgreSQL database while keeping the blockchain as the single source of truth.
    
    - **Hybrid Analytics Approach:**  
      For advanced analytics, we combine computed results from our backend with real-time data fetched via Next.js API routes using the Aptos SDK from the Movement Bardock Testnet. This ensures scalable, real-time analytics without overburdening blockchain resources.

---
## What's Next for Hyvve?
  - Looking ahead, we're pushing the boundaries of privacy-preserving data contribution to empower our users even further. Our next phase will enable AI models to be trained directly on the contributed data right on the **Movement** chain, while ensuring complete data confidentiality and data handling compliance (e.g HIPAA). Using advanced techniques such as **federated learning**, **homomorphic encryption**, and **secure multi-party computation (MPC)**, we will allow model training to occur on encrypted data. 

    This means that although the AI benefits from the insights hidden within the dataset, the raw data itself is never exposed—not even to the campaign creator. In this way, contributors and users can have complete confidence that their sensitive information remains private while still fueling powerful, decentralized AI development.

  - User Feedback: We highly value our community's input. As we continue to refine and improve the platform, we are actively seeking user feedback to ensure that our services meet your needs and expectations. Please share your suggestions and insights through our official channels.


___

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

