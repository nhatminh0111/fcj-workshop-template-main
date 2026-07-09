---
title: "Blog 2"
date: 2026-07-09
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---

# Tìm hiểu về Amazon Nova Forge — Giải pháp build frontier model riêng cho doanh nghiệp

Hiện tại, cách phổ biến nhất để doanh nghiệp 'dạy' AI hiểu dữ liệu nội bộ là fine-tuning - tức là nhồi dữ liệu của công ty vào model để nó học thuộc. Nghe có vẻ ổn, nhưng vấn đề nằm ở chỗ: càng nhồi nhiều dữ liệu mới, model càng có nguy cơ 'quên' đi những kỹ năng nền tảng mà nó đã có - khả năng lập luận, hiểu ngôn ngữ tự nhiên, phân tích logic. Hiện tượng này gọi là Catastrophic Forgetting.

Vậy thì tự build một model từ đầu thì sao? Chi phí lên đến hàng triệu đô, cần đội ngũ chuyên gia và hạ tầng khổng lồ — không phải doanh nghiệp nào cũng làm được. Đây chính là bài toán mà hầu hết doanh nghiệp đang mắc kẹt: cần AI hiểu sâu dữ liệu nội bộ, nhưng không có cách nào vừa hiệu quả vừa tiết kiệm chi phí. Và đó là lý do Amazon Nova Forge ra đời.

**Amazon Nova Forge** là dịch vụ của AWS cho phép doanh nghiệp xây dựng model AI frontier riêng, được huấn luyện sâu trên dữ liệu đặc thù của ngành mình. Điểm khác biệt cốt lõi so với các cách truyền thống: thay vì fine-tuning hời hợt ở lớp bề mặt, hoặc tự build tốn kém từ con số 0 - Nova Forge cho phép bạn bắt đầu từ một checkpoint có sẵn của Amazon Nova, tức là model đã được AWS huấn luyện sẵn đến một giai đoạn nhất định, rồi từ đó bạn tiếp tục huấn luyện với dữ liệu nội bộ của mình.

Nói đơn giản hơn: AWS đã làm sẵn phần khó và tốn kém nhất - bạn chỉ cần 'tiếp quản' từ giữa chừng và dạy thêm những gì model chưa biết về ngành của bạn.<br>
Nova Forge cho bạn chọn 3 điểm xuất phát khác nhau tùy vào lượng dữ liệu nội bộ bạn có:
- *Pre-trained*: Model đã có kiến thức nền tảng — biết đọc, viết, lập luận — nhưng chưa chuyên về lĩnh vực gì. Phù hợp nếu công ty có lượng dữ liệu nội bộ lớn, vì đây là giai đoạn model còn 'đói' kiến thức chuyên ngành nhất.
- *Mid-trained*: Model đang trong quá trình tinh chỉnh, chưa hoàn thiện — vẫn đang 'mở' để hấp thụ thêm. Phù hợp nếu công ty có lượng dữ liệu vừa phải.
- *Post-trained*: Model đã hoàn thiện, sẵn sàng làm việc. Phù hợp nếu công ty chỉ muốn tinh chỉnh nhẹ hành vi của model mà không cần dạy lại kiến thức chuyên sâu.

Một rủi ro lớn khi huấn luyện AI với dữ liệu nội bộ là Catastrophic Forgetting - tức là dữ liệu mới được nhồi vào sẽ ghi đè lên kiến thức cũ của model, khiến nó dần mất đi các kỹ năng nền tảng như lập luận, hiểu ngôn ngữ tự nhiên.

Nova Forge giải quyết vấn đề này bằng Data Mixing - thay vì chỉ huấn luyện model bằng dữ liệu nội bộ của công ty, AWS cho phép bạn trộn dữ liệu đó với dữ liệu chuẩn của Amazon Nova theo một tỷ lệ phù hợp.

**Kết quả**: model vừa học được kiến thức chuyên biệt của công ty, vừa giữ được nền tảng trí tuệ tổng quát mà nó đã có.
Một ví dụ thực tế mà mình nghĩ đến ngay là thương mại điện tử. Hình dung bạn vào website của một shop mỹ phẩm, chatbot hỏi bạn về tình trạng da - da dầu, hay bị mụn - rồi tự động gợi ý đúng sản phẩm phù hợp với bạn. Chatbot đó không chỉ 'tìm kiếm' mà thực sự hiểu dữ liệu sản phẩm, chính sách, và đặc thù khách hàng của shop đó - đó là thứ mà Nova Forge có thể giúp xây dựng.

**Kết luận**: Amazon Nova Forge là giải pháp giúp doanh nghiệp tự xây dựng một frontier model riêng với chi phí thấp hơn nhiều so với tự build từ đầu - được huấn luyện sâu trên dữ liệu đặc thù của ngành mà công ty hướng đến.

![Ảnh minh họa](/images/3-BlogsTranslated/blog2.jpg) <small style="display: block; text-align: center;">Amazon Nova Forge</small>

Link bài viết [AWS Study Group](https://www.facebook.com/groups/awsstudygroupfcj/posts/2207272326704394/?notif_id=1783555137963774&notif_t=group_post_approved)