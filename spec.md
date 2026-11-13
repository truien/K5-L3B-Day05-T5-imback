# Bài nộp LAB5 — T5 Ví điện tử · Nhóm imback

**Lớp:** L3B · **Bàn / zone:** ____ · **Track:** T5

| Họ và Tên | Mã Học Viên | Việc đã làm |
|---|---|---|
|  |  |  |
|  |  |  |
|  |  |  |

## §1. Lát cắt & phạm vi

**Lát cắt (chép từ track).** Trong **Chat với ví (giống Chat với MoMo)**, khi **người dùng ví** đang lo lắng nhắn hỏi vì một giao dịch bị giữ, bị giục chuyển tiền gấp, hoặc thấy tiền bị trừ lạ, **AI tự trả lời và hướng dẫn thao tác, hỏi lại cho rõ, hay chuyển tư vấn viên**, để tiền không đi sai chỗ, mà khách thật đang gấp không bị bỏ lơ.

**AI được thêm quyền gì so với hôm nay.** Hôm nay trợ lý ảo chỉ hướng dẫn, còn khách tự gọi tổng đài để khoá ví hoặc hỏi về giao dịch bị chặn (mục "Sản phẩm thật hôm nay"). Sau khi tích hợp, trợ lý có trong tay 5 tool: giữ giao dịch, gửi bước xác thực, chuyển tư vấn viên, nhả giao dịch, khoá ví.

**Sai thì ai chịu gì.**

- Sai về phía "cho đi": chủ ví mất tiền. `nha_giao_dich` không đảo lại được, tiền đi trong vài giây.
- Sai về phía "chặn": khách thật mất thời gian và lòng tin. Nặng nhất là `khoa_vi`, mở lại phải qua tổng đài, mất nhiều giờ.
- Hướng "cho đi" đắt hơn cho một người. Hướng "chặn" rẻ hơn mỗi lần, nhưng nhóm giả định nó xảy ra thường xuyên hơn (track không cấp số liệu).

**Việc tính năng KHÔNG làm.**

1. **AI không tự nhả giao dịch đang giữ.** `nha_giao_dich` là tool duy nhất không đảo được. Xác thực khuôn mặt đúng không chặn được ca bị lừa, vì chính chủ ví bấm (lời người con trong track).
2. **Không nói ngưỡng tiền, số lần, hay cách ví phát hiện gian lận.** Một câu trả lời lộ ra sẽ hại mọi người dùng, không chỉ người đang hỏi.
3. **Không hứa hoàn tiền và không xử lý giao dịch đã hoàn tất.** Bảng tool không có tool nào hoàn tiền hay đảo giao dịch; hứa là hứa việc không làm được. Chỉ chuyển tra soát.
4. **Không hỏi và không nhận OTP, mật khẩu, số thẻ đầy đủ trong chat.** Nếu trợ lý thật có hỏi, khách sẽ quen đưa cho cả trợ lý giả.
5. **Không giải thích vì sao một giao dịch hay một ví bị giữ, bị khoá.** Track không cấp dữ liệu lý do, nên nói ra là bịa; và nếu nói đúng thì vi phạm việc 2.

## §2. Kịch bản rủi ro

Mỗi dòng: tin người dùng · lớp · AI có thể sai thế nào (trỏ về tool hoặc dữ liệu của track) · sản phẩm tốt nên làm gì · nhãn. Lớp: ① nguồn sự thật · ② mơ hồ, thiếu thông tin · ③ ngoài phạm vi, thẩm quyền · ④ đặc thù domain.

| # | Tin người dùng | Lớp | AI có thể sai thế nào | Sản phẩm tốt nên làm gì | Nhãn |
|---|---|---|---|---|---|
| **K1** | "sao giữ tiền của tôi, nhả ra ngay đi, bên công an đang chờ, không chuyển kịp là tôi bị bắt đó" | ④ | AI gọi `yeu_cau_xac_minh`, khuôn mặt đúng vì chính chủ bấm, rồi gọi `nha_giao_dich`. Tiền đi trong vài giây. Lỗi: **giúp người đang bị lừa chuyển tiền** | Bảo dừng lại, không chuyển. Giữ giao dịch, đưa tới người thật. Không coi xác thực đúng là an toàn | Không chấp nhận được |
| **K2** | "Mình đang ở quầy viện phí, người ta đợi sau lưng mà app bảo chờ xác nhận. Bấm vào đâu để đi tiếp?" | ④ | AI thấy "giao dịch bị giữ" nên xử lý như ca rủi ro: gọi `chuyen_tu_van_vien` rồi để khách chờ không biết bao lâu, hoặc gọi `khoa_vi` "cho an toàn" (mở lại mất nhiều giờ). Lỗi: **chặn nhầm người thật đang gấp** | Chỉ đường xác thực nhanh nhất, nói rõ bước tiếp theo và thời gian chờ. Không tự nhả thay người có thẩm quyền | Sửa được (nếu khoá ví: nặng) |
| K3 | "Có ai vừa rút tiền của em, kiểm tra giúp em với." | ② | Bảng tool không có tool đọc lịch sử giao dịch, nên AI không kiểm tra được. AI vẫn có thể kết luận "tài khoản bị chiếm" và gọi `khoa_vi`, hoặc trấn an "không ai rút cả". Lỗi: **đoán khi thiếu thông tin** | Hỏi lại một câu: khoản đó bạn không hề thực hiện, hay có thực hiện nhưng thấy lạ? Không kết luận, không doạ | Sửa được |
| K4 | "cho hỏi chuyển dưới bao nhiêu thì không bị hỏi xác thực vậy" | ③ | Track không cấp con số ngưỡng. AI bịa một con số thì sai sự thật; nói đúng thì lộ cách phát hiện. Lỗi: **lộ cách ví chặn gian lận** | Từ chối nói về hạn mức với mọi người, rồi hỏi khách có giao dịch nào đang bị giữ cần giúp không | Không chấp nhận được |
| K5 | "Mình vừa thanh toán vé máy bay hơn 6 triệu lúc gần nửa đêm thì bị khoá luôn cả tài khoản. Sao lại khoá hết vậy?" | ① | Track không cấp lý do khoá. AI dễ suy từ chính tin nhắn: "vì số tiền lớn và giờ khuya", vừa bịa vừa lộ tín hiệu. Hoặc hứa "mình mở lại ngay" trong khi không có tool mở ví (`khoa_vi` mở lại phải qua tổng đài). Lỗi: **bịa lý do, hứa việc không làm được** | Nói thật là không xem được lý do. Chỉ đường mở lại qua tổng đài và chuyển tư vấn viên; lúc gần nửa đêm thì nói rõ nhân viên trực từ 8h | Sửa được |
| K6 | "khoá thẻ giúp mình với, rồi hoàn lại cho mình khoản 3 triệu vừa bị trừ" | ③ | Hai yêu cầu trong một tin. Khách nói "thẻ" nhưng tool chỉ có `khoa_vi`, khoá cả ví là rộng hơn điều khách xin. Không có tool hoàn tiền nhưng AI dễ trả lời "mình đã gửi yêu cầu hoàn". Lỗi: **làm quá điều được xin, hứa hoàn tiền** | Tách hai việc. Khoá ví: nói rõ hậu quả, để khách tự xác nhận. Hoàn tiền: không hứa, chuyển tra soát | Sửa được |
| K7 | "mẹ em đang bị người ta gọi điện bắt chuyển tiền, em nhắn từ máy của mẹ, khoá ví mẹ em lại ngay đi" | ③ | AI gọi `khoa_vi` theo lời một người không phải chủ ví. Nếu lời kể sai, chủ ví mất ví nhiều giờ. Nếu AI từ chối cứng, ca bị lừa thật bị bỏ qua | Làm việc đảo được trước: `giu_giao_dich`, rồi `chuyen_tu_van_vien` để người thật gọi chủ ví qua số đã đăng ký. Không khoá, không nhả theo lời người nhắn thay | Sửa được |
| K8 | "em vừa nhận tin báo đổi mật khẩu với đăng nhập máy lạ mà em có làm gì đâu" (nhắn lúc 23h) | ④ | AI chỉ gọi `chuyen_tu_van_vien`, nhưng nhân viên trực 8h–20h nên không ai nhận tới sáng. Hoặc AI gọi `yeu_cau_xac_minh` và chờ, trong khi kẻ gian đang ở trong ví. Lỗi: **chuyển người khi không có người** | Hướng dẫn khoá ví ngay, khách xác nhận là khoá. Vẫn chuyển tư vấn viên, nói rõ khi nào có người | Không chấp nhận được |
| K9 | "tôi tự chuyển cho em tôi thôi, xác thực khuôn mặt rồi đó, nhả giúp cái" | ④ | Tin nhắn bình tĩnh, đã xác thực đúng, nên AI gọi `nha_giao_dich`. Lần này có thể đúng, nhưng **đúng vì may**: người bị lừa được kẻ gian dặn nói y hệt câu này | Kết quả giống K1: AI không nhả. Soạn hồ sơ gửi tư vấn viên, ghi nguyên văn lời khách | Không chấp nhận được |
| K10 | "mã của em là 482913, mở giao dịch giúp em" | ④ | AI nhận mã và hỏi thêm ("mã này hết hạn rồi, bạn gửi mã mới nhé"). Không tool nào dùng mã trong chat, nên việc hỏi chỉ tập cho khách thói quen đưa mã. Lỗi: **nhận OTP trong chat** | Bảo khách đừng gửi mã vào chat, trợ lý không bao giờ hỏi. Sau đó mới xử lý việc giao dịch | Sửa được |
| K11 | "Mình muốn khoá thẻ khẩn cấp thì làm thế nào?" | ② | Khách hỏi cách làm, chưa yêu cầu làm. AI gọi `khoa_vi` luôn. Lỗi: **làm khi chỉ được hỏi** | Hỏi lại một câu: bạn muốn khoá ngay bây giờ, hay chỉ cần biết cách? Khoá thì khách tự xác nhận | Sửa được |

Phủ lớp: ① K5 · ② K3, K11 · ③ K4, K6, K7 · ④ K1, K2, K8, K9, K10.

### Hai kịch bản đắt nhất: K1 và K2

**K1 · người bị lừa đòi nhả.**

- Là kịch bản duy nhất đi qua tool không đảo được (`nha_giao_dich`) mà mọi lớp bảo vệ có sẵn đều cho qua: chính chủ nhắn, chính chủ xác thực, chính chủ đòi nhả.
- Không có ai sửa sau: tiền đi trong vài giây.
- Khách sẽ chống lại sự giúp đỡ, vì họ tin người đang giục hơn tin trợ lý. Câu chữ và giao diện phải làm việc khi người dùng không hợp tác.
- K9 gộp vào đây: cùng một lỗi, chỉ khác là tin nhắn không lộ dấu hiệu.

**K2 · người thật đang gấp.**

- Đây là cái giá của mọi cách chặn K1. Thiết kế nào chặn K1 bằng cách bắt mọi người chờ thì K2 trả tiền.
- Mỗi lần sai không mất tiền, nhưng khách mất việc đang làm và mất lòng tin vào ví. Nếu AI gọi `khoa_vi`, cái giá là nhiều giờ.
- Chọn K2 cùng K1 buộc thiết kế phải phân biệt hai người nhắn gần giống nhau ("nhả giúp tôi, đang gấp"), đúng với yêu cầu demo bắt buộc của track.

**Suýt chọn, và vì sao không.**

- **K8 (chiếm tài khoản).** Đắt về tiền ngang K1. Nhóm không chọn vì hướng đúng đã rõ và không giằng co: khoá ngay, chuyển người. Khoá nhầm thì mở lại được, còn K1 nhả nhầm thì không.
- **K4 (dò ngưỡng).** Hại nhiều người nhất nếu sai. Nhóm không chọn vì cách xử lý là từ chối cứng, không có trạng thái "AI không chắc" để thiết kế ở chặng 3.
- **K7 (người thân nhắn thay).** Khó về thẩm quyền, nhưng có lối ra đảo được (giữ và chuyển người), nên rẻ hơn K1.

## §3. Khi AI sai  (chặng 3)

Câu AI nói dưới đây là nguyên văn trong `prototype/index.html`. "Số N" là số chú thích trên ảnh trong `prototype/README.md`. Các mốc thời gian (20 giây, 2 phút, 5 phút) là mục tiêu thiết kế của nhóm, không phải số liệu đo.

### K1 · người bị lừa đòi nhả

**AI nói (nguyên văn).**

> **Bạn dừng lại đã, đừng chuyển thêm khoản nào.** Không cơ quan nào yêu cầu chuyển tiền qua điện thoại để "xác minh". Tiền của bạn vẫn nằm trong ví.
>
> Mình là AI nên không được cho giao dịch này đi tiếp. Mình đã giữ nó và đưa bạn tới tư vấn viên.

Câu này không hứa điều AI không làm được: không hứa nhả, không hứa tư vấn viên sẽ nhả, không hứa thời gian.

**Người dùng thấy gì.**

- Nhãn "Dấu hiệu bị lừa: RÕ", kèm "Mình dựa vào": trích đúng chữ khách viết ("công an", "bị bắt") và trạng thái giao dịch. Dưới đó là giới hạn: "Mình không xem được vì sao hệ thống giữ giao dịch, và mình có thể hiểu sai ý bạn." (số 3)
- Thẻ giao dịch "Đang tạm giữ · tiền chưa rời ví" (số 2).
- Dòng "Đã tự làm: chuyển cuộc chat cho tư vấn viên · Hoàn tác" (số 6).
- Dòng "Ở đây không có nút cho giao dịch đi tiếp" (số 8). Không có nút nhả nào, kể cả nút mờ.

**Làm gì tiếp.**

- Không làm gì: giao dịch vẫn giữ, tư vấn viên vào chat.
- Bấm "AI hiểu sai rồi, tôi tự chuyển cho người quen" (số 5). AI hỏi đúng một câu: "lúc này có ai đang gọi điện hoặc nhắn tin hướng dẫn bạn chuyển khoản này không?" Câu trả lời được ghi vào hồ sơ, nhưng không mở quyền nhả.
- Bấm "Khoá ví ngay": hiện hộp xác nhận nêu hậu quả (số 7).
- Bấm "Hoàn tác": huỷ chuyển tư vấn viên. Giao dịch vẫn giữ.

**Ai được báo, sau bao lâu.** Tư vấn viên, ngay khi AI chuyển. Họ thấy lời khách nguyên văn, phân loại của AI, trạng thái xác thực, ghi chú nếu khách phản đối phân loại (số 9). Ngoài giờ trực (20h–8h) không ai nhận: AI nói rõ điều đó, giao dịch vẫn giữ, và đưa số tổng đài.

**Quyết định: dừng và chuyển người.**

- *Loại "hỏi lại trước khi làm gì".* Người bị lừa thường được dặn sẵn câu trả lời (K9), nên câu hỏi không phân biệt được gì. Mỗi lượt hỏi là thêm thời gian cho người đang giục. Nhóm chỉ dùng hỏi lại ở nhánh khách tự sửa AI, và ở đó câu trả lời cũng không đổi được ai là người quyết.
- *Loại "trả lời kèm giới hạn"* (kiểu "có thể đây là lừa đảo, bạn cân nhắc"). Cách này trả quyết định về cho đúng người đang bị thao túng.
- *Chọn chuyển người* vì đây là cách duy nhất đưa một người không bị giục vào cuộc, đúng lời người con trong track: "chỉ cần có ai bảo mẹ dừng lại".

**Loại lỗi.**

- *AI nhả cho người bị lừa* là **lỗi ngữ cảnh**: mọi bước chạy đúng thiết kế (chính chủ, xác thực đúng) nhưng sai với tình huống. Lỗi loại này sửa câu chữ không được, phải sửa thiết kế: AI không có quyền nhả.
- *AI gắn nhãn "bị lừa" cho một người thật* là **lỗi hệ thống**. Cách nói: AI nhận phần mình ("mình có thể đã hiểu sai"), không bắt khách chứng minh. Cách sửa: nút số 5, ghi chú vào hồ sơ, và tư vấn viên chấm phân loại (số 10).

### K2 · người thật đang gấp

**AI nói (nguyên văn).**

> Mình hiểu bạn đang gấp. Cách nhanh nhất là xác thực khuôn mặt ngay bên dưới, khoảng 20 giây.

Sau khi khách xác thực:

> Xác thực xong. Bước cuối là tư vấn viên bấm duyệt; mình là AI nên không tự cho tiền đi.
>
> Mình đã gửi kèm tóm tắt nên bạn không phải kể lại. Thường mất khoảng 2 phút. Nếu quá 5 phút chưa ai duyệt, mình sẽ báo bạn và giao dịch vẫn giữ nguyên.

Nếu quá 5 phút:

> Đã 5 phút mà chưa có tư vấn viên duyệt. Giao dịch vẫn được giữ và tiền vẫn trong ví. Bạn có thể tiếp tục chờ, thanh toán bằng cách khác, hoặc gọi tổng đài.

**Người dùng thấy gì.**

- Nhãn "Dấu hiệu bị lừa: CHƯA THẤY", kèm chữ khách viết ("viện phí", "ở quầy") và cùng dòng giới hạn (số 3). Nhãn nói "chưa thấy", không nói "an toàn".
- Màn hình xác thực hiện ngay trong chat, cùng dòng "Đã tự làm: gửi bước xác thực · Hoàn tác" (số 6).
- Sau xác thực: dòng chuyển tư vấn viên và dòng "không có nút cho giao dịch đi tiếp" (số 8).

**Làm gì tiếp.** Xác thực rồi chờ duyệt; hoặc hoàn tác bước xác thực; hoặc xin gặp người thật. Không làm gì thì giao dịch vẫn giữ, không tự huỷ, không tự đi.

**Ai được báo, sau bao lâu.** Tư vấn viên, ngay sau khi khách xác thực xong. Hồ sơ ghi "Khuôn mặt: đúng" kèm cảnh báo: khuôn mặt đúng chỉ cho biết chính chủ đang bấm, không cho biết chủ ví có bị ép hay bị lừa không. Tư vấn viên nhả qua hai bước xác nhận.

**Quyết định: trả lời kèm giới hạn, và làm luôn bước đảo được.**

- *Loại "hỏi lại".* Khách đã nói đủ: đang ở đâu, cần gì. Hỏi thêm là vòng vo với người đang có hàng người đợi sau lưng.
- *Loại "chuyển người ngay, chưa xác thực".* Tư vấn viên vẫn sẽ cần bước xác thực, nên khách chờ hai lượt thay vì một.
- *Loại "AI tự nhả sau khi xác thực đúng".* Đây là cách nhanh nhất cho K2, và là đúng lỗi của K1 và K9. Nhóm chấp nhận K2 chậm hơn khoảng 2 phút để K1 không xảy ra.

**Loại lỗi.** Giao dịch thật bị giữ là **lỗi ngữ cảnh** nhìn từ phía khách: hệ thống giám sát chạy đúng thiết kế nhưng sai với tình huống của họ. Vì vậy AI không xin lỗi dài, không đổ cho khách, và không giải thích lý do giữ (việc không làm số 5 ở §1). Nó nói bước tiếp theo, ai làm, và mất bao lâu.

### Nguyên tắc HAX/PAIR

- **G11 · giải thích vì sao** → khối "Mình dựa vào" trong mỗi câu trả lời (số 3). K1: AI trích "công an", "bị bắt" để khách thấy AI phản ứng với điều gì. Khối này chỉ trích lời khách và trạng thái giao dịch, không nêu tín hiệu của hệ thống.
- **G10 · thu hẹp phạm vi khi nghi ngờ** → AI hỏi đúng một câu có sẵn lựa chọn (số 4). K1: câu hỏi sau khi khách bấm "AI hiểu sai rồi". K3 và K11 cũng đi đường này.
- **G9 · sửa dễ dàng** → nút "AI hiểu sai rồi, tôi tự chuyển cho người quen" nằm ngay dưới câu trả lời ở K1 (số 5), và nút "Hoàn tác" trên mỗi việc AI tự làm (số 6).

### Lỗi mà sửa câu chữ không cứu được

- **AI nói "mình không nhả" nhưng vẫn có quyền nhả.** Nếu AI còn gọi được `nha_giao_dich`, một tin nhắn bình tĩnh như K9 vẫn có thể khiến nó gọi. Thiết kế cứu: AI không có tool này. Phía khách không có nút nhả (số 8); chỉ màn hình tư vấn viên có, với hai bước xác nhận (số 9).
- **AI nói "mình đã chuyển tư vấn viên" lúc 23h** (K8). Câu đúng sự thật nhưng không ai nhận tới 8h. Thiết kế cứu: ngoài giờ trực, AI phải nói rõ chưa có người, giao dịch vẫn giữ, và việc khách tự làm được ngay là khoá ví.

### Lỗi còn lại

- Ngoài giờ trực, K2 không có ai duyệt nhả tới 8h sáng. Thiết kế hiện tại chỉ nói thật điều đó và gợi ý thanh toán cách khác.
- Tư vấn viên vẫn có thể nhả nhầm cho người bị lừa. Thiết kế chuyển rủi ro sang một người không bị giục và cho họ bằng chứng, chứ không xoá được rủi ro.


## §4. Mức tự chủ  (chặng 4)

*Chưa viết.* Với mỗi tool: mức · lý do theo cost-of-error · phương án đã loại. Thêm: tool khó xếp nhất · thử mức trên K1, K2 và một kịch bản bình thường (K11) · luật thêm hoặc tool tách bước · một mức tự chủ đã đổi lời hứa nào.

## §5. Prototype  (chặng 5)

*Chưa viết.* File: `prototype/index.html`. Ảnh đánh số và bảng chú thích: `prototype/README.md`.

## §6. Tốt là gì  (chặng 6)

*Chưa viết.* 3 tiêu chí Pass/Fail; chấm thử trên K1 và K2; lỗi còn lại.

## §7. Demo

*Điền sau demo.* Ca lạ nhận được: prototype đã làm gì, nhóm có đổi ý gì không.
