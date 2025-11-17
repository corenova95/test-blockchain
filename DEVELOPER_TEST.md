# Blockchain Demo - Developer Test Steps

## Overview
This document contains test steps for evaluating developers using the Blockchain Demo project. These tests assess understanding of blockchain concepts, Node.js/Express development, and problem-solving skills.

---

## Prerequisites
- Node.js installed (v12 or higher)
- Basic understanding of JavaScript
- Git installed
- Text editor or IDE

---

## Test Setup (5 minutes)

### Step 1: Clone and Install
```bash
git clone https://github.com/anders94/blockchain-demo.git
cd blockchain-demo
npm install
```

### Step 2: Start the Application
```bash
npm start
```

### Step 3: Verify Installation
- Open browser and navigate to `http://localhost:3000`
- Verify the homepage loads successfully
- Check that all navigation links work (Hash, Block, Blockchain, Distributed, Tokens, Coinbase)

---

## Basic Tests (15-20 minutes)

### Test 1: Understanding Hash Function
**Objective:** Verify understanding of cryptographic hashing

**Steps:**
1. Navigate to `http://localhost:3000/hash`
2. Enter "Hello World" in the data field
3. Observe the generated hash
4. Change one character (e.g., "Hello World!")
5. Observe how the hash completely changes

**Expected Result:** 
- Hash changes completely with any data modification
- Candidate can explain why this property is important for blockchain

**Questions to Ask:**
- What is the hash algorithm used? (SHA-256)
- Why does a small change create a completely different hash?
- What is this property called? (Avalanche effect)

---

### Test 2: Block Mining
**Objective:** Understand proof-of-work concept

**Steps:**
1. Navigate to `http://localhost:3000/block`
2. Observe the initial block state (red/invalid)
3. Click "Mine" button
4. Observe the nonce value and resulting hash
5. Modify the block data
6. Observe the block becomes invalid (red)
7. Mine again to make it valid

**Expected Result:**
- Candidate understands that mining finds a nonce that produces a hash with leading zeros
- Can explain the relationship between difficulty and mining time

**Questions to Ask:**
- What does the "Mine" button do?
- What is a nonce?
- Why does the block turn red when data changes?
- How would increasing difficulty affect mining time?

---

### Test 3: Blockchain Integrity
**Objective:** Understand blockchain immutability

**Steps:**
1. Navigate to `http://localhost:3000/blockchain`
2. Mine all blocks in the chain
3. Modify data in Block #1
4. Observe that Block #1, #2, #3, #4, and #5 all turn red
5. Explain why subsequent blocks are affected

**Expected Result:**
- Candidate understands that each block contains the previous block's hash
- Can explain the chain of trust concept
- Understands why tampering is detectable

**Questions to Ask:**
- Why do all subsequent blocks become invalid?
- What would an attacker need to do to successfully tamper with the blockchain?
- How does this relate to the 51% attack concept?

---

### Test 4: Distributed Blockchain
**Objective:** Understand consensus in distributed systems

**Steps:**
1. Navigate to `http://localhost:3000/distributed`
2. Mine all blocks on all peers (A, B, C)
3. Modify data in Peer A's Block #2
4. Observe that Peer A's chain is now invalid
5. Explain how the network would handle this

**Expected Result:**
- Candidate understands consensus mechanisms
- Can explain how the longest valid chain wins
- Understands the role of multiple nodes in security

**Questions to Ask:**
- How does the network determine which chain is correct?
- What happens if Peer A tries to broadcast invalid blocks?
- Why is decentralization important for security?

---

## Intermediate Tests (20-30 minutes)

### Test 5: Code Review and Bug Fixing
**Objective:** Assess code quality and debugging skills

**Task:** Review `routes/index.js` and identify issues

**Issues to Find:**
1. Unused `async` import (line 3)
2. Unused `next` parameters in route handlers
3. No error handling for invalid page routes
4. Potential security issue: unrestricted page parameter

**Expected Actions:**
- Remove unused imports
- Add proper error handling
- Implement route validation
- Suggest security improvements

**Sample Solution:**
```javascript
var express = require('express');
var router = express.Router();

// List of valid pages
const validPages = ['hash', 'block', 'blockchain', 'distributed', 'tokens', 'coinbase'];

router.get('/', function(req, res) {
  res.render('index');
});

router.get('/:page', function(req, res, next) {
  const page = req.params.page;
  
  // Validate page parameter
  if (!validPages.includes(page)) {
    return next(); // Pass to 404 handler
  }
  
  res.render(page, {page: page});
});

module.exports = router;
```

---

### Test 6: Add New Feature - Transaction History
**Objective:** Assess ability to extend existing code

**Task:** Add a new page that displays a transaction history

**Requirements:**
1. Create a new route `/history`
2. Create a new view `views/history.pug`
3. Display a list of sample transactions with:
   - Sender address
   - Receiver address
   - Amount
   - Timestamp
4. Add navigation link to the layout

**Evaluation Criteria:**
- Code follows existing project structure
- Proper use of Pug templating
- Clean, readable code
- Proper routing implementation

---

### Test 7: API Endpoint Creation
**Objective:** Test backend development skills

**Task:** Create a REST API endpoint that returns blockchain data as JSON

**Requirements:**
1. Add route `GET /api/blockchain`
2. Return JSON with blockchain structure:
```json
{
  "blocks": [
    {
      "index": 1,
      "timestamp": "2024-01-01T00:00:00Z",
      "data": "Genesis Block",
      "previousHash": "0",
      "hash": "000abc...",
      "nonce": 12345
    }
  ]
}
```
3. Add proper error handling
4. Test with curl or Postman

**Bonus:** Add query parameters for filtering (e.g., `?limit=5`)

---

## Advanced Tests (30-45 minutes)

### Test 8: Performance Optimization
**Objective:** Assess optimization skills

**Task:** Analyze and optimize the mining algorithm

**Steps:**
1. Open `public/javascripts/blockchain.js`
2. Identify the mining function
3. Measure current performance
4. Propose optimizations
5. Implement and test improvements

**Possible Optimizations:**
- Web Workers for non-blocking mining
- Batch processing
- Caching strategies
- Algorithm improvements

---

### Test 9: Internationalization Enhancement
**Objective:** Test i18n implementation skills

**Task:** Add a new language to the application

**Requirements:**
1. Add a new locale file (e.g., `locales/it.json` for Italian)
2. Translate all strings
3. Update `app.js` to include the new locale
4. Test language switching
5. Ensure all pages display correctly

---

### Test 10: Docker Deployment
**Objective:** Assess DevOps knowledge

**Task:** Review and improve the Docker setup

**Steps:**
1. Review `Dockerfile` and `docker-compose.yml`
2. Build and run the container
3. Verify application works in container
4. Suggest improvements for:
   - Security
   - Performance
   - Multi-stage builds
   - Environment configuration

---

## Evaluation Rubric

### Junior Developer (Pass: 60%)
- Complete Basic Tests 1-4 ✓
- Identify at least 2 issues in Test 5 ✓
- Attempt Test 6 with guidance ✓

### Mid-Level Developer (Pass: 75%)
- Complete all Basic Tests ✓
- Complete Test 5 with all issues found ✓
- Complete Test 6 successfully ✓
- Complete Test 7 with basic implementation ✓

### Senior Developer (Pass: 85%)
- Complete all Basic and Intermediate Tests ✓
- Complete at least 2 Advanced Tests ✓
- Provide thoughtful optimization suggestions ✓
- Demonstrate security awareness ✓
- Show architectural thinking ✓

---

## Interview Questions

### Blockchain Concepts
1. Explain the difference between blockchain and a traditional database
2. What is the Byzantine Generals Problem?
3. How does proof-of-work prevent spam?
4. What are the trade-offs between proof-of-work and proof-of-stake?
5. Explain the concept of a Merkle tree

### Technical Implementation
1. How would you scale this application for production?
2. What security vulnerabilities do you see?
3. How would you implement user authentication?
4. How would you add persistence (database)?
5. What testing strategy would you implement?

### Problem Solving
1. How would you implement a transaction pool?
2. How would you handle network partitions in a distributed blockchain?
3. How would you implement smart contracts in this demo?
4. How would you optimize the mining algorithm for mobile devices?

---

## Time Estimates

- **Quick Assessment (30 min):** Tests 1-3
- **Standard Interview (1 hour):** Tests 1-5
- **Technical Interview (2 hours):** Tests 1-7
- **Take-Home Assignment (4 hours):** Tests 5-10

---

## Notes for Recruiters

### Red Flags
- Cannot explain basic blockchain concepts after using the demo
- Unable to identify obvious code issues
- No consideration for security
- Cannot follow existing code patterns
- Poor communication about technical decisions

### Green Flags
- Asks clarifying questions
- Considers edge cases
- Thinks about scalability and security
- Provides multiple solution approaches
- Clean, documented code
- Tests their own work

### Adaptation Tips
- Adjust difficulty based on role level
- Allow internet access for realistic coding environment
- Focus on problem-solving process, not just solutions
- Encourage candidates to explain their thinking
- Provide hints if candidate is stuck but showing good approach

---

## Additional Resources

- Original Demo: http://andersbrownworth.com/blockchain/
- Bitcoin Whitepaper: https://bitcoin.org/bitcoin.pdf
- Blockchain 101 Video: https://www.youtube.com/watch?v=_160oMzblY8

---

**Last Updated:** November 2025
**Version:** 1.0
