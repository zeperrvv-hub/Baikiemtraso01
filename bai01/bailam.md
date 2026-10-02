

### Câu 1
Trong C#, **Value Types** và **Reference Types** khác nhau chủ yếu ở cách lưu trữ và cách dữ liệu được sao chép trong bộ nhớ.

**Value Types** là kiểu dữ liệu mà biến chứa trực tiếp giá trị của nó. Khi một biến kiểu giá trị được gán cho một biến khác, hệ thống tạo ra một bản sao độc lập của giá trị đó. Vì vậy, việc thay đổi biến mới không làm thay đổi biến ban đầu. Trong cách giải thích cơ bản, các biến kiểu giá trị cục bộ thường được liên hệ với vùng nhớ **Stack**, nơi dữ liệu được quản lý theo cơ chế vào sau ra trước và có tốc độ truy xuất nhanh.

Ngược lại, **Reference Types** không lưu trực tiếp toàn bộ dữ liệu của đối tượng trong biến mà lưu một **tham chiếu**, tức là địa chỉ hoặc thông tin dùng để xác định đối tượng nằm ở đâu trong bộ nhớ. Đối tượng thực tế thường được tạo trên vùng nhớ **Heap**. Khi một biến kiểu tham chiếu được gán cho biến khác, tham chiếu được sao chép chứ không phải toàn bộ đối tượng. Vì vậy, nhiều biến có thể cùng tham chiếu đến một đối tượng, và việc thay đổi dữ liệu thông qua một biến có thể được nhìn thấy từ biến còn lại.

Tóm lại, điểm khác biệt cốt lõi là **Value Type sao chép giá trị**, còn **Reference Type sao chép tham chiếu**. Ngoài ra, việc nói “Value Type luôn ở Stack, Reference Type luôn ở Heap” chỉ là cách giải thích đơn giản; trên thực tế vị trí lưu trữ còn phụ thuộc vào ngữ cảnh sử dụng.

### Câu 2
Trong C# 9 trở lên, `init` được đưa vào để hỗ trợ việc tạo ra các đối tượng có trạng thái gần như bất biến sau khi khởi tạo.

Một thuộc tính sử dụng `set` có thể được gán lại nhiều lần sau khi đối tượng đã được tạo, miễn là phạm vi truy cập cho phép. Điều này phù hợp với các đối tượng có dữ liệu cần thay đổi trong suốt quá trình chương trình chạy.

Trong khi đó, thuộc tính sử dụng `init` chỉ cho phép gán giá trị tại thời điểm khởi tạo đối tượng. Sau khi quá trình khởi tạo kết thúc, thuộc tính đó không thể được thay đổi theo cách thông thường. Điều này giúp bảo vệ tính nhất quán của dữ liệu và hạn chế việc vô tình thay đổi các thông tin quan trọng.

Trong thực tế, `init` thường được sử dụng với những thông tin chỉ nên xác định một lần, chẳng hạn như mã sinh viên, mã đơn hàng, mã nhân viên, ngày tạo hoặc các thông tin cấu hình ban đầu. Nhờ đó, chương trình dễ kiểm soát trạng thái của đối tượng hơn và giảm nguy cơ phát sinh lỗi do thay đổi dữ liệu ngoài ý muốn.

Như vậy, sự khác biệt chính là `set` cho phép thay đổi thuộc tính sau khi đối tượng đã được tạo, còn `init` chỉ cho phép thiết lập giá trị trong giai đoạn khởi tạo.

### Câu 3
Trong lập trình hướng đối tượng, `virtual` và `override` được sử dụng để thực hiện **đa hình tại thời điểm chạy**.

Từ khóa `virtual` được khai báo trong lớp cha. Khi một phương thức được đánh dấu là `virtual`, lớp cha cho phép các lớp con được quyền thay đổi cách thực hiện của phương thức đó. Có thể hiểu `virtual` là một cơ chế cho phép mở rộng hành vi của lớp cha mà không cần sửa trực tiếp lớp cha.

Từ khóa `override` được sử dụng trong lớp con để cung cấp một phiên bản triển khai mới cho phương thức `virtual` đã được định nghĩa ở lớp cha. Phương thức `override` phải có cùng tên, kiểu trả về và danh sách tham số phù hợp với phương thức ở lớp cha.

Ý nghĩa quan trọng của cơ chế này là khi chương trình sử dụng một biến có kiểu của lớp cha nhưng đối tượng thực tế lại thuộc lớp con, hệ thống sẽ xác định phương thức phù hợp tại thời điểm chạy. Nếu lớp con đã ghi đè phương thức, phiên bản của lớp con sẽ được thực thi.

Do đó, có thể hiểu ngắn gọn rằng `virtual` thể hiện **quyền cho phép ghi đè**, còn `override` thể hiện **việc thực hiện ghi đè**. Hai từ khóa này là nền tảng quan trọng của tính đa hình trong C#.

### Câu 4
Một thành phần được khai báo là `static` thuộc về **bản thân lớp**, chứ không thuộc về từng đối tượng cụ thể được tạo ra từ lớp đó.

Thông thường, khi sử dụng toán tử `new`, chương trình tạo ra một đối tượng mới. Các thành phần không phải `static` sẽ gắn với từng đối tượng, nghĩa là mỗi đối tượng có thể có giá trị riêng của mình.

Ngược lại, thành phần `static` chỉ tồn tại một bản dùng chung cho toàn bộ lớp. Tất cả các đối tượng được tạo ra từ lớp đó đều sử dụng chung thành phần `static` này. Vì bản chất của nó không phụ thuộc vào bất kỳ đối tượng riêng lẻ nào nên việc truy cập thông qua một object instance sẽ gây hiểu nhầm rằng thành phần đó thuộc về đối tượng.

Vì vậy, C# yêu cầu truy cập thành phần `static` thông qua **tên lớp**. Cách này phản ánh đúng bản chất rằng thành phần đó thuộc về lớp và được chia sẻ bởi tất cả các đối tượng của lớp.

Nói cách khác, thành phần `static` có vòng đời và phạm vi gắn với lớp, còn thành phần thông thường có vòng đời và dữ liệu gắn với từng object.
