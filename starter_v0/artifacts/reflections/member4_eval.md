# Self-Reflection — Nguyễn Chí Công (Evaluation Specialist)

### Nguyễn Chí Công — 02634
- **Vai trò/phần việc được nhận:** Evaluation & Test Author (Tác giả Đánh giá & Kiểm thử). Chịu trách nhiệm thiết kế, xây dựng và xác thực bộ dữ liệu đánh giá nhóm độc lập `starter_v0/data/eval_group.json`.
- **Những gì tôi đã thay đổi trong repo chung:** Thiết kế và lập trình hoàn chỉnh tệp `starter_v0/data/eval_group.json` với đúng 10 test case gốc (5 single-turn và 5 multi-turn) bao phủ toàn diện các failure mode thực tế của IT Helpdesk; viết bản tự nhận xét chi tiết và xác thực cấu trúc bộ kiểm thử tại `starter_v0/artifacts/reflections/member4_eval.md`.
- **File hoặc artifact liên quan:** `starter_v0/data/eval_group.json`, `starter_v0/artifacts/reflections/member4_eval.md`, run kết quả `runs/v3_B_group_openai_20260914T224547113767.json`.
- **Commit hash hoặc pull request:** Commit `8732964`, commit `a6ea3f5`, Pull Request #4 (`2ed4284`).
- **Một quyết định kỹ thuật tôi đã đưa ra và lý do:** Quyết định thiết kế 5 ca multi-turn tập trung vào các tình huống biên khó: sửa đổi mã máy giữa chừng (G06), đổi dịch vụ nhưng giữ môi trường (G07), hủy yêu cầu tạo ticket (G08), gọi song song hai công cụ (G09), và cập nhật tiêu đề báo cáo sự cố (G10). Lý do: Đây là các hành vi người dùng thực tế hay gặp nhất mà các bài kiểm thử đơn lẻ không thể phát hiện được.
- **Khó khăn tôi gặp và cách tôi xử lý:** Đảm bảo đúng định dạng schema của lab để `run_eval.py` có thể chấm tự động mà không bị vấp lỗi `KeyError` hoặc sai `failure_type`. Tôi đã viết script Python nhỏ để kiểm tra tính hợp lệ của schema và đếm số lượng case trước khi tạo PR.
- **Điều tôi học được từ phần việc này:** Hiểu rõ tầm quan trọng của việc xây dựng bộ benchmark khách quan; một bộ kiểm thử tốt giúp nhóm phát hiện ra các điểm mù trong thiết kế agent mà bộ base cố định chưa bao phủ hết.
- **Nếu làm lại, tôi sẽ cải thiện điều gì:** Tôi sẽ thiết kế thêm các ca kiểm thử phức tạp 3-4 lượt hội thoại với tình huống người dùng liên tục đổi ý để kiểm tra giới hạn chịu tải ngữ cảnh của agent.

---

## 1. Tóm tắt đóng góp

Tôi được phân công vai trò **Evaluation & Test Author** trong nhóm. Trách nhiệm của tôi là thiết kế và xác thực bộ dữ liệu đánh giá nhóm cho IT Helpdesk Agent.

Trong phần đóng góp này, tôi đã tạo bộ kiểm thử nhóm gốc tại `starter_v0/data/eval_group.json` với đúng 10 test case:

- 5 test case single-turn
- 5 test case multi-turn

Các test case được thiết kế để đánh giá khả năng của mô hình trong việc:

- định tuyến đến đúng công cụ
- truyền đúng tham số
- duy trì ngữ cảnh qua nhiều lượt hội thoại
- cập nhật thông tin trước đó khi người dùng sửa lại
- yêu cầu làm rõ khi cần thiết
- từ chối các yêu cầu không liên quan đến IT
- tránh sử dụng công cụ không cần thiết
- duy trì đúng ranh giới xác nhận trước khi thực hiện các hành động ghi hoặc thay đổi dữ liệu

## 2. Mục tiêu thiết kế kiểm thử

Bộ đánh giá nhóm được thiết kế để bao phủ các dạng lỗi phổ biến nhất trong bài lab này, thay vì chỉ lặp lại các test case cố định trong bộ base.

Các chủ đề chính bao gồm:

- định tuyến dịch vụ và xử lý môi trường
- kiểm tra thiết bị với giá trị `check` chính xác
- định tuyến tới knowledge base
- yêu cầu làm rõ khi thiếu employee ID hoặc thiếu ngữ cảnh
- xử lý các yêu cầu ngoài phạm vi
- xử lý việc người dùng sửa thông tin qua nhiều lượt hội thoại
- hủy hành động sau khi trước đó đã yêu cầu thực hiện
- sử dụng nhiều công cụ song song cho các yêu cầu hỗn hợp
- xử lý yêu cầu chỉ định dạng nội dung mà không gọi lại dữ liệu một cách không cần thiết

## 3. Các file liên quan

- `starter_v0/data/eval_group.json`
- `starter_v0/artifacts/reflections/member4_eval.md`

## 4. Xác thực và bằng chứng

Tôi đã xác thực cấu trúc JSON và kiểm tra số lượng test case theo từng loại:

- tổng số test case: 10
- single-turn: 5
- multi-turn: 5

Lệnh xác thực được sử dụng:

```powershell
cd "C:\Users\Chi Cong\K4A-Day04-GooseGooseDuck\starter_v0"

python -c "import json; d=json.load(open(r'data\eval_group.json', encoding='utf-8')); print('total=', len(d['cases'])); print('single=', sum('query' in c for c in d['cases'])); print('multi=', sum('turns' in c for c in d['cases']))"
```

Kết quả quan sát được:

```text
total= 10
single= 5
multi= 5
```

Điều này xác nhận rằng bộ dữ liệu đáp ứng đúng định dạng và số lượng yêu cầu của group suite.

## 5. Bài học rút ra

Vai trò này rất quan trọng vì một bộ đánh giá tốt không chỉ kiểm tra xem mô hình có trả lời được câu hỏi hay không. Nó còn xác minh xem agent có hành xử đúng trong các tình huống helpdesk thực tế hay không, chẳng hạn như:

- thay đổi asset ID giữa cuộc hội thoại
- thay đổi service nhưng vẫn giữ đúng ngữ cảnh environment
- hủy một hành động trước đó khi người dùng rút lại yêu cầu
- lựa chọn nhiều hơn một công cụ khi yêu cầu cần nhiều nguồn bằng chứng
- ưu tiên chỉ dẫn mới nhất của người dùng so với các thông tin ở những lượt trước

Đây chính là những trường hợp biên giúp phát hiện liệu agent có thực sự hiểu cách định tuyến công cụ và duy trì ngữ cảnh hội thoại hay không.

## 6. Ghi chú cuối cùng

Với vai trò **Evaluation & Test Author**, tôi đã đóng góp bộ kiểm thử nhóm gốc, giúp nhóm đánh giá xem helpdesk agent có đang được cải thiện theo cách có thể đo lường và tái lập hay không. Bộ dữ liệu này hữu ích trong việc xác thực độ chính xác của định tuyến công cụ, tính đúng đắn của tham số, khả năng xử lý hội thoại nhiều lượt và việc tuân thủ các ranh giới hành động trước khi dự án được hoàn thiện.
