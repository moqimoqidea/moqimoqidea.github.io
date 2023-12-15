---
title: Vba With Chatgpt
date: 2023-12-15T15:51:54+08:00
lastmod: 2023-12-15T15:51:54+08:00
author: moqimoqidea
categories:
  - category1
tags:
  - tag1
  - tag2
draft: true
---

通过与 ChatGPT 不断交流，完成 VBA 代码的编写。

<!--more-->

## 使用 GPT4 生成 VBA 代码

### 起因

某天，一个朋友发我一个 Excel 截图如下:

![20231207-screen-excel](/images/20231207-screen-excel.jpg)

并问我: `有什么办法，能按照这个数量，然后把表格分成这么多行，把数量都拆成1`

### 梳理需求

以直白的文字梳理需求如下:

输入:

```txt
2 AA
3 BBB
```

运行代码后，输出:

```txt
1 AA
1 AA
1 BBB
1 BBB
1 BBB
```

### 开始工作

考虑到我这位朋友没有编程环境，所以我想到了用 Excel VBA 来实现。但是由于我对 VBA 不了解，所以我想到了用 GPT4 来生成 VBA 代码。

经过多轮调试，最终交付的代码如下:

```vba
Sub SplitQuantities()
    Dim ws As Worksheet
    Set ws = ActiveSheet ' Or set a specific worksheet

    ' Define variables
    Dim StartRow As Long, EndRow As Long, QuantityColumn As Long
    Dim i As Long, j As Long, Quantity As Long

    ' Prompt the user for input
    StartRow = Application.InputBox("Enter the starting row number:", "Start Row", Type:=1)
    EndRow = Application.InputBox("Enter the ending row number:", "End Row", Type:=1)
    QuantityColumn = Application.InputBox("Enter the quantity column number:", "Quantity Column", Type:=1)

    ' Ensure StartRow is greater than EndRow
    If EndRow > StartRow Then
        Dim temp As Long
        temp = StartRow
        StartRow = EndRow
        EndRow = temp
    End If

    Application.ScreenUpdating = False ' Turn off screen updating

    ' Traverse from the last row upwards
    For i = StartRow To EndRow Step -1
        ' Check if Quantity is numeric and greater than 1
        If IsNumeric(ws.Cells(i, QuantityColumn).Value) And ws.Cells(i, QuantityColumn).Value > 1 Then
            Quantity = ws.Cells(i, QuantityColumn).Value
            ' Insert the required number of new rows with quantity 1
            ws.Rows(i).Copy
            ws.Rows(i + 1).Resize(Quantity - 1).Insert Shift:=xlDown
            ws.Cells(i, QuantityColumn).Resize(Quantity).Value = 1 ' Set quantity to 1 for all affected rows
        End If
    Next i

    Application.CutCopyMode = False ' Clear the clipboard
    Application.ScreenUpdating = True ' Turn screen updating back on
End Sub
```

### 交付用户

通过与朋友沟通让其安装 Excel 宏，然后运行 Excel 宏。每次他需要输入三个参数，分别是 `起始行`、`结束行`、`数量列`。然后就可以得到结果了。

朋友使用后觉得非常神奇，我也非常开心。

### 下一步

代码还有可以优化的地方，当前的性能不够优秀。逻辑上其从最后一行开始遍历，如果遇到数量大于 1 的行，就复制一行，然后把数量减 1，直到数量为 1 为止。这里是否可以在第一步就把该复制的行数计算完毕？只是更新每一行的内容？交给后面得空探索。
