# Mini guideline — bài làm cá nhân

> Điền nội dung này trước khi bắt đầu gán nhãn. Giữ lại các quyết định sau khi self-QC.

## Định nghĩa lớp

| Lớp | Định nghĩa của bạn | Ví dụ/điểm phân biệt |
| --- | --- | --- |
| `car` | Xe ô tô con (chủ yếu thiết kế để chở từ 2 đến 5 người, bao gồm Sedan, Hatchback, SUV nhỏ/trung).| Đặc điểm: Thân xe liền khối, gầm thấp đến trung bình, tập trung vào sự tiện nghi và di chuyển cá nhân/gia đình.
Ví dụ: Toyota Camry, Honda Civic, Mazda CX-5.|
| `truck` |Xe tải (thiết kế chuyên dụng để chở hàng hóa, tài sản với tải trọng từ nhẹ đến nặng).|Đặc điểm: Thường có cabin rời với thùng hàng phía sau (thùng hở, thùng kín hoặc xe đầu kéo); gầm cao, động cơ khỏe.Ví dụ: Hyundai Porter, Isuzu NPR, xe đầu kéo Freightliner, Ford F-150. |
| `bus` |Xe khách / xe buýt (thiết kế để vận chuyển số lượng lớn hành khách, thường từ 16–29 chỗ trở lên đến 50+ chỗ).|Đặc điểm: Thân xe dài, trần cao, không gian hành khách rộng rãi, có cửa lên xuống dành riêng cho nhiều người và lối đi ở giữa.Ví dụ: Xe buýt nội đô, Hyundai Universe, Thaco Mobihome. |
| `van` |Xe tải van / xe bán tải khoang kín (thiết kế hình khối vuông chữ nhật, dung tích khoang chứa rộng, dùng chở hàng nhẹ hoặc kết hợp chở người 7–16 chỗ).|Đặc điểm: Chiều cao lớn, cửa phía sau mở đôi hoặc cửa hông dạng trượt (sliding door). Nhỏ hơn bus nhưng chở hàng tối ưu hơn car.Ví dụ: Ford Transit, Toyota HiAce, Mercedes-Benz Sprinter. |

## Quy tắc chung

- Box bám sát vật thể nghĩa là: khung giới hạn (bounding box) được vẽ sát khít với các mép ngoài cùng của đối tượng trong ảnh, không thừa quá nhiều khoảng trống (background) xung quanh nhưng cũng không cắt xén vào phần thân của vật thể.
- Vật thể bị che khuất: Box vẫn ôm sát phần thấy được của vật thể (phần lộ ra ngoài), hoặc tuân theo quy chuẩn gán nhãn của tập dữ liệu (dataset) — có thể dự đoán/ngoại suy phần bị che khuất dựa trên hình dáng toàn vẹn của vật thể đó nếu biết trước ranh giới.
- Vật thể bị cắt bởi rìa ảnh: Box sẽ bám sát các phần còn lại của vật thể nằm bên trong khung hình, còn cạnh bị cắt bởi rìa ảnh thường sẽ được căn thẳng sát theo đúng mép biên của bức ảnh (border của ảnh).
- Vật thể rất nhỏ/mơ hồ: Box vẫn cố gắng bao trọn lấy vùng pixel chứa vật thể đó một cách chính xác nhất có thể; tuy nhiên, với các vật thể quá mờ hoặc quá nhỏ (ít pixel), box thường có độ trễ/sai số lớn hơn hoặc được gán nhãn theo kích thước tối thiểu do quy định của bài toán.

## Ghi lại quyết định cho ít nhất ba ca mơ hồ

| Ảnh | Tình huống | Quyết định cuối | Lý do theo schema |
| --- | --- | --- | --- |
| `drive_...` | | | |
| `drive_...` | | | |
| `drive_...` | | | |
