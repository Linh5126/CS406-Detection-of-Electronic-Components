# Detection of Electronic Components

> Đồ án CS406 – Xử lý ảnh và ứng dụng  
> Nhận diện và phân loại linh kiện điện tử trong ảnh bằng các mô hình Object Detection.

## 1. Giới thiệu

Đồ án tập trung xây dựng hệ thống nhận diện linh kiện điện tử trong ảnh. Hệ thống có khả năng phát hiện vị trí linh kiện bằng bounding box và phân loại linh kiện theo từng nhãn tương ứng.

Bài toán được triển khai trên bộ dữ liệu **ElectroCom61**, gồm nhiều loại linh kiện điện tử như Arduino, resistor, capacitor, sensor, motor, relay module, OLED display, breadboard và nhiều linh kiện khác.

Mục tiêu chính của đồ án:

- Xây dựng mô hình nhận diện linh kiện điện tử tự động.
- So sánh các hướng tiếp cận object detection một giai đoạn và hai giai đoạn.
- Cải tiến YOLOv9 bằng các cơ chế attention khác nhau.
- Xây dựng demo trực quan bằng Streamlit để kiểm thử mô hình trên ảnh và video.

## 2. Thành viên nhóm

| MSSV | Họ tên |
|---|---|
| 22520053 | Nguyễn Đức Anh |
| 22521611 | Trần Khoa Tuấn |
| 23520845 | Lê Xuân Song Lĩnh |
| 23521085 | Nguyễn Trọng Nhân |

## 3. Dataset

Đồ án sử dụng bộ dữ liệu **ElectroCom61**.

- Nguồn dữ liệu: [Kaggle - ElectroCom61](https://www.kaggle.com/datasets/faiyazabdullah/electrocom61)
- Số lượng ảnh: 2,121 ảnh
- Số lượng annotation: 12,937 annotation
- Số lớp: 61 loại linh kiện điện tử
- Tỷ lệ chia dữ liệu:
  - Train: 70% – 1,478 ảnh
  - Validation: 20% – 438 ảnh
  - Test: 10% – 205 ảnh

Một số lớp trong bộ dữ liệu:

- Resistor
- Diode
- Capacitor
- Arduino Uno
- Arduino Nano
- ESP32
- Breadboard
- Relay Module
- LCD Display
- OLED Display
- Servo Motor
- DC Motor
- Buzzer
- Sensors

## 4. Phương pháp thực hiện

Đồ án thử nghiệm và so sánh nhiều mô hình object detection khác nhau.

### 4.1. Two-stage detector

Mô hình hai giai đoạn được sử dụng để làm hướng so sánh:

- **Faster R-CNN + ResNet50-FPN**

Mô hình này gồm hai bước chính:

1. Region Proposal Network đề xuất các vùng có khả năng chứa vật thể.
2. Detection Head phân loại và tinh chỉnh bounding box.

Ưu điểm của Faster R-CNN là độ chính xác tốt, đặc biệt với vật thể nhỏ. Tuy nhiên, tốc độ suy luận thường chậm hơn các mô hình one-stage.

### 4.2. One-stage detector

Các mô hình one-stage được sử dụng:

- **YOLOv8**
- **YOLOv9 baseline**
- **YOLOv9 + ECA Attention**
- **YOLOv9 + Coordinate Attention**
- **YOLOv9 + Global Context Attention**
- **YOLOv9 + SimAM Attention**
- **YOLOv9 + Triplet Attention**

YOLO được chọn vì có tốc độ xử lý nhanh, dễ triển khai và phù hợp với các bài toán nhận diện thời gian thực.

### 4.3. Cải tiến YOLOv9 bằng Attention

Đồ án thử nghiệm thêm các attention module vào YOLOv9 nhằm cải thiện khả năng tập trung vào vùng linh kiện quan trọng trong ảnh.

Các cơ chế attention được thử nghiệm:

| Attention Module | Ý nghĩa |
|---|---|
| ECA | Efficient Channel Attention, tăng cường thông tin theo kênh với chi phí thấp |
| CA | Coordinate Attention, kết hợp thông tin vị trí và kênh |
| GC | Global Context Attention, khai thác ngữ cảnh toàn cục |
| SimAM | Attention không tham số, nhẹ và dễ tích hợp |
| Triplet | Attention theo nhiều chiều không gian và kênh |

## 5. Kết quả thực nghiệm

Các mô hình được đánh giá bằng các chỉ số:

- Precision
- Recall
- mAP@50
- mAP@50:95
- F1-Score

| Model | Precision | Recall | mAP@50 | mAP@50:95 | F1-Score |
|---|---:|---:|---:|---:|---:|
| YOLOv9 + ECA | 0.895 | 0.921 | 0.934 | 0.630 | 0.908 |
| YOLOv9 + CA | 0.871 | 0.910 | 0.927 | 0.614 | 0.890 |
| YOLOv9 + GC | 0.891 | 0.915 | 0.941 | 0.621 | 0.903 |
| YOLOv9 + SimAM | 0.879 | 0.912 | 0.935 | 0.624 | 0.895 |
| YOLOv9 + Triplet | 0.889 | 0.927 | 0.931 | 0.615 | 0.908 |
| YOLOv9 Baseline | 0.888 | 0.915 | 0.938 | 0.622 | 0.901 |
| Faster R-CNN + ResNet50-FPN | 0.8163 | 0.8433 | 0.7918 | 0.5042 | 0.8296 |
| YOLOv8 | 0.812 | 0.800 | 0.843 | 0.543 | 0.806 |

Nhận xét:

- **YOLOv9 + ECA** là mô hình ổn định nhất, cải thiện đồng thời F1-Score và mAP@50:95.
- **YOLOv9 + GC** đạt mAP@50 cao nhất.
- **YOLOv9 + Triplet** có Recall cao nhất, phù hợp khi cần phát hiện nhiều đối tượng hơn.
- Các biến thể YOLOv9 nhìn chung cho kết quả tốt hơn YOLOv8 và Faster R-CNN trên bộ dữ liệu ElectroCom61.

## 6. Cấu trúc thư mục

```text
CS406/
├── docs/
│   ├── Báo_cáo_đồ_án_cs406.docx
│   └── seminar.pptx
│
├── readme.txt
│
└── source/
    ├── two_stage.ipynb
    ├── yolov8.ipynb
    ├── yolov9.ipynb
    │
    ├── yolov9/
    │   ├── train.py
    │   ├── val.py
    │   ├── detect.py
    │   ├── requirements.txt
    │   ├── ElectroCom-61_v2/
    │   │   └── data.yaml
    │   ├── models/
    │   ├── utils/
    │   └── weights/
    │
    └── project/
        ├── app.py
        ├── compare_all_models.py
        ├── detect.py
        ├── export.py
        ├── data/
        ├── models/
        ├── utils/
        └── runs/
            ├── electrocom61_yolov9/
            ├── electrocom61_eca/
            ├── electrocom61_ca13/
            ├── electrocom61_gc/
            ├── electrocom61_simam/
            └── electrocom61_triplet/
```

## 7. Cài đặt môi trường

Nên sử dụng Python 3.9 hoặc mới hơn.

Cài đặt các thư viện cần thiết:

```bash
pip install torch torchvision torchaudio
pip install ultralytics
pip install streamlit opencv-python pillow numpy pandas matplotlib seaborn tqdm pyyaml
```

Nếu chạy phần YOLOv9 gốc:

```bash
cd source/yolov9
pip install -r requirements.txt
```

Nếu sử dụng Google Colab, có thể chạy trực tiếp các notebook:

- `source/yolov8.ipynb`
- `source/yolov9.ipynb`
- `source/two_stage.ipynb`

## 8. Cách chuẩn bị dữ liệu

### Bước 1: Tải dataset từ Kaggle

Dataset được tải tại:

```text
https://www.kaggle.com/datasets/faiyazabdullah/electrocom61
```

Có thể tải thủ công hoặc dùng Kaggle API.

Nếu dùng Kaggle API trên Colab:

```bash
pip install kaggle
kaggle datasets download -d faiyazabdullah/electrocom61
unzip electrocom61.zip -d electrocom61_data
```

### Bước 2: Đặt dữ liệu cho YOLOv9

Đưa các thư mục `train`, `valid`, `test` vào:

```text
source/yolov9/ElectroCom-61_v2/
```

Cấu trúc mong muốn:

```text
ElectroCom-61_v2/
├── train/
│   ├── images/
│   └── labels/
├── valid/
│   ├── images/
│   └── labels/
├── test/
│   ├── images/
│   └── labels/
└── data.yaml
```

File `data.yaml` cần trỏ đúng đường dẫn train, validation và test.

## 9. Huấn luyện mô hình

### 9.1. Huấn luyện YOLOv8

Mở notebook:

```text
source/yolov8.ipynb
```

Notebook gồm các bước:

1. Cài đặt `ultralytics` và `kaggle`.
2. Tải dataset ElectroCom61.
3. Khởi tạo YOLOv8.
4. Huấn luyện mô hình.
5. Đánh giá bằng Precision, Recall, mAP.
6. Chạy thử trên ảnh test.

### 9.2. Huấn luyện Faster R-CNN

Mở notebook:

```text
source/two_stage.ipynb
```

Notebook gồm các bước:

1. Tải dataset.
2. Chuyển dữ liệu YOLO format sang format phù hợp với PyTorch.
3. Xây dựng Faster R-CNN với ResNet50-FPN backbone.
4. Huấn luyện và đánh giá mô hình.

### 9.3. Huấn luyện YOLOv9 + Attention

Mở notebook:

```text
source/yolov9.ipynb
```

Hoặc chạy trực tiếp trong thư mục `source/yolov9`.

Ví dụ huấn luyện YOLOv9 + ECA Attention:

```bash
cd source/yolov9

python train.py \
  --workers 4 \
  --device 0 \
  --batch 8 \
  --epochs 25 \
  --img 640 \
  --data ElectroCom-61_v2/data.yaml \
  --weights weights/gelan-c.pt \
  --cfg models/detect/gelan-c-attention-eca.yaml \
  --hyp data/hyps/hyp.scratch-high.yaml \
  --name electrocom61_eca \
  --patience 20 \
  --save-period 10 \
  --cache ram \
  --cos-lr
```

Có thể thay `--cfg` để huấn luyện các biến thể khác:

```text
models/detect/gelan-c-attention-eca.yaml
models/detect/gelan-c-attention-ca.yaml
models/detect/gelan-c-attention-gc.yaml
models/detect/gelan-c-attention-simam.yaml
models/detect/gelan-c-attention-triplet.yaml
models/detect/gelan-c.yaml
```

Sau khi huấn luyện, kết quả thường nằm trong:

```text
runs/train/
runs/val/
runs/test/
```

## 10. Chạy demo Streamlit

Demo nằm trong thư mục:

```text
source/project/
```

### Bước 1: Chuẩn bị trọng số mô hình

Sau khi huấn luyện xong, copy file `best.pt` tương ứng vào các thư mục sau:

```text
source/project/runs/electrocom61_yolov9/weights/best.pt
source/project/runs/electrocom61_eca/weights/best.pt
source/project/runs/electrocom61_ca13/weights/best.pt
source/project/runs/electrocom61_gc/weights/best.pt
source/project/runs/electrocom61_simam/weights/best.pt
source/project/runs/electrocom61_triplet/weights/best.pt
```

Lưu ý: trong source hiện tại, các thư mục `weights/` đã được tạo sẵn nhưng chưa kèm file `best.pt`. Cần bổ sung trọng số trước khi chạy demo đầy đủ.

### Bước 2: Chạy ứng dụng

```bash
cd source/project
streamlit run app.py
```

Sau khi chạy, mở đường dẫn mà Streamlit hiển thị trên terminal, thường là:

```text
http://localhost:8501
```

## 11. Chức năng của demo

Ứng dụng Streamlit hỗ trợ:

- Chọn model YOLOv9 đã huấn luyện.
- Upload ảnh để nhận diện linh kiện.
- Upload video để chạy detection theo từng frame.
- Điều chỉnh confidence threshold.
- Điều chỉnh IoU threshold.
- Chọn kích thước ảnh đầu vào.
- Hiển thị bounding box, nhãn và confidence.
- Thống kê số lượng object được phát hiện.
- Hiển thị bảng chi tiết từng object.
- So sánh kết quả giữa nhiều model khác nhau.

Các tab chính trong giao diện:

| Tab | Chức năng |
|---|---|
| Ảnh | Upload ảnh và xem kết quả detection |
| Video | Upload video và chạy detection |
| So sánh Models | So sánh nhiều mô hình trên cùng một ảnh |
| Thông tin | Xem thông tin model và attention mechanism |

## 12. So sánh nhiều mô hình bằng script

Ngoài giao diện Streamlit, có thể dùng script:

```bash
cd source/project
python compare_all_models.py path/to/image.jpg 0.25
```

Trong đó:

- `path/to/image.jpg`: đường dẫn ảnh cần kiểm thử.
- `0.25`: confidence threshold.

Script sẽ in ra:

- Thời gian inference.
- Số object phát hiện được.
- Confidence trung bình.
- Phân bố object theo class.
- Mô hình nhanh nhất.
- Mô hình phát hiện nhiều object nhất.
- Mô hình có confidence trung bình cao nhất.

## 13. Tài liệu đi kèm

Trong thư mục `docs/` có:

```text
Báo_cáo_đồ_án_cs406.docx
seminar.pptx
```

Các file này chứa báo cáo và slide thuyết trình của đồ án.

## 14. Lưu ý khi chạy project

- Cần tải dataset từ Kaggle trước khi train.
- Cần tải trọng số YOLOv9 ban đầu, ví dụ `gelan-c.pt`, từ repository YOLOv9 chính thức.
- Cần chỉnh đường dẫn trong notebook nếu chạy ngoài Google Colab.
- Với demo Streamlit, cần copy các file `best.pt` vào đúng thư mục `source/project/runs/.../weights/`.
- Nếu class name hiển thị không đúng, kiểm tra lại file `data.yaml` của ElectroCom61 và đường dẫn cấu hình khi load model.
- Nếu chạy trên CPU, thời gian inference sẽ chậm hơn đáng kể so với GPU.
- Các file trọng số `.pt` thường có dung lượng lớn, nên dùng Git LFS hoặc Google Drive nếu đưa project lên GitHub.

## 15. Hướng phát triển

Một số hướng phát triển tiếp theo:

- Tối ưu mô hình để chạy tốt hơn trên thiết bị tài nguyên thấp.
- Thử nghiệm thêm các backbone nhẹ hơn.
- Bổ sung đo FPS và latency chi tiết cho từng mô hình.
- Triển khai demo real-time bằng webcam.
- Tích hợp mô hình vào hệ thống kiểm tra linh kiện tự động.
- Mở rộng dataset với nhiều loại linh kiện và nhiều điều kiện ánh sáng hơn.
- Thử nghiệm thêm các kỹ thuật augmentation cho vật thể nhỏ.
- Đánh giá mô hình trên dữ liệu thực tế ngoài dataset.

## 16. Kết luận

Đồ án đã xây dựng và đánh giá hệ thống nhận diện linh kiện điện tử dựa trên các mô hình object detection hiện đại. Kết quả cho thấy các biến thể YOLOv9, đặc biệt là **YOLOv9 + ECA Attention**, đạt hiệu quả tốt và ổn định trên bộ dữ liệu ElectroCom61.

Hệ thống không chỉ dừng lại ở huấn luyện mô hình mà còn có demo trực quan bằng Streamlit, giúp kiểm thử mô hình trên ảnh, video và so sánh nhiều biến thể khác nhau một cách thuận tiện.
