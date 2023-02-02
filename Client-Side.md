# 11. Cross-site scripting

## Tổng quan

- XSS là một lỗ hổng bảo mật web cho phép kẻ tấn công xâm phạm các tương tác mà người dùng có với một ứng dụng. Nó cho phép kẻ tấn công phá vỡ `the same origin policy`, được thiết kế để tách biệt các trang web khác nhau với nhau.

- XSS thường cho phép kẻ tấn công giả dạng người dùng nạn nhân, thực hiện bất kỳ hành động nào mà người dùng có thể thực hiện và truy cập vào bất kỳ dữ liệu nào của người dùng. Nếu người dùng nạn nhân có quyền truy cập đặc quyền trong ứng dụng, thì kẻ tấn công có thể có toàn quyền kiểm soát tất cả chức năng và dữ liệu của ứng dụng.

## Các kiểu XSS

- Reflected XSS
- Stored XSS
- DOM-Based XSS

## Hậu quả

- Trong một ứng dụng phần mềm tài liệu quảng cáo, nơi tất cả người dùng đều ẩn danh và tất cả thông tin đều được công khai, tác động thường sẽ rất nhỏ.

- Trong một ứng dụng chứa dữ liệu nhạy cảm, chẳng hạn như giao dịch ngân hàng, email hoặc hồ sơ chăm sóc sức khỏe, tác động thường sẽ nghiêm trọng.

- Nếu người dùng bị xâm nhập có đặc quyền nâng cao trong ứng dụng, thì tác động nói chung sẽ rất nghiêm trọng, cho phép kẻ tấn công kiểm soát hoàn toàn ứng dụng dễ bị tấn công và xâm phạm tất cả người dùng cũng như dữ liệu của họ.

## Find and test for XSS

- Phần lớn các lỗ hổng XSS có thể được tìm thấy nhanh chóng và đáng tin cậy bằng cách sử dụng Burp Suite's web vulnerability scanner.

## Ngăn chặn

- Lọc input.
- Mã hóa dữ liệu trên đầu ra.
- Dùng response headers thích hợp.
- Content Security Policy.

# 12. Cross-origin resource sharing (CORS)

## Tổng quan

- Chia sẻ tài nguyên trên nhiều nguồn gốc (CORS) là một cơ chế trình duyệt cho phép truy cập có kiểm soát vào các tài nguyên nằm bên ngoài một miền nhất định. Nó mở rộng và thêm tính linh hoạt cho chính sách same-origin policy (SOP).

- Tuy nhiên, nó cũng tiềm ẩn nguy cơ xảy ra các cuộc tấn công giữa các miền nếu chính sách CORS của trang web được định cấu hình và triển khai kém. CORS không phải là biện pháp bảo vệ chống lại các cuộc tấn công có nguồn gốc chéo, chẳng hạn như giả mạo yêu cầu trên nhiều trang web (CSRF).

## Ngăn chặn

- Cấu hình đúng của các cross-origin requests.
- Chỉ cho phép các trang web đáng tin cậy.
- Tránh whitelist rỗng.
- Tránh wildcards trong mạng nội bộ.
- CORS không thay thế cho các chính sách bảo mật phía máy chủ.

# 13. DOM-based vulnerabilities

## Tổng quan

- DOM là biểu diễn phân cấp của trình duyệt web về các phần tử trên trang. Các trang web có thể sử dụng JavaScript để thao tác các nút và đối tượng của DOM, cũng như các thuộc tính của chúng. Bản thân thao tác DOM không phải là vấn đề. Trên thực tế, nó là một phần không thể thiếu trong cách thức hoạt động của các trang web hiện đại. Tuy nhiên, JavaScript xử lý dữ liệu không an toàn có thể kích hoạt các cuộc tấn công khác nhau. Các lỗ hổng dựa trên DOM phát sinh khi một trang web chứa JavaScript lấy giá trị mà kẻ tấn công có thể kiểm soát, được gọi là nguồn và chuyển giá trị đó vào một chức năng nguy hiểm, được gọi là `sink`.

## Ngăn chặn

- Không có hành động đơn lẻ nào có thể loại bỏ hoàn toàn mối đe dọa của các cuộc tấn công dựa trên DOM. Tuy nhiên, nói chung, cách hiệu quả nhất để tránh các lỗ hổng dựa trên DOM là tránh cho phép dữ liệu từ bất kỳ nguồn không đáng tin cậy nào tự động thay đổi giá trị được truyền tới bất kỳ `sink` nào.

- sanitize hoặc mã hóa dữ liệu. Đây có thể là một nhiệm vụ phức tạp và tùy thuộc vào ngữ cảnh mà dữ liệu sẽ được chèn vào, có thể bao gồm sự kết hợp giữa thoát JavaScript, mã hóa HTML và mã hóa URL theo trình tự thích hợp.

# 14. WebSockets

## Tổng quan

- WebSockets được sử dụng rộng rãi trong các ứng dụng web hiện đại. Chúng được bắt đầu qua HTTP và cung cấp các kết nối lâu dài với giao tiếp không đồng bộ theo cả hai hướng.

- WebSockets được sử dụng cho tất cả các loại mục đích, bao gồm thực hiện các hành động của người dùng và truyền thông tin nhạy cảm. Hầu như bất kỳ lỗ hổng bảo mật web nào phát sinh với HTTP thông thường cũng có thể phát sinh liên quan đến giao tiếp WebSockets.

## WebSockets security vulnerabilities

- Sử dụng thông báo WebSocket để khai thác lỗ hổng.
- Sử dụng WebSocket handshake để khai thác lỗ hổng.
- Sử dụng cross-site WebSocket để khai thác lỗ hổng.
- Tham khảo: https://portswigger.net/web-security/websockets

## Bảo mật kết nối WebSocket

- Sử dụng giao thức wss:// (WebSockets trên TLS).
- Hardcode URL của WebSockets endpoint và chắc chắn không kết hợp dữ liệu do người dùng kiểm soát vào URL này.
- Bảo vệ thông báo bắt tay WebSocket chống lại CSRF, để tránh các lỗ hổng chiếm quyền điều khiển WebSocket trên nhiều trang web.
- Xử lý dữ liệu nhận được qua WebSocket là không đáng tin cậy theo cả hai hướng. Xử lý dữ liệu một cách an toàn trên cả máy chủ và máy khách, để ngăn chặn các lỗ hổng dựa trên đầu vào như SQL injection và cross-site scripting.

# 15. Cross-site request forgery (CSRF)

## Tổng quan

- Cross-site request forgery (CSRF) là một lỗ hổng bảo mật web cho phép kẻ tấn công khiến người dùng thực hiện các hành động mà họ không có ý định thực hiện. Nó cho phép kẻ tấn công phá vỡ một phần chính sách `same origin policy`, được thiết kế để ngăn các trang web khác nhau can thiệp lẫn nhau.

## Hậu quả

- Trong CSRF, kẻ tấn công khiến người dùng nạn nhân thực hiện một hành động không cố ý. Ví dụ: điều này có thể là thay đổi địa chỉ email trên tài khoản của họ, thay đổi mật khẩu hoặc thực hiện chuyển tiền. Tùy thuộc vào bản chất của hành động, kẻ tấn công có thể có toàn quyền kiểm soát tài khoản của người dùng.

- Nếu người dùng bị xâm phạm có vai trò đặc quyền trong ứng dụng, thì kẻ tấn công có thể có toàn quyền kiểm soát tất cả dữ liệu và chức năng của ứng dụng.

## Ngăn chặn

- `CSRF tokens`: CSRF token là một giá trị duy nhất, bí mật và không thể đoán trước được tạo bởi ứng dụng phía máy chủ và được chia sẻ với máy khách. Khi cố gắng thực hiện một hành động nhạy cảm, chẳng hạn như gửi biểu mẫu, ứng dụng khách phải bao gồm mã thông báo CSRF chính xác trong yêu cầu. Điều này khiến kẻ tấn công rất khó xây dựng một yêu cầu hợp lệ thay cho nạn nhân.

- `SameSite cookies`: SameSite là một cơ chế bảo mật của trình duyệt xác định khi nào cookie của một trang web được bao gồm trong các yêu cầu bắt nguồn từ các trang web khác. Vì yêu cầu thực hiện các hành động nhạy cảm thường yêu cầu cookie phiên được xác thực, các hạn chế SameSite thích hợp có thể ngăn kẻ tấn công kích hoạt các hành động này trên nhiều trang web.

- `Referer-based validation`: Một số ứng dụng sử dụng HTTP Referer header để chống lại các cuộc tấn công CSRF, thông thường bằng cách xác minh rằng yêu cầu bắt nguồn từ miền riêng của ứng dụng. Điều này thường kém hiệu quả hơn so với xác thực mã thông báo CSRF.