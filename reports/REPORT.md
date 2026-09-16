# Báo cáo Ngày 4 - Keypoint & Pose

Họ tên: Nguyễn Quang Huy  
Nhóm: Cá nhân  
Ngày: 2026-09-16

> Báo cáo dùng số liệu được tái tạo trực tiếp từ nhãn hiện tại bằng các tool của repo. Những dữ liệu không được lưu lại (thời gian gán, kết quả lần chấm trước rework và ảnh dự đoán test) được ghi rõ là chưa có, không tự ước lượng.

## 1. Nhãn của tôi

| Chỉ số                                  |                                  Giá trị |
| --------------------------------------- | ---------------------------------------: |
| Số ảnh đã gán                           |                                       20 |
| Số skeleton                             |                                       28 |
| v=2 / v=1 / v=0                         |                            375 / 77 / 24 |
| Trung bình số khớp có `v > 0` mỗi người |                                    16.14 |
| Thời gian trung bình mỗi ảnh            | Không có log thời gian để tính chính xác |

Ba khớp có `%v=1` cao nhất, theo `reports/visibility_report.md`:

1. `left_ear`: 12/28, tương đương 43%.
2. `right_ear`: 11/28, tương đương 39%.
3. `left_knee`: 8/28, tương đương 29%.

Đây đúng là các vùng hay phải suy luận, nhưng theo hai nguyên nhân khác nhau. Tai thường vẫn ở trong khung nhưng bị tóc, mũ hoặc hướng quay của đầu che khuất, vì vậy khó xác nhận bằng quan sát trực tiếp và được đặt `v=1`. Đầu gối lại hay bị phương tiện, người khác hoặc tư thế ngồi che; vị trí giải phẫu phải được ước lượng theo trục hông–đùi–cẳng chân. Vì vậy `%v=1` cao phản ánh mức độ bị che, không tự động có nghĩa là điểm đã được đặt sai.

## 2. Chấm với gold

Repo chỉ còn kết quả của **một lần chấm trên nhãn hiện tại**; không có artifact của lần chạy trước để lập cột trước/sau rework. Vì vậy cột hiện tại dưới đây không được trình bày như một kết quả “đã rework sạch”.

| Chỉ số                                |      Trước rework | Hiện tại (chưa rework xong) |
| ------------------------------------- | ----------------: | --------------------------: |
| OKS trung bình                        | Không có artifact |                      0.8332 |
| OKS@0.50                              | Không có artifact |                      0.9310 |
| OKS@0.75                              | Không có artifact |                      0.8276 |
| Lỗi `dao_trai_phai`                   | Không có artifact |                           3 |
| Lỗi `nham_nguoi`                      | Không có artifact |                           2 |
| Lỗi `xoa_khop_bi_che`                 | Không có artifact |                           0 |
| Người gold / ghép được / thiếu / thừa | Không có artifact |             29 / 28 / 1 / 0 |

Về điểm số, kết quả hiện tại đạt ngưỡng `mean OKS >= 0.75` và `OKS@0.75 >= 0.70`. Tuy nhiên bài **chưa qua cổng cuối của rubric** vì vẫn còn ba lỗi `dao_trai_phai`. Ngoài ra còn hai lỗi `nham_nguoi`, một người bị thiếu, chín lỗi `truot_han` và 42 lỗi `lech_nhe`. Có 43 trường hợp cờ khác gold và 71 trường hợp gold để `v=0` trong khi bài có gán; theo README, hai nhóm này là thông tin chẩn đoán về guideline và không bị trừ OKS.

### Các sửa đổi cần thực hiện ở vòng rework kế tiếp

Do không có báo cáo lần chạy cũ nên không thể khẳng định chính xác những gì đã được sửa giữa hai lần chạy. Từ `outputs/eval_vs_gold.json`, các sửa đổi ưu tiên hiện còn lại là:

- `train_13`, gold người #1: bổ sung skeleton đang thiếu và gán đủ 17 keypoint. Model cũng phát hiện 3 người trong khi nhãn hiện tại chỉ có 2 người.
- `train_10`, người #1: kiểm tra lại toàn bộ cặp trái/phải; đặt lại `left_eye`, `right_eye`, `right_ear`, hai vai, hai khuỷu tay và hai cổ tay đang bị báo `truot_han`.
- `train_02`, người #1: kiểm tra và đổi lại các cặp trái/phải; skeleton hiện có OKS 0.5130 so với gold.
- `train_15`, người #2: kiểm tra và đổi lại các cặp trái/phải; skeleton hiện có OKS 0.5985 so với gold.
- `train_03`, skeleton của tôi #1 (ghép với gold người #2): chuyển `right_elbow` và `right_wrist` về đúng người.

### Lỗi đảo trái/phải

Tool phát hiện lỗi đảo trái/phải tại `train_02` người #1, `train_10` người #1 và `train_15` người #2. Chưa có ảnh visualize được lưu trong repo để kết luận khách quan từng ảnh là dễ hay khó. Nguyên nhân có khả năng là đã đọc trái/phải theo phía của ảnh thay vì theo cơ thể người; đây là giả thuyết cần kiểm tra lại bằng `tools/visualize_pose.py`, không phải kết luận chỉ từ điểm số. Riêng `check_pose_labels.py` còn cảnh báo hình học tại `train_16.txt:2` ở cặp vai và cặp hông; gold vẫn cho hai skeleton `train_16` OKS cao (0.9772 và 0.8966), nên cảnh báo heuristic này cần xem bằng hình trước khi sửa.

## 3. Kiểm chéo

Bạn cùng nhóm: Không có — bài thực hiện cá nhân.

| Khớp          | Tôi | Bạn cùng nhóm | Lệch | Nguyên nhân                                         |
| ------------- | --: | ------------: | ---: | --------------------------------------------------- |
| Không áp dụng |   — |             — |    — | Không có bộ nhãn thứ hai để chạy chế độ `--compare` |

Không thể kết luận khớp lệch `%v=1` nhiều nhất giữa hai người khi chưa có `reports/visibility_compare.md`. Đây là deliverable còn thiếu nếu bài bắt buộc phải có kiểm chéo. Rule cá nhân đã được ghi trong `GUIDELINE_MINI.md`: khớp không nhìn thấy nhưng vẫn còn trong khung phải được ước lượng từ các phần cơ thể liền kề và gán `v=1`; chỉ gán `v=0` khi khớp thật sự nằm ngoài mép ảnh.

## 4. Model

Nguồn số liệu: `outputs/eval_model.json`, tập test có 10 ảnh và 13 người.

| Chỉ số         | `yolo26n-pose` gốc | Sau fine-tune |   Chênh |
| -------------- | -----------------: | ------------: | ------: |
| pose_mAP50     |             0.8450 |        0.8450 | +0.0000 |
| pose_mAP50-95  |             0.6853 |        0.6908 | +0.0055 |
| pose_precision |             0.9734 |        0.9792 | +0.0058 |
| pose_recall    |             0.8462 |        0.8462 | +0.0000 |
| box_mAP50      |             0.9785 |        0.9600 | -0.0185 |
| box_mAP50-95   |             0.8119 |        0.8041 | -0.0078 |

Cấu hình chính: `yolo26n-pose.pt`, `imgsz=640`, `batch=8`, tối đa 80 epoch, AdamW do `optimizer=auto` chọn, seed `20260915`, GPU Tesla T4. Early stopping dừng ở epoch 39; checkpoint tốt nhất là epoch 9. Tập train chỉ có 20 ảnh/28 skeleton, còn tập test có 10 ảnh/13 skeleton, nên các thay đổi nhỏ phải được diễn giải thận trọng.

### Trả lời năm câu hỏi ở cuối notebook

1. **`pose_mAP50-95` thay đổi bao nhiêu?** Chỉ số tăng `0.0055`, từ 0.6853 lên 0.6908. Đồng thời pose precision tăng `0.0058`, còn pose recall và pose mAP50 không đổi. Điều này cho thấy fine-tune chỉ cải thiện nhẹ độ chính xác định vị ở các ngưỡng OKS chặt hơn; với tập test rất nhỏ, chưa đủ bằng chứng để nói model đã tổng quát tốt hơn. Việc box mAP giảm cũng cho thấy model đã đánh đổi một phần khả năng định vị người sau khi thích nghi với 20 ảnh train.

2. **`box_mAP` và `pose_mAP` chênh nhau bao nhiêu?** Sau fine-tune, `box_mAP50-95 - pose_mAP50-95 = 0.8041 - 0.6908 = 0.1133`; ở ngưỡng 50, chênh lệch là `0.9600 - 0.8450 = 0.1150`. Model tìm và bao người dễ hơn đặt chính xác 17 khớp, vì pose yêu cầu vừa phát hiện đúng người, vừa xác định nhiều điểm nhỏ và xử lý che khuất/trái-phải.

3. **Một ảnh test model đoán sai thuộc loại nào?** Ở `test_02`, log inference báo model phát hiện `2 persons`, trong khi `dataset/labels/test/test_02.txt` chỉ có 1 skeleton và ảnh gốc chỉ có một người thật ở phía phải. Dự đoán người thứ hai vì vậy là một false positive; nếu gọi theo bốn loại của slide 43 thì đây gần nhất với lỗi **trượt hẳn**, vì một skeleton đã được đặt vào vùng không có cơ thể người tương ứng. Kết luận này dựa trên sự thống nhất giữa ảnh gốc và nhãn test về số người, không suy ra chỉ từ mAP.

4. **Ảnh nào có OKS thấp nhất giữa model và nhãn của tôi? Ai đúng?** Cặp thấp nhất là `train_10`, OKS model–nhãn bằng 0.022. Gold cho chính skeleton nhãn này OKS 0.0171, báo đảo trái/phải và chín keypoint trượt hẳn, nên bằng chứng hiện có cho thấy nhãn của tôi sai nghiêm trọng; model có khả năng gần đúng hơn ở skeleton được ghép. Tuy nhiên model còn dự đoán 2 người trong khi nhãn có 1, còn gold cũng chỉ có 1 người, nên model có một detection dư; do đó không thể nói toàn bộ dự đoán của model ở ảnh này là đúng.

5. **Ảnh tôi gán tệ nhất có đồng thời là ảnh model đoán tệ nhất không?** Nếu tính cả độ bao phủ, `train_13` là ảnh gán tệ nhất vì thiếu gold người #1, được tính OKS 0.0. Nó không phải cặp model–nhãn có OKS thấp nhất: hai cặp được ghép ở `train_13` đạt 0.663 và 0.936, trong khi model phát hiện 3 người còn nhãn chỉ có 2; sự lệch số người lại phù hợp với kết quả gold là nhãn đang thiếu một người. Nếu chỉ xét các skeleton đã ghép, `train_10` là thấp nhất ở cả so với gold (0.0171) và so với model (0.022), cho thấy vấn đề chính nằm ở nhãn của ảnh này hơn là một ca model bất đồng ngẫu nhiên.

## 5. Một rule evidence đã dùng

Ở `train_01`, người thứ 2, hai đầu gối vẫn được suy ra là nằm trong khung dù phần chân bị che/cắt ở vùng dưới ảnh, nên tôi đặt điểm ước lượng và dùng `v=1`. Hai cổ chân đã nằm ngoài mép dưới nên tôi dùng `v=0` và không giữ tọa độ giả. Căn cứ là chuỗi hông–đùi vẫn cho phép ước lượng đầu gối, trong khi không còn vùng ảnh hợp lệ để đặt cổ chân. Rule được dùng là: còn trong khung nhưng bị che thì `v=1`; chỉ khi khớp thực sự ra ngoài khung mới dùng `v=0`.

## 6. Kết luận và trạng thái nộp

- Định dạng nhãn đạt: 20/20 file, 28 skeleton, đủ 17 keypoint mỗi skeleton.
- Chất lượng hiện tại đạt ngưỡng OKS về điểm số, nhưng chưa đạt yêu cầu cuối vì còn ba lỗi đảo trái/phải, hai lỗi nhầm người và thiếu một người.
- Fine-tune cải thiện rất nhẹ pose mAP50-95 (`+0.0055`) nhưng làm giảm box mAP50-95 (`-0.0078`); với dữ liệu nhỏ, chưa thể kết luận cải thiện tổng quát.
- Trước khi nộp cuối, cần rework các ảnh ưu tiên ở Mục 2, chạy lại evaluator cho đến khi không còn `dao_trai_phai`, rồi cập nhật bảng trước/sau rework.
- Phần kiểm chéo và nhận xét lỗi trên ảnh test vẫn thiếu bằng chứng tương ứng (`reports/review_partner.md`, `reports/visibility_compare.md` và ảnh prediction từ Colab).
