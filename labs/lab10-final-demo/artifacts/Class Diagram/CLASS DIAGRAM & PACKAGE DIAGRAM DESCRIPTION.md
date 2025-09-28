# Thiết kế Class Diagram và Package Diagram

## Mục tiêu

Từ Use Case và Sequence Diagram đã phân tích, thiết kế **Class Diagram** để thể hiện chi tiết lớp, thuộc tính, phương thức, và các mối quan hệ.  
Sau đó gom nhóm các lớp theo chức năng thành **Package Diagram** để hình dung kiến trúc tổng thể.

---

## 1. Class Diagram

### 1.1. Nhóm Auth (Xác thực & Người dùng)

- **User**
  - `- userId: String`
  - `- name: String`
  - `- phone: String`
  - `- email: String`
  - `- role: UserRole`
  - `- status: String`
  - `+ getDisplayName(): String`
- **Customer** kế thừa `User`
  - `+ defaultPaymentMethod: String`
- **Staff** kế thừa `User`
  - `+ staffCode: String`
- **Manager** kế thừa `Staff`
  - `+ level: int`
- **Session**
  - `+ sessionId: String`
  - `+ expiresAt: DateTime`
- **AuthService**
  - `+ verifyZaloToken(token:String): User`
  - `+ issueSession(userId:String): Session`
  - `+ logout(sessionId:String): void`

**Quan hệ:** Customer, Staff, Manager kế thừa từ User. AuthService quản lý Session.

---

### 1.2. Nhóm Restaurant (Nhà hàng & Thực đơn)

- **Restaurant**
  - `+ id: String`
  - `+ name: String`
  - `+ address: String`
  - `+ rating: float`
  - `+ openHours: String`
  - `+ depositPolicyId: String`
- **Table**
  - `+ id: String`
  - `+ code: String`
  - `+ capacity: int`
  - `+ area: String`
  - `+ status: TableStatus`
- **MenuItem**
  - `+ id: String`
  - `+ name: String`
  - `+ price: decimal`
  - `+ isAvailable: bool`
- **Review**
  - `+ id: String`
  - `+ restaurantId: String`
  - `+ customerId: String`
  - `+ rating: int`
  - `+ comment: String`
  - `+ createdAt: DateTime`

**Quan hệ:**

- Một Restaurant có nhiều Table, MenuItem và Review.

---

### 1.3. Nhóm Booking (Đặt bàn)

- **Booking**
  - `+ id: String`
  - `+ restaurantId: String`
  - `+ customerId: String`
  - `+ time: DateTime`
  - `+ partySize: int`
  - `+ status: BookingStatus`
  - `+ depositAmount: decimal`
  - `- holdExpiresAt: DateTime`
  - `+ applyVoucher(code:String): bool`
  - `+ holdSlot(): bool`
  - `+ confirm(): void`
  - `+ cancel(reason:String): void`
  - `+ checkIn(): void`
- **BookingTimeline**
  - `+ id: String`
  - `+ bookingId: String`
  - `+ eventType: String`
  - `+ at: DateTime`
  - `+ message: String`
- **PolicyService**
  - `+ getDepositPolicy(bookingId:String): String`
  - `+ calcRefund(bookingId:String, cancelAt:DateTime): decimal`

**Quan hệ:**

- Customer có thể tạo nhiều Booking.
- Booking thuộc về 1 Restaurant.
- Booking có nhiều BookingTimeline.
- Staff quản lý Booking.
- PolicyService liên quan đến Booking.

---

### 1.4. Nhóm Payment (Thanh toán)

- **PaymentIntent**
  - `+ id: String`
  - `+ bookingId: String`
  - `+ amount: decimal`
  - `+ status: PaymentStatus`
  - `+ method: String`
  - `+ create(method:String): bool`
  - `+ capture(): bool`
  - `+ refund(amount:decimal): bool`
- **Refund**
  - `+ id: String`
  - `+ paymentIntentId: String`
  - `+ amount: decimal`
  - `+ status: String`
  - `+ createdAt: DateTime`
- **PaymentGateway**
  - `+ createQR(amount:decimal, orderId:String): String`
  - `+ chargeCard(token:String, amount:decimal): bool`
  - `+ refund(orderId:String, amount:decimal): bool`

**Quan hệ:**

- Booking gắn với PaymentIntent.
- PaymentIntent có thể tạo Refund.
- PaymentIntent dùng PaymentGateway để xử lý.

---

### 1.5. Nhóm Notify & Reporting

- **NotificationService**
  - `+ pushToUser(userId:String, title:String, body:String): void`
  - `+ sendReminder(bookingId:String): void`
- **ReportService**
  - `+ dailyReport(date:DateTime): Report`

**Quan hệ:**

- NotificationService gửi thông báo cho Customer.
- ReportService gửi báo cáo cho Manager.

---

### 1.6. Các Enum (Kiểu liệt kê)

- **UserRole**: Customer, Staff, Manager, Admin
- **BookingStatus**: Pending, Confirmed, Cancelled, CheckedIn
- **PaymentStatus**: Pending, Paid, Refunded, Failed
- **TableStatus**: Available, Reserved, Occupied

---

Sơ đồ lớp thể hiện các đối tượng chính trong hệ thống, thuộc tính, phương thức và quan hệ kế thừa/liên kết.

![Class Diagram](../../../lab06/Class-diagram.png)

---

## 2. Package Diagram

### 2.1. Packages

- **Auth**  
  Quản lý đăng nhập, phiên làm việc, và thông tin người dùng (User, Customer, Staff, Manager).

- **Restaurant**  
  Quản lý thông tin nhà hàng, bàn, thực đơn, và đánh giá.

- **Booking**  
  Xử lý quy trình đặt bàn, giữ bàn, check-in, và timeline sự kiện.

- **Payment**  
  Quản lý các giao dịch đặt cọc/thanh toán, tích hợp cổng thanh toán, hoàn tiền.

- **Notify**  
  Gửi thông báo cho khách hàng và nhà hàng.

- **Reporting**  
  Sinh báo cáo doanh thu, đặt bàn, và hiệu suất hoạt động.

---

### 2.2. Quan hệ giữa các Package

- **Auth → Booking**: User sau khi đăng nhập mới có thể đặt bàn.
- **Booking → Restaurant**: Đặt bàn gắn với một nhà hàng cụ thể.
- **Booking → Payment**: Đặt bàn yêu cầu đặt cọc/thanh toán.
- **Payment → Notify**: Kết quả thanh toán được gửi về cho khách hàng.
- **Booking → Notify**: Thông báo xác nhận hoặc hủy đặt bàn.
- **Reporting → Booking** và **Reporting → Payment**: Dùng dữ liệu đặt bàn và thanh toán để sinh báo cáo.

---

Sơ đồ gói gom các lớp vào từng package theo chức năng (Auth, Booking, Payment, Restaurant, Notify, Reporting) và chỉ ra sự phụ thuộc giữa các gói.

![Package Diagram](../../../lab06/Package-diagram.png)
