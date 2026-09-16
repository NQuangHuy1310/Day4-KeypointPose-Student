# Mini guideline - nhóm: cá nhân | người gán: Nguyễn Quang Huy | ngày: 2026-09-16

> Điền file này **trong lúc** gán nhãn, không phải sau khi xong. Mỗi lần bạn dừng lại
> hơn 10 giây để phân vân, đó là một dòng phải ghi vào đây.

## 1. Luật bắt buộc (đã thống nhất cả lớp - không sửa)

- Bộ 17 điểm COCO, đúng tên, đúng thứ tự. Lấy từ file `.SVG` chung.
- Mọi người trong ảnh đều có **đủ 17 điểm**. Điểm không dùng được thì gắn cờ, không xoá.
- Trái/phải tính theo **cơ thể người**, không theo bức ảnh.
- Bị che, còn trong khung -> `v = 1`, **vẫn đặt chấm** ở vị trí ước lượng.
- Ra ngoài mép ảnh -> `v = 0`, **không** đặt chấm.
- Không dùng `Hidden` (`h`) - nó không được lưu vào file.

## 2. Luật của nhóm bạn (phải điền)

| Tình huống                                        | Luật nhóm bạn chọn                                                                                                                            | Vì sao                                                                                                                                                                                                        |
| ------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Hông của người mặc quần áo dài                    | Ước lượng tâm hông theo trục vai–thân–đùi. Dùng `v = 2` khi đường bao thân và đùi vẫn đủ để định vị; dùng `v = 1` khi bị vật hoặc tư thế che. | Dữ liệu có `left_hip`: 25 điểm `v=2`, 3 điểm `v=1`; `right_hip`: 26 điểm `v=2`, 2 điểm `v=1`. `train_08`, người 1 có `left_hip` là `v=1`. ![Ảnh mẫu train_08](dataset/images/train/train_08.jpg)              |
| Tai bị tóc hoặc mũ bảo hiểm che một phần          | Nếu không nhìn rõ tâm tai nhưng đầu vẫn trong ảnh, đặt điểm ước lượng và dùng `v = 1`; chỉ dùng `v = 2` khi vị trí tai nhìn đủ rõ.            | `left_ear` có 12/28 điểm `v=1`, `right_ear` có 11/28 điểm `v=1`. `train_07`, người 1 có cả hai tai là `v=1`. ![Ảnh mẫu train_07](dataset/images/train/train_07.jpg)                                           |
| Người bị cắt ở mép ảnh (chỉ thấy từ hông trở lên) | Khớp đã ra ngoài ảnh dùng `v = 0` và không giữ tọa độ. Khớp còn trong ảnh nhưng bị che dùng `v = 1` và vẫn đặt điểm.                          | Cả 17 điểm `v=0` hiện tại đều là đầu gối hoặc cổ chân. Trong `train_01`, hai người đều có hai cổ chân `v=0`. ![Ảnh mẫu train_01](dataset/images/train/train_01.jpg)                                           |
| Cổ tay nằm sau tay lái / sau thân mình            | Theo hướng cẳng tay để ước lượng tâm cổ tay và dùng `v = 1`; không chuyển thành `v = 0` nếu cổ tay vẫn nằm trong ảnh.                         | `train_06`, người 1 có `right_wrist` là `v=1`; dữ liệu cũng có cổ tay `v=1` tại `train_09`, `train_11`, `train_14`, `train_19` và `train_20`. ![Ảnh mẫu train_06](dataset/images/train/train_06.jpg)          |
| Hai người chồng lên nhau                          | Hoàn thành đủ 17 điểm của một người rồi mới sang người tiếp theo. Khớp bị người kia che vẫn thuộc skeleton ban đầu và dùng `v = 1`.           | `train_03` có hai skeleton: người 1 có `left_elbow` là `v=1`, người 2 có `right_elbow` là `v=1`. ![Ảnh mẫu train_03](dataset/images/train/train_03.jpg)                                                       |
| Người nhỏ đến mức nào thì không gán nữa           | Vẫn gán khi phóng to còn phân biệt được người và có căn cứ đặt đủ 17 điểm; không tự bỏ người chỉ vì kích thước nhỏ.                           | Skeleton nhỏ nhất trong dữ liệu hiện tại là `train_19`, người 1, với box khoảng 60×155 px. Đây là ca tham chiếu nhỏ nhất, không phải ngưỡng tuyệt đối. ![Ảnh mẫu train_19](dataset/images/train/train_19.jpg) |

Với mỗi luật, chèn **một ảnh mẫu** (screenshot từ CVAT) thay vì chỉ viết một câu.
Slide 12 nói rõ: khớp không có bề mặt nhìn thấy được thì phải có ảnh mẫu, không phải
một câu văn chung chung.

## 3. Ba ca mơ hồ đã gặp (bắt buộc, ghi ít nhất 3)

### Ca 1 - ảnh `train_01`, người thứ `2`, khớp `left_knee`, `right_knee`, `left_ankle`, `right_ankle`

- Mơ hồ ở chỗ nào: Phần chân bị cắt bởi mép dưới ảnh; cần phân biệt đầu gối còn trong ảnh nhưng bị che với cổ chân đã ra ngoài ảnh.
- Bạn quyết thế nào: Hai đầu gối được đặt tại vị trí ước lượng và dùng `v = 1`; hai cổ chân dùng `v = 0` và không giữ tọa độ.
- Vì sao: `train_01.txt`, dòng 2 ghi `v=1` cho hai đầu gối và `v=0` cho hai cổ chân.
- Nếu người khác quyết ngược lại thì model học sai cái gì: Nếu đổi đầu gối thành `v=0`, model mất supervision cho khớp còn trong ảnh; nếu đặt cổ chân `v=1` trên mép ảnh, model sẽ học vị trí cổ chân giả.

### Ca 2 - ảnh `train_07`, người thứ `1`, khớp `left_ear`, `right_ear`

- Mơ hồ ở chỗ nào: Hai tai nằm trong vùng đầu nhưng bị mũ bảo hiểm che nên không nhìn trực tiếp được tâm tai.
- Bạn quyết thế nào: Đặt hai điểm tai theo cấu trúc đầu và dùng `v = 1` cho cả `left_ear` và `right_ear`.
- Vì sao: Đầu vẫn nằm trong ảnh và `train_07.txt` ghi cả hai tai là `v=1`; đây là bị che trong khung, không phải ra ngoài ảnh.
- Nếu người khác quyết ngược lại thì model học sai cái gì: Dùng `v=0` sẽ bỏ supervision cho tai; dùng `v=2` sẽ coi tọa độ ước lượng là một điểm nhìn thấy rõ.

### Ca 3 - ảnh `train_08`, người thứ `1`, khớp `left_hip`, `left_knee`, `left_ankle`

- Mơ hồ ở chỗ nào: Nửa chân trái bị thân xe máy che; các khớp không nhìn trực tiếp được nhưng vẫn nằm trong ảnh.
- Bạn quyết thế nào: Ước lượng chuỗi `left_hip`–`left_knee`–`left_ankle` theo tư thế ngồi và đặt cả ba khớp là `v = 1`.
- Vì sao: `train_08.txt` ghi đúng ba khớp này là `v=1`, trong khi các khớp nhìn rõ được giữ `v=2`; người này không có khớp `v=0`.
- Nếu người khác quyết ngược lại thì model học sai cái gì: Dùng `v=0` sẽ dạy model bỏ chân khi bị xe che; dùng `v=2` sẽ khiến model quá tin vào tọa độ chưa quan sát trực tiếp.

## 4. Sau khi so visibility report với bạn cùng nhóm

- Khớp lệch `%v=1` nhiều nhất: Không áp dụng — bài thực hiện cá nhân.
- Nguyên nhân là **guideline chưa rõ** hay **một trong hai bên gán sai**: Không áp dụng — không có bài của thành viên khác để so sánh.
- Luật mới bổ sung vào mục 2 sau khi thống nhất: Không áp dụng — giữ các luật cá nhân rút ra từ visibility report và dữ liệu keypoint hiện tại.
