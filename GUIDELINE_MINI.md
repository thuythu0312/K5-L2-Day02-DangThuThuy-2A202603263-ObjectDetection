# Mini guideline — bài làm cá nhân

> Điền nội dung này trước khi bắt đầu gán nhãn. Giữ lại các quyết định sau khi self-QC.

## Định nghĩa lớp

| Lớp | Định nghĩa của bạn | Ví dụ/điểm phân biệt |
| --- | --- | --- |
| `car` | Xe con, sedan, SUV, taxi và xe bán tải | Taxi vẫn là xe con. Xe bán tải thuộc lớp này theo schema của bài |
| `truck` | Xe tải chở hàng hoặc xe đầu kéo | Nhận diện qua cabin, thùng hàng hoặc cấu trúc đầu kéo |
| `bus` | Xe buýt, xe khách hoặc minibus chở khách có đặc điểm nhận diện rõ | Thân dài, nhiều cửa sổ hành khách hoặc cửa lên xuống |
| `van` | Xe van hoặc minivan chở hàng hay chở người | Thân liền khoang, nóc cao. Kiểm tra dáng thân và cửa khi khó phân biệt với minibus |


## Quy tắc chung

- Box bám sát vật thể nghĩa là: vẽ hình chữ nhật nhỏ nhất bao phần xe nhìn thấy. Mỗi xe dùng một box riêng
- Vật thể bị che khuất: vẫn được gán nếu phần thấy đủ để nhận diện lớp. Vẽ theo phần nhìn thấy
- Vật thể bị cắt bởi rìa ảnh: được gán nếu còn đủ dấu hiệu nhận diện. Box dừng tại biên ảnh
- Vật thể rất nhỏ/mơ hồ: phóng to để xem hình dáng. Nếu vẫn thiếu dấu hiệu nhận diện, em xin phép ghi lại để trao đổi trên lớp học

## Ghi lại quyết định cho ít nhất ba ca mơ hồ

| Ảnh | Tình huống | Quyết định cuối | Lý do theo schema |
| --- | --- | --- | --- |
| `drive_001` | Cột ven đường che một phần xe trắng ở làn bên phải | Gán car quanh phần xe nhìn thấy nếu ở ảnh gốc vẫn xác định được biên box | Phần thân còn thấy đủ để nhận diện xe con |
| `drive_004` | Xe đỏ cam ở góc dưới bên phải bị cắt qua mép dưới ảnh | Gán car, giới hạn box ở mép ảnh | Hình dáng và phần nóc còn thấy đủ để nhận diện xe con |
| `drive_008` | Xe buýt xanh vàng và xe tải thùng đỏ ở gần nhau | Gán hai box riêng với lớp bus và truck tương ứng | Xe buýt có dãy cửa sổ hành khách, còn xe tải có thùng chở hàng riêng |

