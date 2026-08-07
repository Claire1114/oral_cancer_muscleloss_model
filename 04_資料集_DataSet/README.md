# Data Set 說明

## 一、資料無法提供之說明

本研究使用之資料來自彰化基督教醫院與馬偕紀念醫院，內容包含患者臨床病歷、治療資訊、放射治療劑量、治療副作用及電腦斷層影像衍生之身體組成指標，屬於受保護之醫療敏感資料。

基於研究倫理、患者隱私、醫院資料治理規範及資料使用授權限制，本研究無法上傳原始資料，亦無法提供可由多項臨床特徵組合而重新識別患者之資料檔案。因此，本資料夾僅提供資料來源、研究樣本數、使用特徵及預測變項之完整說明，不附實際 data set。

## 二、資料來源與研究對象

- 研究設計：回顧性隊列研究。
- 收案期間：2010 年至 2021 年。
- 開發隊列：彰化基督教醫院（Changhua Christian Hospital, CCH）。
- 外部驗證隊列：馬偕紀念醫院（MacKay Memorial Hospital, MMH）。
- 研究對象：新確診為口腔癌，並接受根治性手術及術後輔助放射治療之患者。
- 資料完整性處理：採用完整個案分析（Complete Case Analysis）。模型所需預測變數如有缺失，即排除該個案，不進行資料插補。

## 三、樣本數與資料切分

### 模型一：治療中預測模型

模型一共納入 903 名患者。

| 資料隊列 | 總樣本數 | 未發生顯著肌肉流失（Label 0） | 發生顯著肌肉流失（Label 1） |
|---|---:|---:|---:|
| CCH 開發隊列 | 572 | 428 | 144 |
| ├─訓練集（約 70%） | 400 | 299 | 101 |
| └─內部驗證集（約 30%） | 172 | 129 | 43 |
| MMH 外部驗證隊列 | 331 | 250 | 81 |
| 合計 | 903 | 678 | 225 |

### 模型二：治療前早期預測模型

模型二共納入 1,024 名患者。

| 資料隊列 | 總樣本數 | 未發生顯著肌肉流失（Label 0） | 發生顯著肌肉流失（Label 1） |
|---|---:|---:|---:|
| CCH 開發隊列 | 636 | 470 | 166 |
| ├─訓練集（約 70%） | 445 | 329 | 116 |
| └─內部驗證集（約 30%） | 191 | 141 | 50 |
| MMH 外部驗證隊列 | 388 | 290 | 98 |
| 合計 | 1,024 | 760 | 264 |

兩個模型因臨床收案基準及預測時間點不同，納入的樣本數並不相同，不可將兩模型樣本數直接相加視為不重複患者總數。

## 四、預測變項（Outcome／Label）

本研究之預測目標為患者接受放射治療後是否發生「顯著肌肉流失」，屬於二元分類任務：

- Label 0：放射治療後骨骼肌指數（Skeletal Muscle Index, SMI）損失小於 4.2%。
- Label 1：放射治療後 SMI 損失大於或等於 4.2%。

SMI 係利用患者治療前及治療後三個月內的電腦斷層掃描影像，取得第三頸椎（C3）骨骼肌橫截面積，換算為等效第三腰椎（L3）骨骼肌橫截面積後，再依身高平方進行標準化。肌肉流失比例以治療前 SMI 與治療後 SMI 的變化百分比計算。

## 五、模型一使用特徵

模型一為「治療中預測模型」，整合治療前基線資料、治療資訊、身體組成指標及治療期間副作用，共使用 18 項特徵。

### 5.1 基本臨床特徵（6 項）

1. 年齡（Age）。
2. 性別（Sex）。
3. 東部協作腫瘤組效能狀態評分（ECOG Performance Status, ECOG PS）。
4. 腫瘤位置（Tumor Site）。
5. 病理分期（Pathological Stage）。
6. 抽菸習慣（Smoking Habit）。

### 5.2 治療與身體組成特徵（4 項）

7. 是否接受同步化學治療（Concurrent Chemotherapy）。
8. 治療前身體質量指數（Pre-treatment BMI）。
9. 治療期間 BMI 變化量（BMI Change）。
10. 治療前骨骼肌指數（Pre-treatment SMI）。

### 5.3 治療期間副作用（8 項）

副作用資料來自放射治療期間的每週門診紀錄；同一患者如有多次紀錄，以治療期間最嚴重程度作為模型輸入。

11. 皮膚炎（Dermatitis）。
12. 黏膜炎（Mucositis）。
13. 口乾（Xerostomia）。
14. 吞嚥困難（Dysphagia）。
15. 疼痛（Pain）。
16. 食慾不振（Anorexia）。
17. 噁心（Nausea）。
18. 疲勞感（Fatigue）。

## 六、模型二使用特徵

模型二為「治療前早期預測模型」，僅使用放射治療開始前即可取得的臨床、治療、營養及放射劑量資訊，共使用 18 項特徵。

### 6.1 基本臨床特徵（5 項）

1. 年齡（Age）。
2. 性別（Sex）。
3. 東部協作腫瘤組效能狀態評分（ECOG Performance Status, ECOG PS）。
4. 腫瘤位置（Tumor Site）。
5. 病理分期（Pathological Stage）。

### 6.2 治療與身體組成特徵（2 項）

6. 是否接受同步化學治療（Concurrent Chemotherapy）。
7. 治療前骨骼肌指數（Pre-treatment SMI）。

### 6.3 發炎、共病與營養特徵（3 項）

8. 中性球與淋巴球比率（Neutrophil-to-Lymphocyte Ratio, NLR）。
9. 查爾森共病指數（Charlson Comorbidity Index, CCI）。
10. 簡易營養評估（Mini Nutritional Assessment, MNA）。

### 6.4 放射治療劑量特徵（8 項）

以下為放射治療計畫中與吞嚥相關之特定解剖區域劑量參數：

11. 頸部食道（Cervical Esophagus, CE）。
12. 環咽肌（Cricopharyngeus Muscle, CPA）。
13. 食道入口肌（Esophagus Inlet Muscles, EIM）。
14. 聲門區（Glottic Larynx, GL）。
15. 咽下縮肌（Inferior Pharyngeal Constrictor Muscle, IPCM）。
16. 咽中縮肌（Middle Pharyngeal Constrictor Muscle, MPCM）。
17. 咽上縮肌（Superior Pharyngeal Constrictor Muscle, SPCM）。
18. 聲門上區（Supraglottic Larynx, SGL）。


