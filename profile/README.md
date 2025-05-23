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
      |         SUI           |  (Verifies Data) <-->  Blockchain Network  <-->   |  Secure Verifier    |
      |  (Data Availability & |            (Walrus Stores & Manages Data)             |   (Tamper-Proof Data) |
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
      | on SUI         |
      +----------------------+

