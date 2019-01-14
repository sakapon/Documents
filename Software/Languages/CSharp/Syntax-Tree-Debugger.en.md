# Build your own debugger using Roslyn's syntax analysis
// This is the 23rd article of [C# Advent Calendar 2018 in Japan](https://qiita.com/advent-calendar/2018/c-sharp).

I tried to make something like a debugger.

### Motivation
- I normally use Visual Studio, but it is troublesome to step through code manually at debugging
  - When the number of steps is large in a loop or the like
  - When I want to check the state of branching or variables lightly

### Solution
- Specify only the time interval of the step and let the debugger run automatically
  - A list of variables is displayed
  - Time interval can be adjusted in real time
- Using the syntax analysis of .NET Compiler Platform (Roslyn), it seems to be realizable if we insert debugging code between each step

### Result
So, as a result of trying to make prototype "Tick-tack Debugger" with WPF, it looked like this.  
As an example, we are figuring out a square root by the Newton's method. (Click to enlarge)

![](https://github.com/sakapon/Samples-2018/blob/master/Images/SyntaxTreeSample/TickTackDebugger.gif)

### Commentary
The following is an outline technical explanation.  
Before creating a WPF application, first try using the console application on the .NET Framework.  
To use C# syntax analysis, install [Microsoft.CodeAnalysis.CSharp](https://www.nuget.org/packages/Microsoft.CodeAnalysis.CSharp) with NuGet.

It is a policy to insert debugging code into the source code to be debugged, compile it dynamically and execute it.  
The source code of the console application is shown below (the whole solution is in [SyntaxTreeSample](https://github.com/sakapon/Samples-2018/tree/master/SyntaxTreeSample)).

https://gist.github.com/sakapon/f6366ea8353c565757073fbd3727598e

In the SyntaxHelper class, C# source code to be debugged is converted to a syntax tree and scanned, and a line of code for debugging is inserted before each statement.

By using the `CSharpSyntaxTree.ParseText` method, you can convert the source code into a SyntaxTree object.  
Also, the superclass representing all nodes, such as methods, statements and expressions, is the SyntaxNode class,
- Parent property
- Ancestors method
- ChildNodes method
- DescendantNodes method

if you know that there is, you can do a rough search.

In addition to this, DebuggerLib is created as a class library that defines methods called from code for debugging.
Let this library go through to notify the position of each statement and the variables and their values that exist immediately before it.

In the Program class, after saving the generated debugging source code to a file, compile it using CodeDomProvider in the System.CodeDom.Compiler namespace, and call its entry point (the Main method).  
It also registers the event handler when the debug code is executed, and stops it for the specified time using the Thread.Sleep method.

Now, if the original source code to be debugged is Program.cs, the following Program.g.cs is generated as the debugging source code.

https://gist.github.com/sakapon/e418ec76781dcff701bd61692f03890c

When we run the console application we created, it will look like the following figure (time interval is 0.3 seconds).

![](https://github.com/sakapon/Samples-2018/blob/master/Images/SyntaxTreeSample/DebuggerConsole.gif)

Based on the above, we created a debugging tool as the WPF application.  
The part of the C# source code on the left is a TextBox and can be edited.
When debugging is executed, highlighting is done by setting each statement to the selected state.  
The part where the variable list is displayed on the right is a DataGrid.

![](https://github.com/sakapon/Samples-2018/blob/master/Images/SyntaxTreeSample/TickTackDebugger-Pi.png)

(An example of figuring out the pi)

This time I tried to make a prototype by the above method, but I think that there is a smart way to insert debug code and compile.

### Remarks
- We can not deal with all possible statements. Also, we only scan the Main method.
- The assembly (EXE) generated at compile time is saved in the `%TEMP%` folder (user's `AppData\Local\Temp`).
- In the TextBox, setting IsInactiveSelectionHighlightEnabled to True may not work.  
  In addition, highlights in the selected state may shift.  
  It might be better to use Run etc in RichTextBox.

### Samples Created
- [SyntaxTreeSample (GitHub)](https://github.com/sakapon/Samples-2018/tree/master/SyntaxTreeSample)

### Version
- .NET Framework 4.7
- [Microsoft.CodeAnalysis.CSharp](https://www.nuget.org/packages/Microsoft.CodeAnalysis.CSharp) 2.10.0
- [ReactiveProperty](https://www.nuget.org/packages/ReactiveProperty/) 5.3.2

### References
- [.NET Compiler Platform SDK (Roslyn API)](https://docs.microsoft.com/en-us/dotnet/csharp/roslyn-sdk/)
