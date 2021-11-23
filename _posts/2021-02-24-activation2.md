---
categories: "ニューラルネットワーク"
title: "活性化関数#2"

---

## ソフトマックス関数

ニューラルネットワークを分類問題に用いる場合、出力層の活性化関数としてソフトマックス関数が使われる。

```vb
'xは配列
Function softmax_function(x)
    Dim c
    Dim sum
    Dim i

    c = max(x)
    For i = 0 To UBound(x)
        sum = sum + Exp(x(i) - c)
    Next

    Dim buf()
    ReDim buf(UBound(x))

    For i = 0 To UBound(x)
        buf(i) = Exp(x(i) - c) / sum
    Next

    softmax_function = buf
End Function


'配列xの最大の要素を返す
Function max(x)
    Dim i
    max = x(0)
    For i = 1 To UBound(x)
        If x(i) > max Then
            max = x(i)
        End If
    Next
End Function
```

前回作成した下記関数も使って

```vb
'配列を[a,b,c]のような文字列へ変換する
Function ToString(x)
    Dim buf()
    Dim i
    
    If IsArray(x) Then
        ReDim buf(UBound(x))
        For i = 0 To UBound(x)
            buf(i) = ToString(x(i)) 
        Next
        ToString = "[" & Join(buf, ",") & "]"
    Else
        ToString = x
    End If
End Function


Function Print(x)
    Msgbox ToString(x)
End Function
```

使用例。

```vb
Print softmax_function(Array(0,1,2)) '>[0.090,0.245,0.665](結果を丸めてます)
```

前回作成したクラスへ、ソフトマックス関数を組み込んでみる。

整理のため、前回のActivation関数をp_Activation関数へ変更しました。`p_`はPrivateであることを意味してます。
{: .notice}

```vb
Class C_Num
    Private m_mode

    Public Property Let mode(val)
        m_mode = LCase(val) '文字列を小文字に置き換えて格納する
    End Property


    '値や配列に活性化関数を作用させる
    Public Function Activation(x)
        If m_mode = "softmax" Then
            Activation = p_Softmax(x)
        
        Else
            Activation = p_Activation(x)
        
        End If
    End Function


    Private Function p_Activation(x)
        Dim buf()
        Dim i
        
        If IsArray(x) Then
            ReDim buf(UBound(x))
            For i = 0 To UBound(x)
                buf(i) = p_Activation(x(i))
            Next
            p_Activation = buf
        Else
            Select Case m_mode
            Case "identity" '恒等関数
                p_Activation = x

            Case "step" 'ステップ関数
                If x > 0 Then
                    p_Activation = 1 
                Else
                    p_Activation = 0
                End If

            Case "relu" 'ReLu関数
                If x > 0 Then
                    p_Activation = x
                Else
                    p_Activation = 0
                End If

            Case "sigmoid" 'シグモイド関数
                p_Activation = 1/(1 + Exp(-x))
            
            Case Else
                'modeを設定し忘れた場合は0を返す
                p_Activation = 0

            End Select
        End If
    End Function


    'ソフトマックス関数
    Private Function p_Softmax(x)
        Dim c
        Dim sum
        Dim i

        c = max(x)
        For i = 0 To UBound(x)
            sum = sum + Exp(x(i) - c)
        Next

        Dim buf()
        ReDim buf(UBound(x))

        For i = 0 To UBound(x)
            buf(i) = Exp(x(i) - c) / sum
        Next

        p_Softmax = buf
    End Function


    '配列xの最大の要素を返す
    Public Function max(x)
        Dim i
        max = x(0)
        For i = 1 To UBound(x)
            If x(i) > max Then
                max = x(i)
            End If
        Next
    End Function


    '配列を[a,b,c]のような文字列へ変換する
    Private Function ToString(x)
        Dim buf()
        Dim i
        
        If IsArray(x) Then
            ReDim buf(UBound(x))
            For i = 0 To UBound(x)
                buf(i) = ToString(x(i)) 
            Next
            ToString = "[" & Join(buf, ", ") & "]"
        Else
            ToString = x
        End If
    End Function


    Public Function Print(x)
        Msgbox ToString(x)
    End Function

End Class
```
