---
tags:
  - ICON
  - project/surfwaves
Last Eddited: 2025-12-01
---
This is the file to track the meanings/changes in each ICON simulation:

# ICON-O Only

| **EXP ID**        | **Coupling** | **Forcing** | **Time**                | **Grid** | **Verti. Mixing**                               | **Import. INFO**                                                                                                                                                                                                    |
| ----------------- | ------------ | ----------- | ----------------------- | -------- | ----------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| mux2501_r2b7      | ICON-O only  | OMIP        | 2000-01-01 → 2030-01-01 | R2B7     | TKE (Default)                                   | Test run on the ICON-O standalone model; Using the default TKE scheme                                                                                                                                               |
| mux2501_r2b7_ck_0 | ICON-O only  | OMIP        | 2000-01-01 → 2030-01-01 | R2B7     | TKE (want to change but accidentaly to default) | Accidentally using the same default setting in TKE scheme (c_k=0.1)                                                                                                                                                 |
| mux2501_r2b7_ck_3 | ICON-O only  | OMIP        | 2000-01-01 → 2030-01-01 | R2B7     | TKE (change c_k)                                | Compare group to ‘mux2501_r2b7’, by only change the c_k from 0.1 to 0.3 (the c_k is the c_u in [ICON TKE Parameterisation](https://www.notion.so/ICON-TKE-Parameterisation-29a69691c52b80e7888ce8d81a77edb5?pvs=21) |
