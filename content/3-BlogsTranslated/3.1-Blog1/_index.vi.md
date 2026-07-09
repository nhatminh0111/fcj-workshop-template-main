---
title: "Blog 1"
date: 2026-07-08
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# XÂY DỰNG PROTEIN RESEARCH COPILOT VỚI AMAZON BEDROCK AGENTCORE

Xây dựng Protein Research Copilot bằng Amazon Bedrock AgentCore. Đây là một hệ thống AI hỗ trợ các nhà nghiên cứu protein tìm kiếm peptide tương đồng và phân tích dữ liệu sinh học chỉ bằng ngôn ngữ tự nhiên.

Trước đây, để tìm các protein hoặc peptide có cấu trúc tương tự, nhà nghiên cứu phải:
- Tìm kiếm thủ công trong dataset lớn
- Chạy các thuật toán so sánh sequence
- Tự phân tích kết quả bằng chuyên môn sinh học

Quy trình này mất nhiều thời gian, dễ sai sót và khó mở rộng khi dữ liệu ngày càng lớn.

Với Protein Research Copilot, AWS kết hợp AI Agents, Vector Search và Foundation Models để tự động hóa toàn bộ quy trình nghiên cứu:
**Natural Language Query Parsing**: AI hiểu câu hỏi tự nhiên như: “Find 10 peptides similar to dengue virus peptide LPAIVREAI” và chuyển thành query có cấu trúc.

**Vector Similarity Search**: Sequence protein được chuyển thành embeddings bằng model chuyên biệt (ESM-C 300M), sau đó tìm kiếm peptide tương tự trong vector database.

**AI-powered Summarization**: Sau khi tìm thấy kết quả, AI sẽ tự động tóm tắt và đưa ra insight khoa học cho researcher.

Hệ thống sử dụng các dịch vụ AWS chính:
- Amazon Bedrock AgentCore — Runtime để deploy và scale AI agent
- Amazon Bedrock — Chạy LLM để reasoning và summarization
- Amazon SageMaker — Deploy embedding model protein
- Amazon Aurora PostgreSQL + pgvector — Lưu embeddings để semantic search
### Những điểm đáng chú ý nhất
Thay vì AI “ghi nhớ” toàn bộ dữ liệu protein, hệ thống sử dụng Retrieval-Augmented Generation (RAG) để:
1. Retrieve đúng dữ liệu liên quan
2. Cung cấp context cho LLM
3. Trả lời chính xác hơn và giảm hallucination
Điều này giúp rút ngắn quy trình nghiên cứu từ hàng giờ xuống còn vài phút.
### Kết luận
Protein Research Copilot cho thấy AI Agents không chỉ dùng cho chatbot mà còn có thể ứng dụng vào các lĩnh vực chuyên sâu như y sinh, dược phẩm và nghiên cứu khoa học. Đây là ví dụ rất hay về cách AWS kết hợp Agentic AI + Vector Database + LLM để xây dựng các hệ thống AI thực tế ở production scale.

![Ảnh kiến trúc bài blog](/images/3-BlogsTranslated/blog1.jpg) <small style="display: block; text-align: center;">Sơ đồ kiến trúc</small>

Link bài viết [AWS Study Group](https://www.facebook.com/photo?fbid=879517288533047)