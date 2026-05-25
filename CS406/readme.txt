Thành viên nhóm
22520053 - Nguyễn Đức Anh
22521611 - Trần Khoa Tuấn
23520845 - Lê Xuân Song Lĩnh
23521085 - Nguyễn Trọng Nhân

link dataset: https://www.kaggle.com/datasets/faiyazabdullah/electrocom61

Hướng dẫn thực thi:

- Đối với yolov8, và Faster R-CNN + ResNet50-FPN backbone: tạo tài khoản kagle, tải aggle.json (kagle API của tài khoản mình) về máy, thực hiện lần lượt theo code trong yolov8.ipynb, two_stage.ipynb nếu như làm trên google colab hoặc điều chỉnh các đường dẫn cho phù hợp khi train ở chỗ khác, có thể điều chỉnh các chỉ số trong code phù hợp với cấu hình máy.

- Đối với yolov9: tải dataset trên kagle về và đưa các folder test, train, valid vào folder ElectroCom-61_v2, dowloads các bản trọng số của yolov9 (https://github.com/WongKinYiu/yolov9) về và đưa vào folder weights của thư mục yolov9 và upload lên drive, tiến hành các lệnh trong yolov9.ipynb để train, val, test các model tương ứng có thể điều chỉnh các chỉ số của model phù hợp nếu muốn train trên máy tính cá nhân sửa lại các đường dẫn trong các file .yaml tương ứng vs đường dẫn (điều chỉnh các đường dẫn thư mục chính xác với cấu trúc thư mục bạn có) kết quả cuối cùng sẽ nằm trong thư mục runs/train, runs/test, runs/valid.

- Sau khi có trọng số tốt nhất đối với từng model đưa các file best.pt vào thư mục project/runs/*/weights tương ứng để tiến hành demo, pip install các thư viện cần thiết cho máy của bạn và thực hiện lệnh streamlit run app.py trên temp của thư mục project để demo
  