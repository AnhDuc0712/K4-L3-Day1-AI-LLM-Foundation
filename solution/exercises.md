# K4 — Ngày 1: Bài Tập & Phản Ánh
## Khám Phá LLM API | Phiếu Thực Hành

**Thời lượng:** 9h00–13h00
**Cách làm:** Trả lời từng câu ngay sau khi hoàn thành block tương ứng —
đừng để dồn hết về cuối buổi. Thay dòng `*Câu trả lời của bạn*` bằng câu
trả lời thật (chấm tự động sẽ đếm số câu đã trả lời).

---

## Block 1 — API Cơ Bản (trả lời sau Checkpoint 1)

### Câu 1.1 — Độ nhạy của temperature
Gọi `call_openai` với temperature 0.0, 0.5, 1.0 và 1.5 dùng prompt
**"Hãy kể cho tôi một sự thật thú vị về Việt Nam."**

**Bạn nhận thấy quy luật gì qua bốn phản hồi?** (2–3 câu)
> theo em ở temperature 0.0 thì nó ngắn gọn có cấu trúc nó sẽ máy móc nhanh gọn và đi thẳng vào đáp án ,ở 0.5 thì tương tự nhưng có thểm 1 chút so sánh ,1.0 thêm lan man và có từ ngữ hoa mỹ hơn,1.5 liệt kê nhiều option hơn .

### Câu 1.2 — Chọn temperature cho sản phẩm
**Bạn sẽ đặt temperature bao nhiêu cho chatbot hỗ trợ khách hàng, và tại sao?**
> Theo em ở temperature khoảng 0.2-0.3 là đủ để đảm bảo tính xác thực ,tính nhất quán và tránh lạc đề.

### Câu 1.3 — Đánh đổi chi phí
Kịch bản: 10.000 người dùng hoạt động mỗi ngày, mỗi người gọi API 3 lần,
mỗi lần trung bình ~350 token đầu ra.

**Ước tính GPT-4o đắt hơn GPT-4o-mini bao nhiêu lần cho workload này? Nêu một
trường hợp GPT-4o xứng đáng với chi phí và một trường hợp nên dùng mini:**
> GPT-4o đắt hơn GPT-4o-mini sẽ đắt hơn khoảng 16.67% , ở gpt-4o-mini ta chỉ nên làm các tác vụ như phân loại ý định, tóm tắt văn bản ngắn chatbox đơn giản,còn ở gpt-4o có thể tư vấn ý tế hỗ trợ kinh doanh

---

## Block 2 — System Prompt & Token (trả lời sau Checkpoint 2)

### Câu 2.1 — Sức mạnh của persona
Gọi `chat_with_system_prompt` hai lần với cùng câu hỏi
**"Giải thích blockchain là gì?"** nhưng hai system prompt khác nhau:
- "Bạn là giáo viên tiểu học, giải thích thật đơn giản cho trẻ 8 tuổi."
- "Bạn là chuyên gia tài chính, trả lời chuyên sâu bằng thuật ngữ kỹ thuật."

**Hai phản hồi khác nhau như thế nào (độ dài, từ vựng, ví dụ)? System prompt
ảnh hưởng đến hành vi model ra sao?** (3–4 câu)
> khi dùng với role là giáo viên tiểu học thì câu trả lời sẽ có xu hướng đơn giản dễ hiểu phù hợp với trẻ 8 tuổi,còn trường hợp role là chuyên gia tài chính thì sẽ có các keyword học thuật hơn 

### Câu 2.2 — tiktoken vs đếm từ
Chọn một đoạn văn tiếng Việt ~100 từ. So sánh số token theo `count_tokens`
(tiktoken) với ước lượng `số từ / 0.75` mà Part 1 đã dùng.

**Hai con số chênh nhau bao nhiêu phần trăm? Vì sao tiếng Việt thường tốn
nhiều token hơn tiếng Anh cùng độ dài?**
> với khoảng 100 từ tiếng việt thì count_tokens trả về khoảng 178 token nhiều hơn ước lượng ra chênh khoảng 27%,nguyên nhân là do tiktoken được huấn luyện chủ yếu trên tiếng anh ,1 phần là dấu thanh, cấu trúc âm tiết và nhiều từ ghép lại.

---

## Block 3 — Streaming & Độ Bền (trả lời sau Checkpoint 3)

### Câu 3.1 — Trải nghiệm người dùng với streaming
**Streaming quan trọng nhất trong trường hợp nào, và khi nào thì
non-streaming lại phù hợp hơn?** (1 đoạn văn)
> Streaming quan trọng nhất trong các ứng dụng tương tác thời gian thực như chatbot, trợ lý code nơi người dùng thấy token chảy ra ngay sau 0.3–0.5 giây, tạo cảm giác phản hồi nhanh và có thể dừng giữa chừng nếu cần. Non-streaming phù hợp hơn khi cần xử lý tự động kết quả (parse JSON, tính toán), gọi API hàng loạt (batch), hoặc câu trả lời rất ngắn.

### Câu 3.2 — Vì sao backoff theo cấp số nhân?
**So với delay cố định (ví dụ luôn chờ 1 giây), exponential backoff có lợi
thế gì khi API bị quá tải? Điều gì xảy ra nếu hàng nghìn client cùng retry
với delay cố định giống nhau?**
> Exponential backoff giúp giảm tải cho server theo thời gian: thay vì hàng nghìn client retry đồng loạt sau 1 giây (gây "thundering herd" làm server càng nghẽn), delay tăng dần (1s, 2s, 4s, 8s...) rải đều các request theo thời gian. Nếu dùng delay cố định giống nhau, tất cả client sẽ tấn công server cùng lúc, khiến API có thể sập hoàn toàn thay vì hồi phục.

---

## Block 4 — Mini-Project (trả lời sau Checkpoint 4)

### Câu 4.1 — Thiết kế persona
**Bạn chọn persona gì cho trợ lý của mình? Viết lại system prompt đó và giải
thích 1–2 lựa chọn từ ngữ quan trọng trong prompt (ví dụ: vì sao yêu cầu
"trả lời ngắn gọn", vì sao chỉ định ngôn ngữ...):**
> Persona tôi chọn: "Bạn là trợ giảng thân thiện của khóa AI, trả lời ngắn gọn bằng tiếng Việt, luôn kết thúc bằng một câu hỏi gợi mở." Cụm "ngắn gọn" tránh model viết lan man, giúp học viên dễ tiếp thu; cụm "bằng tiếng Việt" đảm bảo không trộn tiếng Anh vào câu trả lời; "câu hỏi gợi mở" biến chatbot thành người dẫn dắt tư duy (Socratic questioning) thay vì công cụ tra cứu.

### Câu 4.2 — Hạn chế & cải thiện
**Trợ lý của bạn hiện có hạn chế lớn nhất là gì (ví dụ: history chỉ 3 lượt,
không có bộ nhớ dài hạn, không kiểm duyệt nội dung...)? Đề xuất một cải
thiện cụ thể và mô tả ngắn cách triển khai:**
> Hạn chế lớn nhất: trợ lý chỉ giữ 3 lượt hội thoại gần nhất, nên quên thông tin quan trọng như tên người dùng hay quy tắc đã đặt ra ở đầu phiên. Cải thiện: triển khai long-term memory bằng cách gọi API phụ để trích xuất fact quan trọng (tên, sở thích, ràng buộc) sau mỗi lượt, lưu vào file JSON/SQLite, rồi tái chèn vào system prompt ở các lượt sau — vừa tiết kiệm token vừa cá nhân hóa trải nghiệm.

---

## Danh Sách Kiểm Tra Nộp Bài

- [ ] `python grade.py` — xem điểm tự động, mục tiêu ≥ 75/100
- [ ] Cả 4 checkpoint pytest đều pass
- [ ] Tất cả 9 câu trong file này đã được trả lời
- [ ] Đã copy bài làm vào folder `solution/`, push lên fork và dán link trên trang bài Lab ở VLearn
