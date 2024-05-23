
## 1. Cách tạo Unit Test
Viết unit test trong C# là một kỹ năng quan trọng cho việc phát triển phần mềm để đảm bảo rằng các đoạn mã của bạn hoạt động đúng như mong đợi. Dưới đây là hướng dẫn cơ bản về cách viết unit test trong C# sử dụng NUnit, một framework phổ biến cho unit testing.

### Bước 1: Cài đặt NUnit và NUnit3TestAdapter


1. Mở dự án của bạn trong Visual Studio.
2. Mở `NuGet Package Manager` bằng cách nhấp chuột phải vào Solution hoặc Project và chọn `Manage NuGet Packages`.
3. Tìm kiếm và cài đặt các gói sau:
    - `NUnit`
    - `NUnit3TestAdapter`

### Bước 2: Tạo Project Unit Test

1. Thêm một project mới vào solution của bạn bằng cách nhấp chuột phải vào Solution và chọn `Add > New Project`.
2. Chọn `Unit Test Project (.NET Core)` hoặc `Unit Test Project (.NET Framework)`, tùy thuộc vào loại dự án bạn đang sử dụng.
3. Đặt tên cho project unit test và nhấn `Create`.

### Bước 3: Viết Unit Test

Giả sử bạn có một lớp `Calculator` với phương thức `Add` mà bạn muốn kiểm tra:

```CSharp
public class Calculator
{
    public int Add(int a, int b)
    {
        return a + b;
    }
}
```

Bây giờ, bạn sẽ viết unit test cho phương thức `Add` này.

1. Trong project unit test, tạo một tệp mới hoặc sử dụng tệp `UnitTest1.cs` đã được tạo sẵn.
2. Viết mã unit test như sau:
```CSharp
using NUnit.Framework;

namespace CalculatorTests
{
    [TestFixture]
    public class CalculatorTests
    {
        [Test]
        public void Add_WhenCalledWithTwoIntegers_ReturnsTheirSum()
        {
            // Arrange
            var calculator = new Calculator();
            int a = 5;
            int b = 10;

            // Act
            int result = calculator.Add(a, b);

            // Assert
            Assert.AreEqual(15, result);
        }
    }
}
```

### Giải thích:

- `[TestFixture]`: Chỉ định rằng lớp này chứa các unit test.
- `[Test]`: Chỉ định rằng phương thức này là một unit test.
- **Arrange**: Khởi tạo các đối tượng và đặt các điều kiện ban đầu cho test.
- **Act**: Thực hiện hành động hoặc phương thức cần kiểm tra.
- **Assert**: Kiểm tra kết quả để xác nhận rằng nó đáp ứng các mong đợi.

### Bước 4: Chạy Unit Test

1. Mở `Test Explorer` trong Visual Studio bằng cách vào `Test > Test Explorer`.
2. Nhấp vào `Run All` để chạy tất cả các unit test.
3. Kiểm tra kết quả để đảm bảo rằng tất cả các test đã chạy thành công.


## 2. 1

