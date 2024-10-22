
Trong mô hình MVVM (Model-View-ViewModel), lazy loading thường được sử dụng để tải dữ liệu hoặc khởi tạo các đối tượng ViewModel chỉ khi chúng cần được hiển thị hoặc sử dụng trong giao diện người dùng. Dưới đây là cách triển khai lazy loading trong một ứng dụng WPF sử dụng MVVM:

1. **ViewModel Lazy Initialization**: Trong ViewModel, bạn có thể sử dụng `Lazy<T>` để khởi tạo các đối tượng ViewModel cần thiết. Dữ liệu sẽ không được tải lên trước mà chỉ khi được yêu cầu.
    
2. **Binding View với ViewModel**: Sử dụng các binding trong XAML để liên kết View với các thuộc tính của ViewModel. Khi các thành phần của View cần dữ liệu từ ViewModel, dữ liệu sẽ được tải lười biếng.
    

Dưới đây là một ví dụ cụ thể về cách triển khai lazy loading trong một ứng dụng WPF MVVM:

### ViewModel:

```CSharp
using System;
using System.ComponentModel;

public class MainViewModel : INotifyPropertyChanged
{
    private Lazy<DataViewModel> lazyDataViewModel;

    public MainViewModel()
    {
        // Khởi tạo lazyDataViewModel nhưng không tải dữ liệu ngay lập tức
        lazyDataViewModel = new Lazy<DataViewModel>(() => new DataViewModel());
    }

    public DataViewModel LazyDataViewModel => lazyDataViewModel.Value;

    // Implement INotifyPropertyChanged để thông báo cho View khi dữ liệu đã được tải
    public event PropertyChangedEventHandler PropertyChanged;
    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}

```

### DataViewModel:

```CSharp
using System;
using System.ComponentModel;

public class DataViewModel : INotifyPropertyChanged
{
    public DataViewModel()
    {
        // Đoạn code này thực hiện việc tải dữ liệu từ cơ sở dữ liệu hoặc từ các nguồn khác
        // Trong trường hợp này, chúng ta sẽ giả lập việc tải dữ liệu
        // (để ý rằng việc tải dữ liệu này chỉ xảy ra khi DataViewModel được yêu cầu)
        Console.WriteLine("Data is being loaded...");
    }

    // Implement INotifyPropertyChanged để thông báo cho View khi dữ liệu đã được tải
    public event PropertyChangedEventHandler PropertyChanged;
    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}

```

### View (XAML):

```xml
<Window x:Class="WpfApp.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        mc:Ignorable="d"
        Title="MainWindow" Height="450" Width="800">
    <Grid>
        <!-- Truy cập LazyDataViewModel từ MainViewModel -->
        <ContentControl Content="{Binding LazyDataViewModel}" />
    </Grid>
</Window>

```

Trong ví dụ này, `MainViewModel` có một thuộc tính `LazyDataViewModel` để truy cập `DataViewModel` bằng cách sử dụng `Lazy<T>`. Khi View yêu cầu `LazyDataViewModel`, `DataViewModel` sẽ được khởi tạo và dữ liệu sẽ được tải lười biếng.