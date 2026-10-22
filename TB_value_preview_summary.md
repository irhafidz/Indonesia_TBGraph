# BPJS TB cohort: value preview of coded columns

Source: `BPJS_TB_Schema.ipynb`, section 8 (value preview), run on the TB2024 CSV exports, 22 October 2026.
Each table shows at most the eight most frequent values. Percentages are of all rows in the table.

Denominators:

| Table | Rows |
|---|---:|
| Membership | 118,747 members |
| FKTP Kapitasi | 380,429 clinic visits |
| FKRTL (hospital) | 235,671 hospital visits |
| FKTP Non-Kapitasi | 17,393 service lines |
| Diagnosis Sekunder | 294,733 secondary-diagnosis rows |

## Summary

1. **The cohort is mostly poor, publicly insured, and highly mortal.**
   - Subsidised members (PBI APBN + PBI APBD) are 47.6%.
   - 65.6% are entitled to ward class III.
   - 14.2% are recorded as deceased (MENINGGAL).
   - Only 70.0% are still active members.
2. **Clinic care is Puskesmas outpatient care.**
   - 74.7% of clinic visits are at a Puskesmas.
   - 94.5% are outpatient (RJTP).
3. **A third of clinic visits are wellness visits with no diagnosis.**
   - 132,820 visits (34.9%) are wellness visits (KUNJUNGAN SEHAT).
   - Exactly these 132,820 visits have no ICD-10 code, and FKP13 marks the same count as wellness visits.
   - The three columns agree, so the missing diagnoses are structural, not data errors.
4. **14.2% of clinic visits end in a referral.**
   - FKP13 = RUJUK LANJUT on 54,008 visits.
   - That is close to the 48,862 distinct referral-origin IDs seen in the hospital table.
   - So most referrals can be traced to a hospital visit; the exact match rate needs the linkage audit.
5. **Hospital primary diagnoses are almost all administrative Z-codes.**
   - The top eight primary diagnoses are all Z-codes, covering 72.1% of hospital visits.
   - Z09 (follow-up exam) alone covers 57.0%.
   - Z50 (rehabilitation) and Z49 (dialysis care) follow.
   - The clinical meaning sits in the secondary diagnoses.
   - This supports treating Z-codes as visit attributes rather than diagnosis nodes, or at least down-weighting them.
6. **TB and its comorbidities show up in the codes.**
   - A15 (respiratory TB, confirmed) is the sixth most common clinic diagnosis (2.3%).
   - A16 (respiratory TB, not confirmed) and B90 (sequelae of TB) rank 4th and 7th among secondary diagnoses.
   - Type 2 diabetes (E11) is the top secondary diagnosis (5.5%) and the second clinic diagnosis (4.2%).
   - Other chronic conditions near the top: COPD (J44), hypertension (I10), chronic kidney disease (N18).
7. **Hospital care is mostly outpatient follow-up.**
   - 90.0% of hospital visits are outpatient (RJTL).
   - 94.7% are discharged "Sehat".
   - Deaths in hospital are 0.4%, against 14.2% deceased in the membership file. Most deaths are not observed as hospital discharges.
8. **Non-capitation is a chronic-disease lab panel.**
   - 69.9% of service lines are promotive (PROMOTIF).
   - The top services are fasting glucose for the PRB/Prolanis programme, plus a lab panel that is ordered together:
     - lipids (HDL, LDL, triglycerides, total cholesterol)
     - ureum
     - creatinine
   - About 1,190 service lines each, consistent with diabetes and hypertension monitoring.

### Data-quality flags

- PSTV04 has one row coded `9`. This looks like an unlabelled or missing code.
- PSTV07 uses the literal string `MISSING` for 25 members instead of a blank.
- PSTV17 has 19 distinct statuses. Most are variants of "inactive" and could be grouped into active / deceased / inactive.
- FKL09 has 23 hospital-class labels that mix government classes (A–D), private "setara" classes and TNI/Polri classes. It needs a mapping to A/B/C/D.

## Membership

### PSTV04 · Relationship within family (6 distinct)

| Value | Meaning | Count | % |
|---|---|---:|---:|
| PESERTA | Main member | 66,206 | 55.8 |
| ANAK | Child | 28,855 | 24.3 |
| ISTRI | Wife | 17,101 | 14.4 |
| TAMBAHAN | Additional dependant | 4,061 | 3.4 |
| SUAMI | Husband | 2,523 | 2.1 |
| 9 | (unlabelled code) | 1 | 0.0 |

### PSTV05 · Sex (2 distinct)

| Value | Meaning | Count | % |
|---|---|---:|---:|
| LAKI-LAKI | Male | 64,384 | 54.2 |
| PEREMPUAN | Female | 54,363 | 45.8 |

### PSTV07 · Entitled ward class (4 distinct)

| Value | Count | % |
|---|---:|---:|
| KELAS III | 77,912 | 65.6 |
| KELAS II | 20,618 | 17.4 |
| KELAS I | 20,192 | 17.0 |
| MISSING | 25 | 0.0 |

### PSTV08 · Membership segment (5 distinct)

| Value | Meaning | Count | % |
|---|---|---:|---:|
| PBI APBN | Subsidised, central government | 31,258 | 26.3 |
| PPU | Wage earners | 29,897 | 25.2 |
| PBPU | Non-wage earners (self-paying) | 26,498 | 22.3 |
| PBI APBD | Subsidised, local government | 25,304 | 21.3 |
| BUKAN PEKERJA | Non-workers | 5,790 | 4.9 |

### PSTV17 · Membership status (19 distinct, top 8)

| Value | Meaning | Count | % |
|---|---|---:|---:|
| AKTIF | Active | 83,143 | 70.0 |
| MENINGGAL | Deceased | 16,884 | 14.2 |
| NON AKTIF KARNA PREMI | Inactive, unpaid premium | 7,645 | 6.4 |
| NON AKTIF DIAKHIR BULAN | Inactive at month end | 4,143 | 3.5 |
| TIDAK DITANGGUNG | Not covered | 1,779 | 1.5 |
| NON AKTIF OLEH PUSAT | Deactivated centrally | 1,432 | 1.2 |
| KELUAR KEMAUAN SENDIRI | Left voluntarily | 1,322 | 1.1 |
| NON AKTIF KARNA DATA GANDA | Inactive, duplicate record | 1,217 | 1.0 |

## FKTP Kapitasi (clinic visits)

### FKP08 · FKTP kind (6 distinct)

| Value | Meaning | Count | % |
|---|---|---:|---:|
| PUSKESMAS | Community health centre | 284,236 | 74.7 |
| KLINIK PRATAMA | Primary clinic | 70,659 | 18.6 |
| DOKTER UMUM | GP practice | 21,758 | 5.7 |
| LABORATORIUM | Laboratory | 2,018 | 0.5 |
| DOKTER GIGI | Dentist | 1,270 | 0.3 |
| JEJARING | Network facility | 488 | 0.1 |

### FKP10 · Service level (3 distinct)

| Value | Meaning | Count | % |
|---|---|---:|---:|
| RJTP | Primary outpatient | 359,358 | 94.5 |
| PROMOTIF | Promotive / preventive | 18,582 | 4.9 |
| RITP | Primary inpatient | 2,489 | 0.7 |

### FKP13 · Discharge status (7 distinct)

| Value | Meaning | Count | % |
|---|---|---:|---:|
| BEROBAT JALAN | Continue outpatient treatment | 191,289 | 50.3 |
| KUNJUNGAN SEHAT | Wellness visit | 132,820 | 34.9 |
| RUJUK LANJUT | Referred onwards | 54,008 | 14.2 |
| SEMBUH | Recovered | 2,118 | 0.6 |
| LAIN-LAIN | Other | 164 | 0.0 |
| PULANG PAKSA | Discharged against advice | 21 | 0.0 |
| MENINGGAL | Died | 9 | 0.0 |

### FKP22 · Visit type (2 distinct)

| Value | Meaning | Count | % |
|---|---|---:|---:|
| KUNJUNGAN SAKIT | Sick visit | 247,609 | 65.1 |
| KUNJUNGAN SEHAT | Wellness visit | 132,820 | 34.9 |

### FKP14A · ICD-10 3-digit code (1,358 distinct, top 8)

| Code | Meaning | Count | % |
|---|---|---:|---:|
| (missing) | Wellness visit, no diagnosis | 132,820 | 34.9 |
| J06 | Acute upper respiratory infection | 19,348 | 5.1 |
| E11 | Type 2 diabetes | 16,110 | 4.2 |
| J00 | Common cold | 13,104 | 3.4 |
| I10 | Essential hypertension | 11,356 | 3.0 |
| K30 | Dyspepsia | 8,685 | 2.3 |
| A15 | Respiratory TB, confirmed | 8,671 | 2.3 |
| R50 | Fever of unknown origin | 7,456 | 2.0 |

## FKRTL (hospital visits)

### FKL09 · Hospital class (23 distinct, top 8)

| Value | Count | % |
|---|---:|---:|
| RS Kelas C | 63,451 | 26.9 |
| RS Swasta Setara Type C | 59,351 | 25.2 |
| RS Kelas B | 44,544 | 18.9 |
| RS Kelas A | 14,827 | 6.3 |
| RS Swasta Setara Type D | 12,859 | 5.5 |
| RS Swasta Setara Type B | 12,587 | 5.3 |
| RS Kelas D | 6,628 | 2.8 |
| RS TNI Polri Kelas III | 5,178 | 2.2 |

### FKL10 · Service level (2 distinct)

| Value | Meaning | Count | % |
|---|---|---:|---:|
| RJTL | Hospital outpatient | 212,055 | 90.0 |
| RITL | Hospital inpatient | 23,616 | 10.0 |

### FKL14 · Discharge status (5 distinct)

| Value | Meaning | Count | % |
|---|---|---:|---:|
| Sehat | Discharged well | 223,190 | 94.7 |
| Tidak Tahu | Unknown | 8,034 | 3.4 |
| Rujuk | Referred | 3,128 | 1.3 |
| Meninggal | Died | 930 | 0.4 |
| Pulang Paksa | Discharged against advice | 389 | 0.2 |

### FKL17A · Primary diagnosis, ICD-10 3-digit (1,065 distinct, top 8)

| Code | Meaning | Count | % |
|---|---|---:|---:|
| Z09 | Follow-up examination after treatment | 134,424 | 57.0 |
| Z50 | Care involving rehabilitation | 15,346 | 6.5 |
| Z49 | Care involving dialysis | 5,651 | 2.4 |
| Z71 | Counselling and medical advice | 3,329 | 1.4 |
| Z86 | Personal history of certain diseases | 3,117 | 1.3 |
| Z48 | Other surgical follow-up care | 2,807 | 1.2 |
| Z03 | Observation for suspected disease | 2,750 | 1.2 |
| Z87 | Personal history of other diseases | 2,396 | 1.0 |

## FKTP Non-Kapitasi (service lines)

### PNK11 · Service level (3 distinct)

| Value | Meaning | Count | % |
|---|---|---:|---:|
| PROMOTIF | Promotive / preventive | 12,155 | 69.9 |
| RJTP | Primary outpatient | 3,089 | 17.8 |
| RITP | Primary inpatient | 2,149 | 12.4 |

### PNK16 · Service / procedure name (47 distinct, top 8)

| Value | Meaning | Count | % |
|---|---|---:|---:|
| Gula Darah Puasa (GDP) - PRB/Prolanis | Fasting glucose, chronic-disease programme | 2,086 | 12.0 |
| Rawat Inap di R. Perawatan Biasa | Inpatient stay, standard ward | 1,890 | 10.9 |
| Kolesterol HDL | HDL cholesterol | 1,194 | 6.9 |
| Kolesterol LDL | LDL cholesterol | 1,194 | 6.9 |
| Kolesterol Trigliserida | Triglycerides | 1,192 | 6.9 |
| Kolesterol Total | Total cholesterol | 1,192 | 6.9 |
| Ureum | Urea | 1,190 | 6.8 |
| Kreatinin | Creatinine | 1,189 | 6.8 |

## Diagnosis Sekunder

### FKL24A · Secondary diagnosis, ICD-10 3-digit (1,356 distinct, top 8)

| Code | Meaning | Count | % |
|---|---|---:|---:|
| E11 | Type 2 diabetes | 16,090 | 5.5 |
| M54 | Dorsalgia (back pain) | 10,750 | 3.6 |
| J44 | COPD | 10,431 | 3.5 |
| A16 | Respiratory TB, not confirmed | 9,994 | 3.4 |
| I10 | Essential hypertension | 9,956 | 3.4 |
| N18 | Chronic kidney disease | 8,507 | 2.9 |
| B90 | Sequelae of tuberculosis | 8,220 | 2.8 |
| K30 | Dyspepsia | 7,946 | 2.7 |
