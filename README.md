# 下載
### 環境
pandas==1.1.2
numpy==1.19.5
torch==1.8.1+cu111
tqdm==4.50.2
sklearn== 0.0
Torchvision==0.7.0
Keras==2.2.4
Pandas==0.25.3
Numpy==1.16.1
Matplotlib==3.1.2
Natsort==7.0.1
Pillow==7.2.0
Opencv-python==4.2.0
Torchsummary==1.5.1
Tqdm==4.50.2
Scipy==1.2.1
prefetch_generator==1.0.1 (若無法安裝可不安裝，執行後續程式碼若該段有問題，跳過即可)

### Efficientnet
`pip install efficientnet_pytorch`
### yolov4
```
git clone https://github.com/AlexeyAB/darknet
cd darknet
mkdir weights
cd weights
wget https://github.com/AlexeyAB/darknet/releases/download/darknet_yolo_v3_optimal/yolov4.conv.137
```
### 中文手寫資料集
※ (file_path) & (output_path) 以實際檔案位置需求作修改、替換，解壓縮後資料夾名稱為 cleaned_data，共684,677個圖片

```
git clone https://github.com/chenkenanalytic/handwritting_data_all.git
cat (file_path)/all_data.zip* > (file_path)/all_data.zip
unzip -O big5 (file_path)/all_data.zip -d (output_path)
```

# 前處理及訓練 (皆使用Jupyter Notebook)
### 執行Data_Preprocessing_Upload.py
於第2個cell需做中文手寫資料集的路徑定義
於第3個cell需做訓練集、YoloV4(darknet)、測試集的路徑定義
### 執行model_train.py (訓練分類模型)
於第3個cell需做訓練集的路徑定義

### 更改yolov4之內容
1. 複製資料夾 - Yolo_Setting中的Makefile至darknet中
2. 複製資料夾 - Yolo_Setting中的obj.names至darknet/data中
3. 複製資料夾 - Yolo_Setting中的obj.data至darknet/data中
4. 複製資料夾 - Yolo_Setting中的yolo-obj.cfg至darknet/cfg中

需依照電腦顯卡種類，更改Makefile中的第20列ARCH部分，詳情可以參照第28列之後的段落
更改完之後回到darknet資料夾下進行compile

`make`

Compile完後可開始進行訓練

`./darknet detector train data/obj.data cfg/yolo-obj.cfg yolov4.conv.137 -mjpeg_port 8090 -map`

訓練完後針對Testing Data進行Predict

`./darknet detector test data/obj.data cfg/yolo-obj.cfg weights/yolo-obj_best.weights -ext_output -dont_show -out result.json<data/test.txt `

### 執行Model_Predict_Upload.py

於第2個cell需進行路徑定義
包含
1. 訓練資料之路徑
2. YoloV4(Darknet)的路徑
3. 測試資料之路徑
4. 測試Label CSV檔路徑
5. 訓練好的分類模型權重路徑

執行完成後，輸出結果將會儲存在訓練好的分類模型權重的路徑下，output.csv# Template
