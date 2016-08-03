---
title: 如何用脚本自动将一个大的Excel文件分割保存成多个Excel文件
date: 2016-08-03 13:30:15
tags: [execl,拆分]
---
将一个Execl按行数拆分为不同的excel文件
<!-- more -->
1. 打开office excel,在sheet表标签上右键 -> 查看代码
2. 将以下代码复制进去
```vba
 Function RowSplit(SourceFileName, DestFileName)
' 功能描述：将数据行数过多的Excel文件分割成多个Excel文件
' 输入：源文件名，目的文件名
' 输出：返回分割是否完全成功，以及分割成功的文件
' 创建人：斯文兔
' 创建时间： 2005-1-6
' 最后修改时间：2005-1-6

Dim intSourceIndex
Dim intSourceRowNO
Dim ForNumber
Dim intDestIndex
Dim xlApp
Const intSplitNO = 1000 '分割行数

 '创建Excel对象
 Set xlApp = CreateObject("EXCEL.APPLICATION")

 '打开选择的Excel文件
    xlApp.Workbooks.Open SourceFileName
    intSourceIndex = xlApp.Workbooks.Count
    xlApp.Workbooks(intSourceIndex).Sheets(1).Select

 '获得数据行数
 intSourceRowNO = xlApp.ActiveSheet.UsedRange.Rows.Count

    '计算循环次数
    If (intSourceRowNO - 1) / intSplitNO = (intSourceRowNO - 1) \ intSplitNO Then
        ForNumber = (intSourceRowNO - 1) \ intSplitNO
    Else
        ForNumber = (intSourceRowNO - 1) \ intSplitNO + 1
    End If

    '判断是否需要分割
 If ForNumber < 2 Then
        RowSplit = False
        Exit Function
    End If

    MsgBox "数据量大于" & intSplitNO * 2 & "行，按 " & intSplitNO & "行一个文件进行分割"

 '开始分割
    For i = 1 To ForNumber
  '复制表头
        xlApp.Rows("1:1").Select
        xlApp.Selection.Copy
        xlApp.Workbooks.Add
        intDestIndex = xlApp.Workbooks.Count
        xlApp.Range("A1").Select
        xlApp.ActiveSheet.Paste
  '复制数据
  xlApp.Workbooks(intSourceIndex).Activate
        xlApp.Rows((i - 1) * intSplitNO + 2 & ":" & i * intSplitNO + 1).Select
        xlApp.Selection.Copy
        xlApp.Workbooks(intDestIndex).Activate
        xlApp.Range("A2").Select
        xlApp.ActiveSheet.Paste
  '关闭Excel的自动提示
        xlApp.DisplayAlerts = False
  '删除新建文件形成的两个空Sheet
        xlApp.Workbooks(intDestIndex).Sheets(2).Delete
        xlApp.Workbooks(intDestIndex).Sheets(2).Delete
  '打开Excel的自动提示
        xlApp.DisplayAlerts = True
        xlApp.Range("A1").Select
  '保存数据
        xlApp.ActiveWorkbook.SaveAs Left(DestFileName, Len(DestFileName) - 4) & i & Right(DestFileName, 4)
  '关闭保存好的文件
        xlApp.ActiveWindow.Close
    Next
 '关闭源文件
    xlApp.Workbooks(intSourceIndex).Close

 '关闭并清除Excel对象
 xlApp.Quit
 Set xlApp = Nothing

    '返回执行成功
 RowSplit = True

End Function


Sub SplitAll()
    Dim strSourceFilename
    Dim strDestFilename

    '创建Excel对象
 Set xlApp = CreateObject("EXCEL.APPLICATION")

 '选择要分割的文件
 strSourceFilename = xlApp.GetOpenFilename
    strDestFilename = strSourceFilename
'    strDestFilename = xlApp.GetSaveAsFilename

    '如果选择了文件，则开始做分割
 If strSourceFilename <> "False" And strDestFilename <> "False" Then
        If RowSplit(strSourceFilename, strDestFilename) Then
   MsgBox "文件分割成功"
  Else
   MsgBox "文件分割失败"
  End If
    End If
 '关闭并清除Excel对象
 xlApp.Quit
 Set xlApp = Nothing

End Sub
```
3. F5运行代码,选择要操作的文件,之后按提示操作
