
## Example 1
```CSharp
using System;
using System.Collections.Generic;

public abstract class BehaviorNode
{
    public abstract BehaviorStatus Tick();
}

public class ActionNode : BehaviorNode
{
    private Func<BehaviorStatus> action;

    public ActionNode(Func<BehaviorStatus> action)
    {
        this.action = action;
    }

    public override BehaviorStatus Tick()
    {
        return action();
    }
}

public class SelectorNode : BehaviorNode
{
    private List<BehaviorNode> children = new List<BehaviorNode>();

    public void AddChild(BehaviorNode child)
    {
        children.Add(child);
    }

    public override BehaviorStatus Tick()
    {
        foreach (var child in children)
        {
            var status = child.Tick();
            if (status != BehaviorStatus.Failure)
            {
                return status;
            }
        }
        return BehaviorStatus.Failure;
    }
}

public class SequenceNode : BehaviorNode
{
    private List<BehaviorNode> children = new List<BehaviorNode>();

    public void AddChild(BehaviorNode child)
    {
        children.Add(child);
    }

    public override BehaviorStatus Tick()
    {
        foreach (var child in children)
        {
            var status = child.Tick();
            if (status != BehaviorStatus.Success)
            {
                return status;
            }
        }
        return BehaviorStatus.Success;
    }
}

public enum BehaviorStatus
{
    Success,
    Failure,
    Running
}


class Program
{
    static void Main(string[] args)
    {
        var tree = new SelectorNode();
        var sequence = new SequenceNode();

        sequence.AddChild(new ActionNode(() =>
        {
            Console.WriteLine("Action 1");
            return BehaviorStatus.Success;
        }));
        sequence.AddChild(new ActionNode(() =>
        {
            Console.WriteLine("Action 2");
            return BehaviorStatus.Success;
        }));
        sequence.AddChild(new ActionNode(() =>
        {
            Console.WriteLine("Action 3");
            return BehaviorStatus.Success;
        }));

        tree.AddChild(sequence);
        tree.AddChild(new ActionNode(() =>
        {
            Console.WriteLine("Action 4");
            return BehaviorStatus.Success;
        }));

        tree.Tick();

        Console.ReadLine();
    }
}
```


## Example 2: Multi node
```CSharp
public static BehaviorRootNode GetLaneTree()
        {
            return new BehaviorTreeBuilder("TeachingBehaviorTree")
                .Selector()
                    .Sequence()
                        .Conditional(new HasMorePositionToGrabCondition()).Executor(new SuccessActionNode())

                        .ConditionalLoop(new CheckGrabberStatusCondition(GrabberStatus.GrabberReady, GrabberStatus.GrabberRunning, GrabberStatus.GrabberAcquired))
                             .Executor(new SleepActionNode(UsableMemoryWaitCondition.WaitKey))   
                        .Executor(new CreateGrabCommandBlockWithPositionCheck())
                    .Close()
                .Close()
                .Build();
        }
```

~ Giải thích:
Đoạn mã trên là một phần của một hệ thống sử dụng behavior tree để điều khiển hành vi của các đối tượng trong một trò chơi hoặc một ứng dụng. Hãy xem xét một cách cụ thể:

- `BehaviorTreeBuilder("TeachingBehaviorTree")`: Đây là việc tạo ra một đối tượng `BehaviorTreeBuilder` với tên "TeachingBehaviorTree". `BehaviorTreeBuilder` là một công cụ giúp xây dựng cây hành vi một cách dễ dàng và linh hoạt.
    
- `.Selector()`: Phương thức `Selector()` tạo ra một node loại Selector trong behavior tree. Node loại này chọn một trong các con node để thực hiện dựa trên kết quả của chúng.
    
- `.Sequence()`: Phương thức `Sequence()` tạo ra một node loại Sequence trong behavior tree. Node loại này thực hiện các con node theo thứ tự, chuyển đến con tiếp theo chỉ khi con trước đó đã thành công.
    
- `.Conditional(new HasMorePositionToGrabCondition())`: Phương thức `Conditional()` tạo ra một node loại Conditional trong behavior tree. Node loại này đánh giá một điều kiện và tiếp tục với một con node chỉ khi điều kiện đó là true. Trong trường hợp này, điều kiện được cung cấp là `HasMorePositionToGrabCondition`.
    
- `.Executor(new SuccessActionNode())`: Phương thức `Executor()` tạo ra một node loại Executor trong behavior tree. Node loại này thực hiện một hành động cụ thể. Trong trường hợp này, hành động được thực hiện là `SuccessActionNode`.
    
- `.Close()`: Phương thức `Close()` kết thúc một nhóm con node (Sequence hoặc Selector) trong behavior tree.
    
- `.Build()`: Phương thức `Build()` kết thúc quá trình xây dựng cây hành vi và trả về một `BehaviorRootNode`, đại diện cho gốc của behavior tree.
    

Về cơ bản, đoạn mã trên tạo ra một behavior tree với một Selector node chứa một Sequence node. Sequence node này chứa các Conditional node và Executor node. Các điều kiện và hành động được xác định bởi các lớp như `HasMorePositionToGrabCondition`, `CheckGrabberStatusCondition`, `SuccessActionNode`, và `SleepActionNode`.

## Example3: Condition
```CSharp
using System;
using System.Collections.Generic;

public abstract class BehaviorNode
{
    public abstract BehaviorStatus Tick();
}

public class ActionNode : BehaviorNode
{
    private Func<BehaviorStatus> action;

    public ActionNode(Func<BehaviorStatus> action)
    {
        this.action = action;
    }

    public override BehaviorStatus Tick()
    {
        return action();
    }
}

public class SelectorNode : BehaviorNode
{
    private List<BehaviorNode> children = new List<BehaviorNode>();

    public void AddChild(BehaviorNode child)
    {
        children.Add(child);
    }

    public override BehaviorStatus Tick()
    {
        foreach (var child in children)
        {
            var status = child.Tick();
            if (status != BehaviorStatus.Failure)
            {
                return status;
            }
        }
        return BehaviorStatus.Failure;
    }
}

public class SequenceNode : BehaviorNode
{
    private List<BehaviorNode> children = new List<BehaviorNode>();

    public void AddChild(BehaviorNode child)
    {
        children.Add(child);
    }

    public override BehaviorStatus Tick()
    {
        foreach (var child in children)
        {
            var status = child.Tick();
            if (status != BehaviorStatus.Success)
            {
                return status;
            }
        }
        return BehaviorStatus.Success;
    }
}

public enum BehaviorStatus
{
    Success,
    Failure,
    Running
}


public enum ConditionStatus
{
    True,
    False
}

public class ConditionNode : BehaviorNode
{
    private Func<ConditionStatus> condition;

    public ConditionNode(Func<ConditionStatus> condition)
    {
        this.condition = condition;
    }

    public override BehaviorStatus Tick()
    {
        if (condition() == ConditionStatus.True)
        {
            return BehaviorStatus.Success;
        }
        else
        {
            return BehaviorStatus.Failure;
        }
    }
}

class Program
{
    static void Main(string[] args)
    {
        // Define a condition: for simplicity, it's just returning True or False alternately
        ConditionStatus currentCondition = ConditionStatus.False;
        Func<ConditionStatus> conditionFunction = () =>
        {
            currentCondition = (currentCondition == ConditionStatus.False) ? ConditionStatus.True : ConditionStatus.False;
            return currentCondition;
        };

        // Create nodes
        var conditionNode = new ConditionNode(conditionFunction);
        var attackNode = new ActionNode(() =>
        {
            Console.WriteLine("Attacking player");
            return BehaviorStatus.Success;
        });
        var patrolNode = new ActionNode(() =>
        {
            Console.WriteLine("Patrolling area");
            return BehaviorStatus.Success;
        });

        // Create behavior tree
        var selectorNode = new SelectorNode();
        selectorNode.AddChild(conditionNode);
        selectorNode.AddChild(attackNode);
        selectorNode.AddChild(patrolNode);

        // Execute behavior tree
        selectorNode.Tick();
        selectorNode.Tick();
        selectorNode.Tick();
        selectorNode.Tick();
    }
}

```


## Example 4

```CSharp
public static BehaviorRootNode GetLaneTree()
    {
        return new BehaviorTreeBuilder("TeachingBehaviorTree")
            .Selector()
                .Sequence()
                    .Conditional(new HasMorePositionToGrabCondition().CheckCondition)
                        .Executor(new SuccessActionNode("1"))
                    .ConditionalLoop(GrabberStatus.GrabberReady, GrabberStatus.GrabberRunning, GrabberStatus.GrabberAcquired)
                        .Executor(new SuccessActionNode("2"))
                    .Executor(new SuccessActionNode("3"))
                .Close()
            .Close()
            .Build();
    }
```

```CSharp
using System;
using System.Collections.Generic;

public abstract class BehaviorNode
{
    public abstract BehaviorStatus Tick();
}

public enum BehaviorStatus
{
    Success,
    Failure,
    Running
}

public enum ConditionStatus
{
    True,
    False
}
public enum GrabberStatus
{
    GrabberReady,
    GrabberRunning,
    GrabberAcquired
}

public class BehaviorRootNode : BehaviorNode
{
    private BehaviorNode rootNode;

    public BehaviorRootNode(BehaviorNode rootNode)
    {
        this.rootNode = rootNode;
    }

    public override BehaviorStatus Tick()
    {
        return rootNode.Tick();
    }
}

public class BehaviorTreeBuilder
{
    private string name;
    private BehaviorRootNode rootNode;
    

    public BehaviorTreeBuilder(string name)
    {
        this.name = name;
        //this.rootNode = new BehaviorRootNode(); // Root node of the behavior tree
    }

    public BehaviorTreeBuilder Selector()
    {
        // Implementation for Selector node
        Console.WriteLine("Selector node created.");
        return this;
    }

    public BehaviorTreeBuilder Sequence()
    {
        // Implementation for Sequence node
        Console.WriteLine("Sequence node created.");
        return this;
    }

    public BehaviorTreeBuilder Conditional(Func<bool> condition)
    {
        // Implementation for Conditional node
        Console.WriteLine("Conditional node created.");
        return this;
    }

    public BehaviorTreeBuilder ConditionalLoop(params GrabberStatus[] statuses)
    {
        // Implementation for ConditionalLoop node
        Console.WriteLine("ConditionalLoop node created.");
        return this;
    }

    public BehaviorTreeBuilder Executor(SuccessActionNode executor)
    {
        // Implementation for Executor node
        Console.WriteLine("Executor node created.");
        return this;
    }

    public BehaviorRootNode Build()
    {
        // Implementation to finalize the behavior tree
        Console.WriteLine("Behavior tree built.");
        return rootNode;
    }

    public BehaviorTreeBuilder Close()
    {
        // Implementation to close a group of nodes
        Console.WriteLine("Closing node group.");
        return this;
    }
}


public class ActionNode : BehaviorNode
{
    private Func<BehaviorStatus> action;

    public ActionNode(Func<BehaviorStatus> action)
    {
        this.action = action;
    }

    public override BehaviorStatus Tick()
    {
        return action();
    }
}

public class ConditionNode : BehaviorNode
{
    private Func<ConditionStatus> condition;

    public ConditionNode(Func<ConditionStatus> condition)
    {
        this.condition = condition;
    }

    public override BehaviorStatus Tick()
    {
        if (condition() == ConditionStatus.True)
        {
            return BehaviorStatus.Success;
        }
        else
        {
            return BehaviorStatus.Failure;
        }
    }
}

public class HasMorePositionToGrabCondition
{
    public bool CheckCondition()
    {
        // Implementation for checking condition
        Console.WriteLine("Checking condition: Has more position to grab?");
        return true; // Replace with actual condition
    }
}

public class CheckGrabberStatusCondition
{
    public CheckGrabberStatusCondition(params GrabberStatus[] statuses)
    {
        // Implementation for checking grabber status
        Console.WriteLine("Checking grabber status...");
    }
}

public class SleepActionNode
{
    public SleepActionNode(Func<bool> condition)
    {
        // Implementation for sleep action node
        Console.WriteLine("Sleeping...");
    }
}

public class CreateGrabCommandBlockWithPositionCheck
{
    public CreateGrabCommandBlockWithPositionCheck()
    {
        // Implementation for creating grab command block
        Console.WriteLine("Creating grab command block with position check...");
    }
}

public class SuccessActionNode
{
    public SuccessActionNode(string msg)
    {
        // Implementation for success action node
        Console.WriteLine("Success action node executed." + msg);
    }
}

class Program
{
    public static void Main(string[] args)
    {
        // Get the behavior tree
        BehaviorRootNode laneTree = GetLaneTree();
        Console.WriteLine("Behavior tree created successfully.");
    }

    public static BehaviorRootNode GetLaneTree()
    {
        return new BehaviorTreeBuilder("TeachingBehaviorTree")
            .Selector()
                .Sequence()
                    .Conditional(new HasMorePositionToGrabCondition().CheckCondition)
                        .Executor(new SuccessActionNode("1"))
                    .ConditionalLoop(GrabberStatus.GrabberReady, GrabberStatus.GrabberRunning, GrabberStatus.GrabberAcquired)
                        .Executor(new SuccessActionNode("2"))
                    .Executor(new SuccessActionNode("3"))
                .Close()
            .Close()
            .Build();
    }
}

```

## Example 5: Specify branch
```CSharp
public static BehaviorRootNode GetLaneTree()
{
    return new BehaviorTreeBuilder("TeachingBehaviorTree")
        .Selector()
            .Sequence()
                .Conditional(new HasMorePositionToGrabCondition().CheckCondition)
                    .Executor(new SuccessActionNode("1"))
                .Close() // Close branch
                .Sequence()
                    .ConditionalLoop(GrabberStatus.GrabberReady, GrabberStatus.GrabberRunning, GrabberStatus.GrabberAcquired)
                        .Executor(new SuccessActionNode("2"))
                    .Close()
                    .Executor(new SuccessActionNode("3"))
                .Close()
            .Close()
        .Close()
        .Build();
}

```

```CSharp
public static BehaviorRootNode GetLaneTree()
{
    return new BehaviorTreeBuilder("TeachingBehaviorTree")
        .Selector()
            .Sequence()
                .Conditional(new HasMorePositionToGrabCondition().CheckCondition)
                    .Executor(new SuccessActionNode("1"))
                .Close()
                .Sequence()
                    .ConditionalLoop(GrabberStatus.GrabberReady, GrabberStatus.GrabberRunning, GrabberStatus.GrabberAcquired)
                        .Executor(new SuccessActionNode("2"))
                    .Close()
                    .Executor(new SuccessActionNode("3"))
                .Close()
            .Close()
        .Close()
        .Build();
}

```