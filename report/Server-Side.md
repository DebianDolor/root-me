# 1. SQL Injetion

## Tổng quan

- SQL injection (SQLi) là một lỗ hổng bảo mật web cho phép kẻ tấn công can thiệp vào các queries mà ứng dụng thực hiện với cơ sở dữ liệu của nó.

- Nó cho phép kẻ tấn công xem dữ liệu mà thường không thể truy xuất được như dữ liệu thuộc về người dùng khác hoặc bất kỳ dữ liệu nhạy cảm khác. 

- SQL injection có thể dẫn đến truy cập trái phép vào dữ liệu nhạy cảm, chẳng hạn như mật khẩu, thẻ tín dụng hoặc thông tin người dùng.

- Trong một số trường hợp, kẻ tấn công có thể chiếm được 1 backdoor trong hệ thống của tổ chức, dẫn đến một sự xâm phạm lâu dài.

## SQL injection

Có rất nhiều lỗ hổng SQL injection, kỹ thuật trong các tình huống khác nhau. Một số ví dụ SQL injection phổ biến bao gồm:

- `Truy xuất dữ liệu ẩn`: Sửa đổi truy vấn SQL để trả về kết quả bị ẩn.

- `Thay đổi logic của ứng dụng`:Thay đổi một truy vấn để can thiệp vào logic của ứng dụng.

- `UNION`: Truy xuất dữ liệu từ các bảng cơ sở dữ liệu khác nhau.

- `Kiểm tra cơ sở dữ liệu`: Trích xuất thông tin về phiên bản và cấu trúc của cơ sở dữ liệu.

- `Blind SQL Injection`: Kết quả của truy vấn không được trả về trong response.

## Nhận biết SQLi

- Nhận diện tự động sử dụng `Burp Suite's web vulnerablity scanner`.

- Thủ công: 
    
    - Thử các kí tự `'`.
    - Thử các cú pháp của SQL và đánh giá phản hồi.
    - Thử các điều kiện Boolean như `OR 1=1`, và đợi phản hồi.
    - Gửi payload được thiết kế để trigger time delay và tìm kiếm sự khác biệt về thời gian cần thiết để phản hồi.

- Có thể tra cheatsheet ở đây: https://portswigger.net/web-security/sql-injection/cheat-sheet

## Ngăn chặn SQLi

- Hầu hết SQLi có thể được ngăn chặn bằng cách sử dụng truy vấn được tham số hóa (còn được gọi là câu lệnh đã chuẩn bị) thay vì nối chuỗi trong truy vấn.

- Truy vấn được tham số hóa có thể được sử dụng cho bất kỳ tình huống nào mà đầu vào không đáng tin cậy xuất hiện dưới dạng dữ liệu trong truy vấn, bao gồm mệnh đề WHERE và các giá trị trong câu lệnh INSERT hoặc UPDATE. 

- Chúng không thể được sử dụng để xử lý đầu vào không đáng tin cậy trong các phần khác của truy vấn, chẳng hạn như tên bảng hoặc cột hoặc mệnh đề ORDER BY.

- Để một truy vấn được tham số hóa có hiệu quả, chuỗi được sử dụng trong truy vấn phải luôn là một hằng số được gán cứng và không bao giờ được chứa bất kỳ dữ liệu biến đổi nào từ bất kỳ nguồn nào.

# 2. Authentication

## Tổng quan

- Authentication là quá trình xác minh danh tính của một người dùng. Nói cách khác, nó liên quan đến việc đảm bảo rằng họ thực sự là người mà họ tuyên bố. Do đó, các cơ chế xác thực mạnh mẽ là một khía cạnh không thể thiếu của bảo mật web hiệu quả.

- Có ba yếu tố xác thực mà các loại xác thực khác nhau có thể được phân loại:
    - knowledge factors
    - possession factors
    - inherence factors

- Authentication VS Authorization:

    - Authentication là quá trình xác minh rằng người dùng thực sự là người mà họ tuyên bố, trong khi Authorization liên quan đến việc xác minh xem người dùng có được phép làm điều gì đó hay không.

## Authentication vulnerabilities

- Nói chung, hầu hết các lỗ hổng trong cơ chế xác thực phát sinh theo một trong hai cách:
    - Các cơ chế xác thực yếu vì chúng không bảo vệ đầy đủ trước brute-force.
    - Lỗi logic hoặc mã hóa kém trong quá trình triển khai cho phép kẻ tấn công bypass các cơ chế xác thực.

## Hậu quả

- Sau khi kẻ tấn công bypass được xác thực hoặc xâm nhập vào tài khoản của người dùng khác, kẻ tấn công sẽ có quyền truy cập vào tất cả dữ liệu và chức năng mà tài khoản này có.

- Nếu attacker có thể xâm phạm tài khoản có đặc quyền cao, chẳng hạn như quản trị viên hệ thống, thì họ có thể có toàn quyền kiểm soát toàn bộ ứng dụng và có khả năng có quyền truy cập vào cơ sở hạ tầng nội bộ.

# 3. Directory traversal

## Tổng quan

- Directory traversal là một lỗ hổng bảo mật web cho phép kẻ tấn công đọc các tệp tùy ý trên máy chủ đang chạy ứng dụng. Điều này có thể bao gồm mã ứng dụng và dữ liệu, thông tin đăng nhập cho hệ thống phụ trợ và các tệp hệ điều hành nhạy cảm.

- Trong một số trường hợp, kẻ tấn công có thể ghi vào các tệp tùy ý trên máy chủ, cho phép chúng sửa đổi dữ liệu hoặc hành vi của ứng dụng và cuối cùng kiểm soát hoàn toàn máy chủ.

## Exploiting file path traversal vulnerabilities

- Nhiều ứng dụng đặt input của người dùng vào đường dẫn tệp thực hiện một số loại bảo vệ chống lại các cuộc tấn công truyền tải đường dẫn và chúng thường có thể bị bypass.

- Nếu một ứng dụng filter Directory traversal do người dùng cung cấp, thì có thể vượt qua lớp bảo vệ bằng nhiều kỹ thuật khác nhau. Tham khảo: (https://portswigger.net/web-security/file-path-traversal)

## Ngăn chặn 

- Cách hiệu quả nhất để ngăn chặn là tránh chuyển hoàn toàn thông tin đầu vào do người dùng cung cấp tới các API. Nhiều function thực hiện điều này có thể được viết lại để cung cấp hành vi tương tự theo cách an toàn hơn.

- Nếu việc trên được coi là không thể tránh khỏi, thì hai lớp bảo vệ nên được sử dụng cùng nhau để ngăn chặn các cuộc tấn công:
    - Ứng dụng phải xác thực input của người dùng trước khi xử lý. Lý tưởng nhất là việc xác thực phải so sánh với whitelist của các giá trị được phép.
    - Sau khi xác thực đầu vào, ứng dụng sẽ nối đầu vào vào thư mục cơ sở và sử dụng API để chuẩn hóa đường dẫn. Nó sẽ xác minh rằng đường dẫn được chuẩn hóa bắt đầu với base directory dự kiến.

# 4. OS command injection

## Tổng quan

- OS command injection là một lỗ hổng bảo mật web cho phép kẻ tấn công thực thi các lệnh hệ điều hành (OS) tùy ý trên máy chủ đang chạy ứng dụng và thường xâm phạm hoàn toàn ứng dụng cũng như tất cả dữ liệu của nó.

- Thông thường, kẻ tấn công có thể tận dụng lỗ hổng chèn lệnh của hệ điều hành để xâm phạm các phần khác của cơ sở hạ tầng lưu trữ, khai thác các mối quan hệ tin cậy để chuyển cuộc tấn công sang các hệ thống khác trong tổ chức.

## Useful commands

Purpose of command	    Linux	        Windows
Name of current user	whoami	        whoami
Operating system	    uname -a	    ver
Network configuration	ifconfig	    ipconfig /all
Network connections	    netstat -an	    netstat -an
Running processes	    ps -ef	        tasklist

## Inject OS command

- Một loạt các shell metacharacters có thể được sử dụng để thực hiện OS command injection. (&, &&, |, ||,...)

- Đôi khi, input mà ta kiểm soát xuất hiện trong dấu ngoặc kép trong lệnh ban đầu. Trong tình huống này, ta cần chấm dứt ngữ cảnh được trích dẫn (sử dụng " hoặc ') trước khi sử dụng các siêu ký tự shell phù hợp để đưa vào một lệnh mới.

## Ngăn chặn

- Cho đến nay, cách hiệu quả nhất để ngăn chặn OS command injection là không bao giờ gọi các lệnh của hệ điều hành từ ứng dụng. Trong hầu hết mọi trường hợp, có nhiều cách thay thế để triển khai chức năng được yêu cầu bằng cách sử dụng API nền tảng an toàn hơn.

- Nếu việc trên được coi là không thể tránh khỏi, thì phải thực hiện xác thực đầu vào mạnh mẽ. Một số ví dụ về xác nhận hiệu quả bao gồm:
    - Xác thực đối với whitelist các giá trị được phép.
    - Xác thực rằng đầu vào là một số.
    - Xác thực rằng đầu vào chỉ chứa các ký tự chữ và số, không có cú pháp hoặc khoảng trắng nào khác.

# 5. Business logic vulnerabilities

## Tổng quan

- Business logic vulnerabilities là những sai sót trong thiết kế và triển khai ứng dụng cho phép kẻ tấn công thực hiện hành vi ngoài ý muốn. Điều này có khả năng cho phép kẻ tấn công thao túng chức năng để đạt được mục tiêu độc hại. Những lỗi này thường là kết quả của việc không lường trước được các trạng thái ứng dụng bất thường có thể xảy ra và do đó, không thể xử lý chúng một cách an toàn.

- Một trong những mục đích chính của logic nghiệp vụ là thực thi các quy tắc và ràng buộc đã được xác định khi thiết kế ứng dụng hoặc chức năng. Nói chung, các quy tắc kinh doanh chỉ ra cách ứng dụng sẽ phản ứng khi một kịch bản nhất định xảy ra. 

- Các lỗ hổng trong logic có thể cho phép kẻ tấn công phá vỡ các quy tắc này. Ví dụ: họ có thể hoàn thành giao dịch mà không cần trải qua quy trình mua hàng dự định. Trong các trường hợp khác, việc xác thực dữ liệu do người dùng cung cấp bị hỏng hoặc không tồn tại có thể cho phép người dùng thực hiện các thay đổi tùy ý đối với các giá trị quan trọng trong giao dịch hoặc gửi thông tin đầu vào vô nghĩa.

## Hậu quả

- Đôi khi, tác động của các lỗ hổng logic kinh doanh có thể khá tầm thường. Nó là một danh mục rộng và tác động rất khác nhau. Tuy nhiên, bất kỳ hành vi ngoài ý muốn nào cũng có khả năng dẫn đến các cuộc tấn công nghiêm trọng nếu kẻ tấn công có thể thao túng ứng dụng theo đúng cách.

- Logic sai lầm trong các giao dịch tài chính rõ ràng có thể dẫn đến tổn thất lớn cho doanh nghiệp do tiền bị đánh cắp, gian lận, v.v.

- Cũng nên lưu ý rằng mặc dù các lỗi logic có thể không cho phép kẻ tấn công hưởng lợi trực tiếp, nhưng chúng vẫn có thể cho phép một bên có ý đồ xấu gây thiệt hại cho doanh nghiệp theo một cách nào đó.

## Ngăn chặn

- Đảm bảo các nhà phát triển và người thử nghiệm hiểu miền mà ứng dụng phục vụ.

- Tránh đưa ra các giả định ngầm định về hành vi của người dùng hoặc hành vi của các phần khác của ứng dụng.

- Điều quan trọng nữa là đảm bảo rằng cả nhà phát triển và người thử nghiệm đều có thể hiểu đầy đủ các giả định này và cách ứng dụng được cho là sẽ phản ứng trong các tình huống khác nhau. Điều này có thể giúp nhóm phát hiện ra các lỗi logic càng sớm càng tốt.

# 6. Information disclosure

## Tổng quan

- Tiết lộ thông tin, còn được gọi là rò rỉ thông tin, là khi một trang web vô tình tiết lộ thông tin nhạy cảm cho người dùng. Tùy thuộc vào ngữ cảnh, các trang web có thể rò rỉ tất cả các loại thông tin cho kẻ tấn công tiềm năng, bao gồm:
    - Dữ liệu về những người dùng khác, chẳng hạn như tên người dùng hoặc thông tin tài chính.
    - Dữ liệu thương mại hoặc kinh doanh nhạy cảm.
    - Chi tiết kỹ thuật về trang web và cơ sở hạ tầng của nó.

- Đôi khi, thông tin nhạy cảm có thể bị rò rỉ một cách bất cẩn cho những người dùng chỉ duyệt trang web theo cách thông thường. Tuy nhiên, thông thường hơn, kẻ tấn công cần khơi gợi việc tiết lộ thông tin bằng cách tương tác với trang web theo những cách không mong muốn.

## Hậu quả

- Các lỗ hổng tiết lộ thông tin có thể có cả tác động trực tiếp và gián tiếp tùy thuộc vào mục đích của trang web và do đó, kẻ tấn công có thể lấy được thông tin gì. Trong một số trường hợp, chỉ hành động tiết lộ thông tin nhạy cảm thôi cũng có thể gây tác động lớn đến các bên bị ảnh hưởng.

- Mặt khác, thông tin kỹ thuật bị rò rỉ, chẳng hạn như cấu trúc thư mục hoặc khung bên thứ ba nào đang được sử dụng, có thể có ít hoặc không có tác động trực tiếp. Tuy nhiên, nếu rơi vào tay kẻ xấu, đây có thể là thông tin quan trọng cần thiết để xây dựng bất kỳ số lượng khai thác nào khác.

## Ngăn chặn

- Đảm bảo rằng mọi người tham gia sản xuất trang web đều nhận thức đầy đủ về thông tin nào được coi là nhạy cảm. Đôi khi thông tin dường như vô hại có thể hữu ích hơn nhiều đối với kẻ tấn công.

- Kiểm tra bất kỳ mã nào để tiết lộ thông tin tiềm ẩn như một phần của quy trình xây dựng hoặc đảm bảo chất lượng của bạn. Sẽ tương đối dễ dàng để tự động hóa một số tác vụ liên quan, chẳng hạn như loại bỏ nhận xét của nhà phát triển.

- Sử dụng các thông báo lỗi chung càng nhiều càng tốt. Không cung cấp cho kẻ tấn công manh mối về hành vi của ứng dụng một cách không cần thiết.

- Đảm bảo rằng bạn hiểu đầy đủ các cài đặt cấu hình và ý nghĩa bảo mật của bất kỳ công nghệ bên thứ ba nào mà bạn triển khai. Dành thời gian để điều tra và vô hiệu hóa bất kỳ tính năng và cài đặt nào mà bạn không thực sự cần.

# 7. Access control vulnerabilities and privilege escalation

## Tổng quan

- Kiểm soát truy cập (hoặc ủy quyền) là việc áp dụng các ràng buộc về việc ai (hoặc cái gì) có thể thực hiện các hành động đã cố gắng hoặc truy cập tài nguyên mà họ đã yêu cầu. Trong ngữ cảnh của các ứng dụng web, kiểm soát truy cập phụ thuộc vào xác thực và quản lý phiên:
    - Authentication
    - Session Management
    - Access Control

- Kiểm soát truy cập bị hỏng là một lỗ hổng bảo mật thường gặp và thường nghiêm trọng. Thiết kế và quản lý các điều khiển truy cập là một vấn đề phức tạp, áp dụng các ràng buộc về kinh doanh, tổ chức và pháp lý đối với việc triển khai kỹ thuật.

## Ngăn chặn

- Không bao giờ chỉ dựa vào che giấu để kiểm soát truy cập. 

- Trừ khi một tài nguyên được thiết kế để truy cập công khai, từ chối truy cập theo mặc định. 

- Bất cứ khi nào có thể, hãy sử dụng một cơ chế duy nhất trên toàn ứng dụng để thực thi các biện pháp kiểm soát truy cập.

- Kiểm tra kỹ lưỡng và kiểm tra các biện pháp kiểm soát truy cập để đảm bảo chúng hoạt động như thiết kế.

# 8. File upload vulnerabilities

## Tổng quan

- Lỗ hổng tải lên tệp là khi máy chủ web cho phép người dùng tải tệp lên hệ thống tệp của nó mà không xác thực đầy đủ những thứ như tên, loại, nội dung hoặc kích thước của chúng.

- Việc không thực thi đúng các hạn chế đối với những điều này có thể có nghĩa là ngay cả chức năng tải lên hình ảnh cơ bản cũng có thể được sử dụng để tải lên các tệp tùy ý và có khả năng gây nguy hiểm. Điều này thậm chí có thể bao gồm các tệp tập lệnh phía máy chủ cho phép thực thi mã từ xa.

- Trong một số trường hợp, bản thân hành động tải tệp lên đã đủ gây ra thiệt hại. Các cuộc tấn công khác có thể liên quan đến yêu cầu HTTP tiếp theo đối với tệp, thường là để kích hoạt quá trình thực thi tệp của máy chủ.

## Hậu quả

- Trong trường hợp xấu nhất, loại tệp không được xác thực hợp lệ và cấu hình máy chủ cho phép một số loại tệp nhất định (chẳng hạn như .php và .jsp) được thực thi dưới dạng mã. Trong trường hợp này, kẻ tấn công có khả năng có thể tải lên tệp mã phía máy chủ có chức năng như web shell, cấp cho họ toàn quyền kiểm soát máy chủ.

- Nếu tên tệp không được xác thực hợp lệ, điều này có thể cho phép kẻ tấn công ghi đè lên các tệp quan trọng chỉ bằng cách tải tệp có cùng tên lên. Nếu máy chủ cũng dễ bị tấn công khi duyệt thư mục, điều này có thể có nghĩa là những kẻ tấn công thậm chí có thể tải tệp lên các vị trí không lường trước được.

- Việc không đảm bảo rằng kích thước của tệp nằm trong ngưỡng dự kiến ​​cũng có thể kích hoạt một dạng tấn công từ chối dịch vụ (DoS), theo đó kẻ tấn công sẽ lấp đầy dung lượng đĩa có sẵn.

## Ngăn chặn

- Kiểm tra phần mở rộng tệp dựa trên whitelist các phần mở rộng được phép. 

- Đảm bảo rằng tên tệp không chứa bất kỳ chuỗi con nào có thể được hiểu là thư mục (../).

- Đổi tên các tệp đã tải lên để tránh xung đột có thể khiến các tệp hiện có bị ghi đè.

- Càng nhiều càng tốt, hãy sử dụng một framework đã thiết lập để xử lý trước các tệp tải lên thay vì cố gắng viết các cơ chế xác thực của riêng mình.

# 9. Server-side request forgery (SSRF)

## Tổng quan

- Giả mạo yêu cầu phía máy chủ (còn được gọi là SSRF) là một lỗ hổng bảo mật web cho phép kẻ tấn công khiến ứng dụng phía máy chủ thực hiện các yêu cầu đến một vị trí ngoài ý muốn.

- Trong một cuộc tấn công SSRF điển hình, kẻ tấn công có thể khiến máy chủ tạo kết nối với các dịch vụ chỉ dành cho nội bộ trong cơ sở hạ tầng của tổ chức. Trong các trường hợp khác, họ có thể buộc máy chủ kết nối với các hệ thống bên ngoài tùy ý, có khả năng làm rò rỉ dữ liệu nhạy cảm, chẳng hạn như thông tin đăng nhập ủy quyền.

## Hậu quả

-  Có thể dẫn đến các hành động hoặc quyền truy cập trái phép vào dữ liệu trong tổ chức, trong chính ứng dụng dễ bị tổn thương hoặc trên các hệ thống phụ trợ khác mà ứng dụng có thể giao tiếp. Trong một số trường hợp, lỗ hổng SSRF có thể cho phép kẻ tấn công thực hiện lệnh tùy ý.

- SSRF gây ra các kết nối với hệ thống bên thứ ba bên ngoài có thể dẫn đến các cuộc tấn công nguy hiểm đến các dịch lưu trữ.

## Common SSRF attacks

- SSRF tấn công vào chính máy chủ
- SSRF tấn công vào các hệ thống back-end khác
- Tham khảo: https://portswigger.net/web-security/ssrf

## SSRF defenses

- SSRF filter bằng blacklist
- SSRF filter bằng whitelist

# 10. XML external entity (XXE) injection

## Tổng quan

- Chèn thực thể bên ngoài XML (còn được gọi là XXE) là một lỗ hổng bảo mật web cho phép kẻ tấn công can thiệp vào quá trình xử lý dữ liệu XML của ứng dụng. Nó thường cho phép kẻ tấn công xem các tệp trên hệ thống tệp của máy chủ ứng dụng và tương tác với bất kỳ hệ thống phụ trợ hoặc bên ngoài nào mà bản thân ứng dụng có thể truy cập.

- Trong một số trường hợp, kẻ tấn công có thể leo thang cuộc tấn công XXE để xâm phạm máy chủ bên dưới hoặc cơ sở hạ tầng phụ trợ khác, bằng cách tận dụng lỗ hổng XXE để thực hiện các cuộc tấn công giả mạo yêu cầu phía máy chủ (SSRF)

## Find and test for XXE vulnerabilities

- Phần lớn các lỗ hổng XXE có thể được tìm thấy nhanh chóng và đáng tin cậy bằng cách sử dụng Burp Suite's web vulnerability scanner.

- Kiểm tra thủ công các lỗ hổng XXE thường bao gồm;
    - Thử nghiệm truy xuất tệp bằng cách xác định một thực thể bên ngoài dựa trên tệp hệ điều hành nổi tiếng và sử dụng thực thể đó trong dữ liệu được trả về trong phản hồi của ứng dụng.
    - Thử nghiệm các lỗ hổng XXE mù bằng cách xác định một thực thể bên ngoài dựa trên URL dẫn đến hệ thống mà bạn kiểm soát và theo dõi các tương tác với hệ thống đó.

## Ngăn chặn

- Hầu như tất cả các lỗ hổng XXE phát sinh do thư viện phân tích cú pháp XML của ứng dụng hỗ trợ các tính năng XML nguy hiểm tiềm tàng mà ứng dụng không cần hoặc không có ý định sử dụng. Cách dễ nhất và hiệu quả nhất để ngăn chặn các cuộc tấn công của XXE là tắt các tính năng đó.

- Nói chung, chỉ cần vô hiệu hóa resolution của các thực thể bên ngoài và vô hiệu hóa hỗ trợ cho XInclude là đủ. Điều này thường có thể được thực hiện thông qua các tùy chọn cấu hình hoặc bằng cách ghi đè hành vi mặc định.
