# Quang Mỹ OS
Quang Mỹ OS là hệ điều hành viết bằng Rust.
Mục tiêu của dự án:
- Viết lại một hệ điều hành hoàn toàn mới, miễn phí, mạnh mẽ.
- Khai thác tính an toàn bộ nhớ và concurrency của Rust.
- Xây dựng micro kernel tối giản, siêu nhỏ.
- Gói cài đặt siêu nhỏ chỉ có kèm 1 terminal, rồi tùy chức năng mà cài thêm các modules giao diện ứng dụng dùng cho cá nhân, mobile, desktop, server, iot, ai, game.

## Roadmap
- [ ] Bootloader cơ bản
- [ ] Micro Kernel tối giản (xử lý ngắt, quản lý bộ nhớ)
- [ ] Hệ thống file đơn giản
- [ ] Driver cơ bản (bàn phím, màn hình)
- [ ] Shell tối giản

## Microkernel và ý tưởng “3 bản check lẫn nhau”
Microkernel thuần túy: chỉ giữ các chức năng tối thiểu (IPC, scheduling, memory management) trong kernel; phần còn lại chạy ở user-space.
Ý tưởng 3 bản kiểm tra lẫn nhau: về lý thuyết, có thể triển khai nhiều kernel instance hoặc domain song song để giám sát nhau, nhưng thực tế hiếm OS nào làm vậy vì chi phí hiệu năng và độ phức tạp.
Thực tế: các hệ điều hành thường chọn cách cô lập thành phần nhạy cảm (ví dụ Secure Enclave, Exclaves) thay vì nhân bản toàn bộ kernel.

macOS/iOS kernel bảo mật thế nào?
XNU kernel: hybrid, kết hợp Mach microkernel và BSD monolithic. Mach cung cấp IPC và quản lý bộ nhớ, BSD cung cấp hệ thống file, process, network.
Secure Enclave: phần cứng riêng biệt trong chip Apple, xử lý khóa mã hóa, Touch ID/Face ID, dữ liệu nhạy cảm.
SPTM (Secure Page Table Monitor): giới thiệu năm 2023, chạy ở mức đặc quyền cao hơn kernel, kiểm soát việc ánh xạ bộ nhớ, ngăn kernel truy cập tùy tiện.
TXM (Trusted Execution Monitor): chịu trách nhiệm xác minh code signing và entitlement, chạy tách biệt khỏi XNU.
Exclaves: các domain cô lập nằm ngoài kernel chính, bảo vệ chức năng quan trọng ngay cả khi kernel bị compromise. Xuất hiện từ iOS 17 và macOS gần đây.

📊 So sánh bảo mật kernel
Mô hình	Ví dụ	Ưu điểm	Nhược điểm
Microkernel thuần	Mach 3.0, Minix	Cô lập tốt, dễ kiểm chứng	IPC overhead, hiệu năng thấp
Hybrid kernel	XNU (macOS/iOS)	Hiệu năng cao, tận dụng BSD	Kernel compromise ảnh hưởng toàn hệ
Hybrid + compartmentalization	XNU + SPTM, TXM, Exclaves	Tăng bảo mật, cô lập chức năng nhạy cảm	Phức tạp, chưa hoàn toàn microkernel

⚠️ Rủi ro & Thực tế
Microkernel “3 bản check lẫn nhau” chưa có OS thương mại nào áp dụng vì chi phí quá lớn.
Apple chọn hướng “compartmentalization” – tách các thành phần nhạy cảm ra khỏi kernel, gần giống microkernel nhưng vẫn giữ hiệu năng của hybrid.
Xu hướng: các OS hiện đại (Windows, Linux, macOS) đều bổ sung cơ chế bảo mật phần cứng (TPM, Secure Enclave) thay vì nhân bản kernel.

👉 Tóm lại: macOS/iOS không dùng microkernel thuần mà hybrid kernel XNU, nhưng Apple đang tiến gần hơn tới mô hình microkernel bằng cách tách các thành phần nhạy cảm (SPTM, TXM, Exclaves). Ý tưởng “3 kernel check lẫn nhau” chưa được áp dụng thực tế; thay vào đó, các OS chọn cách cô lập chức năng quan trọng để giảm nguy cơ khi kernel bị tấn công.

Bạn có muốn mình phân tích thêm so sánh với Linux (monolithic) và Windows (hybrid + virtualization-based security) để thấy Apple khác biệt thế nào không?

Apple không dùng microkernel thuần túy cho macOS/iOS mà dùng XNU – một hybrid kernel kết hợp Mach (microkernel) và BSD (monolithic). Tuy nhiên, để tăng bảo mật, Apple gần đây đã đưa vào các cơ chế mới như Secure Page Table Monitor (SPTM), Trusted Execution Monitor (TXM) và “exclaves” nhằm cô lập các chức năng nhạy cảm khỏi kernel chính.

(lung tung)

## Tham khảo Rust OS trên YouTube
Xem thêm tại: [Rust OS trên YouTube](https://www.youtube.com/results?search_query=rust+os)
[![Rust OS Playlist](https://upload.wikimedia.org/wikipedia/commons/4/42/YouTube_icon_%282013-2017%29.png)](https://www.youtube.com/playlist?list=PLib6-zlkjfXkdCjQgrZhmfJOWBk_C2FTY)
[![Rust OS Playlist](https://upload.wikimedia.org/wikipedia/commons/4/42/YouTube_icon_%282013-2017%29.png)](https://youtube.com/playlist?list=PL980gcR1LE3LBuWuSv2CL28HsfnpC4Qf7)

## ...

## License
[Unlicense](LICENSE).
