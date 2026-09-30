# 人臉辨識考勤系統

這是一個以 Python、OpenCV 與 LBPH（Local Binary Patterns Histograms）實作的人臉辨識考勤系統。程式會先以資料夾中的人臉照片訓練辨識模型，再透過攝影機即時偵測人臉；辨識成功時，會記錄人員的到達狀態與時間。

## 功能

- 使用 Haar Cascade 偵測影像中的人臉
- 使用 OpenCV LBPH 模型進行人臉辨識
- 支援三位已登錄人員
- 透過攝影機進行即時辨識
- 首次辨識成功時記錄到達時間
- 程式結束後輸出所有人員的最終考勤狀態

## 專案結構

```text
.
├── face01/                              # 人員 1 的訓練照片
├── face02/                              # 人員 2 的訓練照片
├── face03/                              # 人員 3 的訓練照片
├── face.yml                             # 已訓練的 LBPH 模型
├── finalproject.py                      # 訓練與即時辨識主程式
├── haarcascade_frontalface_default.xml  # Haar Cascade 人臉偵測模型
├── 影像處理期末專題-人臉辨識考勤系統.pdf
└── 影像期末-人臉辨識系統.pptx
```

## 執行環境

- Python 3
- 可正常使用的攝影機
- Python 套件：
  - `numpy`
  - `opencv-contrib-python`

> 必須安裝 `opencv-contrib-python`，因為 `cv2.face.LBPHFaceRecognizer_create()` 位於 OpenCV contrib 模組中。

## 安裝

```bash
python -m venv .venv
```

Windows PowerShell：

```powershell
.\.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
pip install numpy opencv-contrib-python
```

## 執行前設定

目前 `finalproject.py` 中的訓練照片與 Haar Cascade 路徑寫死為 `C:\final\...`。執行前請選擇以下其中一種方式：

1. 將整個專案放到 `C:\final`；或
2. 修改 `finalproject.py` 內的路徑，使其指向本機的實際專案位置。

程式中的人員姓名與 ID 定義於 `attendance` 字典：

```python
attendance = {
    '1': {"name": "Ling Chi Ni", "status": "未到", "time": None},
    '2': {"name": "Kuo Miao Hsuan", "status": "未到", "time": None},
    '3': {"name": "Tiang Shin", "status": "未到", "time": None},
}
```

如需更換辨識對象，請同步更新訓練照片、對應 ID 與此字典。

## 使用方式

```bash
python finalproject.py
```

程式的執行流程如下：

1. 讀取 `face01`、`face02`、`face03` 中的照片。
2. 偵測照片中的人臉並訓練 LBPH 模型。
3. 將模型儲存為 `face.yml`。
4. 開啟預設攝影機並進行即時辨識。
5. 辨識成功後，在終端顯示姓名與到達時間。
6. 在攝影機視窗按下 `q` 結束程式，並輸出最終考勤狀態。

## 辨識判定

程式將 LBPH 回傳的距離值存於 `confidence`，並以 `confidence < 60` 作為辨識成功條件。此數值越小，代表輸入人臉與訓練資料越接近；實際門檻可依光線、鏡頭與資料品質調整。

## 注意事項

- 本專案包含真人臉部照片與姓名等個人資料，請勿在未取得當事人同意的情況下公開、散布或用於其他目的。
- 請確認攝影機未被其他應用程式占用。
- 訓練照片的檔名格式與程式中的讀取規則必須一致。
- 若偵測不到人臉，請檢查照片清晰度、光線、正面角度與檔案路徑。
- 本專案適合作為課程展示與技術練習；若用於正式考勤，仍需補強資料保護、權限控管、錯誤處理與辨識準確度驗證。

## 技術

- Python
- OpenCV
- NumPy
- Haar Cascade
- LBPH Face Recognizer
