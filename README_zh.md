# 中国农业与气候省级面板（2000—2024）

**数据与说明版本：1.0.0｜更新日期：2026-11-04（Asia/Shanghai）**


## 1. 资源定位与验证范围

本资源为中国大陆31个省级行政区的统计与气候数据集合，覆盖农业生产、水利、气象干旱、受灾、政策和经济指标。年度主表2000—2024年共775个省年键，月表共9,300个省年月键。**键平衡不等于所有变量完整。** 空白读取为NaN，缺失不是零，不自动插值。

1.0.0为本地数据/说明整理版本，不代表已经上传GitHub或获得归档DOI；也不是一键复现全文的代码包。本次核对包含键、覆盖范围、部分派生公式与正文—结果文件对应关系，未重跑全部回归。

## 2. 文件与合并

| 文件 | 行×列 | 含义 |
|---|---:|---|
| `data/master_panel.csv` | 775×126 | 联合年度主表；包含季节气象及同伴受灾、政策、结局字段 |
| `data/climate_annual_supplement.csv` | 775×15 | 额外年度气候及省代码，不重复主表已有季节字段 |
| `data/climate_monthly.csv` | 9,300×12 | 九个月度气象字段及三个键 |
| `data/drought_monthly.csv` | 9,300×6 | 月度SPI、SPEI、EDDI及三个键 |
| `data/water_intensity_denominator.csv` | 775×3 | 农业用水强度使用的原水模块播种面积分母 |
| `data/welfare/family_operating_income_old.csv` | 314×3 | 旧口径收入：2004—2013年31省，加湖南2000—2003年 |
| `data/welfare/net_operating_income_new.csv` | 341×3 | 新口径收入：2014—2024年31省 |
| `data/welfare/primary_industry_value_added.csv` | 775×3 | 第一产业增加值，与主表对应序列重复视图 |
| `data/policy/grain_sown_area.csv` | 775×3 | 粮食播种面积的来源视图 |
| `data/policy/small_irrigation_batches.csv` | 31×7 | 省级政策分批元数据，键仅Province |
| `data/policy/high_standard_farmland.csv` | 372×4 | 高标准农田存量与新增量 |
| `data/policy/small_irrigation_intensity.csv` | 775×5 | 县数累计与政策强度 |

年表使用`Province, Year`一对一合并，主表与年度气候补充合并后775×139。两张月表使用`Province, Year, Month`一对一合并后9,300×15。不要把月表连接产生的重复年度值当成9,300个独立省年观测。`first_year`为政策年份，不是水库完工年份。

两个`data/source_workbooks/`工作簿为英文命名/表头版本，数值保留，但Excel字节不等于原工作簿。文件/字段原名映射见[名称映射](docs/naming_map.csv)，工作表清单见[工作簿清单](docs/workbook_sheet_inventory.csv)。所有表文件名与字段保持英文；原始中文地区类别保留，并提供英文值标签。

## 3. 来源与气候处理

- 共同省级统计序列由作者从国家统计局和相关年鉴整理。受灾、政策等供方专属字段仍须逐项按字典归属，不能未经核验全部笼统归给同一来源。
- 气象来自张凌、胡英屹等在国家冰川冻土沙漠科学数据中心发布的《多源数据融合的中国高分辨多要素气象驱动产品（ChinaMet）》（DOI：10.12072/ncdc.nieer.db6722.2025）；采用0.1°、2000—2024年逐月/逐年产品。
- SPI、SPEI、EDDI来自Ji Jiachen、Miao Chiyuan团队在Zenodo发布的GHM_drought全球气象干旱资料（DOI：10.5281/zenodo.18045718），原始分辨率0.5°，只用历史2000—2024年。月指标为1个月累积尺度，年度补充指标为1年累积尺度，非月指标简单年平均。
- 省级提取按格点中心归属，以cos(latitude)近似面积加权，非作物面积加权。生长季统一4—9月：降水、petPM求和，温度算术平均。
- 气候基准期1981—2010年，`ZPRE_G`使用样本标准差（ddof=1）。`DryShock_G=max(0,-ZPRE_G)`，`WetShock_G=max(0,ZPRE_G)`；湿润异常不等于实际洪灾受灾面积。
- 本产品中SPI/SPEI/EDDI越小越干，`EDDI_min`为4—9月月度EDDI的最小值。月表保留petPM和petHG，不能互换算法。

## 4. 关键口径与计算

逐列定义见[双语变量字典](docs/data_dictionary.csv)和[英文变量字典](docs/data_dictionary_en.csv)，12个输入CSV共190个“表—字段”条目，不是190个独立变量。完整公式与限制见[指标口径中文](docs/indicator_definitions_zh.md)及[英文](docs/indicator_definitions_en.md)。

- 粮食单产=`Grain_Output / Grain_Sown_Area ×10000`，单位kg/ha；趋势偏离=`(实际值−省级线性趋势值)/趋势值`。
- 灌溉代理=`Effective_Irrigation_Area / Total_Sown_Area`，是比值而非百分数。15个观测超过1，涉及北京、西藏、新疆；分子分母口径有差异，不自动截断，不解释为严格实际灌溉比例。
- 机械化强度=`10×机械总动力/总播种面积`；化肥强度=`10000×化肥量/总播种面积`；用电强度=`100000×农村用电量/总播种面积`，换算对应输入单位。
- 农业用水强度=`agri_water_use×100000/Water_Module_Sown_Area`，单位m³/ha。分母使用单列保留的原水模块播种面积，不是主表最终总播种面积；不是纯灌溉定额或用水效率。2012年牲畜用水分类变化须保留。
- 人均GDP直接自然对数，没有据此作不变价处理。农林水支出存在跨年分类变化，不进入本次论文保留的回归。
- `rate_h=h_sz/本省2004年粮食播种面积`，仅2004—2024年定义，不乘100。受灾原表的单位/分类仍以字典标记为准。`G_h`是历史阈值首次超过年份，不是0/1处理变量。
- `Resistance`正值表示低于剔除首次灾害事件年份拟合的趋势，越大不是越抗灾；`Recovery_k=|Resistance_t0|−|Resistance_(t0+k)|`，只存放在事件+k年份，缺失不补零。
- `profit_deviation`是第一产业增加值偏离，不是农业净利润。新旧住户经营收入分开保留，不能拼成同口径连续序列。批次总县数与强度表累计县数存在未调和差异，不静默覆盖。


