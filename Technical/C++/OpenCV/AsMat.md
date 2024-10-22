
Trong OpenCV, cú pháp `Mat[Rect]` thực sự là một hình thức của `operator()`, nơi `Rect` được chuyển đổi thành hai đối tượng `Range` đại diện cho các hàng và cột. Ví dụ, một `Rect` với các giá trị `(x, y, width, height)` sẽ được chuyển đổi thành:

- `_rowRange: Range(y, y + height)`
- `_colRange: Range(x, x + width)`