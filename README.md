# ⚽ DS108 - Football Prediction

This project focuses on predicting football matches's results based on historical data.

## 📌 Table of Contents

- [Project Structure](#project-structure)
- [Prediction App](#prediction-app)
- [Dataset](#dataset)
- [Methodology](#methodology)
- [Final Model](#final-model)
- [How to Run the Prediction Interface](#how-to-run-the-prediction-interface)
- [License](#license)
- [Credits](#credits)

## 📁 Project Structure:

```
Football-Match-Prediction/
├── requirements.txt # Danh sách thư viện cần thiết
├── README.md # Mô tả tổng quan dự án
├── Project Report.pdf # Báo cáo chi tiết
├── Project Presentation.pptx # Slide trình bày
├── .gitignore # Tệp cấu hình để Git bỏ qua file/thư mục
├── Source Code/ # Thư mục chứa mã nguồn (notebooks, scripts)
├── Data and Model/ # Thư mục chứa dữ liệu và mô hình huấn luyện
```

Giải thích chức năng:
- `DataPreprocessing_Rolling.ipynb`Tiền xử lý dữ liệu theo tiêu chuẩn *Medallion Architecture* và tạo đặc trưng bằng phương pháp **Rolling Window**.
- `DataPreprocessing_Rolling_Form.ipynb`Tương tự như trên nhưng bổ sung đặc trưng từ thông tin **Form** (hiệu suất gần đây của đội bóng).
- `EDA_Visualization.ipynb`Trực quan hóa dữ liệu với các biểu đồ phục vụ phân tích và đánh giá.
- `Main_Rolling.ipynb`Huấn luyện và kiểm thử các mô hình sử dụng đặc trưng từ **Rolling Window**.
- `Main_Rolling_Form.ipynb`Huấn luyện và kiểm thử mô hình với đặc trưng từ **Rolling Window** và **Form**.
- `TimeSplitValidation.ipynb`Đánh giá khả năng tổng quát của mô hình bằng kỹ thuật **TimeSeriesSplit** (walk-forward validation).

---

## ⚙️ Prediction App:

- `predictor.py`Load mô hình và dữ liệu; định nghĩa hàm dự đoán cho giao diện thử nghiệm.
- `PredictApp.py`
  Xây dựng giao diện người dùng bằng **Streamlit**, tương tác với `predictor.py`.

---

## 📦 Dataset:

- `FPro_data.csv`Dữ liệu gốc thu thập từ web crawling:[fbref](https://fbref.com/en/) — tương ứng với **Bronze Layer**.
- `silver_FPro_data.csv`Dữ liệu đã được xử lý — đạt chuẩn **Silver Layer**, được tạo bởi các file `DataPreprocessing`.
- `gold_FPro_data_rolling.csv`Dữ liệu Silver có thêm đặc trưng **Rolling Window** — đạt chuẩn **Gold Layer**, được tạo bởi `DataPreprocessing_Rolling.ipynb`.
- `gold_FPro_data_combination.csv`
  Dữ liệu Silver có thêm đặc trưng **Rolling Window** và **Form** — đạt chuẩn **Gold Layer**, được tạo bởi `DataPreprocessing_Rolling_Form.ipynb`.

---

## 🧠 Methodology

Quy trình dự đoán kết quả trận đấu bóng đá được xây dựng theo kiến trúc **Medallion Architecture** gồm 3 lớp dữ liệu chính: Bronze, Silver và Gold. Mỗi lớp dữ liệu được xử lý và tăng cường đặc trưng theo các bước sau:

### 1. 📥 Dữ liệu đầu vào (Bronze Layer)
- Dữ liệu được thu thập từ web (cụ thể là trang Football-Data) và lưu vào file `FPro_data.csv`.
- Bao gồm thông tin các trận đấu như tên đội, tỷ số, ngày thi đấu, giải đấu, v.v.

### 2. 🔧 Tiền xử lý và chuẩn hóa (Silver Layer)
- Làm sạch dữ liệu, chuyển đổi kiểu dữ liệu, mã hóa nhãn (label encoding) và xử lý thiếu.
- Dữ liệu sau khi xử lý được lưu thành `silver_FPro_data.csv`.

### 3. 🪞 Tạo đặc trưng chuyên sâu (Gold Layer)
#### a. Rolling Statistics
- Áp dụng cửa sổ trượt (**Rolling Window**) để tạo đặc trưng thống kê như số trận thắng/thua/hòa gần nhất, tổng bàn thắng/bị thủng,...
- Được thực hiện bởi `DataPreprocessing_Rolling.ipynb`, lưu kết quả vào `gold_FPro_data_rolling.csv`.

#### b. Form của đội bóng
- Xây dựng đặc trưng hiệu suất gần đây (form), đặc trưng có tính trọng số dựa trên sức mạnh hiện tại.
- Kết hợp với rolling để tạo đặc trưng tổng hợp, lưu tại `gold_FPro_data_combination.csv`.

### 4. 🤖 Huấn luyện và đánh giá mô hình
- Các mô hình thử nghiệm bao gồm: **Logistic Regression**, **Random Forest**, **XGBoost**.
- Đánh giá bằng `accuracy`, `classification report`, và `confusion matrix`.
- Kỹ thuật **TimeSeriesSplit (walk-forward validation)** được sử dụng để đảm bảo không rò rỉ thời gian trong dự đoán.

### 5. ✅ Mô hình cuối cùng
- Mô hình XGBoost với đặc trưng từ **Rolling Window + Form** cho kết quả tốt nhất.
- Được lưu thành `Final_chosen_model.pkl` để sử dụng trong ứng dụng dự đoán.

### 6. 🌐 Ứng dụng dự đoán
- Xây dựng giao diện bằng **Streamlit**, nhập tên đội chủ nhà và đội khách để dự đoán kết quả.
- Giao diện gọi hàm từ `predictor.py` để load mô hình và dữ liệu Gold đã xử lý.


## 🤖 Final Model:

- **`Final_chosen_model.pkl`**
  Mô hình **XGBoost** với tham số tối ưu, được chọn làm mô hình dự đoán cuối cùng — được lưu từ `Main_Rolling_Form.ipynb`.

---

## 🚀 How to Run the Prediction Interface

1. Cài đặt thư viện
   ```
   pip install streamlit
   ```
2. Trong terminal, di chuyển đến thư mục chứa file **`PredictApp.py`** và chạy lệnh:
   ```
   streamlit run 22520550_22520884_PredictApp.py
   ```
3. Để thoát ứng dụng, nhấn tổ hợp phím **`Ctrl + C`** trong Terminal

--- 

## 📄 License
This project is for educational purposes. The dataset is attributed to the original website [fbref](https://fbref.com/en/) and were created by the project team.

---

## 👨‍🏫 Credits

Lương Anh Huy - 22520550

Phan Công Minh - 22520884

Instructor: TS. Nguyễn Gia Tuấn Anh
            CN. Trần Quốc Khánh

---

