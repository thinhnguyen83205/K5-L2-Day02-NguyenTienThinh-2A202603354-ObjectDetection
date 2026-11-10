# Báo cáo cá nhân — Ngày 2 Object Detection

Đối chiếu cấu trúc và yêu cầu nộp trong [SUBMISSION.md](../../SUBMISSION.md).

## 1. Thông tin học viên

- Họ tên: Nguyễn Tiến Thịnh
- MSSV: 2A202603354
- Email: ai26.thinhnt@vinuni.edu.vn
- Lớp: chưa điền.
- Link repo bài làm: https://github.com/thinhnguyen83205/K5-L2-Day02-NguyenTienThinh-2A202603354-ObjectDetection.git
- Link Colab đã chạy: https://colab.research.google.com/drive/1qlDgiyrmshtkXHngAZKxPt9CqEug7dV4?usp=sharing

## 2. Schema và mini guideline

Dùng schema `car=0`, `truck=1`, `bus=2`, `van=3` và quy tắc box theo [mini guideline](../../GUIDELINE_MINI.md). Guideline hiện có định nghĩa lớp và quy tắc chung, nhưng ba dòng ghi ca mơ hồ cụ thể vẫn chưa điền; cần bổ sung các case đã tự quan sát và hỏi giảng viên nếu chưa phân biệt chắc `bus`/`van`.

## 3. Annotation và self-QC

- Số file nhãn train: 50.
- Kết quả checker: đạt cấu trúc 50/50 file (`tools/check_yolo_labels.py`, Python 3.13.15).
- 10 ảnh eval có prediction được tạo trong Colab: `drive_051.jpg`–`drive_060.jpg`, lưu tại [assets/eval_predictions/visuals](assets/eval_predictions/visuals). Việc tạo ảnh prediction không thay cho bước tự rà nhãn train bằng mắt; cần tự mở ít nhất 10 ảnh train và ghi kết quả.
- Chưa xác định lỗi annotation cụ thể bằng rà ảnh. Gold evaluator ghi nhận 399 FP và 220 FN; cần kiểm tra từng ảnh để xác định nguyên nhân, không suy luận nguyên nhân chỉ từ các con số này.
- Commit khóa nhãn ban đầu: `82466bb` (`add label`, 2026-11-10).

## 4. Gold QC và rework

| Chỉ số | Trước sửa | Sau sửa nếu có |
|---|---:|---:|
| TP | 433 | Chưa rework |
| FP | 399 | Chưa rework |
| FN | 220 | Chưa rework |
| Precision | 0.5204 | Chưa rework |
| Recall | 0.6631 | Chưa rework |
| F1 | 0.5832 | Chưa rework |
| mean_iou_of_matches | 0.7915 | Chưa rework |
| passed | Không | Chưa rework |

Kết quả lần đầu được lưu tại [outputs/annotation_evaluation.json](../../outputs/annotation_evaluation.json); thư mục `outputs/` bị `.gitignore` bỏ qua mặc định. Chưa rework. F1 chưa đạt 0.80 nên nhãn chưa qua cổng QC để train theo hướng dẫn; kết quả train dưới đây được chạy trên bộ nhãn hiện tại, không thay đổi kết luận QC.

## 5. Kết quả YOLO

- Runtime: Google Colab, Python 3.13.16, Tesla T4 (CUDA:0, 14,913 MiB), Ultralytics 8.4.208, Torch 2.14.1+cu130.
- Huấn luyện `yolo26n.pt` trong 50 epochs, `imgsz=640`, `batch=-1` (auto), `seed=20260909`; không thay đổi các tham số so với notebook.
- Best checkpoint trong Colab: `/content/Day2-Lab/outputs/yolo26_runs/vehicle_annotation/weights/best.pt`. Bản nằm trong thư mục assets local không đưa vào Git theo quy định loại trừ `.pt`.

| Chỉ số trên 10 ảnh eval | Giá trị |
|---|---:|
| Precision | 0.4537 |
| Recall | 0.4796 |
| mAP50 | 0.3691 |
| mAP50–95 | 0.2477 |

Artifacts: [10 ảnh prediction](assets/eval_predictions/visuals), [log và biểu đồ train](assets/yolo26_runs/vehicle_annotation), [kết quả đánh giá trên eval](assets/yolo26_runs/vehicle_eval). Notebook báo cáo 113 instances trên 10 ảnh. Theo lớp: `car` recall 0.883 (94 instances), `truck` 0.400 (5), `bus` 0.500 (2), `van` 0.135 (12); `van` có recall thấp nhất. Ví dụ `drive_058.jpg`: nhãn eval có 6 `van`, trong khi output prediction liệt kê 30 `car`, 1 `truck`, 5 `bus` và không có `van`; xem [ảnh prediction drive_058.jpg](assets/eval_predictions/visuals/drive_058.jpg). Đây là minh họa cho recall `van` thấp, chưa đủ để kết luận nguyên nhân của lỗi.

Trả lời bốn câu hỏi cuối notebook:

1. `van` có recall thấp nhất (0.135); `drive_058.jpg` minh họa trường hợp có 6 van trong nhãn eval nhưng không có van trong danh sách prediction.
2. Nếu mAP50 cao nhưng mAP50–95 thấp, cần kiểm độ khít của box ở các IoU threshold cao: box có quá rộng/hẹp, lệch cạnh hoặc bao gồm nền hay không.
3. Có thể kiểm tra nhầm `van`/`bus`: `drive_058.jpg` có van trong nhãn nhưng output có bus mà không có van. Đây mới là dấu hiệu để rà ảnh, chưa đủ để kết luận guideline gây lỗi. Tập eval cũng chỉ có 12 van, 2 bus và 5 truck so với 94 car nên số liệu các lớp ít cần được diễn giải thận trọng.
4. Không dùng eval để train để tránh rò rỉ dữ liệu và giữ split đánh giá. Notebook dùng chính 10 ảnh eval làm validation trong lúc train/chọn `best.pt`, vì vậy đây không phải test set độc lập.

## 6. Khó khăn và cách xử lý

| Bước/thời điểm | Lỗi quan sát | Cách đã thử | Kết quả/còn thiếu |
|---|---|---|---|
| Gold QC local | Windows console mặc định không xuất được một số ký tự tiếng Việt; evaluator đã ghi JSON nhưng báo lỗi khi in kết quả. | Chạy lại với `PYTHONIOENCODING=utf-8`. | Checker chạy đạt; evaluator xác nhận F1 0.5832, chưa đạt ngưỡng. |
| Hoàn thiện bài | Colab page cần đăng nhập Google để chỉnh/lưu; notebook hiện có output và assets prediction/train. | Đọc output đã có, không chạy lại notebook. | Kết quả được ghi vào báo cáo; cần tự rà ảnh, bổ sung ba ca guideline và rework nhãn. |

## 7. Tự đánh giá theo rubric

Đã có 50 file nhãn qua checker, kết quả gold evaluation, Colab metrics và assets. Gold QC chưa đạt F1 yêu cầu; chưa rework, chưa rà đủ 10 ảnh train bằng mắt và chưa hoàn thiện ba ca mơ hồ trong guideline. Cần làm các phần này trước khi kết luận bài hoàn tất. Đối chiếu [RUBRIC.md](../../RUBRIC.md).

## 8. Công cụ AI và tài liệu tham khảo

CodeX