# Mini guideline — bài làm cá nhân

> Điền nội dung này trước khi bắt đầu gán nhãn. Giữ lại các quyết định sau khi self-QC.

## Định nghĩa lớp

| Lớp | Định nghĩa của bạn | Ví dụ/điểm phân biệt |
| --- | --- | --- |
| `car` |xe ô tô có 2-7 chỗ | xe đi gia đình nhỏ gọn tính cả SUV, bán tải|
| `truck` |xe ô tô có thùng hoặc khoang chở hàng to | có thùng xe chở hàng lớn|
| `bus` | xe mang hình hộp chữ nhật có cỡ to | có kính cỡ to nhiều cửa sổ dài và nó mang hình hộp chữ nhật|
| `van` |xe từ 6,7 chỗ trở lên, có hoặc có khoang chở hàng, có thể mang dáng thùng |tính cả những xe chuyên chở hàng và những xe đi gia đình có 6,7 chỗ chở lên |

## Quy tắc chung

- Box bám sát vật thể nghĩa là: các phương tiện nằm sát nhau hoặc Bounding Box có sự tiếp giáp, vẫn tiến hành gán nhãn nếu có thể xác định được phương tiện.
- Vật thể bị che khuất:  Đối với vật thể bị che khuất trên 50%, nếu không thể xác định rõ phương tiện thì không tiến hành gán nhãn. Trường hợp vẫn có thể nhận diện chính xác phương tiện, cần xem xét theo tiêu chí nhận diện và mức độ che khuất của guideline.
- Vật thể bị cắt bởi rìa ảnh: Đối với vật thể bị khung hình cắt mất trên 50%, nếu không thể xác định được phương tiện thì không tiến hành gán nhãn. Nếu vẫn có thể nhận diện chính xác phương tiện, tiếp tục thực hiện gán nhãn.
- Vật thể rất nhỏ/mơ hồ: Không gán nhãn đối với phương tiện ở quá xa, có kích thước quá nhỏ hoặc hình ảnh quá mờ khiến không thể xác định được đối tượng. Trường hợp hình ảnh bị mờ nhưng vẫn đủ rõ để nhận diện phương tiện thì vẫn tiến hành gán nhãn. Không gán nhãn đối với vật thể vừa bị mờ vừa bị che khuất trên 50%, hoặc có góc quan sát không thuận lợi khiến đối tượng trở nên mơ hồ, kể cả khi có thể phỏng đoán hoặc nhận diện được phương tiện.

## Ghi lại quyết định cho ít nhất ba ca mơ hồ

| Ảnh | Tình huống | Quyết định cuối | Lý do theo schema |
| --- | --- | --- | --- |
| `drive_003` |xe tải bị cắt ở viền ảnh không thấy đầu xe | không gán | bị che quá 50% và không nhận rõ có đầu xe hay không|
| `drive_020` |xe bị chắn sau cầu thang | có gán |vẫn nhìn rõ đầu xe và đít xe , xác định được rõ loại xe|
| `drive_050` |xe đỏ bị cắt ở viền ảnh | không gán | bị cắt quá 50% |
