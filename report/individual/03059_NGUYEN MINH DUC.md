# Báo cáo cá nhân — Ngày 2 Object Detection
## 1. Thông tin học viên

- Họ tên: Nguyễn Minh Đức
- MSSV: 03059
- Email:nguyenminhduc221202@gmail.coom
- Lớp:L2A
- Link repo bài làm:https://github.com/nguyenminhduc221202-ops/K5-L2A-DAY02-NGUYENMINHDUC-03059-ObjectDetection-
- Link Colab đã chạy:https://colab.research.google.com/drive/1xCthp0xVeFTmKV8gezZBoh9oI4h19g4K#scrollTo=hZQ-3KwSwZQL

## 2. Schema và mini guideline

Dẫn tới [GUIDELINE_MINI.md](../../GUIDELINE_MINI.md). Tóm tắt ba ca mơ hồ đã quyết định, cách áp dụng schema và phần cần giảng viên làm rõ.

## 3. Annotation và self-QC

- Số file nhãn train:
- Kết quả checker và lỗi đã xử lý:
- Danh sách ít nhất 10 ảnh đã rà bằng mắt:
- Lỗi bỏ sót/box/class phát hiện khi rà và cách sửa:
- Commit khóa nhãn ban đầu:

## 4. Gold QC và rework

| Chỉ số | Trước sửa | Sau sửa nếu có |
|---|---:|---:|
| TP | 507 | - |
| FP | 171 | - |
| FN | 146 | - |
| Precision | 0.7478 | - |
| Recall | 0.7764 | - |
| F1 | 0.7618 | - |
| mean_iou_of_matches | 0.9768 | - |
| passed | False | - |

Dẫn tới `outputs/annotation_evaluation.json` trong repo. Nếu rework, ghi ảnh, lỗi đã kiểm, quyết định sửa/giữ và lý do. Không suy nguyên nhân chỉ từ FP/FN. Không có rework thì ghi rõ.

## 5. Kết quả YOLO

- Runtime, GPU/CPU, phiên bản Ultralytics và Torch:
 - GPU: Tesla T4 (CUDA:0)
  - Ultralytics: 8.4.208
  - Torch: 2.1.4+cu130
- Các tham số thay đổi so với notebook nếu có và lý do:Không thay đổi
- Đường dẫn best checkpoint trong phiên chạy:`/content/Day2-Lab/outputs/yolo26_runs/vehicle_annotation/weights/best.pt`

| Chỉ số trên 10 ảnh eval | Giá trị |
|---|---:|
| Precision | 0.1837 |
| Recall | 0.3204 |
| mAP50 | 0.3046 |
| mAP50–95 | 0.2157 |

Đặt cả 2 folder `assets/eval_predictions/` và `assets/yolo26_runs/` (giải nén từ `day2_colab_outputs.zip`) cạnh báo cáo và dẫn link tương đối. Không chỉ ghi số; chỉ ra ít nhất một ảnh và vật thể cụ thể.

Trả lời bốn câu hỏi cuối notebook:


**1. Lớp nào có recall thấp nhất? Chỉ ra một ảnh eval minh họa.**
- **Lớp có recall thấp nhất**: Lớp `bus` có Recall = 0.0 (không phát hiện được trường hợp nào trong số 2 instances thực tế trên tập eval).
- **Ảnh minh họa**: `drive_053.jpg` hoặc `drive_058.jpg` có chứa xe bus lớn nhưng mô hình không nhận diện đúng hoặc nhận diện nhầm sang lớp khác do số lượng mẫu train quá ít.

**2. Nếu mAP50 cao nhưng mAP50-95 thấp, box đang gặp vấn đề gì?**
- Khi mAP50 cao nhưng mAP50-95 thấp, mô hình có khả năng xác định đúng vị trí và phân loại vật thể tương đối tốt (ở ngưỡng IoU lỏng lẻo là 0.5), nhưng **độ chính xác về tọa độ của bounding box (khớp khung hình) còn kém**. Khi tăng ngưỡng IoU từ 0.5 lên 0.95, số lượng box đạt chuẩn giảm mạnh do vẽ box bị lệch, quá rộng hoặc quá hẹp so với thực tế.

**3. Có lỗi `truck`/`bus`/`van`/`car` nào liên quan tới guideline không?**
- Có. Ranh giới phân định giữa các dòng xe lớn như `truck` (xe tải), `bus` (xe khách), `van` (xe bán tải/16 chỗ) và `car` (xe con) rất dễ bị nhầm lẫn nếu người gán nhãn không tuân thủ chặt chẽ tài liệu hướng dẫn (Guideline). Ví dụ, các dòng SUV lớn hoặc xe bán tải cỡ lớn đôi khi bị gán nhãn sai lệch giữa `car` và `truck` hoặc `van`, dẫn đến mô hình học sai đặc trưng.

**4. Vì sao không dùng 10 ảnh eval để train dù chúng có nhãn?**
- **Tránh hiện tượng rò rỉ dữ liệu (Data Leakage)**: Tập `eval` đóng vai trò là tập kiểm thử độc lập để đánh giá khách quan khả năng tổng quát hóa của mô hình trên dữ liệu mới chưa từng thấy. Nếu đưa 10 ảnh này vào quá trình huấn luyện, độ đo đánh giá sẽ cực kỳ cao (bị overfit) nhưng không phản ánh đúng chất lượng thực tế của mô hình khi triển khai ngoài thực tế.

## 6. Khó khăn và cách xử lý

| Bước/thời điểm | Lỗi quan sát | Cách đã thử | Kết quả/còn thiếu |
|---|---|---|---|
| Huấn luyện mô hình YOLO | Số lượng mẫu huấn luyện quá ít (50 ảnh) dẫn đến mô hình gặp khó khăn khi học các lớp thiểu số như `bus` hay `truck`. | Sử dụng mô hình tiền huấn luyện `yolo26n.pt` làm checkpoint nền tảng để tinh chỉnh (finetune). | Precision đạt 0.1837, mô hình chưa nhận diện tốt các lớp hiếm như `bus` (Recall = 0.0). |
| Chất lượng gán nhãn | F1-score gán nhãn đạt 0.7618, chưa vượt qua ngưỡng yêu cầu 0.8 để pass hệ thống đánh giá tự động (`passed: False`). | Kiểm tra phân bố và kích thước gán nhãn trên các khung hình phức tạp. | Điểm trung bình trùng khớp (mean_iou_of_matches) đạt cực kỳ cao 0.9768, cho thấy các box gán trùng khớp đều có độ chính xác vị trí rất tốt. |

## 7. Tự đánh giá theo rubric
Đối chiếu theo RUBRIC.md:

Gán nhãn & Định dạng dữ liệu: Hoàn thành đầy đủ 50 ảnh huấn luyện, tổ chức đúng thư mục cấu trúc YOLO. Định dạng file nhãn .txt chuẩn chỉnh.
Đánh giá F1/IoU nhãn: Đã thực hiện đo đạc với kết quả F1 đạt 0.7618, Mean IoU đạt 0.9768. Do điểm F1 hiện tại là 0.76 (dưới 0.80), hệ thống đánh giá trả về passed: False. Cần tinh chỉnh thêm nhãn bị bỏ sót (FN) và nhãn sai lớp (FP) để nâng cao điểm gán nhãn.
Huấn luyện mô hình: Chạy hoàn chỉnh quá trình huấn luyện 50 epochs với model yolo26n.pt, lưu được checkpoint tốt nhất best.pt và các đồ thị trực quan.

Đối chiếu [RUBRIC.md](../../RUBRIC.md): phần nào đã có kết quả, phần nào còn thiếu hoặc chưa kiểm được. Không tự nhận hoàn thành bước chưa chạy.

## 8. Công cụ AI và tài liệu tham khảo
Tài liệu hướng dẫn chính thức của Ultralytics YOLO26/YOLOv8.
Guideline mini đính kèm trong repo đề bài để chuẩn hóa định nghĩa bounding box.

Ghi công cụ đã dùng, dùng ở bước nào và phần đã tự kiểm tra; không dùng thì ghi rõ. Liệt kê nguồn ngoài repo được sử dụng. Không ghi mật khẩu/token hoặc đính kèm gold train.
