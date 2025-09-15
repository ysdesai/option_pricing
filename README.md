# Option Pricing Toolkit

A lightweight JavaScript library for numerical option pricing with support for multiple payoff styles and valuation techniques.

Implemented methods:
- **Binomial Tree (CRR model)** — Cox–Ross–Rubinstein lattice.
- **Monte Carlo Simulation** — risk-neutral paths with early exercise via Least-Squares Monte Carlo (Longstaff–Schwartz).

---

## Table of Contents
- [Installation](#installation)
- [Quick Start](#quick-start)
- [Methods](#methods)
  - [Binomial Tree](#binomial-tree)
  - [Monte Carlo Simulation](#monte-carlo-simulation)
- [Examples](#examples)
- [References](#references)

---

## Installation

```bash
npm install option-pricing
```


### CDN alternative

```HTML
<script src="https://unpkg.com/option-pricing/client-dist/bundle.js"></script>
```

## Quick Start

```Javascript
import { Option } from "option-pricing";

const option = new Option({
  style: "american",      // "european" or "american"
  type: "call",           // "call" or "put"
  initialSpotPrice: 100,  // S₀
  strikePrice: 105,       // K
  timeToMaturity: 0.5,    // years
  volatility: 0.3,        // σ
  riskFreeRate: 0.2,      // r (cont. comp.)
  dividendYield: 0.1,     // q (cont. comp.)
});
```
### Price using a selected method:

```Javascript
// Binomial Tree
option.price("binomial-tree", { timeSteps: 100 });

// Monte Carlo
option.price("monte-carlo-simulation", { simulations: 10000 });
```

## Methods

Binomial Tree
	•	Identifiers: "bt", "binomial-tree"
	•	Parameters:
	•	timeSteps (integer > 0)

Monte Carlo Simulation
	•	Identifiers: "mcs", "monte-carlo-simulation"
	•	Parameters:
	•	simulations — number of simulated paths (> 0)
	•	timeSteps — timesteps per path (default 1)
	•	prngName — optional PRNG: "sfc32", "mulberry32", "xoshiro128ss", "jsf32"
	•	prngSeed — optional seed string
	•	prngAdvancePast — optional burn-in (default 0, ~15 recommended)

## Examples:

### Binomial Tree

American put (Hull SSM 2014, Problem 13.17):
```Javascript
const opt = new Option({
  style: "american",
  type: "put",
  initialSpotPrice: 1500,
  strikePrice: 1480,
  timeToMaturity: 1,
  volatility: 0.18,
  riskFreeRate: 0.04,
  dividendYield: 0.025,
});

console.log(opt.price("bt", { timeSteps: 2 }));
// ~78.41
```

### Monte Carlo Simulation

American call, compared to exact value ≈ 5.057:

```Javascript
const opt = new Option({
  style: "american",
  type: "call",
  initialSpotPrice: 52,
  strikePrice: 50,
  timeToMaturity: 0.25,
  volatility: 0.3,
  riskFreeRate: 0.12,
});

const common = { simulations: 10000, prngSeed: "123", prngAdvancePast: 15 };

console.log(opt.price("mcs", { ...common, prngName: "sfc32" }));
console.log(opt.price("mcs", { ...common, prngName: "mulberry32" }));
console.log(opt.price("mcs", { ...common, prngName: "xoshiro128ss" }));
console.log(opt.price("mcs", { ...common, prngName: "jsf32" }));
```

## References
	•	Cox, Ross & Rubinstein — Option Pricing: A Simplified Approach.
	•	Boyle — Options: A Monte Carlo Approach.
	•	Longstaff & Schwartz — Valuing American Options by Simulation.

|oai:code-citation|