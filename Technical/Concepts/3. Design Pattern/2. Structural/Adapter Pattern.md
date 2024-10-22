# Nguồn tham khảo

1. Erich Gamma, John Vlissides, Richard Helm, Ralph Johnson - Design Patterns: Elements of Reusable Object-Oriented Software
2. Alexander Shvets (refactoring.guru) - Dive Into Design Patterns
3. Elisabeth Freeman, Kathy Sierra - Head First Design Patterns
4. https://viblo.asia/p/huong-dan-adapter-design-pattern-ORNZqd7bK0n


Nó cho phép các đối tượng có cấu trúc khác nhau, không tương thích với nhau có thể làm việc chung được với nhau.
![[AdapterPatternStruct.png]].

- **Object** using **Adapter**: là đối tượng adopt `NewProtocol` và sử dụng protocol.
- **NewProtocol**: là protocol được dùng để khai báo các giao thức sẽ được sử dụng trong Adapter Pattern.
- **Legacy Object**: là đối tượng cần sử dụng Adapter Pattern để giúp nó có thể tương thích được với những phần còn lại.
- **Adapter**: là thành phần đóng vai trò là bộ chuyển đổi, giúp cho đối tượng sử dụng và đối tượng được sử dụng tuy không tương thích với nhau nhưng vẫn có thể làm việc được với nhau.


1. Problem
![[DrawAPoint_AdapterPattern.png]]
Draw a point

2. Struct
![[Pasted image 20240604092537.png]]

3. Code 
```CSharp
using System;

namespace Adapter
{
    public interface IGraph
    {
        void Point(double x, double y);
    } 
    
    class PolarGraph
    {
        public void Point(double r, double t)
        {
            Console.WriteLine("Polar Coordinate Point: P(" + r + ", " + t + ")");
        }
    }
    //Adapter triển khai interface mà client sử dụng.
    class PolarGraphAdapter : IGraph 
    {
        private readonly PolarGraph polarGraph;
        public PolarGraphAdapter(PolarGraph polarGraph)
        {
            //Lấy reference đến object cần phải thích ứng.
            this.polarGraph = polarGraph; 
        }
		
        //Implement method Point của interface.
        public void Point(double x, double y)
        {
			//Nhận tung độ và hoành độ x và y, xử lý thành độ dài và góc quay r, t
            double r = Math.Sqrt(x * x + y * y);
            double t = Math.Atan2(y, x);
			//Gọi method Point từ object polarGraph. 
            polarGraph.Point(r, t);
        }   
    }
    
    class Program
    {
        static void Main(string[] args)
        {
            PolarGraph polarGraph = new PolarGraph();
            IGraph graph = new PolarGraphAdapter(polarGraph);
            graph.Point(3, 4);
            //Output: Polar Coordinate Point: P(5, 0.9272952180016122)
        }
    }
}

```