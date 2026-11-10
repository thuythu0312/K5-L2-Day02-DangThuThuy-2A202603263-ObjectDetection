# Báo cáo cá nhân — Ngày 2 Object Detection

Đổi tên file theo `MSSV_HoTen.md`, ví dụ `20241234_NguyenVanAn.md`. Điền kết quả thật, xóa dòng hướng dẫn khi nộp. Xem [quy định nộp](../../SUBMISSION.md).

## 1. Thông tin học viên

- Họ tên: Đặng Thu Thúy
- MSSV: 2A202603263
- Email: thuythu0965@gmail.com
- Lớp: Khóa V - Level2-3
- Link repo bài làm: https://github.com/thuythu0312/K5-L2-Day02-DangThuThuy-2A202603263-ObjectDetection
- Link Colab đã chạy: https://colab.research.google.com/drive/1eVjwVUQezZqcBySAunQqe3bo-0zsm-aG#scrollTo=ja-WuN8qnY6V

## 2. Schema và mini guideline

Dẫn tới [GUIDELINE_MINI.md](../../GUIDELINE_MINI.md). Tóm tắt ba ca mơ hồ đã quyết định, cách áp dụng schema và phần cần giảng viên làm rõ.

## 3. Annotation và self-QC

- Số file nhãn train: 50
- Kết quả checker và lỗi đã xử lý: check_yolo_labels.py xác nhận 50/50 file hợp lệ. Không có lỗi định dạng trong lần kiểm tra này
- Danh sách ít nhất 10 ảnh đã rà bằng mắt: Codex đã xem tổng quan ảnh gốc drive_001 đến drive_012. Chưa có xác nhận học viên đã rà thủ công từng box trên các ảnh này
- Lỗi bỏ sót/box/class phát hiện khi rà và cách sửa: Bản gợi ý YOLO11n từng xuất không có lớp van. Đây là bộ nhãn khác với bộ đang có trong thư mục này. Chưa có nhật ký sửa từng box để xác nhận quá trình rework
- Commit khóa nhãn ban đầu: Chưa xác minh. Notebook ghi commit clone để chạy là eda56dd (Them labels train).

## 4. Gold QC và rework

| Chỉ số | Trước sửa | Sau sửa nếu có |
|---|---:|---:|
| TP | Chưa xác minh | 653 |
| FP | Chưa xác minh| 0 |
| FN | Chưa xác minh| 0 |
| Precision | Chưa xác minh | 1,000 |
| Recall | Chưa xác minh| 1,000 |
| F1 |Chưa xác minh | 1,000 |
| mean_iou_of_matches | Chưa xác minh| 1,000 |
| passed | Chưa xác minh| true |

Dẫn tới `outputs/annotation_evaluation.json` trong repo. Nếu rework, ghi ảnh, lỗi đã kiểm, quyết định sửa/giữ và lý do. Không suy nguyên nhân chỉ từ FP/FN. Không có rework thì ghi rõ.

## 5. Kết quả YOLO

- Runtime, GPU/CPU, phiên bản Ultralytics và Torch: Google Colab, CPU Intel Xeon 2,20 GHz, Python 3.13.18, Ultralytics 8.4.208 và Torch 2.14.1+cpu
- Các tham số thay đổi so với notebook nếu có và lý do: Cấu hình ghi trong log là YOLO26n, 50 epoch, imgsz=640, batch=-1. Notebook tự chọn CPU khi CUDA không khả dụng. Chưa thấy thay đổi thủ công so với cấu hình lưu trong notebook
- Đường dẫn best checkpoint trong phiên chạy: /content/Day2-Lab/outputs/yolo26_runs/vehicle_annotation/weights/best.pt

| Chỉ số trên 10 ảnh eval | Giá trị |
|---|---:|
| Precision | 0,5794 |
| Recall | 0,4935 |
| mAP50 | 0,5570 |
| mAP50–95 | 0,3821 |

Đặt cả 2 folder `assets/eval_predictions/` và `assets/yolo26_runs/` (giải nén từ `day2_colab_outputs.zip`) cạnh báo cáo và dẫn link tương đối. Không chỉ ghi số; chỉ ra ít nhất một ảnh và vật thể cụ thể.

Trả lời bốn câu hỏi cuối notebook:

1. Lớp nào recall thấp nhất? Ảnh nào minh họa?
Lớp có recall thấp nhất là bus, bằng 0,000.Ví dụ, nhãn eval của [drive_051](assets/eval_predictions/visuals/drive_051.jpg) có một xe buýt ở vùng trên bên phải. Log dự đoán của ảnh này ở ngưỡng 0,25 chỉ ghi 6 xe car
2. Nếu mAP50 cao nhưng mAP50–95 thấp, cần kiểm điều gì trên box?
Kiểm tra độ sát của box. mAP50 cao hơn mAP50–95 khoảng 0,175. So sánh box dự đoán với box chuẩn ở các cạnh và ở phần xe bị che hoặc cắt khung. Box lệch vị trí hoặc kích thước có thể đạt IoU 0,50 nhưng trượt các ngưỡng cao hơn.
3. Lỗi class nào có thể liên quan guideline? Dữ kiện nào hỗ trợ nhận xét?
Rà các ca bus và van nhìn giống nhau từ phía sau.** Log ghi recall bus là 0,000 và van là 0,709, với chỉ 2 box bus trong tập eval. Dữ kiện này cho thấy cần xem lại các ca xe buýt, nhưng chưa đủ để kết luận model nhầm bus thành van hoặc lỗi do guideline.
4. Vì sao không dùng 10 ảnh eval để train? Vai trò validation của chúng trong notebook là gì?
Giữ 10 ảnh eval ngoài tập train. Notebook dùng chúng làm validation để theo dõi chỉ số và chọn checkpoint tốt nhất, sau đó tính lại metrics cho checkpoint đó. Do cùng tập này tham gia chọn checkpoint, kết quả là điểm validation trên 10 ảnh, chưa phải điểm trên tập kiểm thử độc lập

## 6. Khó khăn và cách xử lý

| Bước/thời điểm | Lỗi quan sát | Cách đã thử | Kết quả/còn thiếu |
|---|---|---|---|
| Cài Docker | Windows báo ảo hóa tắt và PowerShell chưa tìm thấy Docker | Bật Intel Virtualization Technology, cài Docker Desktop và mở PowerShell mới | Container kiểm tra chạy thành công |
| Rà nhãn CVAT | YOLO11n COCO thiếu lớp van riêng | Dùng dự đoán làm gợi ý và xuất ảnh viền màu để đối chiếu | Cần xác nhận học viên đã tự rà từng xe và từng lớp |
| Gold QC | Nhãn train hiện tại trùng hoàn toàn với gold | So sánh nội dung cả 50 cặp file | Cần xác minh lại commit khóa nhãn và nguồn gốc bộ file |

## 7. Tự đánh giá theo rubric

Đã có bộ nhãn đúng cấu trúc, ba ví dụ cho guideline, log train YOLO26n, bốn chỉ số validation và 10 ảnh dự đoán. Còn thiếu xác nhận rà box thủ công, commit khóa nhãn, nguồn gốc kết quả Gold QC và link Colab chia sẻ.

## 8. Công cụ AI và tài liệu tham khảo

Ghi công cụ đã dùng, dùng ở bước nào và phần đã tự kiểm tra; không dùng thì ghi rõ. Liệt kê nguồn ngoài repo được sử dụng. Không ghi mật khẩu/token hoặc đính kèm gold train.
Đã dùng Codex để hỗ trợ cài Docker và CVAT, tích hợp YOLO11n, xuất nhãn gợi ý, kiểm tra cấu trúc, đối chiếu file và tổng hợp log. YOLO26n là model trong notebook huấn luyện

