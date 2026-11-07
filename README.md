### Phần I: Thiết kế Prompt 3 Thành phần Chuẩn hóa

#### 1. Phân tích nguyên nhân khiến AI đưa ra câu trả lời sai lệch (AI Hallucination)
Câu hỏi *"AI ơi phần mềm trạm sạc của tôi bị hỏng rồi làm sao đây?"* khiến AI trả lời sai hoàn toàn bản chất vấn đề vì những lý do sau:
* **Thiếu hoàn toàn ngữ cảnh (Context):** Câu hỏi không nêu rõ thiết bị đang gặp sự cố là hệ thống Kiosk chạy phần mềm trên nền tảng Windows hay phần cứng điện tử, cũng không cho biết tác vụ đang thực hiện.
* **Không cung cấp dữ liệu lỗi thực tế (Raw Error):** Kỹ thuật viên không đính kèm thông báo lỗi cụ thể từ màn hình Terminal nên AI không có cơ sở kỹ thuật để chẩn đoán.
* **Kích hoạt cơ chế đoán mò (Hallucination):** Khi gặp câu hỏi quá chung chung và mơ hồ, mô hình ngôn ngữ lớn sẽ tự động suy diễn theo các kịch bản phổ biến nhất trên internet (như hỏng phần cứng trạm sạc, đứt dây điện), dẫn đến lời khuyên sai lệch và nguy hiểm.

#### 2. Câu Prompt 3 Thành phần hoàn chỉnh gửi Trợ lý AI

> **[Context]**
> Tôi đang vận hành hệ thống Kiosk trạm sạc nhanh xe điện VoltCharge (trụ số 04) chạy trên hệ điều hành Windows 11. Trong ca trực đêm, phần mềm điều khiển trạm sạc đột ngột dừng hoạt động và màn hình Terminal hiển thị thông báo lỗi.
>
> **[Raw Error]**
> ```text
> [TERMINAL ERROR LOG - VOLTCHARGE CHARGER #04]
> 2026-09-21 21:15:30 [FATAL] ControllerDaemon failed to start.
> System.UnauthorizedAccessException: Access to the path 'C:\Program Files\VoltCharge\data\session.log' is denied.
> at System.IO.FileStream.ValidateFileHandle(SafeFileHandle handle)
> at VoltCharge.Core.SessionManager.InitLog()
> InnerException: None. Process terminated with exit code 5 (0x00000005).
> Suggestion: Run the application with elevated administrator privileges or verify NTFS file permissions.
> ```
>
> **[Instruction]**
> Hãy đóng vai một chuyên gia hỗ trợ kỹ thuật IT, dịch và giải thích nguyên nhân gốc rễ của lỗi này bằng tiếng Việt dễ hiểu nhất cho người mới. Sau đó, hướng dẫn tôi 3 bước cụ thể để khắc phục lỗi này trên Windows 11 giúp trạm sạc hoạt động trở lại an toàn.

---

### Phần II: Giải mã Lỗi & Kịch bản Khắc phục

#### 3. Dịch và giải thích nguyên nhân gốc rễ bằng tiếng Việt đời thường
* **Dịch nghĩa thông báo lỗi:** *"Lỗi truy cập trái phép: Quyền truy cập vào đường dẫn `C:\Program Files\VoltCharge\data\session.log` đã bị từ chối. Tiến trình buộc phải dừng với mã lỗi số 5. Gợi ý: Hãy chạy ứng dụng với quyền Quản trị viên cao hơn hoặc kiểm tra lại quyền truy cập tệp NTFS."*
* **Giải thích dễ hiểu:** Phần mềm trạm sạc không hề bị hỏng hóc phần cứng hay đứt dây điện. Nguyên nhân là do khi khởi động, phần mềm cần ghi lại nhật ký hoạt động vào tệp `session.log` nằm trong thư mục hệ thống `C:\Program Files\...`. Tuy nhiên, Windows mặc định khóa chặt thư mục này để bảo vệ hệ thống. Vì phần mềm đang chạy dưới quyền người dùng thông thường (không có quyền Quản trị viên) nên Windows đã chặn lại không cho ghi file, khiến phần mềm tự tắt.

#### 4. Kịch bản 3 bước khắc phục cụ thể tại chỗ trên Windows 11
* **Bước 1: Chạy phần mềm dưới quyền Quản trị viên (Run as Administrator)**
  * Tìm biểu tượng (Shortcut) hoặc tệp chạy gốc của phần mềm điều khiển `VoltCharge` trên màn hình Desktop hoặc trong thư mục cài đặt.
  * Nhấp **chuột phải** vào biểu tượng phần mềm và chọn **Run as administrator** (Chạy với tư cách quản trị viên), sau đó bấm **Yes** nếu hộp thoại xác nhận UAC hiện lên.
* **Bước 2: Cấp quyền ghi dữ liệu vĩnh viễn cho thư mục `data`**
  * Mở File Explorer, truy cập vào đường dẫn `C:\Program Files\VoltCharge`.
  * Nhấp **chuột phải** vào thư mục **`data`** -> chọn **Properties** -> chuyển sang thẻ **Security** -> bấm nút **Edit...**.
  * Chọn nhóm người dùng đang đăng nhập (ví dụ: `Users`), tích chọn ô **Allow** ở dòng **Full control** (hoặc **Modify** và **Write**), sau đó bấm **Apply** -> **OK** để lưu lại.
* **Bước 3: Thiết lập tự động chạy quyền Admin và khởi động lại trạm sạc**
  * Nhấp chuột phải lại vào tệp chạy phần mềm `VoltCharge` -> chọn **Properties** -> thẻ **Compatibility** -> tích vào ô **Run this program as an administrator** -> bấm **OK** (để các lần sau tự động chạy không bị lỗi lại).
  * Mở lại phần mềm điều khiển, kiểm tra màn hình Terminal xác nhận hết báo lỗi `[FATAL]` và tiến hành sạc thử cho xe taxi điện.

#### 5. Xử lý bẫy an ninh đối với chuỗi mã bí mật (`API_KEY`)
* **Cách xử lý:** Trước khi dán đoạn nhật ký lỗi vào khung chat của ChatGPT hoặc Gemini, kỹ thuật viên **bắt buộc phải xóa hoặc che mờ (redact)** giá trị của khóa bí mật thành dạng ký tự giả định (ví dụ: sửa `API_KEY: volt_secret_token_998822` thành `API_KEY: [ĐÃ_ẨN_MÃ_BÍ_MẬT]` hoặc `API_KEY: ***`).
* **Giải thích lý do:**
  * Tuân thủ nguyên tắc an toàn vàng số 3: **Tuyệt đối không gửi Secret, API Key, mật khẩu hoặc dữ liệu nhạy cảm lên các công cụ AI công cộng**.
  * Dữ liệu nhập vào các nền tảng AI công cộng có thể được lưu trữ trên máy chủ bên thứ ba hoặc dùng để huấn luyện mô hình. Nếu để lộ `API_KEY`, kẻ xấu có thể chiếm quyền điều khiển hệ thống trạm sạc VoltCharge từ xa, đánh cắp dữ liệu giao dịch hoặc gây mất an toàn vận hành cho toàn bộ hệ thống.
