# Báo cáo đánh giá và phân tích Prompt V1 vs Prompt V2 (RAGAS Evaluation)

## 1. Bảng tổng hợp kết quả (Evaluation Results)

| Metric | Prompt V1 (Ngắn gọn, súc tích) | Prompt V2 (Chi tiết, cấu trúc) | Winner | Target (≥ 0.8) |
| :--- | :---: | :---: | :---: | :---: |
| **Faithfulness** | **0.9781** | **0.9412** | ← V1 | ✅ Đạt (cả 2 đều ≥ 0.9) |
| **Answer Relevancy** | **0.8203** | **0.8654** | ← V2 | ✅ Đạt |
| **Context Recall** | **0.9730** | **0.9730** | Hoà | ✅ Đạt |
| **Context Precision** | **1.0000** | **1.0000** | Hoà | ✅ Đạt |

---

## 2. Phân tích chi tiết so sánh V1 vs V2

### 2.1. Faithfulness (Độ trung thực với Context)
* **Kết quả:** Prompt V1 đạt **0.9781**, cao hơn Prompt V2 (**0.9412**).
* **Nguyên nhân:**
  * **Prompt V1** yêu cầu LLM *"Giữ câu trả lời ngắn gọn, súc tích (2-4 câu), trực diện vào vấn đề"*. Do phạm vi trả lời hẹp và trực diện, mô hình chỉ trích xuất các sự thật cốt lõi từ context mà không suy diễn hay mở rộng, từ đó giảm thiểu tối đa rủi ro hallucination và đạt độ trung thực gần như tuyệt đối (~97.8%).
  * **Prompt V2** yêu cầu *"Trả lời chi tiết, có tổ chức và lập luận chặt chẽ (3-5 câu), dùng gạch đầu dòng"*. Khi mở rộng lời giải thích và cấu trúc câu dài hơn, mô hình có xu hướng sinh thêm các từ nối và câu dẫn dắt, khiến điểm faithfulness bị giảm nhẹ nhưng vẫn duy trì ở mức xuất sắc (> 0.94).

### 2.2. Answer Relevancy (Độ liên quan tới câu hỏi)
* **Kết quả:** Prompt V2 đạt **0.8654**, cao hơn Prompt V1 (**0.8203**).
* **Nguyên nhân:**
  * Prompt V2 cung cấp câu trả lời đầy đủ, bao quát các khía cạnh và có cấu trúc rõ ràng (định dạng danh sách/gạch đầu dòng), giúp câu trả lời giải quyết trọn vẹn hơn các câu hỏi đa ý hoặc yêu cầu so sánh/liệt kê.

### 2.3. Context Recall & Context Precision
* **Kết quả:** Cả hai phiên bản đều đạt điểm tương đương (**Context Recall: 0.9730**, **Context Precision: 1.0000**).
* **Nguyên nhân:** Cả hai phiên bản đều sử dụng chung một Retriever (`FAISS` vectorstore với `top_k = 3` và embedding model tương đồng), do đó chất lượng truy xuất tài liệu tham khảo cho 50 câu hỏi thử nghiệm là hoàn toàn đồng nhất.

---

## 3. Kết luận
* Cả hai phiên bản Prompt đều vượt qua ngưỡng yêu cầu của bài tập (`Faithfulness ≥ 0.8`), thậm chí đều đạt trên **0.9** (đạt tiêu chí thưởng).
* Tùy thuộc vào kịch bản sử dụng thực tế:
  * Chọn **V1** cho chatbot hỏi đáp nhanh, tóm tắt trực diện và tiết kiệm token.
  * Chọn **V2** cho hệ thống tư vấn chuyên sâu, báo cáo phân tích cần câu trả lời chi tiết và đa chiều.

