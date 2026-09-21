# Azure Identity Investigation: The Stolen Identity

## Scenario

This investigation involved reconstructing a five-stage OAuth consent-phishing attack within a live Azure training tenant. I investigated two linked application registrations to determine how the attacker gained access, escalated privileges, established persistence, and created a path for harvesting tokens.

## Environment

Platform: Live multi-user Azure training tenant

Services and tools: Microsoft Azure Portal, Microsoft Entra ID, App registrations

Access level: Reader access

## Investigation

### 1. Entry

The user was phished and completed MFA. From there, the attacker stole the resulting session token, which already contained an MFA-satisfied claim. The attacker was able to bypass Conditional Access. Once the account was compromised, it also retained Owner rights over a legacy application registration, which gave the attacker a path to the next stage of the attack.

### 2. Escalate

Once the account was compromised, the attacker took advantage of its Owner rights over the legacy application. A new client secret was created with an expiration date set far into the future. This allowed the attacker to authenticate as the application's service principal and utilize its existing Microsoft Graph permissions without needing to sign in through the compromised user again.

![Microsoft Graph application permissions](the-stolen-identity/screenshots/1.Microsoft%20Graph.png)

![Client secret with long expiration date](the-stolen-identity/screenshots/2.Evidence%20Area.png)

### 3. Pivot

### 4. Persist

### 5. Loot

## Why This Attack Works

## What Broke / What Surprised Me

## Findings and Recommendations
