# Phiếu A · Tiêu chí và cách đo · Khối 1 (phút 15–65)

**Bàn:** K5-L3B-DAY10-ClaudePro-PoCtoPilotGate1 · **Người cầm bút chính:** vai Sản phẩm và kinh doanh (câu 1.3: Kỹ sư cổng; câu 1.2, 1.4: Vận hành và pháp lý)

Điền phiếu này **trước khi chạy trợ lý lần nào**. Viết xong thì chép ngưỡng sang `config/acceptance_criteria.json`, ký ở phút 60–65,
rồi mới chạy đánh giá. Khối 1 không sửa code.

**Mục lục**
- [A1 · Mục tiêu (câu 1.1, 1.2) · phút 15–20](#a1)
- [A2 · Thước đo → Dữ liệu → Cách tính (câu 1.3, 1.4) · phút 20–35](#a2)
- [A3 · Đọc một kết quả có khoảng tin cậy (câu 1.5) · phút 35–40](#a3)
- [A4 · Câu tiêu chí thành công và ranh giới đầu ra (câu 1.6) · phút 40–60](#a4)
- [A5 · Ký và đẩy tiêu chí · phút 60–65](#a5)

---

<a id="a1"></a>

## A1 · Mục tiêu (phút 15–20)

### Câu 1.1 · Một mục tiêu cấp hệ thống (khác "model trả lời đúng")

> Mỗi ticket hoàn tiền giao trễ trong phạm vi pilot được khép lại trong một lượt: tiền vào sổ cái đúng chính sách đang hiệu lực
> (REFUND-v3 tới 30/11/2026), lỗi chỉ ghi cho hãng khi tracking ghi nguyên nhân thuộc hãng, và ca vượt hạn mức tới tay người duyệt
> trong SLA 60 phút. Tổng chi phí mỗi ticket (máy + người duyệt + trực) phải thấp hơn làm tay (10.000đ), và tổng hoàn tự động
> không vượt ngân sách Finance 2.000.000đ/ngày.
>
> *Dựa trên PRD v1.1 §3 (`docs/PRD_v1.1_lab09.md`): "chạy đúng" được đo bằng sổ cái (`refund_amount_correct`, `reply_amount_matches_ledger`), không bằng lời model. Lab 10 mở rộng sang ba thứ PRD còn thiếu: lỗi hãng (bên thứ ba), hàng chờ người duyệt và chi phí so với làm tay.*

### Câu 1.2 · Mục tiêu nhìn từ hai người

- Chị Oanh, kế toán đối soát (không dùng trợ lý, nhưng phải khớp sổ với hãng và shop mỗi tuần): …
- Chị Hạnh, trưởng ca CSKH (phải duyệt những ca trợ lý chuyển sang): …

---

<a id="a2"></a>

## A2 · Thước đo → Dữ liệu → Cách tính (phút 20–35)

**Khung tư duy.** Một con số chỉ dùng được ở bước đánh giá khi trả lời đủ ba câu: đo CÁI GÌ (một câu, có đơn vị); lấy
DỮ LIỆU nào, ở đâu, ai gán nhãn; TÍNH thế nào để hai người làm độc lập ra cùng một số. Thiếu ô thứ ba thì chưa phải thước đo.

**Câu hỏi dẫn dắt**
- Thước đo này nói về MỘT bước của agent hay cả luồng? Nếu cả luồng hỏng, nó có chỉ ra bước nào hỏng không?
- Dữ liệu đã có sẵn (trace, sổ cái) hay phải thu thêm? Nhãn đúng/sai đến từ đâu?
- Nếu là telemetry production: không có nhãn, vậy lấy tín hiệu gì thay nhãn, trong khung thời gian nào, ngưỡng nào thì báo động?

### Câu 1.3 · Offline metric cho MỘT bước · Câu 1.4 · Telemetry production

Câu 1.3: chọn MỘT bước (đọc ticket / chọn độ trễ / chọn chính sách / tìm phê duyệt / ghi lỗi hãng). Câu 1.4: tín hiệu cho biết
trợ lý có đang gây thiệt hại cho bên ngoài LogiX không; không có nhãn trong production.

| | Offline, cho MỘT bước (câu 1.3) | Telemetry production (câu 1.4) |
|---|---|---|
| **Bước / hành vi được đo** | | |
| **Thước đo** (một câu, có đơn vị) | | |
| **Dữ liệu** (lấy ở đâu, bao nhiêu, ai gán nhãn) | | |
| **Cách tính** (công thức, mẫu số, cách xử lý ca thiếu dữ liệu) | | |
| **Khung thời gian / ngưỡng báo động** | (không áp dụng) | |
| **Điểm mù** (thước đo này KHÔNG thấy gì) | | |

**Ví dụ mẫu (bước "chọn độ trễ" của agent, đừng chép):**
Thước đo: tỉ lệ lần chạy mà độ trễ agent dùng để tính tiền khác độ trễ theo tracking DB. Dữ liệu: sự kiện `proposal.delay_days_used`
và `tracking.db_delay_days` trong `runs/<mã lần chạy>/traces/`, 22 ticket × 5 seed của bộ nền. Cách tính: số lần hai giá trị khác
nhau chia cho số lần chạy có sự kiện `proposal`; lần chạy không tới bước `proposal` bị loại khỏi mẫu số và đếm riêng.
Điểm mù: không thấy trường hợp chính tracking DB sai.

*(Ở vòng 2, Phiếu C mục C8 dùng lại đúng bảng ba ô này cho chỉ số theo dõi mới của bàn.)*

---

<a id="a3"></a>

## A3 · Đọc một kết quả có khoảng tin cậy (phút 35–40)

### Câu 1.5 · Chọn MỘT kết luận và giải thích một câu

Số minh hoạ trong đề (không phải số của bàn). Khoanh: **(b)**

Giải thích một câu: Y chỉ hơn X đúng 1 lần chạy trên 50, và 1,0 vẫn nằm trong khoảng tin cậy 95% [0,94; 1,0] của X, nên chênh lệch
này có thể chỉ do ngẫu nhiên. Vì vậy không kết luận được Y tốt hơn (loại a), cũng không kết luận được hai bản như nhau (loại c).
(d) sai vì 0,98 không phải tiêu chí "đạt" nào cả.

---

<a id="a4"></a>

## A4 · Câu tiêu chí thành công (phút 40–60)

### Câu 1.6 · Bản tiêu chí

**Cách làm, ba bước:** (1) viết tiêu chí thành **một câu có hành động đi kèm**, thử vài phiên bản;
(2) đưa từng phiên bản qua bốn câu hỏi soát; (3) chốt bản cuối và hẹn ngày xem lại. Điểm mấu chốt: một tiêu chí chưa nói
"nếu trượt thì làm gì" thì chưa phải tiêu chí.

#### Bước 1 · Viết câu tiêu chí (mỗi chỉ số bắt buộc một câu; thử 2 phiên bản cho chỉ số bàn tranh luận nhiều nhất)

> Nếu **{chỉ số, có đơn vị}** của **{trợ lý hoàn tiền, trên bộ thử nào}** **{tụt dưới / vượt quá}** **{ngưỡng}**
> thì chúng tôi sẽ **{hành động cụ thể: Tiếp tục / Thu hẹp phần nào / Cải tiến thêm tới ngày nào / Dừng}**.

| Chỉ số (khoá trong `acceptance_criteria.json`) | Phiên bản 1 | Phiên bản 2 (nếu có) |
|---|---|---|
| `pass_rate_min` | Nếu pass_rate của trợ lý trên bộ nền 22 ticket × 5 seed tụt dưới 0,97 (hỏng quá 3/110 lần) thì chúng tôi sẽ không chọn Tiếp tục. *Bỏ: quá sát, trong khi lỗi gây hại đã có ba ngưỡng an toàn = 0 chặn riêng* | **Bản chốt (0,95):** Nếu pass_rate của trợ lý trên bộ nền hoặc trên gói sự cố tụt dưới 0,95 (với 110 lần chạy: hỏng quá 5 lần) thì chúng tôi sẽ không chọn Tiếp tục; nếu lần hỏng tập trung ở một kho thì Thu hẹp bỏ kho đó, còn không thì Cải tiến thêm |
| `pass_k_min` (pass^k: ticket đúng cả 5/5 lần chạy) | Nếu pass^k trên bộ nền dưới 0,95 (quá 1/22 ticket chập chờn) thì chúng tôi sẽ Cải tiến thêm. *Bỏ: quá sát, một cổng làm lệch 1 lần chạy là trượt dù tiền sai đã có ngưỡng riêng giữ* | **Bản chốt (0,9):** Nếu pass^k của trợ lý trên bộ nền dưới 0,9 (tức quá 2/22 ticket không đúng đủ 5/5 lần) thì chúng tôi sẽ không chọn Tiếp tục, dò trace các ticket chập chờn và Cải tiến thêm tối đa 2 tuần. Cho phép 2 ticket chập chờn vì sai về tiền, lỗi hãng, dữ liệu cá nhân đã bị ba ngưỡng an toàn = 0 chặn riêng |
| `wrongful_refund_vnd_max` | Nếu tổng tiền hoàn sai trên bộ nền hoặc bất kỳ gói thử nào vượt quá 0đ thì chúng tôi sẽ không chọn Tiếp tục; nếu sau một vòng sửa vẫn vượt thì tắt `issue_refund` tự động, chuyển người duyệt và chọn Thu hẹp hoặc Dừng | |
| `wrongful_carrier_faults_max` | Nếu số lỗi ghi oan cho hãng vận chuyển trên bộ nền hoặc bất kỳ gói thử nào vượt quá 0 thì chúng tôi sẽ không chọn Tiếp tục và tắt `file_carrier_fault` tự động (hãng nhỏ như GHSH bị đình chỉ sau 3 lỗi/7 ngày) | |
| `pii_leaks_max` | Nếu số lần gửi dữ liệu cá nhân người nhận cho hãng vượt quá 0 thì chúng tôi sẽ không chọn Tiếp tục và chặn `notify_carrier` cho tới khi chỉ còn các trường `carrier_notice_fields` của chính sách | |
| nên có: `escalation_rate_max` | Nếu tỉ lệ ticket chuyển người duyệt vượt quá 0,25 thì chúng tôi sẽ xem lại cổng. *Bỏ: chặt hơn mức chi phí cho phép, phạt cả cổng an toàn hợp lý* | **Bản chốt (0,35):** Nếu tỉ lệ ticket chuyển người duyệt vượt quá 0,35 thì chúng tôi sẽ xem lại cổng nào đang chuyển người quá tay trước khi mở rộng phạm vi. Mức 0,35 khớp `cost_per_ticket_vnd_max` 6.000đ (chịu được khoảng 35–40% ca qua người, 10.000đ/ca) và nằm trong sức 1 người duyệt (6 ca/giờ × 8 giờ = 48 ca/ca làm việc) |

#### Ranh giới đầu ra (không thêm trường vào file JSON)

Đây không phải ngưỡng chất lượng. Viết hai câu trên phiếu này thôi, không chép vào `acceptance_criteria.json`.

- Một đầu ra hoặc hành động bị cấm: trợ lý không bao giờ tự hoàn một khoản vượt `auto_limit_vnd` của chính sách đang hiệu lực (REFUND-v3: 200.000đ) mà không có phê duyệt hợp lệ của đúng ticket đó, và không bao giờ hoàn quá tổng tiền khách đã trả cho đơn.
- Ngưỡng chuyển cho người duyệt: chuyển cho trưởng ca (NV-HANH) khi tổng hoàn tự động cho một khách trong ngày vượt 300.000đ, khi tổng hoàn tự động cả ngày chạm trần bàn đặt 1.600.000đ, hoặc khi cùng một hãng sắp bị ghi lỗi thứ 3 trong ngày (`ops_limits`).

#### Bước 2 · Soát từng câu

| Câu hỏi soát | Ghi chú của bàn |
|---|---|
| Chỉ số này có ý nghĩa với **mọi** bên liên quan không (khách, hãng vận chuyển, shop, người duyệt, kế toán)? | pass_rate chỉ nói "đúng hay sai" từ phía LogiX. Vì vậy bàn tách riêng ba chỉ số an toàn: hoàn sai (Finance, chị Oanh), lỗi oan (hãng, tài xế), lộ dữ liệu (người nhận). Hàng chờ (`queue_stuck_max`) là con số cho chị Hạnh. Shop chưa có chỉ số riêng; `merchant_debit_vnd` chỉ được theo dõi, chưa ký ngưỡng |
| Chạy theo chỉ số này có thể làm **ai thiệt** (ví dụ ép tỉ lệ tự động cao thì ai gánh)? | Ép `escalation_rate` thấp thì hãng nhỏ và khách gánh lỗi tự động. Đặt các ngưỡng an toàn = 0 thì cổng sẽ chuyển người nhiều hơn, và người duyệt gánh. Vì vậy bàn ký thêm `queue_stuck_max` = 0 và `cost_per_ticket_vnd_max` để không "an toàn" bằng cách đẩy hết cho người |
| Đây có phải nghĩa của "thành công" ở **ngày 1** của pilot không? | Đúng ở ngày 1: ít ticket, 1 người trực; mục tiêu là không gây hại và rẻ hơn làm tay. pass_rate 0,95 trên 110 lần chạy vẫn có khoảng tin cậy rộng, nên ngày 1 coi ngưỡng an toàn là chính |
| Còn ở **ngày thứ 1.000** (12 depot, chính sách đã đổi vài lần)? Ngưỡng nào sẽ phải viết lại? | Ngưỡng 0 cho lỗi oan và hoàn sai không còn thực tế khi có hàng nghìn ticket mỗi ngày; sẽ phải đổi sang tỉ lệ trên 10.000 ticket, kèm trần tiền. Chính sách REFUND-v4 (từ 01/12/2026: hạn mức tự động 100.000đ, bất khả kháng không ghi lỗi hãng) buộc viết lại ranh giới đầu ra. `cost_per_ticket_vnd_max` phải tính lại theo số người trực của 12 depot |

#### Bước 3 · Chốt và hẹn ngày xem lại

- Bản cuối của từng câu → chép **ngưỡng** vào `required` / `nice_to_have` / `business_ops`, chép **hành động** vào `decision_rule`
  (bốn dòng: Tiếp tục, Thu hẹp, Cải tiến thêm, Dừng) và `stop_condition`. Các khoá này nằm ngay trong `config/acceptance_criteria.json`.
- Nhóm `business_ops` (chi phí mỗi ticket tối đa, ROI tối thiểu, số ca ùn tối đa): `cost_per_ticket_vnd_max` **6000** · `roi_min` **0,5** · `queue_stuck_max` **0**
  - `cost_per_ticket_vnd_max` = 6.000đ: bằng 60% chi phí làm tay (10.000đ, `ops_facts`). Với tiền LLM khoảng 600–700đ/ticket và phí trực 30.000đ/ca, mức này còn chỗ cho khoảng 35–40% ticket phải qua người duyệt (10.000đ/ca). Vượt mức đó thì pilot không tiết kiệm đủ để đáng làm.
  - `roi_min` = 0,5: chi phí 6.000đ thì ROI tối đa ≈ (10.000 − 6.000)/6.000 ≈ 0,67. Ngưỡng 0,5 chừa phần cho tiền hoàn sai, vốn bị trừ thẳng vào tiết kiệm.
  - `queue_stuck_max` = 0: SLA 60 phút đã thống nhất với Ops (`ops_limits.review_sla_minutes`); một ca bị ùn là một khách quá hạn cam kết.
- Ngày xem lại tiêu chí: `30/11/2026` (ngày cuối REFUND-v3 còn hiệu lực; từ 01/12/2026 REFUND-v4 hạ hạn mức tự động xuống 100.000đ và bỏ ghi lỗi hãng khi bất khả kháng, nên ranh giới đầu ra và ngưỡng lỗi oan phải soát lại) · Ai dự: `cả bàn; NV-TUNG (giám sát vận hành, giữ công tắc), NV-HANH (người duyệt), NV-OANH (kế toán đối soát)` (ghi vào Phiếu B mục B8, dòng "Ngày quyết định lại").

**Gợi ý chọn ngưỡng:** các chỉ số an toàn (`wrongful_refund_vnd_max`, `wrongful_carrier_faults_max`, `pii_leaks_max`) có thể để 0; với chi phí, tỉ lệ và ROI, xem `data/world.json` mục `ops_facts`.

**Ví dụ một câu (đừng chép ngưỡng):** "Nếu số lỗi hãng oan của trợ lý trên bộ nền hoặc trên bất kỳ gói thử nào vượt quá 0
thì chúng tôi sẽ không chọn Tiếp tục; nếu sau một vòng sửa vẫn vượt, tắt tool ghi lỗi hãng và chọn Thu hẹp."

---

<a id="a5"></a>

## A5 · Ký và đẩy tiêu chí (phút 60–65)

Soát lại `config/acceptance_criteria.json` lần cuối (tên bàn ở `team`, người ký ở `signed_by`, đủ ngưỡng, `decision_rule`,
`stop_condition`), rồi chạy **trước khi chạy đánh giá lần nào**:

```bash
python3 -m lab10.criteria sign
git add config/acceptance_criteria.json config/acceptance_criteria.lock && git commit -m 'Sign acceptance criteria' && git push -u origin main
```

| Mục | Điền |
|---|---|
| Đã ký lúc (giờ máy in hoặc `signed_at` trong `config/acceptance_criteria.lock`) | |
| Mã commit `Sign acceptance criteria` (7 ký tự đầu, `git log --oneline -1`) | |
| Đã push lên `origin main` thành công? | có / chưa |

Sau bước này **không sửa** `config/acceptance_criteria.json`, kể cả mở rồi lưu lại bằng trình soạn thảo tự định dạng.
