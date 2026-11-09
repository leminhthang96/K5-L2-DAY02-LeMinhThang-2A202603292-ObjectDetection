# Mini guideline — bài làm cá nhân

## Định nghĩa lớp

| Lớp | Định nghĩa của bạn | Ví dụ/điểm phân biệt |
| --- | --- | --- |
| `car` | Xe con, sedan, SUV, taxi và xe bán tải. | Dáng xe con; xe bán tải vẫn thuộc car theo schema. |
| `truck` | Xe tải chở hàng, xe đầu kéo. | Có thùng hoặc phần kéo hàng; không gồm xe bán tải. |
| `bus` | Xe buýt, minibus chở khách. | Thân xe chở khách, thường có nhiều hàng cửa kính. |
| `van` | Xe van, minivan chở hàng hoặc chở người. | Thân thường cao, dạng hộp. Khi nhìn ngang, tôi thấy một số xe dài và có nhiều ô kính hơn car; đây chỉ là dấu hiệu tham khảo, không phân loại chỉ bằng số ô kính. |

## Quy tắc chung

- Box bám sát vật thể nghĩa là: ôm sát phần thân xe nhìn thấy, không lấy thêm đường hoặc xe bên cạnh.
- Vật thể bị che khuất: vẽ box riêng quanh phần nhìn thấy nếu đủ nhận diện lớp, không đoán phần bị che.
- Vật thể bị cắt bởi rìa ảnh: box dừng đúng rìa ảnh, không đoán phần ngoài ảnh.
- Vật thể rất nhỏ/mơ hồ: không tự đoán lớp; ghi lại trường hợp chưa rõ để hỏi giảng viên.

## Ghi lại quyết định cho ít nhất ba ca mơ hồ

| Ảnh | Tình huống | Quyết định cuối | Lý do theo schema |
| --- | --- | --- | --- |
| `drive_003.jpg` | Các xe nối tiếp nhau nên che khuất nhau. | Vẽ box riêng quanh phần nhìn thấy của từng xe khi đủ nhận diện lớp. | Mỗi xe là một vật thể riêng, không gộp nhiều xe vào một box. |
| `drive_031.jpg` | Xe ở xa chỉ thấy đầu hoặc đuôi nên khó phân biệt loại xe. | Gán khi đủ đặc điểm nhận diện; trường hợp chưa rõ thì ghi lại để hỏi giảng viên. | Không tự đoán lớp khi thiếu thông tin. |
| `drive_017.jpg` | Xe phía bên kia đường bị cây che nên khó phân biệt. | Vẽ box quanh phần xe nhìn thấy nếu nhận diện được lớp; chưa rõ thì ghi lại để hỏi giảng viên. | Không đoán phần bị che hoặc lấy cây vào box. |