# Explainable Machine Learning for Predicting Muscle Loss in Oral Cavity Cancer: Development of On-treatment and Pretreatment Risk Models

利用可解釋性機器學習預測口腔癌患者之肌肉流失：開發治療中與治療前之風險模型

> Master’s Thesis Project｜Biomedical Informatics  
> National Yang Ming Chiao Tung University

## Project Overview

放射治療可能造成口腔癌患者肌肉流失，進而影響治療耐受性、生活品質及臨床預後。

本研究使用跨院區臨床資料，建立兩個具可解釋性的機器學習模型，用於預測患者接受放射治療後發生顯著肌肉流失的風險：

- Model 1：治療中預測模型（On-treatment Prediction Model）
- Model 2：治療前早期預測模型（Pretreatment Prediction Model）

研究使用 Random Forest、XGBoost 與 CatBoost 建立分類模型，並透過 SHAP 分析解釋個別特徵對預測結果的影響。

## Prediction Target

本研究將預測任務定義為二元分類：

- Label 0：放射治療後骨骼肌指數（SMI）損失小於 4.2%
- Label 1：放射治療後骨骼肌指數（SMI）損失大於或等於 4.2%

SMI 由治療前及治療後電腦斷層影像中的骨骼肌橫截面積換算而得。

## Models

### Model 1：On-treatment Prediction Model

模型一整合治療前基線資料與治療期間資訊，共使用 18 項特徵。

主要特徵類型：

- 基本臨床特徵
- 治療資訊
- 治療前 BMI 與 SMI
- 治療期間 BMI 變化
- 皮膚炎、黏膜炎、口乾等治療副作用
- 吞嚥困難、疼痛、食慾不振、噁心及疲勞

最終選用 CatBoost 作為模型一的主要模型。

外部驗證結果：

- ROC-AUC：0.990
- F1-score：0.909

重要預測特徵包含食慾不振、吞嚥困難及噁心。

### Model 2：Pretreatment Prediction Model

模型二僅使用放射治療開始前可取得的資訊，共使用 18 項特徵，以提供較早期的風險評估。

主要特徵類型：

- 基本臨床特徵
- 治療前 SMI
- 是否接受同步化學治療
- 中性球與淋巴球比率（NLR）
- 查爾森共病指數（CCI）
- 簡易營養評估（MNA）
- 吞嚥相關器官之放射治療劑量參數

最終選用 Random Forest 作為模型二的主要模型。

外部驗證結果：

- ROC-AUC：0.913
- F1-score：0.740

重要預測特徵包含 MNA、咽上縮肌放射劑量、聲門上區放射劑量及化學治療。

## Dataset

本研究採用回顧性臨床隊列資料：

- 開發隊列：彰化基督教醫院（CCH）
- 外部驗證隊列：馬偕紀念醫院（MMH）
- 收案期間：2010–2021 年

### Sample Size

| Model | CCH Development Cohort | MMH External Validation | Total |
|---|---:|---:|---:|
| Model 1 | 572 | 331 | 903 |
| Model 2 | 636 | 388 | 1,024 |

兩個模型的收案條件與預測時間點不同，因此樣本數不可直接相加視為不重複患者總數。

### Data Availability

本研究資料包含患者臨床病歷、治療資訊、放射治療劑量及影像衍生特徵，屬於受保護的醫療敏感資料。

基於下列限制，本 Repository 不提供原始資料或個案層級資料：

- 患者隱私保護
- 研究倫理規範
- 資料使用授權限制

資料欄位、樣本數、特徵及預測變項的詳細說明，請參考：

[04_資料集_DataSet/README.md](04_資料集_DataSet/README.md)

## Repository Structure

```text
.
├── 01_論文電子檔/
│   └── 碩士論文 PDF
├── 02_口試電子檔/
│   └── 論文口試簡報
├── 03_程式碼_SourceCode/
│   ├── 01_Model1_描述性統計.ipynb
│   ├── 01_Model2_描述性統計.ipynb
│   ├── 02_Model1_模型訓練.ipynb
│   ├── 02_Model2_模型訓練.ipynb
│   ├── 03_Model1_SHAP.ipynb
│   ├── 03_Model2_SHAP.ipynb
│   ├── requirements.txt
│   └── README.md
├── 04_資料集_DataSet/
│   └── README.md
├── 05_海報_Poster/
│   └── Research poster
└── README.md
