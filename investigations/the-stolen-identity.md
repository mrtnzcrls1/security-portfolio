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

### 3. Pivot

### 4. Persist

### 5. Loot

## Why This Attack Works

## What Broke / What Surprised Me

## Findings and Recommendations
