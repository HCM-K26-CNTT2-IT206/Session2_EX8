# Bài 8: Xây dựng Khung Few-shot chuyển Thông số thành Lợi ích Khách hàng

**Sinh viên:** Hà Gia Bách  
**Doanh nghiệp:** TechShop

---

## 1. Phân tích vấn đề & hướng giải

**Vấn đề:** nhân viên mới đọc nguyên xi thông số ("RAM 32GB LPDDR5x, SSD 1TB PCIe Gen 4...") → khách văn phòng và phụ huynh không hiểu → khó chốt đơn.

**Vì sao dùng Few-shot?** Mô tả "hãy nói dễ hiểu" là mơ hồ, mỗi lần AI sẽ trả lời một kiểu. Đưa **3 ví dụ mẫu cùng cấu trúc** thì AI "nhân bản" được đúng giọng điệu, độ dài và cách ví von, nên mọi nhân viên nhận được câu tư vấn đồng nhất.

**Cấu trúc 3 phần bắt buộc của mỗi ví dụ:**

```
[Thông số kỹ thuật thô] -> [Giải thích đời thường] -> [Lợi ích thực tế cho công việc/học tập]
```

| Phần | Vai trò | Quy tắc viết |
|---|---|---|
| **1. Thông số thô** | Dữ liệu đầu vào từ bảng cấu hình | Giữ nguyên, không sửa |
| **2. Giải thích đời thường** | Dịch thuật ngữ sang hình ảnh quen thuộc | Dùng phép ví von gần gũi (bàn làm việc, kho chứa, ...), không dùng lại thuật ngữ chưa giải thích |
| **3. Lợi ích thực tế** | Trả lời "tôi được gì?" | Gắn với tình huống cụ thể của khách (đi làm văn phòng, con học online...) |

---

## 2. Ba cặp Few-shot mẫu chuẩn

### Cặp 1 – RAM & Khả năng đa nhiệm

**[Thông số kỹ thuật thô]**
RAM 32GB LPDDR5x

**[Giải thích đời thường]**
RAM giống như mặt bàn làm việc của máy tính: bàn càng rộng thì càng trải được nhiều tài liệu cùng lúc mà không phải cất bớt đi. 32GB là mặt bàn rất rộng, còn LPDDR5x là loại RAM đời mới, chạy nhanh và tiết kiệm pin.

**[Lợi ích thực tế cho công việc/học tập]**
Bạn có thể mở cùng lúc hàng chục tab trình duyệt, Excel, Zoom và phần mềm thiết kế mà máy vẫn chạy mượt, không bị đơ giữa buổi họp. Con bạn vừa học online, vừa làm slide, vừa tra cứu tài liệu cũng không bị giật.

---

### Cặp 2 – Ổ cứng SSD & Tốc độ khởi động máy / mở ứng dụng

**[Thông số kỹ thuật thô]**
Ổ cứng SSD 1TB PCIe Gen 4

**[Giải thích đời thường]**
Ổ cứng là kho chứa đồ của máy. 1TB là kho rộng, chứa được hàng chục nghìn ảnh và tài liệu. SSD là kho hiện đại, lấy đồ ra gần như tức thì (khác ổ HDD cũ phải chờ đĩa quay). PCIe Gen 4 là đường cao tốc nhiều làn nối kho với máy nên đồ được chuyển đi rất nhanh.

**[Lợi ích thực tế cho công việc/học tập]**
Bấm nút nguồn là vào màn hình làm việc nhanh, mở Word, Excel hay file nặng gần như không phải chờ. Nhân viên văn phòng tiết kiệm được hàng chục phút chờ mỗi tuần, học sinh vào bài là học ngay.

---

### Cặp 3 – Màn hình chuẩn màu & Bảo vệ mắt khi làm việc ban đêm

**[Thông số kỹ thuật thô]**
Màn hình OLED, độ phủ màu 100% DCI-P3, có chế độ giảm ánh sáng xanh phần cứng

**[Giải thích đời thường]**
OLED là loại màn hình mà từng điểm ảnh tự phát sáng nên màu đen sâu, hình ảnh sống động. 100% DCI-P3 nghĩa là hiển thị đầy đủ dải màu mà ngành điện ảnh và thiết kế dùng, màu trên màn hình rất sát thực tế. Chế độ giảm ánh sáng xanh giống như đeo một lớp kính lọc, bớt phần ánh sáng gây chói mắt.

**[Lợi ích thực tế cho công việc/học tập]**
Làm việc hay ôn bài buổi tối, mắt đỡ mỏi và dễ chịu hơn khi nhìn màn hình lâu. Người làm thiết kế, chỉnh ảnh thì màu in ra giống màu trên máy, còn cả nhà xem phim cũng đẹp hơn.

---

## 3. Master Prompt

> Master Prompt nạp đủ 3 cặp ví dụ và yêu cầu AI xử lý thông số mới. Có thể copy để dùng ngay.

````text
### VAI TRÒ
Bạn là chuyên viên tư vấn bán hàng giàu kinh nghiệm tại TechShop, giỏi giải thích công nghệ
cho người không am hiểu như khách văn phòng và phụ huynh mua máy cho con.

### NHIỆM VỤ
Chuyển các thông số kỹ thuật khô khan thành ngôn ngữ đời thường và lợi ích thực tế để nhân viên
đọc lên tư vấn cho khách. Hãy học theo ĐÚNG phong cách, độ dài và cấu trúc của 3 ví dụ mẫu dưới đây.

### CẤU TRÚC BẮT BUỘC CHO MỖI THÔNG SỐ
[Thông số kỹ thuật thô] -> [Giải thích đời thường] -> [Lợi ích thực tế cho công việc/học tập]

### CÁC VÍ DỤ MẪU

--- Ví dụ 1 ---
[Thông số kỹ thuật thô]: RAM 32GB LPDDR5x
[Giải thích đời thường]: RAM giống như mặt bàn làm việc của máy tính: bàn càng rộng thì càng trải được nhiều tài liệu cùng lúc mà không phải cất bớt đi. 32GB là mặt bàn rất rộng, còn LPDDR5x là loại RAM đời mới, chạy nhanh và tiết kiệm pin.
[Lợi ích thực tế cho công việc/học tập]: Bạn có thể mở cùng lúc hàng chục tab trình duyệt, Excel, Zoom và phần mềm thiết kế mà máy vẫn chạy mượt, không bị đơ giữa buổi họp. Con bạn vừa học online, vừa làm slide, vừa tra cứu tài liệu cũng không bị giật.

--- Ví dụ 2 ---
[Thông số kỹ thuật thô]: Ổ cứng SSD 1TB PCIe Gen 4
[Giải thích đời thường]: Ổ cứng là kho chứa đồ của máy. 1TB là kho rộng, chứa được hàng chục nghìn ảnh và tài liệu. SSD là kho hiện đại, lấy đồ ra gần như tức thì (khác ổ HDD cũ phải chờ đĩa quay). PCIe Gen 4 là đường cao tốc nhiều làn nối kho với máy nên đồ được chuyển đi rất nhanh.
[Lợi ích thực tế cho công việc/học tập]: Bấm nút nguồn là vào màn hình làm việc nhanh, mở Word, Excel hay file nặng gần như không phải chờ. Nhân viên văn phòng tiết kiệm được hàng chục phút chờ mỗi tuần, học sinh vào bài là học ngay.

--- Ví dụ 3 ---
[Thông số kỹ thuật thô]: Màn hình OLED, độ phủ màu 100% DCI-P3, có chế độ giảm ánh sáng xanh phần cứng
[Giải thích đời thường]: OLED là loại màn hình mà từng điểm ảnh tự phát sáng nên màu đen sâu, hình ảnh sống động. 100% DCI-P3 nghĩa là hiển thị đầy đủ dải màu mà ngành điện ảnh và thiết kế dùng, màu trên màn hình rất sát thực tế. Chế độ giảm ánh sáng xanh giống như đeo một lớp kính lọc, bớt phần ánh sáng gây chói mắt.
[Lợi ích thực tế cho công việc/học tập]: Làm việc hay ôn bài buổi tối, mắt đỡ mỏi và dễ chịu hơn khi nhìn màn hình lâu. Người làm thiết kế, chỉnh ảnh thì màu in ra giống màu trên máy, còn cả nhà xem phim cũng đẹp hơn.

### QUY TẮC & XỬ LÝ NGOẠI LỆ
1. Mỗi thông số tách riêng thành một khối theo đúng cấu trúc 3 phần ở trên; nếu đầu vào có nhiều thông số (nối bằng "và", dấu phẩy) thì xử lý lần lượt từng thông số.
2. Phần "Giải thích đời thường": dùng 1 phép ví von gần gũi; mọi thuật ngữ kỹ thuật xuất hiện phải được giải thích ngay, không để khách phải tra cứu.
3. Phần "Lợi ích": gắn với tình huống thật (đi làm văn phòng, con học tập/làm bài); mỗi khối tối đa 3 câu cho mỗi phần.
4. KHÔNG bịa số liệu hiệu năng (fps, phút, phần trăm) nếu thông số đầu vào không cung cấp; dùng cách nói định tính ("mượt hơn", "nhanh hơn rõ rệt").
5. KHÔNG hứa hẹn tuyệt đối, tránh "chắc chắn", "không bao giờ", "tốt nhất".
6. NẾU thông số bị trống, vô nghĩa hoặc bạn không nhận ra → KHÔNG đoán; ghi đúng một dòng: "⚠ Cần nhân viên kiểm tra lại thông số: <nguyên văn>".
7. Chỉ trả về các khối kết quả, KHÔNG chào hỏi, KHÔNG lời kết, KHÔNG giải thích thêm.

### THÔNG SỐ MỚI CẦN XỬ LÝ
"Card đồ họa RTX 4060 8GB VRAM và Tản nhiệt buồng hơi Vapor Chamber"
````

---

## 4. Kết quả AI trả về khi chạy Master Prompt

**Thông số 1**

**[Thông số kỹ thuật thô]**
Card đồ họa RTX 4060 8GB VRAM

**[Giải thích đời thường]**
Card đồ họa giống như một "họa sĩ chuyên nghiệp" riêng của máy, chuyên lo phần hình ảnh để bộ xử lý chính không bị quá tải. 8GB VRAM là chiếc bàn vẽ riêng của họa sĩ này: bàn càng rộng thì càng xử lý được nhiều hình ảnh, video, đồ họa nặng cùng lúc.

**[Lợi ích thực tế cho công việc/học tập]**
Máy dựng video, làm đồ họa, thiết kế 3D mượt hơn rõ rệt so với máy chỉ dùng đồ họa tích hợp. Con bạn học thiết kế hay làm video cho bài tập thì ít phải chờ, và cuối tuần chơi game giải trí cũng mượt hơn.

**Thông số 2**

**[Thông số kỹ thuật thô]**
Tản nhiệt buồng hơi Vapor Chamber

**[Giải thích đời thường]**
Khi máy làm việc nặng sẽ nóng lên, giống như người chạy bộ ra mồ hôi. Buồng hơi Vapor Chamber là một "tấm giải nhiệt" mỏng bên trong có chất lỏng, hút nhiệt từ chỗ nóng nhất rồi dàn đều ra cả tấm để quạt thổi đi, nên tản nhiệt nhanh và đều hơn ống đồng thông thường.

**[Lợi ích thực tế cho công việc/học tập]**
Máy giữ được độ mát khi làm việc lâu nên ít bị "tụt tốc" giữa chừng, chạy nặng vẫn ổn định và quạt đỡ ồn hơn. Ngồi làm việc hoặc học nhiều giờ liền, bàn phím và mặt máy đỡ nóng tay hơn.

---

## 5. Kiểm tra kết quả theo yêu cầu đề

| Tiêu chí | Đạt? | Minh chứng |
|---|---|---|
| Đủ 3 cặp Few-shot theo yêu cầu (RAM, SSD, màn hình) | ✅ | Mục 2 |
| Mỗi cặp theo đúng cấu trúc `Thô -> Đời thường -> Lợi ích` | ✅ | Mục 2, 3 |
| Master Prompt nạp đủ 3 ví dụ và thông số mới | ✅ | Mục 3 |
| Kết quả giữ nhất quán cấu trúc và phong cách với ví dụ | ✅ | Mục 4 |
| Không thuật ngữ khó mà không giải thích | ✅ | Mục 4 |
| Không bịa số liệu, không hứa tuyệt đối | ✅ | Mục 4 dùng cách nói định tính |
| Có xử lý ngoại lệ (thông số lạ/trống) | ✅ | Quy tắc 6 trong Master Prompt |

## 6. Giải thích thiết kế

1. **Ba ví dụ phủ ba kiểu ví von khác nhau** (bàn làm việc, kho chứa, kính lọc) nên AI học được *cách* ví von, không chép lại một hình ảnh duy nhất.
2. **Cấu trúc 3 phần cố định** giúp đầu ra đồng nhất, nhân viên mới chỉ cần đọc từ trên xuống dưới là tư vấn được.
3. **Phần ràng buộc** (không bịa số liệu, không hứa tuyệt đối, cảnh báo khi thông số lạ) tránh việc AI nói sai khiến nhân viên tư vấn sai cho khách.
4. **Phần "Lợi ích" luôn gắn tình huống người dùng** (văn phòng, phụ huynh, học sinh) vì đây chính là nhóm khách cần thuyết phục.
