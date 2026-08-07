# Source Code 說明

## 一、專案說明

本資料夾收錄碩士論文「利用可解釋性機器學習預測口腔癌患者之肌肉流失：開發治療中與治療前之風險模型」所使用之模型訓練與 SHAP 可解釋性分析程式。

程式分為兩個預測模型：

- 模型一：治療中預測模型（On-treatment Prediction Model），整合治療前基線資料、治療資訊、身體組成指標及治療期間副作用。
- 模型二：治療前早期預測模型（Pre-treatment Early Prediction Model），使用治療前臨床資料、營養與共病指標及放射治療劑量特徵。

兩個模型皆以「放射治療後骨骼肌指數（SMI）損失是否大於或等於 4.2%」作為二元分類預測變項。

## 二、程式檔案

| 檔案 | 模型 | 用途 |
|---|---|---|
| `01_Model1_描述性統計.ipynb` | 模型一 | 進行模型一資料檢查與描述性統計，依醫院（Institute）、資料分組（Group）及有無肌肉流失進行樣本特徵比較 |
| `01_Model2_描述性統計.ipynb` | 模型二 | 確認模型二樣本數、欄位數、連續型與類別型特徵，並依醫院（Institute）、資料分組（Group）及有無肌肉流失進行描述性統計與特徵比較 |
| `02_Model1_模型訓練.ipynb` | 模型一 | 匯入已分好的訓練集、內部驗證集與外部驗證集，執行 SMOTE-NC、Bootstrap、超參數選擇，以及 RF／XGBoost／CatBoost 模型重訓與驗證；產生 ROC、PR、校準曲線、決策曲線並保存 SHAP 分析輸入 |
| `02_Model2_模型訓練.ipynb` | 模型二 | 匯入並切分原始資料、進行訓練集 Spearman 相關分析，執行 SMOTE-NC、Bootstrap、超參數選擇，以及 RF／XGBoost／CatBoost 模型重訓與內外部驗證；產生 ROC、PR、校準曲線與決策曲線 |
| `03_Model1_SHAP.ipynb` | 模型一 | 載入模型一 CatBoost 模型結果，產生 SHAP importance plot、beeswarm plot、dependence plot 及個別樣本 force plot |
| `03_Model2_SHAP.ipynb` | 模型二 | 載入模型二 Random Forest 模型結果，產生 SHAP importance plot、beeswarm plot、dependence plot 及個別樣本 force plot |


三、執行環境

- 雲端運算環境：台灣杉二號（TWCC）。
- 作業系統：Linux。
- Linux 核心版本：4.18.0-305.131.1.el8_4.x86_64。
- 系統詳細資訊：Linux-4.18.0-305.131.1.el8_4.x86_64-x86_64-with-glibc2.39。
- 程式語言：Python 3.12.3。
- Python 執行檔：`/usr/bin/python3`。
- Notebook 核心：IPython 9.13.0、ipykernel 7.2.0。
- 程式編輯器／Notebook 前端及版本：`TWCC 開發容器 — Jupyter Notebook`。
- 網路伺服器及版本：未使用；本資料夾內容為離線模型訓練及分析 Notebook，並非網頁伺服器程式。

### 核心套件版本

- Python 3.12.3
- scikit-learn 1.7.2
- XGBoost 3.1.2
- CatBoost 1.2.8
- SHAP 0.50.0

所有 Python 套件版本統一記錄於 `requirements.txt`，作為建立實驗環境的唯一套件版本清單。

## 四、安裝方式

在終端機進入本資料夾後建立虛擬環境：

```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

Windows 啟用虛擬環境可使用：

```powershell
.venv\Scripts\activate
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

啟動 Jupyter Notebook：

```bash
jupyter notebook
```

## 五、建議執行順序

### 模型一

1. 準備經授權的模型一資料檔，並確認 Notebook 內讀取的檔名及工作表名稱正確。
2. 執行 `01_Model1_描述性統計.ipynb`，檢查資料與產生描述性統計結果。
3. 依序執行 `02_Model1_模型訓練.ipynb` 的所有儲存格。
4. 確認已產生 `Model_1_Results_include/` 及 SHAP 所需模型檔。
5. 執行 `03_Model1_SHAP.ipynb` 產生可解釋性圖表。

### 模型二

1. 準備經授權的模型二資料檔，並將 Notebook 中的資料路徑改為實際檔案位置。
2. 執行 `01_Model2_描述性統計.ipynb`，檢查資料與產生描述性統計結果。
3. 依序執行 `02_Model2_模型訓練.ipynb` 的所有儲存格。
4. 確認已產生 `Model_2_Results_include/` 及各模型的 SHAP bundle。
5. 執行 `03_Model2_SHAP.ipynb` 產生可解釋性圖表。

SHAP Notebook 必須在相對應的模型訓練 Notebook 成功完成並輸出模型檔後才能執行。

## 六、輸入資料

程式使用 Excel 格式的個案層級臨床資料，並依工作表區分訓練集、內部驗證集及外部驗證集。資料欄位、樣本數、模型一與模型二各自使用的 18 項特徵，以及預測變項定義，詳見 `../04_資料集_DataSet/README.md`。

由於資料來自醫院，屬於醫療敏感資料，受患者隱私、研究倫理及資料使用授權限制，因此資料不包含原始資料或個案層級資料。


## 七、輸出結果

模型訓練 Notebook 會依執行內容產生下列結果：

- Bootstrap 每次迭代的評估指標及最佳超參數。
- Random Forest、XGBoost 與 CatBoost 模型結果。
- 內部驗證與外部驗證之預測機率及分類結果。
- ROC、PR、校準曲線及決策曲線。
- 模型與 SHAP 分析所需的 Joblib／PKL bundle。
- SHAP importance、beeswarm、dependence 及 force plot。

主要輸出資料夾為：

- `Model_1_Results_include/`
- `Model_2_Results_include/`
