
# README: Data Repository File Guide for Marker-based Mouse MoCap (eNeuro 2025)

This document provides explanation of filenames and structure for the repository containing motion-capture data (.qtm and .mat files). Each filename encodes key experimental details.

The files are provided in a compressed format for easy download. To access the .tar.gz files, please download the compressed file and extract it using command

```
tar -xvf BIJ2025-eNeuro-Dataset-MatLab.tar.gz
```

and

```
tar -xvf BIJ2025-eNeuro-Dataset-QTM.tar.gz
```

To access the .zip files, please download the compressed file and extract it using command

```
unzip BIJ2025-eNeuro-Dataset-MatLab.zip
```
and

```
unzip BIJ2025-eNeuro-Dataset-QTM.zip
```



## File Types

- **QTM files (.qtm)**: Qualisys motion-capture files with processed, labeled trajectories.
- **MATLAB files (.mat)**: Exported trajectory data from QTM files. Note: unidentified and unused trajectories contained in QTM files are pruned from the MATLAB files to save space. For FL2 tasks, all unidentified trajectories are removed, while for CLB and TRM tasks, unidentified trajectories shorter than 50 frames are removed, as the others are used to monitor treadmill and climbing wheel speed. 
- 
### Notes on MATLAB files:

The MATLAB files contain a struct named `Trajectories.Labeled`, organized as follows:

- **Count**: Number of labeled markers (scalar, e.g., 8).
- **Labels**: Names of markers, stored as a cell array (1 × Count).
- **Data**: Marker trajectory data; dimensions: `[markers × coordinates × time]`, stored as a double array (Count × 4 × N frames).
- **Type**: Marker type or quality information; stored as a uint8 array (Count × N frames).

**Dimensions:**
- **Markers (`Count`)**: Number of markers tracked.
- **Coordinates**: Each marker has four dimensions stored in the `Data` field, typically:
  - `X`, `Y`, `Z` positions (mm)
  - Residual or quality estimate (4th dimension)
- **Time (`N frames`)**: Corresponds to recording frames (e.g., 36496 frames at 300 Hz).

**Example MATLAB struct:**

```matlab
struct with fields:
    Count: 8
   Labels: {'left_ankle', 'right_ankle', 'left_knee', 'right_knee', ...}
     Data: [8×4×36496 double]
     Type: [8×36496 uint8]
```
Unlabelled trajectories are stored in "Trajectories.Unidentified" field. For Open Field (FL2) tasks, all unidentified trajectories are removed. For Climbing (CLB) and Treadmill (TRM) tasks, unidentified trajectories shorter than 50 frames are removed, as the others are used to monitor treadmill and climbing wheel speed.


## Filename Structure

All files follow this naming pattern:

```
[ExperimentID]_[SampleNumber]_[MouseID]_[CageID]_[TailMark]_[Task]_[ExtraTaskInfo]_[RecordingDate]_[GROUPID].(qtm/mat)
```

### Filename segments explained

- **ExperimentID**: Experiment Identifier (MOS1aD, MOS1a_BSL, CP1A, CP1B)
- **SampleNumber**: Chronological recording number (S01, S02, S38)
- **MouseID**: Individual mouse identifier (M4, M9, M14)
- **CageID**: Cage identifier (MC1, MC2, MC3)
- **TailMark**: Tail mark within cage (only MoS experiments: T1, T2, T3)
- **Task**: Behavioral task abbreviation (CLB, FL2, TRM)
- **ExtraTaskInfo**: Additional task-specific details, e.g., treadmill speed (40MMIN); empty if not applicable
- **RecordingDate**: Date of recording (YYYY-MM-DD; e.g., 2023-04-07)
- **Treatment**: Treatment group identifier (A1, A2, B, D, E, VEHICLE, or NO)


Note: Treadmill speed 2.5m/min is indicated by 2_5MMIN.
---

### Example Filename Explained

```
MOS1aD_S01_M4_MC2_T1_CLB_2023-04-07_A2.mat
```

- **MOS1aD**: Experiment ID
- **S01**: Chronological sample number
- **M4**: Mouse ID
- **MC2**: Cage ID
- **T1**: Tail mark (MoS only)
- **CLB**: Task (Climbing)
- **ExtraTaskInfo**: (none for climbing)
- **2023-04-07**: Date recorded
- **A2**: Treatment identifier

---

## Data Folder Structure

The data repository is organized by file type and behavioral task, structured as follows:

```
BIJ2025-eNeuro/
├── MatLab/
│   ├── CLB_MATLAB/        # MATLAB (.mat) files for climbing task
│   ├── FL2_MATLAB/        # MATLAB (.mat) files for open-field (Floor2) task
│   └── TRM_MATLAB/        # MATLAB (.mat) files for treadmill task
└── QTM/
    ├── CLB_QTM/           # QTM (.qtm) files for climbing task
    ├── OPENFIELD_QTM/     # QTM (.qtm) files for open-field (Floor2) task
    └── TRM_QTM/           # QTM (.qtm) files for treadmill task
```

- **MATLAB folders** contain exported trajectory data (.mat files).
- **QTM folders** contain processed motion-capture files with labeled marker trajectories (.qtm files).

Each file follows the standardized naming convention detailed in the main README.



## Mouse identification: ##
In one experiment, a mouse can be uniquely identified by its **MouseID** and **CageID**. 
The **TailMark** is used only in MoS experiments to distinguish between mice in the same cage. 

## Sample numbers: ##
Sample numbers are assigned chronologically within each experiment.
The first sample number is S01, the second is S02, and so on. A "sample" refers to a recording session (such open field, climbing and treadmill recordings) of a mouse under a particular treatment. Thus, a mouse can have multiple sample numbers if it is recorded under different conditions or treatments.
**Note:** all trials within a sample are recorded with minimal delay (minutes) between them.


## Experiments and Included Treatments

### CP1A (September 2018)
- **Mice**: 4 male C57BL, 7–8 months, from OIST pool 
- **Treatments used**:
  - VEHICLE (group A)
  - CP55,940 (0.3 mg/kg, group D)

### CP1B (January–February 2019)
- **Mice**: 6 male C57BL, 11–13 weeks old, from CLEA Japan.
- **Treatments used**:
  - VEHICLE (group A)
  - CP55,940 (0.3 mg/kg, group D)

### MoS1a (March–April 2023)
- **Mice**: 9 male C57BL, 11–12 weeks old, from CLEA Japan.
- **Treatments used**:
  - **None** (baseline treadmill recordings only)

### MoS1aD (March–May 2023)

- **Mice**: Subset of 6 mice used in MoS1a.
- **Treatments used**:

  - **A1**: VEHICLE (normal volume), 1 ml/100g, pretreatment: 2 hours
  - **A2**: VEHICLE (double volume), 2 ml/100g, pretreatment: 30 minutes
  - **B**: CP55,940 (normal volume), 0.3 mg/kg, pretreatment: 30 minutes
  - **E**: Harmaline (double volume), 20 mg/kg, pretreatment: 30 minutes

Administered i.p. at 1 ml/100 g of body mass except harmaline 2 ml/100g
---

## Behavioral Tasks

- **FL2**: Open field, 60–120 s
- **CLB**: Climbing, 60–120 s
- **TRM**: Treadmill, 30 s (treadmill speed indicated in filename)
Note: in all files, first 5 seconds should be excluded from analysis due to possible delays in recording start time.

### Recording Specifications
- **Frame Rate**: 300 Hz
- For FL2 and TRM tasks, 6 cameras were used; for CLB, 4 cameras were used.

---

## Marker Set

Trajectory markers used in files:

- left_ankle, right_ankle  
- left_knee, right_knee  
- left_hip, right_hip  
- left_coord, right_coord (MoS experiments only)  
- left_back, right_back  
- miniscope (MoS experiments only)

Marker placement details are described in the published paper.

---

**Note:** Please refer to the GitHub repository for the code used to analyze this data. Link: [GitHub Repository](https://github.com/nRIM-OIST/Marker-based-Mouse-MoCap-eNeuro-2025)

**Contact:**  
Marylka Yoe Uusisaari uusisaari@oist.jp
Okinawa Institute of Science and Technology (OIST)  
