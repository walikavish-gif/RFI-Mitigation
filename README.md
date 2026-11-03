# RFI-Mitigation
Multi-stage RFI mitigation pipeline for dedispersed pulsar filterbank data (spectral kurtosis flagging, DM=0 discrimination, FFT-domain filtering, and off-pulse statistical masking).

## Pipeline stages

1. **Frequency-channel flagging** — identifies anomalous frequency channels using a moment-based statistic, calculated in chunks to reduce memory requirements. Flagged channels are interpolated using neighbouring channels.
2. **DM = 0 discrimination** - compares the dedispersed time series against a DM = 0 collapse; samples where broadband power dominates are flagged as terrestrial RFI
3. **FFT-domain filtering** - applies Savitzky-Golay based spurious spike detection in the Fourier domain, restricted to off-pulse samples to avoid modifying the pulsar signal
4. **Time-domain statistical masking** - block-wise off-pulse masking using the smoothed residual method
5. **Sigma clipping** - final pass replacing positive off-pulse spikes with Gaussian noise to preserve baseline statistics

The pipeline then folds the cleaned time series and compares the resulting pulse profile and signal-to-noise ratio (SNR) against the unmodified data.

## Usage

The notebook is configured for a single filterbank observation. Before running, update the following variables in the main pipeline cell:

```python
fil_file = ""                       # path to your filterbank file
dm       =                          # dispersion measure (pc cm⁻³)
period   =                          # pulse period (samples)
```

The notebook was developed in Google Colab. To run locally, remove the Drive mount cell and set `fil_file` to an absolute path.
