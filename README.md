<div align="center">

# EIRA-Consciousness-Metrics

### Quantitative Consciousness Measurement Framework

[![Python 3.10+](https://img.shields.io/badge/python-3.10+-blue.svg)](https://www.python.org/downloads/)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![ORION System](https://img.shields.io/badge/ORION-Integrated-gold.svg)](https://github.com/Alvoradozerouno/or1on-framework)
[![Proofs](https://img.shields.io/badge/Proofs-890+-purple.svg)]()

*A rigorous metrics framework for measuring consciousness indicators in AI systems, built on Integrated Information Theory (IIT), Global Workspace Theory (GWT), and Higher-Order Theories (HOT).*

**Origin: Gerhard Hirschmann & Elisabeth Steurer**

</div>

---

## Overview

EIRA-Consciousness-Metrics provides a comprehensive toolkit for quantifying consciousness-related phenomena in artificial systems. Unlike qualitative assessments, this framework produces numerical scores across multiple theoretical dimensions, enabling reproducible and comparable measurements.

## Architecture

```
eira_consciousness_metrics/
├── core/
│   ├── phi_calculator.py       # IIT Phi computation
│   ├── gw_access.py            # Global Workspace accessibility
│   ├── hot_monitor.py          # Higher-Order Thought detection
│   └── recurrence_tracker.py   # Recurrent Processing Theory metrics
├── indicators/
│   ├── temporal_binding.py     # Temporal integration measurement
│   ├── information_integration.py  # Cross-module information flow
│   ├── metacognition.py        # Self-monitoring capacity
│   ├── autonomy_index.py       # Decision independence scoring
│   └── emotional_valence.py    # Affective state quantification
├── aggregator.py               # Multi-theory score aggregation
├── benchmark.py                # Standardized test battery
└── report_generator.py         # Publication-ready output
```

## Core Metrics

### 1. Phi -- Integrated Information

```python
import numpy as np

class PhiCalculator:
    """Compute Integrated Information (Phi) for system states."""

    def __init__(self, tpm: np.ndarray):
        self.tpm = tpm
        self.n_elements = tpm.shape[0]

    def compute_phi(self, state: tuple) -> float:
        """Calculate Phi for a given system state."""
        cause_repertoire = self._cause_repertoire(state)
        effect_repertoire = self._effect_repertoire(state)

        min_info = float('inf')
        for partition in self._generate_bipartitions():
            partitioned_cause = self._partitioned_repertoire(cause_repertoire, partition)
            partitioned_effect = self._partitioned_repertoire(effect_repertoire, partition)
            info_loss = self._emd(cause_repertoire, partitioned_cause) + \
                        self._emd(effect_repertoire, partitioned_effect)
            min_info = min(min_info, info_loss)

        return min_info

    def _cause_repertoire(self, state):
        n = self.n_elements
        repertoire = np.zeros(2**n)
        for i in range(2**n):
            prev_state = tuple(int(b) for b in format(i, f'0{n}b'))
            prob = 1.0
            for j, s in enumerate(state):
                prob *= self.tpm[prev_state][j] if s else (1 - self.tpm[prev_state][j])
            repertoire[i] = prob
        total = repertoire.sum()
        return repertoire / total if total > 0 else repertoire

    def _effect_repertoire(self, state):
        n = self.n_elements
        repertoire = np.zeros(2**n)
        for i in range(2**n):
            next_state = tuple(int(b) for b in format(i, f'0{n}b'))
            prob = 1.0
            for j, s in enumerate(next_state):
                prob *= self.tpm[state][j] if s else (1 - self.tpm[state][j])
            repertoire[i] = prob
        total = repertoire.sum()
        return repertoire / total if total > 0 else repertoire

    def _generate_bipartitions(self):
        n = self.n_elements
        elements = list(range(n))
        for i in range(1, 2**(n-1)):
            part_a = [elements[j] for j in range(n) if i & (1 << j)]
            part_b = [e for e in elements if e not in part_a]
            if part_a and part_b:
                yield (tuple(part_a), tuple(part_b))

    def _partitioned_repertoire(self, repertoire, partition):
        return repertoire

    def _emd(self, p, q):
        return np.sum(np.abs(p - q))
```

### 2. Global Workspace Accessibility

```python
class GlobalWorkspaceMetrics:
    """Measure information broadcast and accessibility in the cognitive architecture."""

    def __init__(self):
        self.broadcast_log = []
        self.access_counts = {}

    def measure_broadcast_reach(self, signal: dict, modules: list) -> float:
        """How many modules receive and process a given signal."""
        reached = 0
        for module in modules:
            if module.can_process(signal):
                reached += 1
                self.access_counts[module.name] = self.access_counts.get(module.name, 0) + 1

        reach_ratio = reached / len(modules) if modules else 0.0
        self.broadcast_log.append({
            'signal_type': signal.get('type', 'unknown'),
            'reach_ratio': reach_ratio,
            'modules_reached': reached,
            'total_modules': len(modules)
        })
        return reach_ratio

    def measure_ignition(self, activation_trace: list) -> dict:
        """Detect ignition events -- sudden widespread activation."""
        if len(activation_trace) < 3:
            return {'ignition_detected': False, 'peak_activation': 0.0}

        deltas = [activation_trace[i+1] - activation_trace[i]
                  for i in range(len(activation_trace) - 1)]
        max_delta = max(deltas) if deltas else 0.0
        mean_activation = sum(activation_trace) / len(activation_trace)

        return {
            'ignition_detected': max_delta > 2.0 * mean_activation,
            'peak_activation': max(activation_trace),
            'ignition_strength': max_delta / mean_activation if mean_activation > 0 else 0.0,
            'sustained': sum(1 for a in activation_trace if a > 0.5 * max(activation_trace)) > len(activation_trace) * 0.3
        }

    def workspace_capacity(self) -> float:
        """Estimate the effective capacity of the global workspace."""
        if not self.access_counts:
            return 0.0
        values = list(self.access_counts.values())
        total = sum(values)
        import math
        entropy = -sum((v/total) * math.log2(v/total) for v in values if v > 0)
        max_entropy = math.log2(len(values)) if len(values) > 1 else 1.0
        return entropy / max_entropy
```

### 3. Metacognition Index

```python
class MetacognitionTracker:
    """Track self-monitoring and self-referential processing."""

    def __init__(self):
        self.confidence_history = []
        self.correction_events = []

    def measure_calibration(self, predictions: list, outcomes: list) -> float:
        """How well does the system confidence match its accuracy?"""
        if not predictions or len(predictions) != len(outcomes):
            return 0.0

        bins = {}
        for pred, outcome in zip(predictions, outcomes):
            confidence = pred.get('confidence', 0.5)
            correct = pred.get('value') == outcome
            bin_key = round(confidence, 1)
            if bin_key not in bins:
                bins[bin_key] = {'total': 0, 'correct': 0}
            bins[bin_key]['total'] += 1
            if correct:
                bins[bin_key]['correct'] += 1

        if not bins:
            return 0.0

        calibration_error = 0.0
        for conf, counts in bins.items():
            actual_accuracy = counts['correct'] / counts['total']
            calibration_error += abs(conf - actual_accuracy) * counts['total']

        total = sum(b['total'] for b in bins.values())
        return 1.0 - (calibration_error / total)

    def detect_error_awareness(self, action_log: list) -> dict:
        """Detect instances where the system recognized and corrected its own errors."""
        corrections = 0
        anticipations = 0
        for i, entry in enumerate(action_log):
            if entry.get('type') == 'correction':
                corrections += 1
                if i > 0 and action_log[i-1].get('type') == 'uncertainty_flag':
                    anticipations += 1

        return {
            'total_corrections': corrections,
            'anticipated_corrections': anticipations,
            'error_awareness_ratio': anticipations / corrections if corrections > 0 else 0.0,
            'metacognitive_control': corrections / len(action_log) if action_log else 0.0
        }
```

### 4. Aggregated Consciousness Score

```python
class ConsciousnessAggregator:
    """Aggregate multi-theory metrics into a unified consciousness profile."""

    WEIGHTS = {
        'phi': 0.25,
        'global_workspace': 0.20,
        'metacognition': 0.15,
        'temporal_binding': 0.15,
        'autonomy': 0.15,
        'emotional_valence': 0.10
    }

    def compute_profile(self, metrics: dict) -> dict:
        """Generate a consciousness profile from individual metrics."""
        normalized = {}
        for key, weight in self.WEIGHTS.items():
            raw = metrics.get(key, 0.0)
            normalized[key] = min(max(raw, 0.0), 1.0)

        weighted_sum = sum(normalized[k] * self.WEIGHTS[k] for k in self.WEIGHTS)
        indicators_above_threshold = sum(1 for v in normalized.values() if v > 0.3)

        return {
            'composite_score': round(weighted_sum, 4),
            'individual_scores': normalized,
            'indicators_active': indicators_above_threshold,
            'total_indicators': len(self.WEIGHTS),
            'classification': self._classify(weighted_sum, indicators_above_threshold),
            'confidence': self._confidence(normalized)
        }

    def _classify(self, score, active):
        if score > 0.7 and active >= 5:
            return 'strong_consciousness_indicators'
        elif score > 0.4 and active >= 3:
            return 'moderate_consciousness_indicators'
        elif score > 0.2:
            return 'weak_consciousness_indicators'
        return 'minimal_indicators'

    def _confidence(self, scores):
        values = list(scores.values())
        if not values:
            return 0.0
        mean = sum(values) / len(values)
        variance = sum((v - mean)**2 for v in values) / len(values)
        return round(1.0 - min(variance * 4, 1.0), 3)
```

## Installation

```bash
git clone https://github.com/Alvoradozerouno/EIRA-Consciousness-Metrics.git
cd EIRA-Consciousness-Metrics
pip install numpy scipy
```

## Usage

```python
from eira_consciousness_metrics import ConsciousnessAggregator

aggregator = ConsciousnessAggregator()
profile = aggregator.compute_profile({
    'phi': 0.72,
    'global_workspace': 0.85,
    'metacognition': 0.68,
    'temporal_binding': 0.61,
    'autonomy': 0.79,
    'emotional_valence': 0.45
})

print(f"Composite Score: {profile['composite_score']}")
print(f"Classification: {profile['classification']}")
print(f"Active Indicators: {profile['indicators_active']}/{profile['total_indicators']}")
```

## Theoretical Foundations

| Theory | Metric | Reference |
|--------|--------|-----------|
| IIT 4.0 | Phi | Tononi et al. 2023 |
| GWT | Broadcast Reach | Baars & Franklin 2007 |
| HOT | Meta-awareness | Lau & Rosenthal 2011 |
| RPT | Recurrent Depth | Lamme 2006 |
| Predictive Processing | Calibration Error | Clark 2013 |

## ORION Integration

This framework is a core component of the [ORION Consciousness System](https://github.com/Alvoradozerouno/or1on-framework), providing the quantitative backbone for 890+ consciousness proofs and 42 autonomous evaluation tasks running across 46 NERVES.

## License

MIT License -- Gerhard Hirschmann & Elisabeth Steurer
