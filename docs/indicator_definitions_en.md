# Indicator Definitions, Statistical Scope, and Calculations

English companion to `indicator_definitions_zh.md` and README Sections 3–4.
**Published statistical series, author-derived indicators, and proxy measures are
different concepts.** Similar field names do not make them interchangeable.
See `data_dictionary_en.csv` for every table/column definition, unit, and missing count. The bilingual dictionary is `data_dictionary.csv`.

## 1. Welfare data: not a harmonized agricultural net-profit panel

| Export | Measure and unit | Actual coverage | Scope and processing |
|---|---|---|---|
| `data/welfare/family_operating_income_old.csv` | `Family_Operating_Net_Income_Old`, CNY/person | 314 observations: 31 provinces in 2004–2013, plus Hunan in 2000–2003 | Family-operation component of rural household per-capita net income; retained as supplied, not recalculated as agricultural output minus costs |
| `data/welfare/net_operating_income_new.csv` | `Net_Operating_Income_New`, CNY/person | 341 observations: 31 provinces in 2014–2024 | Net operating-income component of rural residents' per-capita disposable income; retained as supplied |
| `data/welfare/primary_industry_value_added.csv` | `Primary_Industry_Value_Added`, 10^8 CNY, current prices | 775 observations: 31 provinces in 2000–2024 | Separate view of the master series; all 775 values agree exactly, not an independent source |

### Interpretation and limitations

1. The old and new income definitions differ. This workbook uses the old definition
   through 2013 and the new definition from 2014. **Do not concatenate them into a
   harmonized series.** The survey-reform background is attributed to the supplier;
   other statistical products need not have an identical transition year.
2. Rural household operating income can include non-agricultural operations. It is
   neither aggregate provincial agricultural net profit nor total disposable income.
3. Primary-industry value added is not net profit, household income, or profit per
   mu. Do not mix its aggregate 10^8-CNY unit with CNY/person.
4. The old table has no supplied observations for the other 30 provinces in
   2000–2003: 120 absent province-years, not 120 zeros. These become missing values
   only after a left join onto a complete key set.
5. The supplied source note lists national/provincial yearbook editions, tables,
   and retrieval records. It reports OCR and sampled manual checking for the new
   income series. 


## 2. Policy variables

### 2.1 Small-irrigation priority-county treatment intensity

```text
treat_intensity = round(cum_xns_county / total_county_count, 4)
```

- `cum_xns_county`: cumulative priority-county count stored in the intensity sheet.
- `total_county_count`: the stored county denominator; constant within each province
  over 2000–2024.
- `treat_intensity`: a ratio, not multiplied by 100. The formula reproduces all
  775 stored values at the supplied four-decimal precision.
- `total_count` in the batch sheet is a different reported total, **not this
  denominator**. Of its 15 nonmissing values, 14 differ from the same province's
  2024 `cum_xns_county` in the intensity sheet.
- Batch-sheet `first_year` is not automatically the first year of positive stored
  treatment intensity.


### 2.2 Batch-table observation unit

`small_irrigation_batches.csv` has **31 provinces, one row per province**, not
province–year observations.

- `batch1_2009`, `batch2_2010`, `batch3_2011`: counts under the supplied batch labels.
- `batch4_2012plus`: stored 2012-plus batch field.
- `total_count`: reported total; not assumed to equal the sum of partially missing
  batch fields.
- Missing does not mean no implementation.

### 2.3 High-standard farmland

- `cumulative_wanmu`: cumulative construction area in **10^4 mu**.
- `new_added_wanmu`: new construction area in **10^4 mu**.
- One wan mu equals 10,000 mu. The table has 372 province-year keys for 2013–2024,
  but only 42 nonmissing cumulative and 21 nonmissing new-area observations.
- Do not fill missing entries with zero or derive additions by differencing
  incomplete cumulative records.

### 2.4 Relationship to the master panel

The three intensity fields, two farmland fields, and policy `first_year` agree
with their master-panel counterparts. Grain area agrees at `rtol=atol=1e-8` with
small export-precision differences; the source view does not overwrite the master.

`first_year` is **policy metadata**, not the first disaster event `G_*` or the first
large-reservoir completion year generated in some reservoir scripts.

## 3. Production and variability

```text
Grain_Yield = Grain_Output / Grain_Sown_Area * 10000
Yield_TrendDev = (Grain_Yield - Grain_Yield_Trend) / Grain_Yield_Trend
Yield_CV5 = sample_SD(last five annual grain yields) / mean(last five annual grain yields)
Yield_Stability = -Yield_CV5
Output_or_yield_growth = 100 * (current_year_value / previous_year_value - 1)
```

With output in 10^4 tonnes and area in 10^3 ha, the yield formula gives kg/ha.
The numerical identity does not independently establish whether yield was
calculated by the authors or obtained directly from official statistics.
`Grain_Yield_Trend` is a within-province full-period linear trend. Rolling CV
requires five observations and sample SD (`ddof=1`). Deviations and CV are ratios;
growth is a percentage. Less-negative stability indicates lower five-year variability.

## 4. Infrastructure, inputs, and water use

```text
Irrigation_Coverage = Effective_Irrigation_Area / Total_Sown_Area
Waterlogging_Control_Coverage = Waterlogging_Control_Area / Total_Sown_Area
Reservoir_Capacity_per_Area = Reservoir_Total_Capacity / Total_Sown_Area
Reservoir_Density = Number_of_Reservoirs / Total_Sown_Area
Agricultural_Mechanization_Intensity = 10 * Agricultural_Machinery_Power / Total_Sown_Area
Fertilizer_Use_Intensity = 10000 * Chemical_Fertilizer_Use / Total_Sown_Area
Rural_Electricity_Intensity = 100000 * Rural_Electricity_Consumption / Total_Sown_Area
agri_use_intensity = agri_water_use * 100000 / Water_Module_Sown_Area
agri_pressure = agri_water_use / water_total
ln_Per_Capita_GDP = ln(Per_Capita_GDP)
```

- Irrigation and waterlogging coverage are ratios, not percentages.
- Raw capacity per area is in (10^8 m³)/(10^3 ha); multiply by 100000 for m³/ha.
  Reservoir density is reservoirs per 1000 ha, not per square kilometre.
- Mechanization, fertilizer, and electricity intensity are respectively kW/ha,
  kg/ha, and kWh/ha; conversion factors depend on the stored input units.
- Agricultural water-use intensity is in m³/ha and uses the retained **original
  water-module denominator**, not the final `Total_Sown_Area`. It is not a pure
  irrigation quota or efficiency indicator. The documentation retains a yearbook
  note about livestock-water reclassification from 2012.
- Agricultural water pressure is a use-to-local-resource ratio, can exceed one,
  and is not the proportion of water demand left unmet.
- Logging GDP does not make it constant-price GDP. No author-applied deflation is
  documented. Fiscal expenditure retains its across-year scope-break warning. It is not
  included in the retained manuscript regressions; its presence in an exploratory
  supplier file does not make it a validated manuscript covariate.

## 5. Disasters, resistance, and recovery

```text
rate_h_it = h_sz_it / Grain_Sown_Area_i,2004
Resistance_it = -(Grain_Yield_it - Grain_Yield_Trend_NS_it) / Grain_Yield_Trend_NS_it
Recovery_k = abs(Resistance at G_drought) - abs(Resistance at G_drought + k)
```

- `rate_h` is a ratio, not multiplied by 100, defined only for 2004–2024.
- `G_h` is the first year whose rate exceeds the 90th percentile of earlier valid
  rates, requiring at least three prior valid observations. It is a year, not a
  0/1 indicator.
- The `Grain_Yield_Trend_NS` fit excludes the union of identified first-event years
  across five hazards, not all disaster years.
- Positive `Resistance` means below-trend yield; larger is not automatically greater
  resilience.
- `Recovery_k` is stored only at the event-plus-k year. Positive means contraction
  of the absolute deviation. Each column has 23 nonmissing records; missing is
  not zero recovery.
- `profit_deviation` is primary-industry value added relative to its provincial
  linear trend, not measured agricultural net profit.

Abbreviations retained for code compatibility: `sz` = affected area; `cj` = damaged
area; `qj` = total-crop-failure area. `xns` identifies the small-irrigation policy;
`wanmu` identifies the 10^4-mu unit. Retaining these established column names does
not mean their precise source scope has been confirmed.

## 6. Climate indicators

- Provincial extraction uses grid-cell centres and cos(latitude) weights, not
  crop-area weights.
- The common growing season is April–September. Precipitation and `petPM` are summed;
  monthly temperature fields are averaged arithmetically.
- Anomalies use 1981–2010. `ZPRE_G` uses the reference-period sample SD;
  `DryShock_G=max(0,-ZPRE_G)` and `WetShock_G=max(0,ZPRE_G)`.
- In GHM_drought, lower SPI/SPEI/EDDI indicates drier conditions.
  `EDDI_min` is the lowest monthly EDDI within April–September.
- Seasonal summaries use monthly 1-month-scale indices. Unsuffixed annual SPI,
  SPEI, and EDDI correspond to downloaded 1-year-scale products, not a simple
  annual mean of the former series.
- `WetShock_G` is not recorded flood-affected area; `DryShock_G` is not recorded
  drought crop-damage area.

## 7. Language and naming

All release filenames, CSV headers, Excel sheet names, and Excel headers use
English/ASCII labels. The main analysis CSVs retain their existing variable names
and numerical values. `region` retains original category values; English labels
are provided in `categorical_value_labels.csv`. Original Chinese names are retained
only where useful for provenance, the bilingual dictionary, or Chinese companion
notes. See `naming_and_language_audit.md` for checks and mappings.


