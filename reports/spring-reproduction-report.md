# Spring Experiment Reproduction Report

## Overview

This report reproduces the `spring` experiment from the Hamiltonian Neural Networks repository.
The goal was to:

1. Train the baseline neural network and the HNN on the ideal mass-spring system.
2. Re-run the repository notebook `analyze-spring.ipynb`.
3. Compare the reproduced metrics against the reference values in `README.md`.
4. Save the generated figures and summarize the conclusions.

## Reproduction Workflow

### 1. Training

The experiment was trained with the repository's default spring training script:

```bash
PYTHONPATH=/tmp/hnn-deps python3 experiment-spring/train.py --verbose
PYTHONPATH=/tmp/hnn-deps python3 experiment-spring/train.py --baseline --verbose
```

This produced:

- `experiment-spring/spring-hnn.tar`
- `experiment-spring/spring-baseline.tar`

These `.tar` files are PyTorch model weights saved via `torch.save(model.state_dict(), path)`.

### 2. Notebook Analysis

The repository notebook was executed in place:

- `analyze-spring.ipynb`

It regenerated the analysis outputs and saved figures to:

- `figures/spring.pdf`
- `figures/spring-integration.pdf`

For embedding in this report, PNG copies were created:

- `reports/spring-assets/spring.png`
- `reports/spring-assets/spring-integration.png`

## Metric Comparison

Reference values below come from `README.md`.

| Metric | Model | Reproduced | README Reference |
| --- | --- | ---: | ---: |
| Train loss | Baseline NN | `3.7099e-02 +/- 1.9133e-03` | `3.7134e-02 +/- 1.9143e-03` |
| Train loss | HNN | `3.6928e-02 +/- 1.9128e-03` | `3.6933e-02 +/- 1.9128e-03` |
| Test loss | Baseline NN | `3.6585e-02 +/- 1.8572e-03` | `3.6656e-02 +/- 1.8652e-03` |
| Test loss | HNN | `3.5916e-02 +/- 1.8302e-03` | `3.5928e-02 +/- 1.8328e-03` |
| Energy MSE | Baseline NN | `1.6800e-01 +/- 2.06e-02` | `1.7077e-01 +/- 2.06e-02` |
| Energy MSE | HNN | `3.7659e-04 +/- 7.90e-05` | `3.8416e-04 +/- 6.53e-05` |

## Figure Outputs

### Vector Field and Trajectory Comparison

![Spring Vector Field](spring-assets/spring.png)

This figure compares:

- ground-truth spring dynamics data
- baseline NN learned vector field
- HNN learned vector field

### Long-Horizon Integration and Energy

![Spring Integration](spring-assets/spring-integration.png)

This figure compares:

- phase-space trajectory prediction
- coordinate MSE over time
- HNN learned conserved quantity
- physical total energy over time

## Interpretation

### What matched well

- The reproduced train and test losses are extremely close to the repository reference values.
- The energy MSE result shows the expected gap between the baseline and the HNN.
- The HNN again preserves energy orders of magnitude better than the baseline NN.

### What this means

- Both models can fit local derivatives on the spring task with similar train/test loss.
- The main advantage of the HNN is not just lower pointwise loss.
- The main advantage appears in long-horizon integration and energy preservation.
- This matches the core claim of the paper and repository: structure helps preserve physically meaningful behavior over time.

## Final Conclusion

The `spring` experiment was successfully reproduced.

Key outcome:

- The reproduced scalar metrics closely match the repository's reported numbers.
- The qualitative plots show the expected behavior.
- The HNN preserves the spring system's energy far better than the baseline neural network during integration.

This reproduction is therefore consistent with the repository's original claim for the ideal mass-spring task.
