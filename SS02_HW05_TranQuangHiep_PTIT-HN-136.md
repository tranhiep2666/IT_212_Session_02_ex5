1. Bối cảnh và ý đồ thiết kế quy trình 2 bước

Trong hệ thống bán vé xe khách trực tuyến GreenBus, việc kiểm tra mã vé (Ticket Code) là chức năng quan trọng vì liên quan trực tiếp đến tính hợp lệ của vé và dữ liệu đặt chỗ.

Mục tiêu của quy trình 2 bước:

Bước 1 (Code Generation): Sử dụng AI để sinh mã nguồn Java cho lớp TicketValidator.
Bước 2 (Independent Auditing): Sử dụng một AI khác (hoặc cùng AI nhưng với vai trò độc lập) để kiểm toán, phản biện, tìm lỗi logic tiềm ẩn và đề xuất cải tiến.

Cách tiếp cận này giúp giảm nguy cơ:

Sai sót nghiệp vụ.
Lỗi xử lý ngày tháng.
NullPointerException.
Sai lệch do múi giờ hệ thống.
Lỗi regex hoặc phân tích chuỗi.
Các lỗi biên (edge cases) khó phát hiện khi chỉ sinh mã nguồn một lần.

Bạn là một Senior Java Backend Developer.

Hãy xây dựng lớp Java tên là TicketValidator dùng để kiểm tra tính hợp lệ của mã vé xe khách trong hệ thống GreenBus.

Yêu cầu nghiệp vụ:

1. Mã vé phải có định dạng:

BUS-XX-YYMMDD

Trong đó:

* BUS- là tiền tố cố định.
* XX là mã tỉnh/thành phố gồm đúng 2 ký tự chữ in hoa A-Z.
* YYMMDD là ngày đi xe gồm đúng 6 chữ số theo định dạng năm-tháng-ngày.

Ví dụ:
BUS-HN-260615
BUS-SG-261231

2. Điều kiện hợp lệ:

* Ticket code không được null.
* Ticket code không được rỗng hoặc chỉ chứa khoảng trắng.
* Phải đúng định dạng tổng thể.
* Mã tỉnh phải gồm đúng 2 ký tự chữ in hoa.
* Ngày phải là ngày thực tế tồn tại.
  Ví dụ:

    * 260231 là không hợp lệ.
    * 261332 là không hợp lệ.
    * 250229 không hợp lệ nếu năm đó không phải năm nhuận.
* Ngày đi xe phải lớn hơn hoặc bằng ngày hiện tại của hệ thống.
* Không được chấp nhận ngày trong quá khứ.

3. Yêu cầu kỹ thuật:

* Sử dụng Java 17.
* Sử dụng thư viện java.time.
* Ưu tiên LocalDate và DateTimeFormatter.
* Không sử dụng java.util.Date hoặc Calendar.
* Tránh NullPointerException.
* Regex chỉ dùng để kiểm tra định dạng ban đầu.
* Sau khi regex khớp phải parse ngày thực sự để kiểm tra tính hợp lệ.

4. Thiết kế:

* Tạo class TicketValidator.
* Tạo phương thức:

public static boolean isValid(String ticketCode)

* Trả về true nếu hợp lệ.
* Trả về false nếu không hợp lệ.

5. Bổ sung:

* Giải thích ngắn gọn các bước xử lý.
* Cung cấp tối thiểu 10 test cases minh họa.
* Chỉ ra các edge cases đã được xử lý.
* Mã nguồn phải biên dịch được.


Bạn là một Independent Software Auditor và Security Testing Engineer.

Nhiệm vụ của bạn KHÔNG phải viết lại mã nguồn ngay lập tức.

Bạn phải đóng vai trò kiểm toán độc lập và phản biện đoạn mã Java được cung cấp.

Hãy thực hiện các bước sau:

1. Đọc toàn bộ mã nguồn.

2. Kiểm tra các nhóm lỗi sau:

A. Business Logic

* Có kiểm tra đầy đủ định dạng BUS-XX-YYMMDD hay không?
* Có kiểm tra đúng 2 ký tự chữ in hoa không?
* Có cho phép dữ liệu sai định dạng lọt qua không?

B. Null Safety

* Có khả năng phát sinh NullPointerException không?
* Có thao tác substring(), split(), charAt() trước khi kiểm tra độ dài không?

C. Date Validation

* Có xác thực ngày thực sự tồn tại không?
* Có chấp nhận ngày 32/13 hoặc 29/02 sai năm nhuận không?
* Có sử dụng parser ở chế độ strict hay không?
* Có lỗi khi chuyển giao năm mới (31/12 -> 01/01) không?

D. Time & Timezone

* Việc sử dụng LocalDate.now() có phụ thuộc múi giờ hệ thống không?
* Nếu hệ thống chạy ở nhiều quốc gia thì có rủi ro gì?
* Có nên truyền Clock hoặc ZoneId từ bên ngoài không?

E. Maintainability

* Regex có quá cứng hoặc khó bảo trì không?
* Có hard-code logic gây khó mở rộng không?

F. Security & Robustness

* Chuỗi đầu vào rất dài có gây vấn đề không?
* Có exception nào chưa được xử lý?
* Có khả năng bypass validation không?

3. Với mỗi vấn đề phát hiện:

* Mô tả lỗi.
* Mức độ nghiêm trọng (Low/Medium/High).
* Ví dụ đầu vào gây lỗi.
* Cách sửa.

4. Sau khi hoàn thành kiểm toán:

* Tạo bảng tổng kết các lỗi.
* Đưa ra phiên bản mã nguồn tối ưu cuối cùng.
* Bắt buộc sử dụng java.time.LocalDate.
* Sử dụng DateTimeFormatter với ResolverStyle.STRICT.
* Giải thích vì sao phiên bản mới an toàn hơn.


Bước 1 – AI sinh mã nguồn lần đầu

User Prompt: (Prompt ở Bước 1)

AI Response:

import java.time.LocalDate;
import java.time.format.DateTimeFormatter;
import java.util.regex.Pattern;

public class TicketValidator {

    private static final Pattern PATTERN =
            Pattern.compile("^BUS-[A-Z]{2}-\\d{6}$");

    public static boolean isValid(String ticketCode) {

        if (ticketCode == null || ticketCode.trim().isEmpty()) {
            return false;
        }

        if (!PATTERN.matcher(ticketCode).matches()) {
            return false;
        }

        String datePart = ticketCode.substring(7);

        try {
            DateTimeFormatter formatter =
                    DateTimeFormatter.ofPattern("yyMMdd");

            LocalDate travelDate =
                    LocalDate.parse(datePart, formatter);

            return !travelDate.isBefore(LocalDate.now());

        } catch (Exception e) {
            return false;
        }
    }
}
Bước 2 – AI Auditor phát hiện lỗi

User Prompt: (Prompt ở Bước 2)

AI Auditor Response:

Phát hiện 1 – Sai cách xử lý năm

Mức độ: High

Mẫu:

DateTimeFormatter.ofPattern("yyMMdd");

Vấn đề:

yy sử dụng năm 2 chữ số.
Java sẽ áp dụng quy tắc century mặc định.
Có thể diễn giải năm không đúng mong muốn.
Dễ phát sinh lỗi khi hệ thống hoạt động lâu dài.
Phát hiện 2 – Không sử dụng STRICT parsing

Mức độ: High

Ví dụ:

BUS-HN-261332

Một số formatter mặc định có thể thực hiện cơ chế "smart parsing", gây hành vi không mong muốn.

Khuyến nghị:

.withResolverStyle(ResolverStyle.STRICT)
Phát hiện 3 – Phụ thuộc múi giờ hệ thống

Mức độ: Medium

LocalDate.now()

Nếu server đặt tại UTC nhưng nghiệp vụ dùng giờ Việt Nam thì có thể lệch ngày tại thời điểm giao ngày.

Khuyến nghị:

LocalDate.now(ZoneId.of("Asia/Ho_Chi_Minh"))

hoặc inject Clock.

Phát hiện 4 – Bắt Exception quá rộng

Mức độ: Low

catch (Exception e)

Nên bắt:

DateTimeParseException
Bảng tổng kết
ID	Vấn đề	Mức độ
1	Năm 2 chữ số mơ hồ	High
2	Không STRICT parsing	High
3	Phụ thuộc timezone server	Medium
4	Catch Exception quá rộng	Low
Mã nguồn Java tối ưu cuối cùng sau kiểm duyệt
import java.time.Clock;
import java.time.LocalDate;
import java.time.ZoneId;
import java.time.format.DateTimeFormatter;
import java.time.format.DateTimeParseException;
import java.time.format.ResolverStyle;
import java.util.regex.Pattern;

public final class TicketValidator {

    private static final Pattern PATTERN =
            Pattern.compile("^BUS-[A-Z]{2}-(\\d{6})$");

    private static final ZoneId BUSINESS_ZONE =
            ZoneId.of("Asia/Ho_Chi_Minh");

    private static final DateTimeFormatter DATE_FORMATTER =
            DateTimeFormatter.ofPattern("uuMMdd")
                    .withResolverStyle(ResolverStyle.STRICT);

    private TicketValidator() {
    }

    public static boolean isValid(String ticketCode) {
        return isValid(ticketCode, Clock.system(BUSINESS_ZONE));
    }

    static boolean isValid(String ticketCode, Clock clock) {

        if (ticketCode == null) {
            return false;
        }

        ticketCode = ticketCode.trim();

        if (ticketCode.isEmpty()) {
            return false;
        }

        var matcher = PATTERN.matcher(ticketCode);

        if (!matcher.matches()) {
            return false;
        }

        String datePart = matcher.group(1);

        try {

            LocalDate travelDate =
                    LocalDate.parse(datePart, DATE_FORMATTER);

            LocalDate today = LocalDate.now(clock);

            return !travelDate.isBefore(today);

        } catch (DateTimeParseException ex) {
            return false;
        }
    }
}