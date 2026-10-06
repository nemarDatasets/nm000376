# Intracranial EEG from 9 adults during a memory-guided actions experiment — preprocessed epochs (BIDS derivative)

Stereo-EEG of **9 patients** with drug-resistant epilepsy (Motol Epilepsy Center, Prague) performing a delayed
memory-guided action task. This is a **derivative** dataset: it re-packages, without further processing, the authors'
preprocessed epochs released on Zenodo (record [13712628](https://doi.org/10.5281/zenodo.13712628), "Dataset: Intracranial
EEG recordings from 9 human adults during a memory-guided actions experiment", CC BY 4.0). The original continuous
recordings are not public.

Reference article: Moraresku S, Hammer J, Dimakopoulos V, Kajsova M, Janca R, Jezdik P, Kalina A, Marusic P, Vlcek K (2025).
*Neural Dynamics of Visual Stream Interactions During Memory-Guided Actions Investigated by Intracranial EEG.*
Neuroscience Bulletin 41(8):1347–1363. https://doi.org/10.1007/s12264-025-01371-x (correction: 10.1007/s12264-025-01453-w).
Analysis code: https://github.com/kamilvlcek/iEEG_scripts/tree/v3.1.0.

## Participants
Nine patients (4 women; mean ± SEM age 37 ± 3 years). Per-patient age, gender, handedness, education, epilepsy duration,
suspected seizure zone and pathology are taken from Supplementary Table S1 of the article (`participants.tsv`).
All had normal or corrected-to-normal vision.

## Task (`task-memact`)
Each trial: jittered fixation (1.9–2.1 s), encoding (2 s; an object — triangle or square — at one position), jittered
delay (3.9–4.1 s), action/recall (2 s; reach to the remembered position with a joystick). Condition 2 = "same" (remember
the position), 3 = "different" (remember position and identity, followed by a two-alternative button question).
160 delayed trials in blocks of 10, preceded by training trials (`Training` = 1); immediate trials are not in the release.
Stimulus presentation: PsychoPy3 v2020.1.3, 15.6-inch monitor at 60 Hz; task and iEEG synchronised by TTL pulses at each
trial start.

## What the authors' data contain (from `data_description.docx`)
- `fsample` 512 Hz; 170 epochs per patient, channels × 5069 samples, time −2.0 to 7.898 s, 0 = onset of the encoding phase.
- Bipolar channels between adjacent contacts (e.g. `A1-A2`); faulty contacts and contacts in the seizure onset zone or
  heterotopic cortex removed; notch filter at 50 Hz and harmonics; downsampled from 2048 Hz.
- `channelInfo` (name, amplifier number, signal type, MNI coordinates), `RjEpochChannel` (channels × epochs rejection
  labels) and `TrialInformationTable`.

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

## Licence
CC BY 4.0 (Zenodo metadata license id `cc-by-4.0`).
