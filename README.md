## 📌 GIỚI THIỆU VÀ MÔ TẢ CHỨC NĂNG CÁC FILE

### 📁 Notebook xử lý và huấn luyện:

- **`DataPreprocessing_Rolling.ipynb`**Tiền xử lý dữ liệu theo tiêu chuẩn *Medallion Architecture* và tạo đặc trưng bằng phương pháp **Rolling Window**.
- **`DataPreprocessing_Rolling_Form.ipynb`**Tương tự như trên nhưng bổ sung đặc trưng từ thông tin **Form** (hiệu suất gần đây của đội bóng).
- **`EDA_Visualization.ipynb`**Trực quan hóa dữ liệu với các biểu đồ phục vụ phân tích và đánh giá.
- **`Main_Rolling.ipynb`**Huấn luyện và kiểm thử các mô hình sử dụng đặc trưng từ **Rolling Window**.
- **`Main_Rolling_Form.ipynb`**Huấn luyện và kiểm thử mô hình với đặc trưng từ **Rolling Window** và **Form**.
- **`TimeSplitValidation.ipynb`**
  Đánh giá khả năng tổng quát của mô hình bằng kỹ thuật **TimeSeriesSplit** (walk-forward validation).

---

### ⚙️ Chương trình ứng dụng dự đoán:

- **`predictor.py`**Load mô hình và dữ liệu; định nghĩa hàm dự đoán cho giao diện thử nghiệm.
- **`PredictApp.py`**
  Xây dựng giao diện người dùng bằng **Streamlit**, tương tác với `predictor.py`.

---

### 📊 Dữ liệu:

- **`FPro_data.csv`**Dữ liệu gốc thu thập từ web crawling — tương ứng với **Bronze Layer**.
- **`silver_FPro_data.csv`**Dữ liệu đã được xử lý — đạt chuẩn **Silver Layer**, được tạo bởi các file `DataPreprocessing`.
- **`gold_FPro_data_rolling.csv`**Dữ liệu Silver có thêm đặc trưng **Rolling Window** — đạt chuẩn **Gold Layer**, được tạo bởi `DataPreprocessing_Rolling.ipynb`.
- **`gold_FPro_data_combination.csv`**
  Dữ liệu Silver có thêm đặc trưng **Rolling Window** và **Form** — đạt chuẩn **Gold Layer**, được tạo bởi `DataPreprocessing_Rolling_Form.ipynb`.

---

### 🤖 Mô hình cuối cùng:

- **`Final_chosen_model.pkl`**
  Mô hình **XGBoost** với tham số tối ưu, được chọn làm mô hình dự đoán cuối cùng — được lưu từ `Main_Rolling_Form.ipynb`.

---

## 🚀 HƯỚNG DẪN CHẠY GIAO DIỆN DỰ ĐOÁN

1. Cài đặt thư viện
   ```
   pip install streamlit
   ```
2. Trong terminal, di chuyển đến thư mục chứa file **`PredictApp.py`** và chạy lệnh:
   ```
   streamlit run 22520550_22520884_PredictApp.py
   ```
3. Để thoát ứng dụng, nhấn tổ hợp phím **`Ctrl + C`** trong Terminal
