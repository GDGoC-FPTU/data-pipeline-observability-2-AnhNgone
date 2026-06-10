# Experiment Report: Data Quality Impact on AI Agent

**Student ID:** AI20K-2A202600618
**Name:** Ngô Thị Ngọc Ánh
**Date:** 10/06/2026

---

## 1. Ket qua thi nghiem

Chay `agent_simulation.py` voi 2 bo du lieu va ghi lai ket qua:

| Scenario | Agent Response | Accuracy (1-10) | Notes |
|----------|----------------|-----------------|-------|
| Clean Data (`processed_data.csv`) | Based on my data, the best choice is Laptop at $1200. |9/10 |Trừ 1 điểm vì agent chỉ dùng logic idxmax() (giá cao nhất = tốt nhất) |
| Garbage Data (`garbage_data.csv`) | Based on my data, the best choice is Nuclear Reactor at $999999. |1/10 |Sản phẩm không tồn tại thực tế, giá outlier |

---

## 2. Phan tich & nhan xet

### Tai sao Agent tra loi sai khi dung Garbage Data?

Garbage data trong thí nghiệm này chứa nhiều vấn đề chất lượng dữ liệu điển hình, mỗi loại gây ra một dạng lỗi khác nhau:
Duplicate IDs: Record id=1 xuất hiện hai lần với 2 sản phẩm khác nhau (Laptop và Banana). Agent không có cơ chế xử lý trùng lặp nên cả hai đều được tính vào tập dữ liệu, làm sai lệch kết quả lọc và thống kê.
Wrong data types: Record "Broken Chair" có price = "ten dollars" thay vì số. Khi agent gọi idxmax() trên cột price, pandas phải xử lý cột có kiểu dữ liệu hỗn hợp (string lẫn number), dễ gây lỗi hoặc bỏ qua record đó hoàn toàn.
Extreme outlier: "Nuclear Reactor" có price = 999999 — một giá trị phi thực tế. Vì agent dùng logic idxmax() để tìm sản phẩm "tốt nhất" dựa trên giá cao nhất, outlier này ngay lập tức chiếm kết quả, đẩy ra câu trả lời vô nghĩa với người dùng.
Null values: Record "Ghost Item" có id = None, category = None, price = 0. Khi lọc theo category, None.str.lower() có thể ném exception nếu không được xử lý, hoặc record bị bỏ qua âm thầm mà không có cảnh báo.
Tổng hợp lại: Agent hoạt động đúng về mặt logic code, nhưng vì không có bước validate dữ liệu đầu vào, nó tin tưởng hoàn toàn vào dữ liệu nhận được — dẫn đến output sai dù model không có lỗi.

---

## 3. Ket luan

Quality Data > Quality Prompt? → Đồng ý.
Dù agent có logic xử lý hợp lý và câu query rõ ràng, kết quả vẫn sai hoàn toàn khi dữ liệu đầu vào có vấn đề. Một outlier duy nhất (price = 999999) đã đủ để phá vỡ toàn bộ câu trả lời. Điều này cho thấy: data quality là nền tảng, prompt chỉ là lớp trên. Cải thiện prompt không thể bù đắp cho dữ liệu bẩn — pipeline ETL với bước validate chặt chẽ là điều kiện tiên quyết trước khi đưa dữ liệu vào bất kỳ hệ thống AI nào.
