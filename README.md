# Con lắc ngược – Phòng thí nghiệm ảo

Mô phỏng robot giữ thăng bằng cho cây gậy dựng ngược, dành cho học sinh **lớp 10**. Chạy ngay trên trình duyệt, không cần cài đặt, không cần phần cứng.

👉 **Mở mô phỏng:** `https://<tên-tài-khoản>.github.io/con-lac-nguoc/`
*(thay `<tên-tài-khoản>` bằng tên GitHub của bạn sau khi bật GitHub Pages)*

---

## Dùng thế nào?

- **Trên máy tính hoặc điện thoại:** mở đường link ở trên, hoặc quét mã QR giáo viên đưa.
- **Không có internet:** tải file `index.html` về rồi nhấp đúp để mở bằng Chrome, Edge, Firefox hoặc Safari.

Mỗi học sinh có thể tự làm trên máy của mình. Phần thử thách đã hoàn thành được lưu lại trên trình duyệt của máy đó.

## Bài học gồm 3 bước

| Bước | Học sinh làm gì | Rút ra điều gì |
|---|---|---|
| **1. Tự tay giữ** | Dùng phím ← → hoặc nút trên màn hình để tự giữ gậy đứng | Cân bằng không bền rất khó giữ. Người phản ứng không kịp nên cần máy tính |
| **2. Dạy robot bằng PID** | Nhập 3 con số P, I, D rồi bấm **Áp dụng**, sau đó bấm **Đẩy** để thử | Mỗi con số có vai trò riêng. Robot giữ được nhờ vòng phản hồi |
| **3. Tự đứng dậy** | Xem robot (hoặc tự mình) đẩy qua lại để đưa gậy từ dưới lên đỉnh | Đẩy đúng nhịp thì cơ năng tăng dần, giống như nhún xích đu |

### P, I, D là gì?

- **P – nghiêng nhiều thì đẩy mạnh.** Giống một cái lò xo kéo gậy về giữa.
- **I – nghiêng lâu thì đẩy thêm.** Cộng dồn độ nghiêng theo thời gian. Với con lắc này, I lớn lại làm cánh tay chạy đi xa, nên thường để I = 0.
- **D – đổ nhanh thì phanh lại.** Nhìn tốc độ đổ để chặn trước, giúp gậy không lắc qua lắc lại.

Bộ số gợi ý để bắt đầu: **P = 150, I = 0, D = 20**. Có sẵn 4 bộ số mẫu (*Bộ số tốt, Bỏ D, P nhỏ, I lớn*) để so sánh. Trước khi bấm Áp dụng, trang sẽ **đoán trước** bộ số vừa nhập có giữ được gậy hay không, nhờ đó học sinh tự kiểm chứng dự đoán của mình.

### 7 thử thách

1. Tự tay giữ gậy đứng 3 giây.
2. Nhập bộ số giúp robot giữ 10 giây.
3. Đẩy gậy 3 lần mà robot vẫn giữ được.
4. Thử D = 0 và xem gậy làm gì.
5. Thử I = 400 rồi bấm Đẩy: cánh tay chạy xa hay gần?
6. Xem robot tự đưa gậy lên đỉnh.
7. Tự tay đưa gậy lên đỉnh.

## Gợi ý cho giáo viên

Có thể dạy trong **4 tiết**:

1. **Vì sao khó?** Cho cả lớp dựng thước hoặc chổi trên tay, rồi làm Bước 1. Nối với bài *các dạng cân bằng* và *moment lực*.
2. **Vòng phản hồi.** Giải thích 3 việc Nhìn → Nghĩ → Đẩy ở cuối trang. Trò chơi gợi ý: một em bịt mắt dựng chổi, một em nhìn và hô hướng, để thấy độ trễ làm hỏng việc giữ thăng bằng.
3. **Chỉnh PID.** Học sinh làm Bước 2 theo nhóm: chỉ dùng P, thêm D, rồi thử I. Ghi lại kết quả vào phiếu và thi xem nhóm nào hoàn thành nhiều thử thách nhất.
4. **Năng lượng.** Làm Bước 3. Nối với bài *thế năng, động năng, bảo toàn cơ năng*.

Câu hỏi kiểm tra nhanh (không cần công thức):
- Vì sao chỉ có P thì gậy cứ lắc mãi?
- Vẽ lại vòng phản hồi gồm 3 bước.
- Vì sao robot phải đẩy qua lại nhiều lần mới đưa được gậy lên đỉnh?

## Mô phỏng này dựa trên gì?

Mô phỏng dựa theo dự án **[Arduino UNO Q Inverted Pendulum (PID UI)](https://projecthub.arduino.cc/Arduino_Genuino/arduino-uno-q-inverted-pendulum-pid-ui-e8833f)**. Mô hình thật gồm động cơ bước NEMA 17 quay cánh tay ngang và encoder 1000 vạch đo góc của gậy. Kiểu con lắc này gọi là **con lắc Furuta** (con lắc ngược quay).

Trong mô phỏng:
- Gậy được coi là thanh đồng chất dài 30 cm, cánh tay dài 15 cm. Đây là kích thước ước lượng, vì dự án gốc không công bố.
- Robot đo góc và tính lực đẩy **500 lần mỗi giây**. Encoder chỉ phân biệt được những thay đổi từ 0.36° trở lên, đúng như cảm biến thật.
- Motor có giới hạn sức đẩy, nên P quá lớn thì robot cũng không đẩy mạnh hơn được nữa.
- Bộ điều khiển dùng cho học sinh là PID đơn giản, kèm một "trợ lý" nhỏ giữ cánh tay không trôi xa. Cách này dễ hiểu hơn PID trong firmware gốc của Arduino.
- Ở Bước 3, robot bơm năng lượng theo nhịp dao động. Khi gậy lên gần đỉnh, robot chuyển sang một bộ giữ thăng bằng có sẵn.

## Cấu trúc thư mục

```
index.html   ← toàn bộ mô phỏng (HTML + CSS + JavaScript trong 1 file)
README.md    ← file này
```

Muốn sửa nội dung, chỉ cần mở `index.html` bằng một trình soạn thảo bất kỳ, sửa rồi tải lên lại.
