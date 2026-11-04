# Agricultural Welfare Panel: Source Note (English)

**Original delivery date:** October 20, 2026  
**Release workbook:** `data/source_workbooks/agricultural_welfare_panel.xlsx`  
**Geography:** 31 provincial-level units, excluding Hong Kong, Macao, and Taiwan  
**Nominal study period:** 2000–2024; availability differs by sheet


## 1. Availability overview

| Indicator | Definition / unit | Coverage | Missingness and source description |
|---|---|---|---|
| Family operating net income, old definition | Component of rural household per-capita net income, CNY/person | 2000–2013, but all 31 provinces only in 2004–2013; Hunan only in 2000–2003 | 314 supplied observations; 120 other province-years absent in 2000–2003; supplier cites direct XLS extraction and a Hunan yearbook supplement |
| Net operating income, new definition | Component of rural residents' per-capita disposable income, CNY/person | All 31 provinces, 2014–2024 | 341 observations, no missing entries within the supplied coverage; supplier reports JPG extraction with RapidOCR |
| Primary-industry value added (PIVA) | 10^8 CNY, current prices | All 31 provinces, 2000–2024 | 775 observations, copied from `master_panel.csv`, not newly collected |
| Agricultural net output | Gross output minus material consumption, MPS framework | No full-period provincial series collected for this release | Not included in this release |
| Province-wide aggregate agricultural net profit | Cost-and-return definition | No complete provincial aggregate collected for this release | No harmonized 31-province by 25-year aggregate supplied |

The income components together provide 655 observations: 310 old-definition
observations in 2004–2013, four Hunan observations in 2000–2003, and 341
new-definition observations in 2014–2024. **This count does not make the old and
new definitions comparable or agriculture-only measures.**

## 2. Source register: data years, yearbook editions, tables, and supplied URLs

### 2.1 Old family operating-income component

For 2004–2013, the supplier identifies the regional rural household per-capita net
income by source table in the China Statistical Yearbook:

| Data year | Yearbook edition | Table | Supplied retrieval reference |
|---:|---:|---|---|
| 2004 | 2005 | 10-22 | `https://www.stats.gov.cn/sj/ndsj/2005/html/J1022C.xls` |
| 2005 | 2006 | 10-22 | `https://www.stats.gov.cn/sj/ndsj/2006/html/J1022C.xls` |
| 2006 | 2007 | 10-22 | `https://www.stats.gov.cn/sj/ndsj/2007/html/J1022C.xls` |
| 2007 | 2008 | 9-22 | `https://www.stats.gov.cn/sj/ndsj/2008/html/J0922C.xls` |
| 2008 | 2009 | 9-22 | `https://www.stats.gov.cn/sj/ndsj/2009/html/J0922C.xls` |
| 2009 | 2010 | 10-22 | `https://www.stats.gov.cn/sj/ndsj/2010/html/J1022C.xls` |
| 2010 | 2011 | 10-22 | `https://www.stats.gov.cn/sj/ndsj/2011/html/J1022C.xls` |
| 2011 | 2012 | 10-22 | `https://www.stats.gov.cn/sj/ndsj/2012/html/J1022C.xls` |
| 2012 | 2013 | 11-21 | `https://www.stats.gov.cn/sj/ndsj/2013/html/Z1121C.xls` |
| 2013 | 2014 | 6-26 | `https://www.stats.gov.cn/sj/ndsj/2014/zk/html/Z0626C.xls` |

For **Hunan only, 2000–2003**, the supplier cites Hunan Statistical Yearbook 2004,
table 7-17, the rural per-capita net-income source composition table:
`http://222.240.193.190/2004tjnj/0207.pdf`.

| Year | Hunan value, CNY/person |
|---:|---:|
| 2000 | 1329.10 |
| 2001 | 1377.42 |
| 2002 | 1376.41 |
| 2003 | 1427.21 |

### 2.2 New net operating-income component

The supplier identifies the rural per-capita disposable-income by source table in
the China Statistical Yearbook:

| Data year | Yearbook edition | Table | Supplied retrieval reference |
|---:|---:|---|---|
| 2014 | 2015 | 6-26 | `https://www.stats.gov.cn/sj/ndsj/2015/html/CH0626.jpg` |
| 2015 | 2016 | 6-30 | `https://www.stats.gov.cn/sj/ndsj/2016/html/0630CH.jpg` |
| 2016 | 2017 | 6-30 | `https://www.stats.gov.cn/sj/ndsj/2017/html/CH0630.jpg` |
| 2017 | 2018 | 6-30 | `https://www.stats.gov.cn/sj/ndsj/2018/html/CH0630.jpg` |
| 2018 | 2019 | 6-30 | `https://www.stats.gov.cn/sj/ndsj/2019/html/C0630.jpg` |
| 2019 | 2020 | 6-30 | `https://www.stats.gov.cn/sj/ndsj/2020/html/C0630.jpg` |
| 2020 | 2021 | 6-31 | `https://www.stats.gov.cn/sj/ndsj/2021/html/C06-31.jpg` |
| 2021 | 2022 | 6-31 | `https://www.stats.gov.cn/sj/ndsj/2022/html/C06-31.jpg` |
| 2022 | 2023 | 6-31 | `https://www.stats.gov.cn/sj/ndsj/2023/html/C06-31.jpg` |
| 2023 | 2024 | 6-31 | `https://www.stats.gov.cn/sj/ndsj/2024/html/C06-31.jpg` |
| 2024 | 2025 | 6-31 | `https://www.stats.gov.cn/sj/ndsj/2025/html/C06-31.jpg` |


### 2.3 Primary-industry value added


The supplier states that the values were copied from
`master_panel.csv:Primary_Industry_Value_Added`, with no changes. The current
release check confirms exact numerical equality at all 775 corresponding keys.
The separate sheet is a convenience view, not independent statistical evidence.

## 3. The 2000–2003 gaps

The release contains no income observations for the other 30 provinces in 2000–2003: 120 province-years were not collected and are retained as missing.


## 4. Units

| English release sheet | Measure | Unit |
|---|---|---|
| `family_income_old_2000_2013` | Old family-operation income component | CNY/person |
| `operating_income_new_2014_2024` | New net operating-income component | CNY/person |
| `primary_value_added_2000_2024` | Primary-industry value added | 10^8 CNY, current prices |


