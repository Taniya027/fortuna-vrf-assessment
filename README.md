# Fortuna VRF Lottery - Technical Assessment

**DISCLAIMER:** This is for **technical assessment purposes only**. The code contained herein was developed as part of a recruitment process and is not intended for production use.

## Overview
This repository contains a security and logic audit of the \FortunaVRFLottery\ smart contract. The primary goal was to resolve accounting discrepancies and strengthen the randomness fulfillment process using Chainlink VRF v2.5.

## Identified Issues & Solutions

### 1. Jackpot Reserve Isolation (Logic Error)
* **The Problem:** The contract previously used \address(this).balance\ to calculate payouts. This caused the accumulated \jackpotReserve\ to be "re-split" every round.
* **The Solution:** Introduced \activePrizePool\ to track ticket revenue independently. Payouts are now calculated strictly from this pool.

### 2. VRF Callback Integrity (Security Risk)
* **The Problem:** The \fulfillRandomWords\ function lacked validation for the incoming \requestId\.
* **The Solution:** Implemented a \pendingRequests\ mapping to track and validate active VRF requests.

## How to Review
The Git history is structured to show the baseline (init) and the subsequent fixes. 
1. Review \FortunaVRFLottery.sol\ for the implementation of internal accounting.
2. Check the \pendingRequests\ logic for fulfillment security.