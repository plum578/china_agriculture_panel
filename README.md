# China Agriculture and Climate Panel (2000–2024)

Provincial agricultural production, water infrastructure and use, reported disaster
areas, policy indicators, and climate conditions in mainland China.

**Data and documentation version:** 1.0.0 | **Updated:** 2026-11-04

[中文说明](README_zh.md)


## 1. Coverage and release design

- **Annual unit:** province–year; 31 provincial-level units, 2000–2024, 775 records.
- **Monthly unit:** province–year–month; the same units and years, 9,300 records.
- **Keys:** `Province`, `Year`, and, for monthly tables, `Month`.
- **Format:** CSV; UTF-8 (some files include a BOM; use `utf-8-sig` when reading).
- **Missing values:** empty cells, read as `NaN`; missing does not mean zero.
- The main annual and monthly panels are balanced in their keys, not necessarily
  complete in every variable. Welfare and policy supplements have their own
  coverage; the old income table is not a balanced 2000–2013 panel.

| File | Records × columns | Contents |
|---|---:|---|
| [data/master_panel.csv](data/master_panel.csv) | 775 × 126 | Shared statistics, seasonal climate indicators, and specific disaster, policy, and outcome fields |
| [data/climate_annual_supplement.csv](data/climate_annual_supplement.csv) | 775 × 15 | Annual weather/drought fields and `province_code`, plus the two annual keys |
| [data/climate_monthly.csv](data/climate_monthly.csv) | 9,300 × 12 | Nine monthly weather fields plus the three monthly keys |
| [data/drought_monthly.csv](data/drought_monthly.csv) | 9,300 × 6 | Monthly SPI, SPEI, and EDDI plus the three monthly keys |
| [data/water_intensity_denominator.csv](data/water_intensity_denominator.csv) | 775 × 3 | Original water-module sown-area denominator and the annual keys |

### Additional welfare and policy tables

No values are merged into or used to overwrite the existing master panel.

| File | Records × columns | Coverage / interpretation |
|---|---:|---|
| [data/welfare/family_operating_income_old.csv](data/welfare/family_operating_income_old.csv) | 314 × 3 | Old household-survey income definition; all 31 provinces in 2004–2013, plus Hunan in 2000–2003 |
| [data/welfare/net_operating_income_new.csv](data/welfare/net_operating_income_new.csv) | 341 × 3 | New income definition; 31 provinces, 2014–2024 |
| [data/welfare/primary_industry_value_added.csv](data/welfare/primary_industry_value_added.csv) | 775 × 3 | Current-price primary-industry value added; exact duplicate view of the master's series |
| [data/policy/grain_sown_area.csv](data/policy/grain_sown_area.csv) | 775 × 3 | Policy-workbook grain-area input; 2000–2024 |
| [data/policy/small_irrigation_batches.csv](data/policy/small_irrigation_batches.csv) | 31 × 7 | Province-only policy-batch metadata; key is `Province`, not province–year |
| [data/policy/high_standard_farmland.csv](data/policy/high_standard_farmland.csv) | 372 × 4 | 2013–2024; only 42 cumulative-area and 21 new-area values are nonmissing |
| [data/policy/small_irrigation_intensity.csv](data/policy/small_irrigation_intensity.csv) | 775 × 5 | Stored annual county-based policy intensity; 2000–2024 |

### The supplied workbooks :
- [agricultural_welfare_panel.xlsx](data/source_workbooks/agricultural_welfare_panel.xlsx): three numeric sheets and two English documentation sheets.
- [agricultural_policy_panel.xlsx](data/source_workbooks/agricultural_policy_panel.xlsx): four numeric sheets.
- [Welfare source note — English](docs/source_notes/agricultural_welfare_panel_notes_en.md): source references, coverage, and units.
- [Welfare source note — Chinese](docs/source_notes/agricultural_welfare_panel_notes_zh.md): an edited Chinese source note.


[docs/workbook_sheet_inventory.csv](docs/workbook_sheet_inventory.csv) maps every
workbook sheet to its exported file or retained documentation sheet. CSV exports
standardize key capitalization to `Province`, `Year`; the two income columns also
have explicit English names. Numerical values and missingness are preserved,
with no interpolation, deflation, unit conversion, or cross-definition splicing.

[docs/data_dictionary_en.csv](docs/data_dictionary_en.csv) is the English-only
variable dictionary; [docs/data_dictionary.csv](docs/data_dictionary.csv) is its
bilingual counterpart with `description_en` and `description_zh`. Each has
**190 table–column entries**, covering every column in the 12 numeric CSV files,
including all 126 columns in the main panel. Entries record labels, units,
formulas, source groups, missing counts, and documentation status. Repeated keys
and duplicate views are documented per table; 190 is not a count of distinct variables.

### Documentation languages and naming

| Document | English | Chinese companion |
|---|---|---|
| Indicator definitions, statistical scope, and calculations | [indicator_definitions_en.md](docs/indicator_definitions_en.md) | [indicator_definitions_zh.md](docs/indicator_definitions_zh.md) |
| Supplied welfare source note | [agricultural_welfare_panel_notes_en.md](docs/source_notes/agricultural_welfare_panel_notes_en.md) | [Chinese note](docs/source_notes/agricultural_welfare_panel_notes_zh.md) |

All current release filenames, CSV headers, and Excel sheet/header labels are
English/ASCII. Existing variable abbreviations are preserved for analysis-code
compatibility: `sz` = affected area, `cj` = damaged area, `qj` = total-crop-failure
area; `xns` identifies the small-irrigation policy and `wanmu` the 10^4-mu unit.
These are documented rather than silently renamed. In particular, `first_year`
is still policy metadata, not reservoir completion timing.

The master `region` category values are retained in their original Chinese form
to avoid recoding the analysis input; English category labels are provided in
[docs/categorical_value_labels.csv](docs/categorical_value_labels.csv).
Original Chinese names occur only in provenance mappings, category labels, and
Chinese/bilingual companions. See [docs/naming_map.csv](docs/naming_map.csv) for
original-to-release names.

[manifest.json](manifest.json) records table dimensions and SHA-256 checksums.
Analysis scripts, complete raw climate inputs, and the full computational environment
are not bundled in this data resource.

## 2. Data provenance

### Provincial statistics and collaborator-specific inputs

The authors identify the **National Bureau of Statistics of China (NBS)** as the
source of the shared provincial statistical series. The authors also supplied
recorded disaster areas, infrastructure series, rural income, rural water-related
investment, and policy workbooks. The dictionary records their current documentation status. 

### Agricultural welfare data sources

According to the accompanying source notes, the old-definition income
series was compiled from the China Statistical Yearbooks, 2005–2014
editions. Observations for Hunan in 2000–2003 were supplemented using
Table 7-17 of the Hunan Statistical Yearbook 2004.

The new-definition income series was compiled from the China Statistical
Yearbooks, 2015–2025 editions. Image-based tables were digitized using
optical character recognition (OCR), followed by sample-based manual
checks.

Detailed year-by-year sources, table references, and collection notes
are provided in the accompanying source documentation. Yearbook edition
years should be distinguished from the observation years recorded in
the data.

The welfare workbook's primary-industry value-added sheet is copied from the master, not a new independent source.

### Meteorological data

The weather source is **ChinaMet: A High-resolution Multi-element Meteorological
Driving Dataset for China via Multi-source Data Fusion**, by Zhang Ling, Hu Yingyi,
and collaborators, distributed through the National Cryosphere Desert Data Center
(DOI: 10.12072/ncdc.nieer.db6722.2025). The project uses the **0.1°** annual and
monthly products for 2000–2024 and reference-period inputs for 1981–2010.

Fields include precipitation, temperature, relative humidity, wind, pressure,
and potential evapotranspiration. `petPM` and `petHG` are retained as distinct source-product fields. 

### Meteorological drought indices

The source of SPI, SPEI, and EDDI is **GHM_drought**, the global **1961–2100**
drought-index dataset by Jiachen Ji, Chiyuan Miao, and collaborators, distributed
on Zenodo (DOI: 10.5281/zenodo.18045718; January 2026 release). Its native resolution is **0.5°**,
not the 0.1° weather-grid resolution. These tables contain the 2000–2024 historical
portion, not future SSP projections. 

The monthly drought table uses **1-month accumulation-scale** indices. The annual
supplement's unsuffixed `SPI`, `SPEI`, and `EDDI` match the downloaded **1-year-scale**
products within absolute tolerance 1e-6 for all 775 observations of each index.
They are not simple annual averages of the monthly 1-month-scale indices.

## 3. Climate processing and interpretation

The supplied provincial extraction scripts use a grid-cell-centre assignment
(`all_touched=False`) and **cos(latitude)**-weighted means. These approximate
area-weighted provincial means, not crop-area-weighted exposure.

Seasonal indicators use a common **April–September** window:

- `PRE_G`: sum of monthly precipitation; `PET_G`: sum of monthly `petPM`.
- `TMPMEAN_G` and `TMPMAX_G`: arithmetic means of the monthly temperature fields.
  `TMPMAX_G` is not the single hottest observed day.
- Climate anomalies use a **1981–2010** reference period. `ZPRE_G` uses the
  reference-period sample standard deviation (`ddof=1`).
- `DryShock_G = max(0, -ZPRE_G)`; `WetShock_G = max(0, ZPRE_G)`.
- `SPI_mean`, `SPEI_mean`, and `EDDI_mean`, and their `_min` counterparts, summarize
  April–September monthly values. In GHM_drought, lower SPI/SPEI/EDDI means drier
  conditions; `EDDI_min` is the lowest monthly EDDI within April–September.

**Meteorological shocks and reported disaster areas are different constructs.**
`WetShock_G` measures unusually wet conditions, not observed flood-affected area;
`DryShock_G` measures a precipitation deficit, not reported drought crop losses.

Monthly files are needed for the focal compound-shock and crop-calendar analyses.
They contain only 2000–2024: reconstructing anomalies from scratch additionally
requires the earlier reference-period inputs. The annual weather fields are kept
as supplied; 

## 4. Additional variables in the main panel

The author's input has **48 columns absent from the earlier panel**. This
count includes retained source variables and constructed outcomes, not 48 new
experimental findings. The dictionary provides individual definitions.

| Group | Fields / examples | 
|---|---|
| Infrastructure | `WaterloggedPreserved_Area`, `Electromechanical_Drainage_Area`, `Water_Saving_Irrigation_Area` |
| Recorded disasters | Five hazards × `sz`, `cj`, `qj`; `total_sz`, `total_qj` |
| Policy exposure | `first_year`, `total_county_count`, `cum_xns_county`, `treat_intensity`, `cumulative_wanmu`, `new_added_wanmu` | 
| Grouping | `main_region`, `north`, `region` | 
| Rural economy | `rural_income`, `water_investment`, `rural_income_deviation` | 
| Disaster normalization and timing | Five `rate_*` and five `G_*` fields |
| Outcomes | `Grain_Yield_Trend_NS`, `Resistance`, `Recovery_1`–`Recovery_3`, `profit_deviation` | 


### Disaster rates and event timing

For each hazard `h`:

```text
rate_h_it = h_sz_it / Grain_Sown_Area_i,2004
```

These are **ratios, not percentages**, and use the province's **2004 grain sown
area**, not the current-year sown area. Rates are defined only for 2004–2024;
2000–2003 are missing by design. `G_h` is the first year in that window when the
rate exceeds the 90th percentile of its prior valid observations, with at least
three prior observations required. The resulting year is mapped to all annual
records for that province; it is not a 0/1 treatment indicator.

It is **not** created by renaming or filling from the earlier panel's `Drought_*` and
`Flood_*` columns. The source versions differ in values and missingness. In
particular, `drought_qj` is populated only within 2004–2014, with 2009 entirely
missing, and `flood_qj` only within 2004–2008. Retaining this version is a release
choice, not a finding that it is more accurate. Do not interpret missing as no loss.

### Outcome definitions

`Grain_Yield_Trend_NS` is a provincial linear yield trend excluding the union of
the five hazards' identified first-event years, **not every disaster year**.

```text
Resistance_it = -(Grain_Yield_it - Grain_Yield_Trend_NS_it)
                / Grain_Yield_Trend_NS_it
Recovery_k = abs(Resistance at G_drought) - abs(Resistance at G_drought + k)
```

Positive `Resistance` means yield is below the fitted trend; a larger value does
not automatically mean greater resilience. `Recovery_k` is stored only in the
corresponding event-plus-k year; each of the three columns has 23 nonmissing
records. Positive recovery means a smaller absolute deviation from trend.

`profit_deviation` is the relative deviation of **Primary_Industry_Value_Added**
from a provincial linear trend. It is a proxy field, **not directly observed
agricultural net profit**. `rural_income_deviation` is a relative income-trend
deviation, not the income growth rate.


### Welfare income measures: scope, units, and breaks

- `Family_Operating_Net_Income_Old` is the **family-operation component of rural
  household per-capita net income**, in CNY/person. The CSV includes 314 observed
  records; 120 province-years are absent in 2000–2003, not zero.
- `Net_Operating_Income_New` is the **net operating-income component of rural
  residents' per-capita disposable income**, also in CNY/person; 341 records in
  2014–2024. The supplied note identifies a household-survey definition change;
  the workbook retains the old definition through 2013 and the new one from 2014.
- These are published household-survey components.
  Household operations can include non-agricultural activities. Neither series
  is agriculture-only aggregate profit, and the two must not be concatenated as
  one harmonized series without a separately justified reconciliation.
- `Primary_Industry_Value_Added` is measured in **10^8 CNY at current prices**.
  It is not agricultural net profit or household income. All 775 exported values exactly
  match the master. Do not append this duplicate as additional observations.
- No new deflation, income detrending, profit calculation, or experimental result
  is introduced by adding these workbooks.

### Policy intensity and construction area

The stored intensity is numerically reproduced for all 775 observations by:

```text
treat_intensity = round(cum_xns_county / total_county_count, 4)
```

It is a **ratio**, not a percentage. The stored denominator is constant within
province over 2000–2024. `total_count` in the batch sheet is not that denominator.

`cumulative_wanmu` and `new_added_wanmu` are construction-area fields in
**10^4 mu (wan mu)**. 

### Other selected shared calculations

The dictionary records the following definitions and raw-unit conversion factors:

```text
Grain_Yield = Grain_Output / Grain_Sown_Area * 10000          [kg/ha]
Yield_TrendDev = (Grain_Yield - Grain_Yield_Trend) / Grain_Yield_Trend
Yield_CV5 = sample_SD(Grain_Yield_t-4:t) / mean(Grain_Yield_t-4:t)
Yield_Stability = -Yield_CV5
Grain_Output_Growth = 100 * (Grain_Output_t / Grain_Output_t-1 - 1)
Grain_Yield_Growth = 100 * (Grain_Yield_t / Grain_Yield_t-1 - 1)

Irrigation_Coverage = Effective_Irrigation_Area / Total_Sown_Area
Waterlogging_Control_Coverage = Waterlogging_Control_Area / Total_Sown_Area
Reservoir_Capacity_per_Area = Reservoir_Total_Capacity / Total_Sown_Area
Reservoir_Density = Number_of_Reservoirs / Total_Sown_Area
Agricultural_Mechanization_Intensity = 10 * Agricultural_Machinery_Power / Total_Sown_Area
Fertilizer_Use_Intensity = 10000 * Chemical_Fertilizer_Use / Total_Sown_Area
Rural_Electricity_Intensity = 100000 * Rural_Electricity_Consumption / Total_Sown_Area
agri_pressure = agri_water_use / water_total
```

`Grain_Output` is in 10^4 tonnes and `Grain_Sown_Area` in 10^3 ha. The yield
identity reproduces the numerical units but does not by itself prove whether a
yield field was downloaded or computed. `Grain_Yield_Trend` is the within-province
full-period linear trend, distinct from the event-excluded `Grain_Yield_Trend_NS`.
Rolling CV requires five observations and sample SD (`ddof=1`); growth rates are
percentages, whereas trend deviations and infrastructure coverage are ratios.
Raw capacity per area is in (10^8 m³)/(10^3 ha), so multiply by 100000 for m³/ha;
reservoir density is reservoirs per 1000 ha. Mechanization, fertilizer, and
rural-electricity intensities are respectively kW/ha, kg/ha, and kWh/ha.
Agricultural water pressure is a use-to-resource ratio, not unmet water demand.

