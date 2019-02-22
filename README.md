# 沪深300指数纯因子组合构建

> WIFA量化组，2019年春。

依据多因子模型，尝试对沪深300指数构建纯因子组合。

# Step 1：因子数据库构建

> 数据来源为万德金融数据库，通过WindPy API获取。

因子数据分为*风格因子*和*风险因子*。

其中风格因子又分为大类因子和细分类因子，最终风格因子会由细分类因子合成。

风格因子共选取以下7个大类中的19个因子：

- VALUE：EPS_TTM/P、BPS_LR/P、CFPS_TTM/P、SP_TTM/P 
- GROWTH：NetProfit_SQ_YOY、Sales_SQ_YOY、ROE_SQ_YOY 
- PROFIT：ROE_TTM、ROA_TTM 
- QUALITY：Debt2Asset、AssetTurnover、InvTurnover 
- MOMENTUM：Ret1M、Ret3M、Ret6M 
- VOLATILITY：RealizedVol_3M、RealizedVol_6M 
- LIQUIDITY：Turnover_ave_1M、Turnover_ave_3M 

风险因子选取以下2个大类中的2个因子：

- INDUSTRY：中信一级行业 
- SIZE：Ln_MarketValue 

由于数据限制和平台选择，最终确定的因子和最初选取的因子比较如下：

最初选取因子|最终确定因子
:--:|:--:
EPS_TTM/P|PE_TTM
BPS_LR/P|PB_LYR
CFPS_TTM/P|PCF_NCF_TTM
SP_TTM/P|PS_TTM
NetProfit_SQ_YOY|YOYPROFIT
Sales_SQ_YOY|YOY_OR
ROE_SQ_YOY|YOYROE
ROE_TTM|ROE_TTM
ROA_TTM|ROA_TTM
Debt2Asset|DEBTTOASSETS
AssetTurnover|ASSETSTURN
InvTurnover|INVTURN
Ret1M|PCT_CHG
Ret3M|PCT_CHG
Ret6M|PCT_CHG
RealizedVol_3M|UNDERLYINGHISVOL_90D
RealizedVol_6M|UNDERLYINGHISVOL_90D
Turnover_ave_1M|TECH_TURNOVERRATE20
Turnover_ave_3M|TECH_TURNOVERRATE60
中信一级行业列表|INDUSTRY_SW
Ln_MarketValue|VAL_LNMV

> 其中“最终确定因子”即为其万德指标字段名。

数据保存在“H3 Data” ("HS300 Data" 的缩写) 文件夹中，格式为CSV，直接用全小写的万德指标名命名。

数据格式如下：
- index: 日期（YYYYMMDD）。(numpy.int64)
- columns: 股票代号（XXXXXX.XX）。(str)

> 即 "<万德指标名>.csv"，如 "pe_ttm.csv"

# Step 2：因子数据处理

## 2.1 去极值

