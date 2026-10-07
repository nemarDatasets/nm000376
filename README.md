# Intracranial EEG from 9 adults during a memory-guided actions experiment — preprocessed epochs (BIDS derivative)

## Overview
Stereo-EEG of **9 patients** with drug-resistant epilepsy (Motol Epilepsy Center, Prague) performing a delayed
memory-guided action task. This is a **derivative** dataset: it re-packages, without further processing, the authors'
preprocessed epochs released on Zenodo (record [13712628](https://doi.org/10.5281/zenodo.13712628), concept DOI
10.5281/zenodo.13712627, "Dataset: Intracranial EEG recordings from 9 human adults during a memory-guided actions
experiment", CC BY 4.0). The original continuous recordings are not public.

Reference article: Moraresku S, Hammer J, Dimakopoulos V, Kajsova M, Janca R, Jezdik P, Kalina A, Marusic P, Vlcek K (2025).
*Neural Dynamics of Visual Stream Interactions During Memory-Guided Actions Investigated by Intracranial EEG.*
Neuroscience Bulletin 41(8):1347–1363. https://doi.org/10.1007/s12264-025-01371-x (PMC12314303; correction:
10.1007/s12264-025-01453-w; preprint: 10.1101/2024.08.20.608807).
Analysis code: https://github.com/kamilvlcek/iEEG_scripts/tree/v3.1.0.

## Participants / cohort
Nine patients (4 women; mean ± SEM age 37 ± 3 years) with drug-resistant epilepsy, enrolled at the Motol Epilepsy Center in
Prague, who underwent iEEG monitoring for localisation of the seizure onset zone before surgery. Per-patient age, gender,
handedness, education, epilepsy duration, suspected seizure zone and pathology are taken from Supplementary Table S1 of
the article (`participants.tsv`); the patient labels P1–P9 are identical in the release file names
(`memact_trials_P<N>.mat`) and in Table S1. All had normal or corrected-to-normal vision. `participants.tsv` also gives,
derived from the release, the number of bipolar channels, the number of electrode labels, and the number of channels with
negative/positive MNI x. The paper notes that more electrodes were implanted in the right hemisphere. For the analyses,
the paper used 369 channels in three regions (IPL: 137 channels in 8 patients; VTC: 169 in 9; hippocampus: 63 in 8; Table 1).
Recording years are not stated in the paper or the release.

## Task (`task-memact`)
Each trial: jittered fixation (1.9–2.1 s, white cross on dark grey), encoding (2 s; a central red cross and two objects:
two identical circles in the "same" condition, or a square and a triangle in the "different" condition; one object always
closer to the cross, distance ratio 1.5), jittered delay (3.9–4.1 s), action/recall (2 s, green cross; joystick reach from
the screen centre to the remembered position of the object closer to the cross). Condition 2 = "same" (remember the
position), 3 = "different" (remember position and identity, followed by a 2-s two-alternative question: green 'A' button =
triangle, red 'B' button = square). A response was correct if the reach trajectory came within a square of ± half the minimum
inter-object distance (10.5 % of the screen width) around the correct object; reaction time ran from the start of the action phase to reaching the correct area.
160 delayed trials in blocks of 10 (one condition per block, subject-controlled breaks, counterbalanced order), preceded by
a task presentation and training trials (`Training` = 1; shortened blocks of five trials per condition, with feedback);
160 immediate trials were also run but are not in the release.
Stimulus presentation: PsychoPy3 v2020.1.3, 15.6-inch TFT notebook monitor at 60 Hz, Xbox wireless controller; task and iEEG
synchronised by TTL pulses at each trial start.

## Acquisition
Eleven to fifteen semi-rigid depth electrodes per patient (diameter 0.8 mm, 8–18 contacts of 2 mm, 1.5 mm apart; DIXI
Medical Instruments), placed solely for clinical pre-surgical evaluation. Medical amplifiers Quantum, NeuroWorks, sampled
at 2048 Hz; recording reference: a white-matter contact per patient. Contact positions from post-implantation CT
co-registered to pre-implantation MRI, labelled by a neurologist and normalised to MNI space with SPM12 (paper, Methods).

## What the authors' data contain (from `data_description.docx`)
- `fsample` 512 Hz; 170 epochs per patient, channels × 5069 samples, time −2.0 to 7.898 s, 0 = onset of the encoding phase.
- Bipolar channels between adjacent contacts (e.g. `A1-A2`); faulty contacts and contacts in the seizure onset zone or
  heterotopic cortex removed; notch filter at 50 Hz and harmonics; downsampled from 2048 Hz.
- `channelInfo` (name, amplifier number, signal type, MNI coordinates), `RjEpochChannel` (channels × epochs rejection
  labels) and `TrialInformationTable`.

## Preprocessing already applied by the source
Downsampling 2048 → 512 Hz; notch filter (4th-order Butterworth band-stop, 1 Hz wide, at 50 Hz and harmonics, zero phase);
removal of bad contacts (visual inspection) and of contacts in the seizure onset zone or heterotopic cortex; bipolar
derivations between adjacent contacts (positions at the centre between the two contacts); epoching. Processing in MATLAB
R2018a (paper, Methods).

## BIDS packaging
- One BrainVision file per patient, the 170 epochs back to back (`RecordingType` = epoched, `EpochLength` = 9.900390625 s);
  `New Segment` markers at epoch starts and `encoding_onset` markers at time 0. Values = the stored float64 values
  rounded to float32 (relative error ≤ 6e-8). Units labelled µV (the release does not state a unit; amplitudes are
  microvolt-scale).
- `events.tsv`: one row per epoch at the encoding onset, with every `TrialInformationTable` column verbatim, the epoch
  number and the number of channels flagged for that epoch. `ResponseTime` contains negative values in the source; they
  are kept as is.
- `channels.tsv`: original label, amplifier number, signal type, and the authors' rejected epochs per channel
  (`rejected_epochs`); the flags are labels only — no data were removed.
- `electrodes.tsv` (`space-IXI549Space`): one MNI point per bipolar channel, as given by the authors (SPM12 normalization).
- `sourcedata/zenodo-13712628/`: the original `.mat` files, `data_description.docx` and the Zenodo record JSON, byte-identical.

## Known caveats
- Units are labelled µV but the release does not state a unit (amplitudes are microvolt-scale).
- `ResponseTime` contains negative values in the source; they are kept as is.
- Rejection flags (`rejected_epochs`, `Trials2Reject`) are the authors' labels only; no data were removed.
- Earlier versions of this README and the `*_ieeg.json` TaskDescription described the encoding display as one object
  (triangle or square) at one position; the paper's Methods describe two objects (two circles, or a square and a
  triangle) with the one closer to the cross to be remembered. The description was corrected on 2026-10-07.
- sub-P9: Supplementary Table S1 lists the suspected seizure zone as "R temporal, parietal, and occipital lobes", but all
  168 released channels have negative MNI x (left hemisphere). The source does not explain this; neither value was changed.
- The number of electrode labels in the channel names exceeds the paper's "eleven to fifteen" electrodes per patient for
  sub-P1, sub-P2, sub-P3 and sub-P9 (16–18 labels); not explained by the source.

## How to load
```python
from mne_bids import BIDSPath, read_raw_bids
bp = BIDSPath(root=".", subject="P1", task="memact", datatype="ieeg")
raw = read_raw_bids(bp)   # 170 epochs back to back; use events.tsv (encoding onsets) to re-epoch
```

## Citation
Moraresku S, et al. (2025) Neural Dynamics of Visual Stream Interactions During Memory-Guided Actions Investigated by
Intracranial EEG. Neurosci Bull 41(8):1347–1363. doi:10.1007/s12264-025-01371-x; data: doi:10.5281/zenodo.13712628.

## Provenance / sources
Zenodo record 13712628 (record JSON and `data_description.docx` under `sourcedata/`); Moraresku et al. 2025 full text
(Europe PMC PMC12314303: Methods, Table 1, Funding, Acknowledgements, Data Availability) and Supplementary Table S1.
Metadata enrichment 2026-10-07 (see CHANGES).

## Licence
CC BY 4.0 (Zenodo metadata license id `cc-by-4.0`).
