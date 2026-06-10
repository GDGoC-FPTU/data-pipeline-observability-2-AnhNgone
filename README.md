[![Open in Visual Studio Code](https://classroom.github.com/assets/open-in-vscode-2e0aaae1b6195c2367325f4f02e2d04e9abb55f0b24a779b69b11b9e10269abc.svg)](https://classroom.github.com/online_ide?assignment_repo_id=24112727&assignment_repo_type=AssignmentRepo)
# Day 10 Lab: Data Pipeline & Data Observability

**Student Email:** ngoclight13@gmail.com
**Name:** Ngô Thị Ngọc Ánh

---

## Mo ta

Bài tập: Hoàn thiện một ETL pipeline đơn giản bằng Python, xử lý dữ liệu sản phẩm từ file JSON qua 4 bước: 
Extract → Validate → Transform → Load ra CSV.

Những gì đã làm:
Extract: Đọc file JSON với json.load(), bắt FileNotFoundError để pipeline không crash khi thiếu file.
Validate: Lọc bỏ record có price <= 0 hoặc category rỗng/None, log số record hợp lệ và bị drop.
Transform: Tính discounted_price = price * 0.9, chuẩn hoá category thành Title Case, thêm cột processed_at là timestamp hiện tại.
Load: Lưu DataFrame ra CSV bằng df.to_csv().

---

## Cach chay (How to Run)

### Prerequisites
```bash
pip install pandas
```

### Chay ETL Pipeline
```bash
python solution.py
```

### Chay Agent Simulation (Stress Test)
Bước 1 – Tạo clean data (chạy pipeline trước):
```bash
python solution.py
```
Bước 2 – Tạo garbage data:
```bash
python generate_garbage.py
```
Bước 3 – Chạy agent simulation với cả 2 bộ dữ liệu:
```bash
python agent_simulation.py
```


---

## Cau truc thu muc

```
├── solution.py              # ETL Pipeline script
├── processed_data.csv       # Output cua pipeline
├── experiment_report.md     # Bao cao thi nghiem
└── README.md                # File nay
```

---

## Ket qua
5 records extracted, 3 records saved, 2 records dropped
