# 指标口径与计算说明

本文对应仓库 README 的第 3、4 节，便于中文核对；英文版为 `indicator_definitions_en.md`。**原始统计指标、作者派生指标、代理变量是三个不同层次**，不能只凭字段名称互相替代。完整逐列定义、单位及缺失数见 `data_dictionary.csv`。

## 一、利润福利数据：不是统一的农业净利润面板

| 导出表 | 指标与单位 | 实际覆盖 | 口径及计算处理 |
|---|---|---|---|
| `data/welfare/family_operating_income_old.csv` | `Family_Operating_Net_Income_Old`，元/人 | 314 条：2004—2013 年 31 省，另有湖南 2000—2003 年 4 条 | 农村居民家庭人均纯收入中的“家庭经营纯收入”分项，按提供值保留，不另用产出减成本计算 |
| `data/welfare/net_operating_income_new.csv` | `Net_Operating_Income_New`，元/人 | 341 条：2014—2024 年 31 省 | 农村居民人均可支配收入中的“经营净收入”分项，按提供值保留 |
| `data/welfare/primary_industry_value_added.csv` | `Primary_Industry_Value_Added`，亿元、当年价 | 775 条：2000—2024 年 31 省 | 第一产业增加值，与主表对应列 775 格完全一致，是独立视图，不是新的数据来源 |

重要限制：

1. 旧、新收入的住户调查定义有差异。本工作簿旧口径止于 2013 年、新口径始于 2014 年；**不把两列直接拼成统一口径序列**。统计改革背景按供方说明记录，不表示所有其他数据产品都以同一年为切换点。
2. 住户经营收入可能包含非农经营，不是全省农业部门加总净利润，也不等同于农户全部可支配收入。
3. 第一产业增加值不是农业净利润、农户收入或每亩收益；不能把亿元与元/人混用。
4. 旧口径 2000—2003 年其余 30 省合计 120 个省年没有提供，原导出表中是缺行，不是 0。与主表左连接后才显示为缺失。
5. 原说明给出了中国统计年鉴和湖南统计年鉴的卷次、表号及来源记录，并说明新口径表使用 OCR、抽样人工核对。本次未重新抓取网页或逐格复核 OCR。
6. 原说明提到第一产业增加值与部分年鉴版本存在差异，并提出历史修订解释；本次只确认它与所给主表一致，**没有独立证实差异原因，也没有据此替换主表**。
7. 两个收入字段只做英文字段命名，未插值、折算、平减、衔接或新增趋势处理。原说明提及的 `raw_income` 等外部原始证据目录不在本次提供的三份文件中，不因此宣称证据链已完整打包。

## 二、政策数据

### 1. 小型农田水利重点县处理强度

```text
treat_intensity = round(cum_xns_county / total_county_count, 4)
```

- `cum_xns_county`：强度表存储的累计重点县数，原样保留。
- `total_county_count`：强度表的县级单位数量分母；所给表中每省在 2000—2024 年内保持不变。
- `treat_intensity`：比率，不乘以 100；775 条均可按上述公式复现。
- 分批表里的 `total_count` 是另一个“合计”字段，不是该分母。其 15 个非缺失值中，14 个与同省强度表 2024 年的 `cum_xns_county` 不相同。
- 分批表 `first_year` 与强度表第一次大于零的年份也不能自动视为一致。
- 保留原表，不根据分批表重新累计或自动填补。

### 2. 分批表的粒度

`small_irrigation_batches.csv` 为 **31 个省份、每省一行**，不是省年面板：

- `batch1_2009`、`batch2_2010`、`batch3_2011`：按原表批次标签保存的县数。
- `batch4_2012plus`：原表 2012 年后批次字段。
- `total_count`：原表列示合计，不假定等于缺失不全的批次字段之和。
- 缺失不代表未实施。

### 3. 高标准农田

- `cumulative_wanmu`：累计建设面积，单位万亩。
- `new_added_wanmu`：新增建设面积，单位万亩。
- 1 万亩为 10,000 亩。原表有 2013—2024 年的 372 个省年键，但实际只有累计面积 42 条、新增面积 21 条非缺失值。
- 不把缺失填 0，不用不完整的累计值差分自动生成新增面积。

### 4. 政策文件与主表的关系

强度三个数值字段和高标准农田两个面积字段与主表对应值一致；政策 `first_year` 也与主表一致。政策文件的粮食播种面积与主表在 `rtol=atol=1e-8` 下相符，存在微小导出精度差异，因此不覆盖主表。

`first_year` 在这里是**政策元数据**，不是首次灾害年 `G_*`，也不是你部分水库脚本里的首次大型水库建成年份。

## 三、生产与波动指标

```text
Grain_Yield = Grain_Output / Grain_Sown_Area × 10000
Yield_TrendDev = (Grain_Yield - Grain_Yield_Trend) / Grain_Yield_Trend
Yield_CV5 = 最近五年单产的样本标准差 / 最近五年单产均值
Yield_Stability = -Yield_CV5
产量或单产增长率 = 100 × (当年值 / 上年值 - 1)
```

产量为万吨、播种面积为千公顷时，单产关系式的结果为千克/公顷。数值恒等关系不单独证明该列是作者计算还是官方直接取得。`Grain_Yield_Trend` 是分省全样本线性趋势；滚动 CV 需要五个观测、标准差使用 `ddof=1`。趋势偏离和 CV 是比率，增长率是百分数；稳定性取负 CV，越接近 0 表示五年波动越小。

## 四、基础设施、投入与用水

```text
Irrigation_Coverage = Effective_Irrigation_Area / Total_Sown_Area
Waterlogging_Control_Coverage = Waterlogging_Control_Area / Total_Sown_Area
Reservoir_Capacity_per_Area = Reservoir_Total_Capacity / Total_Sown_Area
Reservoir_Density = Number_of_Reservoirs / Total_Sown_Area
Agricultural_Mechanization_Intensity = 10 × Agricultural_Machinery_Power / Total_Sown_Area
Fertilizer_Use_Intensity = 10000 × Chemical_Fertilizer_Use / Total_Sown_Area
Rural_Electricity_Intensity = 100000 × Rural_Electricity_Consumption / Total_Sown_Area
agri_use_intensity = agri_water_use × 100000 / Water_Module_Sown_Area
agri_pressure = agri_water_use / water_total
ln_Per_Capita_GDP = ln(Per_Capita_GDP)
```

- 灌溉覆盖、除涝覆盖是比率，不乘以 100。
- 库容按原单位相除为“亿立方米/千公顷”，转为立方米/公顷需再乘 100000；水库密度是座/千公顷，不是座/平方公里。
- 机械化、化肥、农村用电强度单位依次为千瓦/公顷、千克/公顷、千瓦时/公顷。
- 农业用水强度为立方米/公顷，必须用单独保留的水模块原始分母，不拿最终 `Total_Sown_Area` 替换。该指标是农业用水/播种面积，不是纯灌溉定额或效率；2012 年起牲畜用水归类变化也应保留说明。
- 农业用水压力是用水量/当地水资源量，可以大于 1，不是缺水需求比例。
- 人均 GDP 只是直接取自然对数，没有因此变成不变价人均 GDP。财政支出保留跨年分类变化警示；不进入本次论文保留的回归。供方探索性文件中保留该列，不等于其已成为经核验的论文控制项。

## 五、灾害、抵抗与恢复

```text
rate_h_it = h_sz_it / 本省2004年粮食播种面积
Resistance_it = -(Grain_Yield_it - Grain_Yield_Trend_NS_it) / Grain_Yield_Trend_NS_it
Recovery_k = abs(Resistance_G_drought) - abs(Resistance_G_drought+k)
```

- `rate_h` 是比率，不乘 100，仅在 2004—2024 年定义。
- `G_h` 是首次超过此前有效观测 90% 分位阈值的年份，至少需要三个此前有效观测，不是 0/1 指标。
- `Grain_Yield_Trend_NS` 的拟合剔除五类灾害首次事件年的并集，不是剔除所有灾害年。
- `Resistance` 正值代表低于趋势，不能直接解释为数值越大韧性越强。
- `Recovery_k` 只在事件后第 k 年记录；正值表示绝对偏离收缩。每列现有 23 条非缺失记录，缺失不是零恢复。
- `profit_deviation` 是第一产业增加值相对其分省线性趋势的偏离，不是实际农业净利润。

## 六、气候指标

- 空间提取采用省域格点中心规则及 `cos(纬度)` 加权，不是作物种植面积加权。
- 生长季统一为 4—9 月：降水和 `petPM` 求和，月度温度字段求算术平均。
- 异常值基期为 1981—2010 年，`ZPRE_G` 使用基期样本标准差；`DryShock_G=max(0,-ZPRE_G)`，`WetShock_G=max(0,ZPRE_G)`。
- 本产品中 SPI/SPEI/EDDI 均为越低越干；`EDDI_min` 为4—9月月度 EDDI 的最小值。
- 季节指标使用月度 1 个月尺度干旱指数；年度无后缀 SPI/SPEI/EDDI 对应所下载的 1 年尺度产品，不等于前者简单年平均。
- `WetShock_G` 不是统计洪涝受灾面积，`DryShock_G` 不是旱灾作物受损面积。

## 七、英文版与名称检查

本文英文版为 `indicator_definitions_en.md`；完整英文变量字典为 `data_dictionary_en.csv`，双语版为 `data_dictionary.csv`。所有发布文件名、CSV 表头和 Excel 工作表/表头已英文化，现有分析字段缩写保留以兼容代码。`sz`、`cj`、`qj` 分别指受灾、成灾、绝收面积，`xns` 指小型农田水利政策字段，`wanmu` 表示万亩单位。原始地区分类值不重编码，英文含义见 `categorical_value_labels.csv`。


