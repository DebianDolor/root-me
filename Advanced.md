# 16. Insecure deserialization

## Tổng quan

- Tuần tự hóa (serialization) là quá trình chuyển đổi cấu trúc dữ liệu phức tạp, chẳng hạn như các đối tượng và trường của chúng, sang định dạng "phẳng hơn" để có thể gửi và nhận dưới dạng luồng byte tuần tự. Tuần tự hóa dữ liệu làm cho nó đơn giản hơn nhiều để:
    - Ghi dữ liệu phức tạp vào bộ nhớ liên tiến trình, tệp hoặc cơ sở dữ liệu
    - Gửi dữ liệu phức tạp, chẳng hạn như qua mạng, giữa các thành phần khác nhau của ứng dụng hoặc trong lệnh gọi API

- Deserialization là quá trình khôi phục luồng byte này thành một bản sao đầy đủ chức năng của đối tượng ban đầu, ở trạng thái chính xác như khi nó được tuần tự hóa. Sau đó, logic của trang web có thể tương tác với đối tượng được giải tuần tự hóa này, giống như với bất kỳ đối tượng nào khác.

- Insecure deserialization là khi dữ liệu do người dùng kiểm soát được giải tuần tự hóa bởi một trang web. Điều này có khả năng cho phép kẻ tấn công thao túng các đối tượng được tuần tự hóa để chuyển dữ liệu có hại vào mã ứng dụng.

## Hậu quả

- Tác động của Insecure deserialization có thể rất nghiêm trọng vì nó cung cấp một entry point để tấn công. Nó cho phép kẻ tấn công sử dụng lại mã ứng dụng hiện có theo những cách có hại, dẫn đến nhiều lỗ hổng khác, thường là thực thi mã từ xa.

- Ngay cả trong trường hợp không thể thực thi mã từ xa, quá trình Insecure deserialization có thể dẫn đến leo thang đặc quyền, truy cập tệp tùy ý và tấn công từ chối dịch vụ.

## Ngăn chặn

- Tránh sử dụng hoàn toàn các tính năng deserialization chung. Dữ liệu được tuần tự hóa từ các phương thức này chứa tất cả các thuộc tính của đối tượng ban đầu, bao gồm các trường riêng tư có khả năng chứa thông tin nhạy cảm. Thay vào đó, ta có thể tạo các phương thức tuần tự hóa theo lớp cụ thể của riêng mình để ít nhất có thể kiểm soát trường nào được hiển thị.

- Thực hiện ký để kiểm tra tính toàn vẹn của dữ liệu. Tuy nhiên, hãy nhớ rằng mọi kiểm tra phải diễn ra trước khi bắt đầu quá trình deserialization.

# 17. Server-side template injection

## Tổng quan

- Server-side template injection là khi kẻ tấn công có thể sử dụng cú pháp mẫu gốc để đưa payload độc hại vào mẫu, sau đó payload này được thực thi phía máy chủ.

- Điều này cho phép kẻ tấn công đưa vào các chỉ thị mẫu tùy ý để thao túng template engine, cho phép chúng kiểm soát hoàn toàn máy chủ.

## Hậu quả

- Server-side template injection có thể khiến các trang web phải hứng chịu nhiều cuộc tấn công khác nhau tùy thuộc vào template engine được đề cập và ứng dụng sử dụng nó chính xác như thế nào. Trong một số trường hợp hiếm gặp, những lỗ hổng này không gây rủi ro bảo mật thực sự. Tuy nhiên, hầu hết thời gian, tác động của việc tiêm mẫu phía máy chủ có thể rất thảm khốc.

- Ở mức nghiêm trọng nhất, kẻ tấn công có khả năng thực thi mã từ xa, chiếm toàn quyền kiểm soát máy chủ phụ trợ và sử dụng nó để thực hiện các cuộc tấn công khác vào cơ sở hạ tầng nội bộ.

- Ngay cả trong trường hợp không thể thực thi mã từ xa đầy đủ, kẻ tấn công vẫn có thể thường xuyên sử dụng tính năng chèn mẫu phía máy chủ làm cơ sở cho nhiều cuộc tấn công khác, có khả năng giành được quyền truy cập đọc vào dữ liệu nhạy cảm và các tệp tùy ý trên máy chủ.

## Ngăn chặn

- Cách tốt nhất để ngăn việc tiêm mẫu phía máy chủ là không cho phép bất kỳ người dùng nào sửa đổi hoặc gửi mẫu mới. Tuy nhiên, điều này đôi khi không thể tránh khỏi do yêu cầu kinh doanh.

- Một trong những cách đơn giản nhất để tránh tạo ra các lỗ hổng chèn mẫu phía máy chủ là luôn sử dụng một template engine "ít logic", chẳng hạn như Mustache, trừ khi thực sự cần thiết. Tách logic khỏi bản trình bày càng nhiều càng tốt có thể làm giảm đáng kể khả năng bạn tiếp xúc với các cuộc tấn công dựa trên mẫu nguy hiểm nhất.

- Một biện pháp khác là chỉ thực thi mã của người dùng trong môi trường sandbox nơi các mô-đun và chức năng nguy hiểm tiềm ẩn đã bị xóa hoàn toàn.

# 18. Web cache poisoning

## Tổng quan

- Nhiễm độc bộ đệm web là một kỹ thuật nâng cao, theo đó kẻ tấn công khai thác hành vi của máy chủ web và bộ đệm để gửi phản hồi HTTP có hại cho những người dùng khác.

- Về cơ bản, nhiễm độc bộ đệm web bao gồm hai giai đoạn. Đầu tiên, kẻ tấn công phải tìm ra cách tạo ra phản hồi từ máy chủ vô tình chứa một số loại payload nguy hiểm. Sau khi thành công, họ cần đảm bảo rằng phản hồi của họ được lưu vào bộ nhớ cache và sau đó được gửi tới các nạn nhân dự định.

- Bộ đệm web bị nhiễm độc có khả năng dẫn đến nhiều cuộc tấn công khác nhau, khai thác các lỗ hổng như XSS, inject JavaScript, open redirect, v.v.

## Hậu quả

- Vì bộ đệm bị nhiễm độc là một phương tiện phân phối hơn là một cuộc tấn công độc lập, nên tác động của việc nhiễm độc bộ đệm web có liên quan chặt chẽ đến mức độ nguy hại của payload được tiêm. Như với hầu hết các loại tấn công, nhiễm độc bộ đệm web cũng có thể được sử dụng kết hợp với các cuộc tấn công khác để tăng tác động tiềm ẩn hơn nữa.

- Phản hồi bị nhiễm độc sẽ chỉ được cung cấp cho những người dùng truy cập trang bị ảnh hưởng trong khi bộ đệm bị nhiễm độc. Do đó, tác động có thể từ không tồn tại đến lớn tùy thuộc vào việc trang đó có phổ biến hay không. Ví dụ: nếu kẻ tấn công tìm cách đầu độc phản hồi được lưu trong bộ nhớ cache trên trang chủ của một trang web lớn, thì cuộc tấn công có thể ảnh hưởng đến hàng nghìn người dùng mà không có bất kỳ tương tác nào sau đó từ kẻ tấn công.

## Ngăn chặn

- Cách tốt nhất để ngăn ngừa ngộ độc bộ đệm web rõ ràng là vô hiệu hóa hoàn toàn bộ nhớ đệm. Mặc dù đối với nhiều trang web, đây có thể không phải là một lựa chọn thực tế, nhưng trong các trường hợp khác, nó có thể khả thi.

- Ngay cả khi cần sử dụng bộ nhớ đệm, việc hạn chế nó ở các static responses cũng có hiệu quả, miễn là ta đủ cảnh giác về những gì ta phân loại là "tĩnh". Chẳng hạn, hãy đảm bảo rằng kẻ tấn công không thể lừa máy chủ phụ trợ truy xuất phiên bản độc hại của tài nguyên tĩnh thay vì phiên bản chính hãng.

# 19. HTTP Host header attacks

## Tổng quan

- HTTP Host header attacks khai thác các trang web dễ bị tổn thương xử lý giá trị của Host header theo cách không an toàn. Nếu máy chủ hoàn toàn tin tưởng vào Host header và không xác thực hoặc thoát khỏi tiêu đề đó đúng cách, thì kẻ tấn công có thể sử dụng thông tin đầu vào này để đưa vào các payload có hại thao túng hành vi phía máy chủ.

- Vì Host header được kiểm soát được bởi người dùng, nên có thể dẫn đến một số vấn đề. Nếu đầu vào không được thoát hoặc xác thực đúng cách, Host header lưu trữ là một vectơ tiềm ẩn để khai thác một loạt các lỗ hổng khác, đáng chú ý nhất là:
    - Web cache poisoning
    - Business logic flaws
    - Routing-based SSRF
    - SQL injection

## Ngăn chặn

- Bảo vệ những đường dẫn URL tuyệt đối.
- Kiểm chứng Host header.
- Whitelist các tên miền được phép.

# 20. JWT attacks

## Tổng quan

- JWT là một định dạng được tiêu chuẩn hóa để gửi dữ liệu JSON được ký bằng mật mã giữa các hệ thống. Về mặt lý thuyết, chúng có thể chứa bất kỳ loại dữ liệu nào, nhưng thường được sử dụng nhất để gửi thông tin ("claims") về người dùng như một phần của cơ chế xác thực, xử lý phiên và kiểm soát truy cập.

- Các cuộc tấn công JWT liên quan đến việc người dùng gửi các JWT đã sửa đổi đến máy chủ để đạt được mục tiêu độc hại. Thông thường, mục tiêu này là bỏ qua xác thực và kiểm soát truy cập bằng cách mạo danh một người dùng khác đã được xác thực.

## Hậu quả

- Tác động của các cuộc tấn công JWT thường nghiêm trọng. Nếu kẻ tấn công có thể tạo mã thông báo hợp lệ của riêng chúng với các giá trị tùy ý, chúng có thể leo thang đặc quyền của chúng hoặc mạo danh người dùng khác, chiếm toàn quyền kiểm soát tài khoản của chúng.

## Ngăn chặn

- Sử dụng thư viện mới để xử lý JWT.
- Đảm bảo rằng đã thực hiện xác minh chữ ký mạnh mẽ trên mọi JWT mà ta nhận được và tính đến các trường hợp cạnh như JWT được ký bằng thuật toán không mong muốn.
- Thực thi một danh sách trắng nghiêm ngặt các máy chủ được phép cho tiêu đề jku.

# 21. HTTP request smuggling

## Tổng quan

- HTTP request smuggling là một kỹ thuật can thiệp vào cách một trang web xử lý các chuỗi yêu cầu HTTP nhận được từ một hoặc nhiều người dùng. Chúng thường rất nghiêm trọng, cho phép kẻ tấn công vượt qua các biện pháp kiểm soát bảo mật, giành quyền truy cập trái phép vào dữ liệu nhạy cảm và trực tiếp xâm phạm những người dùng ứng dụng khác.

## Ngăn chặn

- Sử dụng HTTP/2 end to end và tắt tính năng hạ cấp HTTP nếu có thể. HTTP/2 sử dụng một cơ chế mạnh mẽ để xác định độ dài của yêu cầu và khi được sử dụng từ đầu đến cuối, vốn đã được bảo vệ chống buôn lậu yêu cầu

- Làm cho front-end server chuẩn hóa các yêu cầu mơ hồ và làm cho máy chủ back-end từ chối bất kỳ yêu cầu nào vẫn còn mơ hồ, đóng kết nối TCP trong quá trình này.

- Đừng bao giờ cho rằng các yêu cầu sẽ không có phần thân. Đây là nguyên nhân cơ bản của cả CL.0 và lỗ hổng client-side desync.

# 22. Essential skills

- Obfuscate các cuộc tấn công bằng mã hóa.
- Sử dụng Burp Scanner trong quá trình kiểm tra thủ công.
- Xác định các lỗ hổng chưa biết
- Tham khảo: https://portswigger.net/web-security/essential-skills