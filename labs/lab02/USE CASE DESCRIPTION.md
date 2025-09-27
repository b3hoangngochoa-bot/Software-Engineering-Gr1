USE CASE DESCRIPTION 

Use Case 1: Tìm kiếm

Use Case ID: UC-01
Use Case Name: Tìm kiếm nhà hàng/bàn trống

Created By: Nhóm 1
Last Updated By: Nhóm 1
Date Created: [27/09/2025]
Date Last Updated: [27/09/2025]

Actor: Customer

Trigger: Customer bấm vào chức năng “Tìm kiếm” trên giao diện ứng dụng.

Description (Summary):
Cho phép khách hàng tìm kiếm nhà hàng theo tên, loại món ăn, vị trí, thời gian, hoặc các tiêu chí khác. Kết quả trả về hiển thị menu, giá và đánh giá, giúp khách đưa ra quyết định đặt bàn.

Preconditions:

1.Hệ thống đã có dữ liệu nhà hàng, menu, bàn trống.

2.Customer đã đăng nhập hoặc sử dụng chế độ khách.

Postconditions:

1.Danh sách nhà hàng phù hợp hiển thị.

2.Customer có thể xem chi tiết và tiếp tục thao tác Đặt bàn.

Priority: Cao.
Frequency of Use: Mỗi lần khách hàng mở app để tìm bàn.

Normal Course (Basic Flow):

1.Customer chọn “Tìm kiếm”.

2.Nhập tiêu chí (ví dụ: tên nhà hàng, địa điểm, loại món, thời gian).

3.Hệ thống xử lý yêu cầu.

4.Hệ thống trả về danh sách kết quả phù hợp.

5.Customer chọn một nhà hàng để xem chi tiết.

Alternative Flow:

UC-01.AC.1: Customer sử dụng bộ lọc nâng cao (giá, đánh giá, khuyến mãi) → hệ thống hiển thị kết quả theo tiêu chí lọc.

Exception Flow:

UC-01.EX.1: Không tìm thấy kết quả → hệ thống hiển thị thông báo “Không có nhà hàng phù hợp” và gợi ý lựa chọn khác.

UC-01.EX.2: Lỗi kết nối mạng → hệ thống thông báo lỗi và yêu cầu thử lại.

Includes: Xem thông tin, xem menu, xem giá, xem đánh giá.
Special Requirements: Thời gian phản hồi kết quả ≤ 3 giây, giao diện thân thiện, hỗ trợ cả mobile.
Assumptions: Dữ liệu nhà hàng trong hệ thống luôn được cập nhật.
Notes and Issues: Cần tích hợp API bản đồ để hiển thị khoảng cách.

Use Case 2: Đặt bàn

Use Case ID: UC-02
Use Case Name: Đặt bàn nhà hàng

Created By: Nhóm 1
Last Updated By: Nhóm 1
Date Created: [27/09/2025]
Date Last Updated: [27/09/2025]


Actor: Customer

Trigger: Customer chọn chức năng “Đặt bàn” từ kết quả tìm kiếm hoặc trang thông tin nhà hàng.

Description (Summary):
Cho phép khách hàng đặt bàn tại nhà hàng, bao gồm nhập thông tin (ngày, giờ, số người), xác nhận, và thanh toán cọc nếu có.

Preconditions:

1.Customer đã chọn nhà hàng cần đặt bàn.

2.Có bàn trống tại thời điểm đặt.

Postconditions:

1.Đơn đặt bàn được lưu trong hệ thống.

2.Customer nhận thông báo xác nhận.

3.Thông tin được lưu trong lịch sử booking.

Priority: Rất cao.
Frequency of Use: Mỗi lần khách hàng muốn đặt chỗ.

Normal Course (Basic Flow):

1.Customer chọn “Đặt bàn”.

2.Hệ thống hiển thị form nhập thông tin (số người, ngày, giờ, yêu cầu đặc biệt).

3.Customer điền thông tin và xác nhận.

4.Hệ thống kiểm tra bàn trống.

5.Nếu cần, hệ thống yêu cầu thanh toán cọc.

6.Customer thanh toán bằng thẻ hoặc QR code.

7.Hệ thống gửi thông báo xác nhận và lưu lịch sử booking.

Alternative Flow:

UC-02.AC.1: Customer không thanh toán cọc (nếu nhà hàng cho phép) → hệ thống lưu booking ở trạng thái “Chưa đặt cọc”.

UC-02.AC.2: Customer chọn “Cancel” trước khi xác nhận → hệ thống hủy thao tác đặt bàn.

Exception Flow:

UC-02.EX.1: Không còn bàn trống → hệ thống thông báo và gợi ý thời gian khác.

UC-02.EX.2: Thanh toán thất bại → hệ thống thông báo lỗi và yêu cầu thử lại hoặc chọn phương thức khác.

Includes: Nhập thông tin, thanh toán cọc, nhận thông báo.
Special Requirements:

Hệ thống đảm bảo bảo mật khi thanh toán (chuẩn PCI DSS).

Thời gian phản hồi xác nhận ≤ 5 giây.

Assumptions: Nhà hàng luôn cập nhật trạng thái bàn trống chính xác.
Notes and Issues: Cần kiểm thử kỹ các trường hợp thanh toán online.