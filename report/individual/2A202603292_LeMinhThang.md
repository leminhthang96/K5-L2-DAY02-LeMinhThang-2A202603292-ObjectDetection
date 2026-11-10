# Báo cáo cá nhân — Ngày 2 Object Detection

Đổi tên file theo `MSSV_HoTen.md`, ví dụ `20241234_NguyenVanAn.md`. Điền kết quả thật, xóa dòng hướng dẫn khi nộp. Xem [quy định nộp](../../SUBMISSION.md).

## 1. Thông tin học viên

- Họ tên: Lê Minh Thắng
- MSSV:2A202603292
- Email:leminhthang78962@gmail.com
- Lớp: 2B-LAB-E403
- Link repo bài làm: https://github.com/leminhthang96/K5-L2-DAY02-LeMinhThang-2A202603292-ObjectDetection
- Link Colab đã chạy: https://colab.research.google.com/drive/1rxVE9yy7UiDgnUszNakDGFYRakl9B5tq?usp=sharing

## 2. Schema và mini guideline

Dẫn tới [GUIDELINE_MINI.md](../../GUIDELINE_MINI.md). Tóm tắt ba ca mơ hồ đã quyết định, cách áp dụng schema và phần cần giảng viên làm rõ.

Schema gồm bốn lớp:
- 0: car — xe con, sedan, SUV, taxi và xe bán tải.
- 1: truck — xe tải chở hàng, xe đầu kéo.
- 2: bus — xe buýt, minibus chở khách.
- 3: van — xe van, minivan chở hàng hoặc chở người.

Ba ca mơ hồ đã ghi nhận:
- drive_003.jpg: nhiều xe nối tiếp và che khuất nhau.
- drive_031.jpg: xe ở xa chỉ thấy đầu hoặc đuôi, khó phân biệt lớp.
- drive_017.jpg: xe bị cây che, khó nhận diện.

Áp dụng box riêng cho từng xe, bám sát phần nhìn thấy và không
đoán phần ngoài ảnh. Số ô kính và chiều dài thân xe chỉ là dấu
hiệu tham khảo khi phân biệt car và van.

Phần cần làm rõ: sự khác biệt về lớp xe và phạm vi gán nhãn
giữa gold được phát với schema, guideline của bài.

## 3. Annotation và self-QC

- Số file nhãn train: 50 file .txt
- Kết quả checker và lỗi đã xử lý: ĐẠT cấu trúc, 50/50 file nhãn hợp lệ.
- Danh sách ít nhất 10 ảnh đã rà bằng mắt: `drive_002.jpg`, `drive_003.jpg`, `drive_008.jpg`, `drive_012.jpg`, `drive_017.jpg`, `drive_024.jpg`, `drive_031.jpg`, `drive_038.jpg`, `drive_044.jpg`, `drive_050.jpg`.
- Lỗi bỏ sót/box/class phát hiện khi rà và cách sửa: Đã kiểm tra xe bị che khuất, xe ở xa và xe khó phân loại; chỉnh box chưa sát và đổi lớp khi xác định nhãn chưa đúng trong CVAT, sau đó export và kiểm tra lại.
- Commit khóa nhãn ban đầu: 770d325

## 4. Gold QC và rework

Bảng dưới ghi kết quả đánh giá mới nhất đã xác nhận.
Không gán kết quả này thành một cặp trước/sau khi chưa đối chiếu
đầy đủ phiên bản nhãn và JSON tương ứng.

| Chỉ số | Kết quả mới nhất |
|---|---:|
| TP | 460 |
| FP | 242 |
| FN | 193 |
| Precision | 0.6553 |
| Recall | 0.7044 |
| F1 | 0.6790 |
| mean_iou_of_matches | 0.7824 |
| passed | false |

- Ngưỡng ghép box: cùng lớp và IoU ≥ 0.50.
- Yêu cầu F1: ≥ 0.80.
- Yêu cầu mean IoU: ≥ 0.70.
- Kết luận: mean IoU đạt, F1 chưa đạt.

Kết quả chi tiết:
[annotation_evaluation.json](../../outputs/annotation_evaluation.json).

Đã có rework trong CVAT và dùng ảnh đối chiếu để kiểm tra.
Các tình huống đã xem gồm xe che nhau ở drive_003.jpg, xe ở xa
ở drive_031.jpg và xe bị cây che ở drive_017.jpg.
Quyết định gán nhãn được ghi trong mini guideline.

Ví dụ drive_002 có 17 box cá nhân, 13 box gold và 10 box khớp.
Các con số cho thấy sự khác biệt nhưng không đủ để kết luận
mọi box không khớp đều là xe gán thừa; cần kiểm tra vị trí,
lớp và phạm vi annotation.

Thông báo Lab Coach được chia sẻ cho biết gold có sự không
thống nhất và cho phép bỏ qua bước kiểm tra để tiếp tục.
Tôi giữ kết quả thực tế là passed=false và tiếp tục train,
không thay ngưỡng đánh giá hoặc sửa JSON thành đạt.


## 5. Kết quả YOLO

- Runtime: Google Colab.
- Python: 3.13.16.
- GPU: NVIDIA Tesla T4.
- Ultralytics: 8.4.208.
- Torch: 2.14.1+cu130.
- Checkpoint khởi tạo: yolo26n.pt.
- Tham số: epochs=50, imgsz=640, batch=-1, seed=20260909.
- Không thay đổi các tham số train so với notebook.
- Dữ liệu: 50 ảnh train và 10 ảnh eval.
- Best checkpoint trong phiên chạy:
  /content/Day2-Lab/outputs/yolo26_runs/vehicle_annotation/weights/best.pt

| Chỉ số trên 10 ảnh eval | Giá trị |
|---|---:|
| Precision | 0.3056 |
| Recall | 0.3309 |
| mAP50 | 0.3254 |
| mAP50–95 | 0.2135 |

Các đầu ra:
- [10 ảnh eval có dự đoán](assets/eval_predictions/visuals/)
- [Log train](assets/yolo26_runs/vehicle_annotation/)
- [Ảnh và chỉ số đánh giá](assets/yolo26_runs/vehicle_eval/)

### 1. Lớp nào recall thấp nhất? Ảnh nào minh họa?

Lớp bus có recall thấp nhất: 0.

Trong [drive_053.jpg](assets/eval_predictions/visuals/drive_053.jpg),
xe buýt lớn ở phía dưới được dự đoán thành van và xuất hiện
các box mang lớp khác chồng lên vùng xe. Đây là ví dụ cần
kiểm tra về nhầm lớp và định vị.

### 2. Nếu mAP50 cao nhưng mAP50–95 thấp, cần kiểm điều gì?

Cần kiểm tra độ sát của box: dư nền, cắt mất thân xe, lệch cạnh
hoặc xử lý xe bị che/cắt khung không nhất quán.
Box có thể đạt IoU 0.50 nhưng không đạt các ngưỡng IoU cao hơn.

### 3. Lỗi class nào có thể liên quan guideline?

Nhầm bus và van có thể liên quan quy tắc phân biệt hai lớp.
Dữ kiện là xe buýt trong drive_053.jpg được dự đoán thành van,
recall bus bằng 0 và recall van khoảng 0.0833.

Cần rà lại định nghĩa lớp và nhãn train. Chưa thể kết luận
guideline là nguyên nhân duy nhất vì tập dữ liệu nhỏ và
số lượng giữa các lớp không cân bằng.

### 4. Vì sao không dùng 10 ảnh eval để train?

Giữ nguyên eval giúp đánh giá trên ảnh không được dùng
để cập nhật trọng số trong quá trình train.

Trong notebook, 10 ảnh này đồng thời làm validation:
theo dõi kết quả qua các epoch và chọn best.pt.
Vì đã dùng để chọn checkpoint, đây không phải test set độc lập.

## 6. Khó khăn và cách xử lý

| Bước/thời điểm | Lỗi quan sát | Cách đã thử | Kết quả/còn thiếu |
|---|---|---|---|
| Annotation | Xe che nhau, xe xa và xe bị cây che | Phóng to, xem phần còn nhìn thấy, ghi ca mơ hồ | Có mini guideline; một số trường hợp vẫn khó phân loại |
| Gold QC | F1 thấp, gold có khác biệt khi đối chiếu | Tạo ảnh đối chiếu, rà và chỉnh nhãn | F1 mới nhất 0.6790; giữ nguyên kết quả thực tế |
| Git | Cảnh báo LF/CRLF | Kiểm tra cảnh báo xuống dòng, tiếp tục commit và push | Push thành công |
| Train/eval | Các lớp có kết quả chênh lệch | Xem bảng chỉ số từng lớp và ảnh dự đoán | Bus và van còn yếu |

## 7. Tự đánh giá theo rubric

Đối chiếu [RUBRIC.md](../../RUBRIC.md):

- Đã tạo 50 file nhãn train và chạy checker đạt cấu trúc.
- Đã có mini guideline với ba ca mơ hồ.
- Đã chạy đánh giá annotation và lưu JSON.
- Annotation chưa đạt ngưỡng F1; đã nêu kết quả và thông báo
  cho phép tiếp tục của Lab Coach.
- Đã train và đánh giá YOLO trên Colab.
- Đã xuất 10 ảnh dự đoán, log train và ảnh chỉ số đánh giá.
- Kết quả model còn hạn chế ở bus và van.
- Chưa đánh giá trên test set độc lập.
- Danh sách 10 ảnh self-QC cần được điền đúng thực tế trước khi nộp.
## 8. Công cụ AI và tài liệu tham khảo

- ChatGPT: hỗ trợ giải thích kiến thức, hướng dẫn thao tác,
  đọc kết quả và góp ý cách diễn đạt guideline, báo cáo.
- Codex: hỗ trợ thao tác repository và tạo ảnh đối chiếu
  nhãn cá nhân với gold.
- Phần tự thực hiện: gán và chỉnh nhãn trong CVAT, chạy checker,
  chạy đánh giá annotation và notebook Colab.
- Phần tự kiểm tra: thứ tự ID lớp, cấu trúc 50 file nhãn,
  kết quả Terminal và ảnh dự đoán.
