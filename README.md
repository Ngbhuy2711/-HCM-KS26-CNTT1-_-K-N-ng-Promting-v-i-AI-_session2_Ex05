Phần 1: Phân tích cơ chế hoạt động của In-Context Learning (ICL)
1. Nguyên nhân thất bại của phương pháp Zero-shot
Trong trường hợp đánh giá: "Máy thiết kế đẹp mê ly... khỏi cần bật lò sưởi vì nhiệt độ CPU lên hẳn 98 độ C!", câu lệnh Zero-shot đã thất bại vì mô hình chỉ thực hiện khớp từ khóa bề mặt (Surface Lexical Matching).

Mô hình quét thấy các từ tích cực độc lập như "đẹp mê ly", "đầm tay" và cấu trúc "khỏi cần..." (thường đi với ý nghĩa tiết kiệm, tiện lợi trong ngữ cảnh thông thường).

Thiếu đi sự hiểu biết sâu về ngữ cảnh miền công nghệ (Domain Context), AI xem 98 độ C là một con số kỹ thuật thuần túy thay vì nhận ra đây là ngưỡng nhiệt độ nguy hiểm dẫn đến hiện tượng bóp hiệu năng (thermal throttling) hoặc hỏng linh kiện.

2. Cơ chế hoạt động của In-Context Learning (Học trong ngữ cảnh)
In-Context Learning (ICL) là một năng lực nổi bật (Emergent Ability) của các Mô hình Ngôn ngữ Lớn (LLM). Cơ chế này hoạt động dựa trên các nguyên lý cốt lõi sau mà không cần thực hiện Fine-tuning (cập nhật trọng số/gradient descent):

Mô hình hóa chuỗi qua cơ chế Self-Attention (Sự chú ý nội tại): Khi đưa các ví dụ mẫu (Few-shot examples) vào prompt, toàn bộ văn bản từ ví dụ đến input thực tế được xử lý chung trong một cửa sổ ngữ cảnh (Context Window). Các lớp Self-Attention lập tức thiết lập mối quan hệ ngữ nghĩa phức tạp giữa các từ ngữ có vẻ mâu thuẫn (ví dụ: "đẹp mê ly" đi kèm với "98 độ C").

Kích hoạt học tập trong không gian ẩn (Implicit In-Context Meta-Learning): Quá trình huấn luyện sơ bộ (Pre-training) đã biến LLM thành một "meta-learner" (kẻ học cách học). Khi đọc các ví dụ mẫu, mô hình nhận diện ra mẫu hình (pattern) và quy luật ẩn của nhiệm vụ (ví dụ: phát hiện câu khen có điều kiện mỉa mai = Tiêu cực). Nó tự động điều chỉnh các trạng thái kích hoạt (activations) trong các tầng ẩn mà không làm thay đổi trọng số mạng (
W).

Định hướng không gian xác suất tiếp theo: Các ví dụ mẫu đóng vai trò là "mồi" (priming) định hướng không gian sinh token tiếp theo. Mô hình hiểu rằng: “À, đây là bài toán phân loại sắc thái sâu, không chỉ đếm từ khóa đơn lẻ”, từ đó điều chỉnh xác suất xuất hiện của nhãn TIÊU CỰC thay vì TÍCH CỰC.ty) của các Mô hình Ngôn ngữ Lớn (LLM). Cơ chế này hoạt động dựa trên các nguyên lý cốt lõi sau mà không cần thực hiện Fine-tuning (cập nhật trọng số/gradient descent):Mô hình hóa chuỗi qua cơ chế Self-Attention (Sự chú ý nội tại): Khi đưa các ví dụ mẫu (Few-shot examples) vào prompt, toàn bộ văn bản từ ví dụ đến input thực tế được xử lý chung trong một cửa sổ ngữ cảnh (Context Window). Các lớp Self-Attention lập tức thiết lập mối quan hệ ngữ nghĩa phức tạp giữa các từ ngữ có vẻ mâu thuẫn (ví dụ: "đẹp mê ly" đi kèm với "98 độ C").Kích hoạt học tập trong không gian ẩn (Implicit In-Context Meta-Learning): Quá trình huấn luyện sơ bộ (Pre-training) đã biến LLM thành một "meta-learner" (kẻ học cách học). Khi đọc các ví dụ mẫu, mô hình nhận diện ra mẫu hình (pattern) và quy luật ẩn của nhiệm vụ (ví dụ: phát hiện câu khen có điều kiện mỉa mai = Tiêu cực). Nó tự động điều chỉnh các trạng thái kích hoạt (activations) trong các tầng ẩn mà không làm thay đổi trọng số mạng ($W$).Định hướng không gian xác suất tiếp theo: Các ví dụ mẫu đóng vai trò là "mồi" (priming) định hướng không gian sinh token tiếp theo. Mô hình hiểu rằng: “À, đây là bài toán phân loại sắc thái sâu, không chỉ đếm từ khóa đơn lẻ”, từ đó điều chỉnh xác suất xuất hiện của nhãn TIÊU CỰC thay vì TÍCH CỰC.

# RULES & GUIDELINES
1. **Phát hiện mỉa mai (Sarcasm Detection):** Cẩn trọng với các cấu trúc "khen bề ngoài - chê bản chất" (ví dụ: khen thiết kế đẹp nhưng tố cáo lỗi nghiêm trọng về nhiệt độ, pin, hiệu năng).
2. **Quy tắc phân loại:** 
   - Nếu có lỗi phần cứng nghiêm trọng (quá nhiệt, sập nguồn, pin hỏng) dù kèm lời khen ngoại hình -> Gói gọn là **TIÊU CỰC**.
   - Nếu phàn nàn về vận chuyển chậm chạp bằng giọng điệu châm biếm -> Gói gọn là **TIÊU CỰC**.
3. **Định dạng Output:** Chỉ xuất ra một khối JSON duy nhất, không kèm văn bản giải thích thừa.

# FEW-SHOT EXAMPLES (VÍ DỤ MẪU)

## Ví dụ 1 (Mỉa mai về nhiệt độ / quá nhiệt):
- Input: "Máy thiết kế đẹp mê ly, cầm rất đầm tay, mùa đông ôm máy chơi game thì khỏi cần bật lò sưởi vì nhiệt độ CPU lên hẳn 98 độ C!"
- Output: {
  "sentiment": "TIÊU CỰC",
  "category": "Nhiệt độ phần cứng",
  "reason": "Sử dụng lời khen ngoại hình để che đậy vấn đề quá nhiệt nghiêm trọng (98 độ C)."
}

## Ví dụ 2 (Mỉa mai về pin / thời lượng sử dụng):
- Input: "Pin trâu khủng khiếp luôn shop ạ, cắm sạc liên tục 3 lần một ngày như sam sung đời cổ, đúng là không rời nổi cục sạc dự phòng!"
- Output: {
  "sentiment": "TIÊU CỰC",
  "category": "Pin",
  "reason": "Dùng từ 'pin trâu' mang tính mỉa mai để phản ánh việc pin yếu, phải sạc nhiều lần."
}

## Ví dụ 3 (Mỉa mai về thời gian giao hàng):
- Input: "Shop giao hàng nhanh khủng khiếp, đặt từ lúc hoa phượng nở năm ngoái mà đến mùa khai giảng năm nay mới nhận được hàng, tuyệt vời!"
- Output: {
  "sentiment": "TIÊU CỰC",
  "category": "Vận chuyển",
  "reason": "Châm biếm thời gian giao hàng cực kỳ chậm trễ bằng các từ ngữ phóng đại thời gian."
}

# TARGET INPUT (DỮ LIỆU CẦN PHÂN TÍCH)
- Input: "Laptop chạy êm ru, mượt mà lắm, mỗi tội mở 3 tab Chrome là quạt tản nhiệt kêu to như máy bay phản lực chuẩn bị cất cánh, pin thì tụt nhanh hơn tụt quần. Rất đáng tiền!"
- Output:{
  "sentiment": "TIÊU CỰC",
  "category": "Hiệu năng & Pin",
  "reason": "Dùng lời khen mỉa mai ('chạy êm ru', 'rất đáng tiền') nhưng thực tế phản ánh tình trạng quạt tản nhiệt quá ồn khi tải nhẹ và pin tụt rất nhanh."
}
