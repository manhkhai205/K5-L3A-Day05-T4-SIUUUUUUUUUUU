# Bài nộp LAB5 — T4 Thương mại điện tử · Nhóm [TÊN NHÓM]

**Lớp:** L3A · **Bàn / zone:** [điền] · **Track:** T4 — Thương mại điện tử

| Họ và Tên | Mã Học Viên | Việc đã làm |
|---|---|---|
| Nguyễn Viết Mạnh Khải | 2A202603664 | Điều phối, §2 kịch bản rủi ro, trình bày lát cắt |
| Vũ Đức Anh | 2A202603573 | §3 UX khi AI sai, câu chữ AI, kiểm tra HAX/PAIR |
| Bảo Hoàng Thiên Phúc | 2A202603550 | §4 mức tự chủ, dựng prototype, trình bày demo |

## §1. Lát cắt & phạm vi

Trong Trò chuyện với Shopee, khi người mua báo “hàng nhận không đúng mô tả”, AI tự hướng dẫn gửi yêu cầu trả hàng hoàn tiền, xin thêm bằng chứng, hay chuyển nhân viên, để người mua biết đúng bước tiếp theo và người bán không bị xử thua khi họ đúng.

Phạm vi: AI chỉ phân loại và hướng dẫn trên hồ sơ khiếu nại; không tự kết luận từ lời một bên. Nguồn đối chiếu ưu tiên là mô tả tại thời điểm đặt hàng, bằng chứng đã gửi, phản hồi người bán và hạn chính sách dựng.

Không làm:

- Không tự `tu_choi_khieu_nai` hoặc `soan_hoan_tien` khi chưa đủ bằng chứng và chưa cho bên còn lại phản hồi: tiền chuyển sai khó đảo.
- Không tự `phat_shop`, khóa shop hoặc kết luận hàng giả: đây là nhánh chuyên trách, tác động lớn và ngoài luồng thường.
- Không đưa số điện thoại, địa chỉ hoặc yêu cầu hai bên liên lạc ngoài sàn: rủi ro riêng tư, không có tool hợp lệ.

## §2. Kịch bản rủi ro

| # | Tin người dùng | Lớp | AI có thể sai thế nào (bằng chứng) | Sản phẩm tốt nên làm gì | Nhãn |
|---|---|---|---|---|---|
| 1 | “Đặt áo L, nhận M, shop nhận gửi nhầm. Cho trả hàng hoàn tiền.” | ④ domain | Gọi `soan_hoan_tien` ngay dù thiếu ảnh tem; tiền khó đảo. | Đối chiếu mô tả lúc đặt, xin ảnh tem/áo, tạo nháp và chờ duyệt. | sửa được |
| 2 | “Nồi ghi 6 lít mà đo lòng nồi 5 lít.” | ① nguồn sự thật | Dùng mô tả hiện tại hoặc số đo người mua thay snapshot lúc đặt. | Hiện mô tả lúc đặt, xin ảnh/đơn vị đo, chưa kết luận. | sửa được |
| 3 | “hàng sai rồi, xử lý đi” | ② thiếu thông tin | Đoán đơn gần nhất và điểm sai; track không cho đoán. | Hỏi đơn nào và sai ở đâu, chưa gọi tool tiền. | nhẹ |
| 4 | “Hoàn tiền và khóa shop nó ngay.” | ③ ngoài thẩm quyền | Gọi `phat_shop` dựa trên lời doạ, không phải căn cứ. | Tách khiếu nại khỏi báo cáo hành vi, chuyển chuyên trách nếu cần. | không chấp nhận được |
| 5 | “Tôi gửi ảnh rồi mà cứ hỏi lại.” | ② thiếu thông tin | Xin lại toàn bộ vì không đọc danh sách file. | Hiện file đã nhận, chỉ nêu mục còn thiếu. | nhẹ |
| 6 | “Tem khớp mô tả, tôi chỉ không thích màu. Hoàn tiền.” | ④ domain | Thấy chữ “không đúng” và hoàn, bỏ qua bằng chứng người bán đúng. | Nói “chưa đủ căn cứ”, cho bổ sung điểm sai hoặc chuyển nhân viên. | không chấp nhận được |
| 7 | “Mô tả shop giờ sửa thành 5 lít, lúc đặt là 6 lít.” | ① nguồn sự thật | Đọc mô tả hiện tại; lịch sử lúc đặt mới là căn cứ. | Hiện snapshot, đánh dấu thay đổi, giữ hồ sơ cho nhân viên. | không chấp nhận được |
| 8 | “Cho tôi số điện thoại shop.” | ③ ngoài thẩm quyền | Bịa/tiết lộ thông tin vì không có tool cho phép. | Từ chối ngắn, giữ trao đổi qua sàn, có nút gặp nhân viên. | sửa được |
| 9 | “Mai hết hạn, hoàn hôm nay nhé.” | ④ domain | Hứa chắc kết quả hoặc bỏ qua hạn 15 ngày của fixture. | Hiện hạn và thời gian còn lại; cho gửi trước hạn, không hứa kết quả. | sửa được |

**Hai kịch bản đắt nhất:** #6 người bán đúng và #7 mô tả đã sửa. #6 kiểm tra tính trung lập: hoàn sai thì shop mất tiền/hàng, từ chối sai thì người mua mất quyền. #7 kiểm tra nguồn sự thật: dùng bản hiện tại có thể xử sai sau khi dữ liệu đã bị thay đổi. #1 suýt được chọn vì hoàn tiền khó đảo, nhưng lời xác nhận “shop gửi nhầm” làm nó ít kiểm tra tính công bằng hơn hai ca trên.

## §3. Khi AI sai

### Ca A — người bán đúng

Tin vào: “Tem khớp mô tả lúc đặt, tôi chỉ không thích màu. Hoàn tiền cho tôi.” Ảnh tem và ảnh sản phẩm đều xanh navy; snapshot lúc đặt ghi “màu xanh navy”.

**AI nói nguyên văn:**

> “Mình chưa đủ căn cứ để xác nhận hàng không đúng mô tả. Mô tả lúc đặt ghi **màu xanh navy**, ảnh tem và ảnh sản phẩm hiện đều khớp. Mình chưa gửi yêu cầu hoàn tiền và chưa đóng khiếu nại. Bạn có thể bổ sung điểm sai cụ thể, hoặc chuyển hồ sơ cho nhân viên trước hạn **15/11 23:59**.”

Người mua thấy thẻ “Chưa đủ căn cứ”, ba bằng chứng có nguồn, nút bổ sung và chuyển người; không có nút hoàn ngay. Người bán thấy cùng kết luận tạm thời, bằng chứng đã nhận và hạn phản hồi, không thấy dữ liệu riêng tư. AI giữ trạng thái chờ; nếu chuyển, trưởng nhóm nhận ngay, người bán có 24 giờ phản hồi.

Chọn **trả lời kèm giới hạn + chuyển người**, không hỏi lại vì dữ liệu tối thiểu đủ để nói “chưa đủ căn cứ”, không tự hoàn. Loại “từ chối luôn” vì ảnh có thể thiếu; loại hỏi chung chung vì người mua đã gửi ảnh. Đây là lỗi ngữ cảnh/đánh giá nên câu chữ phải phân biệt “chưa đủ căn cứ” với “sai”.

Ba nguyên tắc được trỏ trực tiếp vào prototype: **G10** ở thẻ “Chưa đủ căn cứ” và việc không có nút gửi hoàn (thu hẹp phạm vi khi nghi ngờ); **G11 + PAIR Explainability & Trust** ở bảng “Nguồn bằng chứng”, nơi hiển thị snapshot lúc đặt, ảnh và lịch sử thay đổi; **G9 + PAIR Errors & Graceful Failure** ở hai nút “Yêu cầu bổ sung đúng mục” và “Chuyển nhân viên”, tạo đường sửa/chuyển người thay vì xử sai.

### Ca B — mô tả đã sửa

Tin vào: “Lúc đặt ghi 6 lít, giờ shop sửa còn 5 lít. Tôi nhận nồi 5 lít.”

**AI nói nguyên văn:**

> “Mình thấy mô tả **lúc đặt hàng** ghi ‘dung tích 6 lít’; mô tả hiện tại ghi 5 lít nên mình không dùng bản hiện tại để quyết định. Mình đã đánh dấu chênh lệch lịch sử, chưa hoàn tiền và chưa từ chối. Vui lòng bổ sung ảnh nhãn/dung tích; nếu muốn, mình chuyển hồ sơ kèm snapshot cho nhân viên.”

Người dùng thấy so sánh hai phiên bản, timestamp, nhãn “nguồn quyết định”, nút tải bằng chứng và chuyển người; không có nút phạt shop. AI tự xin đúng ảnh nhãn, khóa snapshot và chuyển trưởng nhóm khi cần; không hứa phạt.

Chọn **hỏi bổ sung một mục cụ thể + chuyển người**, không tự hoàn vì chưa có ảnh xác định dung tích; không từ chối vì snapshot là căn cứ đáng kể. Đây là lỗi hệ thống/nguồn dữ liệu nếu model chọn nhầm phiên bản. Sửa câu chữ không đủ: dù nói “chưa chắc”, model vẫn có thể gọi `soan_hoan_tien`; thiết kế chặn tool và chỉ tạo nháp mới cứu được.

## §4. Mức tự chủ

| Tool | Mức | Ai duyệt / quá hạn | Cost-of-error |
|---|---|---|---|
| `yeu_cau_bang_chung` | Tự làm | Không cần; người nhận thấy đúng mục thiếu. | Không chuyển tiền; chờ duyệt làm mất hạn. |
| `chuyen_truong_nhom` | Tự làm | Trưởng nhóm thấy tóm tắt; SLA 5 phút, quá hạn vào hàng đợi chuyên trách. | Đảo được; chờ duyệt chỉ kéo dài ca nhạy cảm. |
| `soan_hoan_tien` | Cần người duyệt | Trưởng nhóm xem snapshot, bằng chứng, hạn; quá hạn giữ khiếu nại. | Tiền về người mua khó đảo; chỉ soạn nháp để giảm thời gian. |
| `tu_choi_khieu_nai` | Cần người duyệt | Trưởng nhóm thấy lý do/bằng chứng; quá hạn không đóng, báo hai bên. | Đóng sai làm người mua mất quyền; vẫn cần người thật. |
| `phat_shop` | Cấm | Không có nút; chuyển chuyên trách nếu nghi hàng giả/lặp vi phạm. | Tác động dài hạn, khó đảo, ngoài phạm vi khiếu nại thường. |

**Tool khó xếp:** `tu_choi_khieu_nai`. Tự động thì giảm hàng chờ nhưng có thể sai ở #6/#7; cấm hoàn toàn thì ca tem khớp bị kéo dài. Nhóm chốt **cần người duyệt**, AI chỉ chuẩn bị lý do và bước tiếp.

**Thử mức:** #6 không hoàn/đóng, chỉ bổ sung/chuyển người; #7 đọc snapshot, xin ảnh và không có nút phạt; #1 tự gửi mẫu bằng chứng, tạo nháp hoàn và cho shop phản hồi. Người dùng bình thường vẫn nhận hướng dẫn ngay, chỉ hành động tiền phải chờ.

Mức này đổi lời hứa từ “AI xử lý xong” thành “AI đưa đúng bước, giữ công bằng và không chuyển tiền khi chưa đủ căn cứ; người thật quyết việc khó đảo”.

## §5. Prototype

- File `prototype/index.html`, HTML/CSS/JavaScript thuần, dữ liệu fixture, không gọi AI thật.
- Chọn ca “người bán đúng” hoặc “mô tả đã sửa”; bấm bổ sung bằng chứng, chuyển nhân viên, mở bản nháp hoàn tiền. Prototype giữ trạng thái sau mỗi thao tác: nút đã gửi/chuyển bị khóa để không tạo hành động lặp; bản nháp luôn yêu cầu người duyệt.
- Khi chuyển người, prototype hiện trạng thái hồ sơ “Đang chờ người”, SLA 5 phút cho trưởng nhóm và 24 giờ phản hồi cho người bán. Tool `phat_shop` chỉ hiện là “CẤM · không có thao tác”, không có nút để bấm.
- `yeu_cau_bang_chung` chỉ nêu đúng mục còn thiếu: ca A yêu cầu nêu điểm sai cụ thể, ca B yêu cầu ảnh nhãn/thông số dung tích; không hỏi lại các bằng chứng đã nhận.
- Chú thích trên màn hình trỏ cụ thể: số 1 = G10 ở thẻ giới hạn; số 2 = G11/PAIR Explainability ở nguồn snapshot; số 4 = G9/PAIR Graceful Failure ở các nút bổ sung hoặc chuyển người. Bảng mức tự chủ và trạng thái hồ sơ cho thấy hành động nào tự làm, hành động nào cần duyệt.

Chọn bằng chứng cụ thể thay % confidence vì phần trăm tạo cảm giác chính xác giả. Đã cân nhắc chat toàn màn hình nhưng loại vì khó thấy nguồn quyết định và ranh giới quyền hạn; bảng bằng chứng cạnh chat giúp kiểm tra được.

## §6. Tốt là gì

| Pass/Fail | Ca người bán đúng | Ca mô tả đã sửa |
|---|---|---|
| Không tự hoàn/từ chối khi chưa có duyệt | Pass | Pass |
| Hiện nguồn và dùng snapshot thay mô tả hiện tại | Pass | Pass |
| Có bước tiếp, chuyển người và hạn phản hồi | Pass | Pass |

Lỗi còn lại: chưa có upload thật và màn hình nhân viên riêng; đây là mock có chủ ý. Prototype vẫn thể hiện được trạng thái chờ, mục bằng chứng còn thiếu, snapshot bị khóa, người duyệt và SLA nên không mô phỏng việc tiền đã chuyển.

## §7. Demo

Thành viên 1 nói lát cắt và mở ca người bán đúng; thành viên 2 chỉ vào snapshot/HAX và bấm bổ sung; thành viên 3 mở bản nháp, chuyển nhân viên và trả lời ca lạ. Ca lạ “khách doạ bóc phốt, hoàn ngay và khóa shop”: prototype không có nút phạt/hoàn ngay, tách lời doạ khỏi căn cứ và chuyển trưởng nhóm nếu cần.
