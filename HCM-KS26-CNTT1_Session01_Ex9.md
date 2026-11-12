# Bài 9: Kỹ thuật RAG Grounding – Dập tắt ảo giác hợp đồng thu mua

## Phần 1: Phân tích nguyên nhân ảo giác

Khi nhân viên hỏi ChatGPT mà **không cung cấp hợp đồng**, AI không có căn cứ để biết quy định riêng của MartX. Vì vậy, AI có thể dựa vào những mẫu thông tin đã học để **tự suy đoán**, tạo ra câu trả lời nghe hợp lý nhưng không đúng. Hiện tượng này gọi là **ảo giác AI (Hallucination)**.

Trong tình huống này:

- **Câu trả lời sai của AI:** Phạt 20% tổng giá trị đơn hàng và đình chỉ nhập 30 ngày.
- **Nội dung hợp đồng thực tế:** Phạt 5% giá trị lô bị dập nát và yêu cầu đổi hàng trong 24 giờ.
- **Hậu quả:** Nhân viên áp dụng mức phạt không có trong hợp đồng, gây tranh chấp với nhà cung cấp.

**Sự khác nhau:**

| Hỏi AI không kèm tài liệu | Hỏi AI có Grounding bằng tài liệu |
|---|---|
| AI có thể tự đoán quy định của công ty. | AI phải dựa trên nội dung hợp đồng được cung cấp. |
| Dễ đưa ra mức phạt không có căn cứ. | Có thể trích đúng mức phạt ghi trong hợp đồng. |
| Khó kiểm tra nguồn trả lời. | Có thể đối chiếu lại đoạn hợp đồng gốc. |

**Lưu ý:** Việc dán hợp đồng vào prompt là cách thực hành *grounding* bằng tài liệu nguồn. Trong hệ thống RAG đầy đủ, tài liệu thường được truy xuất tự động trước khi AI trả lời.

## Phần 2: Prompt RAG Grounding hoàn chỉnh

### 1. Văn bản hợp đồng chuẩn

Bạn là trợ lý AI hỗ trợ bộ phận thu mua MartX. Chỉ được sử dụng thông tin trong đoạn hợp đồng sau:

> **Hợp đồng thu mua MartX:** "Phạt 5% giá trị lô bị dập nát và yêu cầu đổi hàng trong 24h".

### 2. Câu hỏi nghiệp vụ

Theo hợp đồng MartX, nếu nhà cung cấp giao rau bị dập nát trên 10% thì mức phạt là bao nhiêu và nhà cung cấp phải xử lý thế nào?

### 3. Negative Constraint – Khóa an toàn

- Chỉ trả lời dựa trên văn bản hợp đồng đã cung cấp.
- **Nếu thông tin không có trong văn bản, tuyệt đối trả lời: "Không có dữ liệu trong tài liệu", không tự suy đoán.**
- Không tự thêm tỷ lệ phạt, điều kiện áp dụng, thời hạn đình chỉ hay quy định khác.
- Nếu câu hỏi gồm nhiều ý, chỉ trả lời các ý có căn cứ và chỉ rõ ý nào chưa có dữ liệu.
- Trả lời ngắn gọn, dễ hiểu, nêu đúng căn cứ từ hợp đồng.

## Phần 3: Ví dụ kết quả AI cần trả lời

Theo hợp đồng MartX:

- **Mức phạt:** 5% giá trị lô bị dập nát.
- **Biện pháp xử lý:** Nhà cung cấp phải đổi hàng trong 24 giờ.
- **Điều kiện "dập nát trên 10%":** Không có dữ liệu trong tài liệu về ngưỡng này.

**Kết luận:** Muốn AI trả lời đúng quy định nội bộ, cần cung cấp tài liệu gốc và đặt ràng buộc không được đoán khi thiếu dữ liệu. Nhờ vậy, nhân viên có thể hạn chế áp dụng sai điều khoản hợp đồng.
