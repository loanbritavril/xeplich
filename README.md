# Xếp Lịch – tự động xếp ca làm việc

Xếp Lịch là app web chạy trong một file `index.html`, tự động xếp ca làm việc theo tuần cho nhân viên từ file Excel/CSV. Lịch tôn trọng ngày nghỉ, ca không làm được, giới hạn số ca và cố gắng chia đều ca giữa mọi người.

Demo (dự kiến): https://loanbritavril.github.io/xeplich/

## Cách dùng

1. Nạp file **Nhân viên** và file **Nhu cầu ca** (`.csv` / `.xlsx`), hoặc bấm **Dùng dữ liệu mẫu**. Có thể tải file mẫu bằng hai nút "Tải file mẫu".
2. Chỉnh các tuỳ chọn ở mục **Quy tắc** (nếu cần).
3. Bấm **Xếp lịch tự động**, xem lịch và thống kê từng nhân viên, rồi **Xuất CSV**, **Xuất Excel** hoặc **In**.

## Định dạng dữ liệu đầu vào

Dòng đầu là tiêu đề. App tự nhận dấu phân cách `,` `;` hoặc Tab, và không phân biệt hoa/thường, có dấu/không dấu của tên cột.

### File nhân viên

Chỉ cột `Tên` là bắt buộc. Cột trống nghĩa là không giới hạn (tối đa) hoặc 0 (tối thiểu).

| Tên | Số ca tối đa/tuần | Số ca tối thiểu | Ngày nghỉ | Ca không làm được |
|---|---|---|---|---|
| Lan | 5 | 3 | CN | |
| Minh | 6 | 4 | | Tối |
| Hùng | 5 | 3 | T7;CN | |
| Mai | 4 | 2 | T2 | |
| Tuấn | 6 | 4 | | |
| Hoa | 5 | 3 | T4 | Sáng |
| Nam | 5 | 3 | | |
| Linh | 4 | 2 | T6;T7 | |

- `Ngày nghỉ`: nhiều ngày ngăn bằng `;` `,` `/` hoặc `|`. Nhận `T2`...`T7`, `CN` (cũng nhận "Thứ hai", "Chủ nhật", Mon...Sun).
- `Ca không làm được`: tên ca, nhiều ca ngăn bằng `;` `,` `/` hoặc `|`.

### File nhu cầu ca

Ba cột bắt buộc: `Ngày`, `Ca`, `Số người`. Thứ tự xuất hiện đầu tiên của các ca trong file được coi là thứ tự trong ngày (ca đầu = ca sớm nhất, ca cuối = muộn nhất).

Dữ liệu mẫu gồm 21 dòng (7 ngày x 3 ca), trích:

| Ngày | Ca | Số người |
|---|---|---|
| T2 | Sáng | 2 |
| T2 | Chiều | 2 |
| T2 | Tối | 1 |
| ... | ... | ... |
| T6 | Tối | 2 |
| T7 | Tối | 2 |
| CN | Tối | 2 |

Ca Sáng và Chiều cần 2 người mọi ngày; ca Tối cần 1 người từ T2 đến T5 và 2 người từ T6 đến CN.

## Quy tắc xếp ca

Luôn áp dụng:
- Không xếp vào ngày nghỉ hoặc ca không làm được của nhân viên.
- Không vượt `Số ca tối đa/tuần`.
- Một người không bị xếp trùng một ca hai lần.
- Ưu tiên đưa nhân viên chưa đạt `Số ca tối thiểu` lên trước, và chia đều số ca giữa mọi người.

Tuỳ chỉnh trên giao diện:

| Tuỳ chọn | Mặc định | Ý nghĩa |
|---|---|---|
| Ca / ngày / người | 1 (1-4) | Số ca tối đa một người làm trong một ngày |
| Ngày làm liên tiếp | 6 (1-7) | Số ngày làm liên tiếp tối đa |
| Không xếp ca cuối ngày rồi ca đầu ngày hôm sau | Bật | Tránh ca cuối hôm trước nối ca đầu hôm sau |
| Số lần thử | 400 (20-5000) | Số lần xếp ngẫu nhiên; nhiều hơn thì có cơ hội ra lịch tốt hơn |

Nếu không đủ người, ô ca bị thiếu được tô đỏ kèm "thiếu N", và có cảnh báo tổng số vị trí còn thiếu.

## Chạy cục bộ

Không cần cài đặt hay build: tải `index.html` về và mở bằng trình duyệt (double-click).

## Deploy lên GitHub Pages

1. Tạo repo `xeplich` trên GitHub và đưa `index.html` (cùng `README.md`) lên nhánh `main`.
2. Vào **Settings > Pages**, chọn **Deploy from a branch**, nhánh `main`, thư mục `/ (root)`, rồi Save.
3. Sau ít phút, app chạy tại `https://<tài-khoản>.github.io/xeplich/`.

## Giới hạn hiện tại

- Đọc/xuất file `.xlsx` cần Internet để tải thư viện SheetJS từ cdnjs. Khi offline vẫn dùng được CSV.
- Chỉ xếp theo một tuần cố định T2 đến CN.
- Lời giải tìm bằng cách thử nhiều lần ngẫu nhiên (dừng sau tối đa khoảng 4 giây), chọn kết quả tốt nhất nên không đảm bảo tối ưu tuyệt đối; bấm xếp lại có thể ra kết quả khác.
- Với file `.xlsx`, chỉ đọc sheet đầu tiên.

## Hướng phát triển

- Xếp nhiều tuần liên tiếp, có cân bằng ca giữa các tuần.
- Cho phép kéo-thả chỉnh tay kết quả sau khi xếp.
- Bổ sung thư viện Excel nội bộ để dùng xlsx khi offline.
- Thêm ràng buộc như kỹ năng/vị trí, nguyện vọng ưu tiên, số giờ thay vì số ca.
