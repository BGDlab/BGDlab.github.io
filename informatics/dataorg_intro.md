Filedump (dicom)
- HM91
    - GCPDicom (garbage label)
- HM92_1234
    - 1234 (proc ord id)
- HM93
    - 1345 
    - 3456
    - 9085
- HM94 --> not included
    - GCPDicom
    - 7790483912084

First step: rename the sessions, keeping dicom format

sourcedata
- HM91
    - procId345_ageDays00001
        - 72348921
            - 58493021
                - ... .dcm
- HM92
    - procId1234_ageDays01000
- HM93
    - procId1345_ageDays01000
    - procId3456_ageDays01000
    - procId9085_ageDays02000

Second step: heudiconv, ideally after this step we can get rid of `sourcedata` (and `dicom`)

rawdata
- sub-HM91
    - ses-procId345_ageDays00001
    - ses-GCPDicom_438129043_1473214123_1432145436_5425432523
        - anat (.nii .json)
            - 
        - dwi
- HM92
    - procId1234_ageDays01000
- HM93
    - procId1345_ageDays01000
    - procId3456_ageDays01000
    - procId9085_ageDays02000

Third step: CuBIDs - correctly identifying scans we know we want to analyze
