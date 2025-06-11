- 4-bit NormalFloat Quantization (NF4) là kỹ thuật nén mô hình bằng cách chuyển trọng số từ dạng
  Float32 sang dữ liệu 4-bit, giúp giảm kích thước mô hình mà vẫn giữ được độ chính xác cao.
  ✅ Tóm tắt:
  ● NF4 dùng 16 giá trị đại diện (bins) phân bố theo phân phối chuẩn đối xứng quanh 0.
  ● Trọng số mô hình được chuẩn hóa về [-1, 1] bằng cách chia cho giá trị tuyệt đối lớn nhất
  (absolute max rescaling).
  ● Sau đó, mỗi giá trị được gán vào bin gần nhất trong 16 giá trị NF4.
  ● Thay vì lưu số thực, ta chỉ lưu chỉ số bin (0 → 15), tương ứng với 4 bit mỗi trọng số.
  🔁 Ví dụ:
  ● Giá trị sau chuẩn hóa: 0.686
  ● Gần với bin 0.7229 → chỉ số bin là 15
  ● Vậy thay vì lưu 0.686 (FP32), ta lưu 15 (int4).