[![DOI](https://img.shields.io/badge/DOI-10.82901%2Fnemar.nm000454-blue)](https://doi.org/10.82901/nemar.nm000454)

# A multimodal epilepsy dataset of paired 3-Tesla and 7-Tesla MRI and intracranial EEG

Redistribution on NEMAR of Pennsieve Discover dataset 662, version 1 (doi:10.26275/aosz-hj9u, licence CC-BY-4.0),
described in Sci Data 2026, doi:10.1038/s41597-026-07540-5. Authors: Alfredo Lucas, Mariam Josyula, Nina Petillo, Joost Wagenaar, Joel Stein, Sandhitsu Das, Nishant Sinha, Kathryn Davis, Tamjid Imtiaz.

## Description from the deposit

This dataset comprises multimodal paired 3T and 7T MRI scans collected from 30 patients with drug-resistant focal epilepsy along with intracranial EEG data.

## Details (from the paper and the deposit)

# c-penn3t7t: Penn 3T-7T Paired Epilepsy Imaging (Pennsieve 662 v1, doi:10.26275/aosz-hj9u)

**Task / paradigm.** 30 patients with drug-resistant focal epilepsy with paired 3T and 7T MRI (structural, resting fMRI); for the 15 who later had intracranial EEG, curated ictal and interictal epochs. Ictal epochs from EMU epileptologist notes; interictal epochs randomly selected while awake, >72 h after implantation, >=2 h before seizures and >=2/6/12 h after subclinical/focal/focal-to-bilateral seizures (doi:10.1038/s41597-026-07540-5 (Sci Data 2026, "A multimodal epilepsy dataset of paired 3-Tesla and 7-Tesla MRI and intracranial EEG", PMC13582879), Methods "Intracranial electroencephalography"). The deposit has 1261 EDF epochs under sourcedata/sub-*/ses-clinical01/ieeg with BIDS channels/events/ieeg.json sidecars.

**Recording system.** Sampling rates 500, 512 or 1024 Hz (paper Methods). Preprocessing described in the paper: 60 Hz notch (Q=30) and 3rd-order Butterworth 1-120 Hz, forward-backward (paper Methods). Amplifier not stated.

**Reference scheme.** Not stated in the paper text extracted (n/a); see the *_ieeg.json iEEGReference of the deposit.

**Electrode types.** ECoG grids and strips, SEEG depth electrodes, or both (paper Methods). SEEG contacts in white matter, outside the brain or noisy were excluded from all recordings (paper Methods).

**Localisation method.** Electrodes manually selected on post-implant CT, mapped to the pre-implant MRI with iEEG-recon; atlas assignment (AAL, Lausanne, DKT) per patient (paper Methods "Electrode localization"). The deposit holds, for 15 subjects, iEEG-recon outputs: electrode names, CT voxel, T1 (T00mri) mm/vox and MNI152NLin2009cAsym mm/vox coordinates, and for 12 subjects module3 atlas assignments (DKT antspynet, Atropos tissue, piriform ASHS).

## What this BIDS version contains and what was changed

- Raw data: the authors' BIDS tree (deposit `files/sourcedata/`) is the root: 30 subjects; `ses-research3T` and
  `ses-research7T` (anat, func resting state), `ses-clinical01` (iEEG, 15 subjects). Task and run labels of the iEEG
  epochs (`task-ictal<n>` / `task-interictal<n>`, one task label per curated epoch) are kept as released;
  `events.tsv` marks each epoch `ictal` or `interictal`.
- iEEG EDF files are the deposit files. Only the EDF header was changed: patient field set to `X X X X` and the day
  of the start date set to 01 (year, month and clock time kept); the data records are byte-identical (sha256 checked,
  `sourcedata/pennsieve-662-v1/edf_scrub.tsv`).
- The EDFs are not filtered: a 60 Hz line peak is present and power above 120 Hz is kept (spectral check of 40 files),
  so they are labelled raw (`SoftwareFilters` n/a). The notch and 1-120 Hz band-pass in the paper are its analysis steps.
- `_ieeg.json`: `PowerLineFrequency` 60 (paper Methods: 60 Hz notch; USA). `_channels.tsv` `units` follow the EDF
  physical dimension of each channel.
- Electrode positions: `*_space-MNI152NLin2009cAsym_electrodes.tsv` (15 subjects) from the deposit's iEEG-recon
  outputs (MNI152NLin2009cAsym mm, plus native pre-implant MRI (T00mri) mm columns, DKT/Atropos labels for 12 subjects).
  `region_coarse` and `atlas_label_aal3_coarse` are added by the NEMAR conversion and are derived values.
- Anatomical MRI: only the images released as defaced are in the BIDS tree; `defaced` is part of the `acq` label
  (`acq-3D_defaced` became `acq-3Ddefaced`, images without an acq label got `acq-defaced`).
  The MP2RAGE inversion images (`inv-1`/`inv-2`) are in `sourcedata/pennsieve-662-v1/files/sourcedata/` with their
  original names, because their sidecars lack `RepetitionTimeExcitation`/`RepetitionTimePreparation`, which BIDS
  requires and which we do not invent. Head images were checked by eye (sagittal projections) before release.
  Heudiconv placeholders were removed from sidecars (`TaskName` `TODO...` -> `rest`, `CogAtlasID` TODO dropped).
- Derivatives from the deposit: `derivatives/fmriprep3T`, `fmriprep7T`, `freesurfer3T`, `freesurfer7T`, `ieeg_recon`
  (same files, regrouped by pipeline).
- Everything else of the deposit is in `sourcedata/pennsieve-662-v1/` with its original relative path (deposit README,
  manifest, changelog, metadata spreadsheets, clinical CT / pre-implant MRI in iEEG-recon space, electrode voxel files).
- Calendar dates in text files are set to day 01 (`sourcedata/pennsieve-662-v1/date_scrub.tsv`).
- Not redistributed here (1249 files; list in `sourcedata/pennsieve-662-v1/withheld.tsv`):
  556 x fMRIPrep report figures/html (slices of the non-defaced T1w); 220 x FreeSurfer whole-head volume computed from non-defaced input (face visible, eye check); 216 x 7T B0 field maps released without defacing (magnitude images show the face); 119 x fMRIPrep preprocessed T1w (whole head; face visible for at least one subject, eye check); 45 x iEEG-recon head image: defacing incomplete for some subjects (eye check); 30 x iEEG-recon registration figure (slices of head MRI/CT); 18 x MP2RAGE UNIT1 image: removal of the face could not be confirmed on the eye check (noise-filled background); inversion images of the same scan are kept; 15 x clinical pre-implant T1w (T00mri): iEEG-recon copies of this scan still show the face for some subjects (eye check); 15 x clinical post-implant CT: facial skeleton/soft tissue not removed (eye check); 9 x FreeSurfer backup transforms (processing timestamp in the file name); 5 x anatomical image not released as defaced; 1 x spreadsheet has date/identifier-like cells or date formats (privacy; participants.tsv carries the clinical columns). They remain available from the original
  Pennsieve deposit.
- Missing values are `n/a`; nothing was added that the paper or the deposit does not state.

## Two truncated interictal epochs (corrected 2026-10-19, version 1.0.3)

Every `events.tsv` has one row that labels the epoch (`ictal` from 30 s to the end of the file, `interictal` from 0 s to
the end of the file); in 1,259 of the 1,261 files the row ends exactly at the end of the recording. In two interictal
epochs of `sub-PID02` (`task-interictal141517`, `task-interictal141764`, run-01) the deposit's row said 100 s, but the EDF
files (byte-identical to the deposit) are shorter: the header holds 91 and 57 data records of 1 s, and the
file sizes hold 91.9863 and 57.6822 records, i.e. the export stopped inside the next record (the incomplete
last record is ignored by EDF readers). Their EDF+ annotation channel is empty, while the two complete interictal files of the same
subject that were checked (`interictal141949`, `interictal146208`) carry an `interictal` annotation over the whole file. `RecordingDuration` already gives 91 s and 57 s. The rows ran 9 s and 43 s past the end of
the data; their `duration` is now the recorded length (91.0 s, 57.0 s), following the dataset's convention that the row
covers the epoch present in the file. The released files are kept in `sourcedata/events-original-2026-10-19/`. Signal files are unchanged; the
missing 9 s and 43 s of these two epochs are not in the deposit.
