---
categories: "ニューラルネットワーク"
title: "活性化関数#1"

---

## 活性化関数

活性化関数は、入力の総和をもとに、どのように発火するかを決定する。

以下のようなものがある。
- 恒等関数
- ステップ関数
- ReLU関数
- シグモイド関数

## 恒等関数

恒等関数はそのものを返す。

```vb
Function identity_function(x)
    identity_function = x
End Function
```

引数をそのまま返すだけ。あまり意味ない。

クラスを作ってみる。

```vb
Class C_Num
    Private m_mode

    Public Property Let mode(val)
        m_mode = LCase(val) '文字列を小文字に置き換えて格納する
    End Property

    Public Function activation(x)
        Select Case m_mode
        Case "", "identity"
            activation = x
        End Select
    End Function
End Class
```

次のように使う。まだ、あまり意味ない。

```vb
Dim nb
Set nb = New C_Num
nb.mode = "identity"
MsgBox nb.Activation(1) '>1
```

恒等関数はあとで扱うことにする。次はステップ関数。

## ステップ関数

ステップ関数は1か0を返す。発火するかしないかの2状態を表す。

```vb
Function step_function(x)
    If x > 0 Then
        step_function = 1 
    Else
        step_function = 0
    End If
End Function
```

引数が値だけでなく、配列である場合にも対応させる。

```vb
Function step_function(x)
    Dim buf()
    Dim i
    
    If IsArray(x) Then
        ReDim buf(UBound(x))
        For i = 0 To UBound(x)
            buf(i) = step_function(x(i))
        Next
        step_function = buf
    Else
        If x > 0 Then
            step_function = 1 
        Else
            step_function = 0
        End If
    End If
End Function
```

値や配列を表示させる関数を作る。

```vb
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
Print step_function(Array(1,0,0.5)) '>[1,0,1]
```

## 恒等関数とステップ関数をまとめる

以上をまとめたクラスを作る。

```vb
Class C_Num
    Private m_mode

    Public Property Let mode(val)
        m_mode = LCase(val) '文字列を小文字に置き換えて格納する
    End Property


    Public Function Activation(x)
        Dim buf()
        Dim i
        
        If IsArray(x) Then
            ReDim buf(UBound(x))
            For i = 0 To UBound(x)
                buf(i) = Activation(x(i))
            Next
            Activation = buf
        Else
            Select Case m_mode
            Case "identity"
                Activation = x

            Case "step"
                If x > 0 Then
                    Activation = 1 
                Else
                    Activation = 0
                End If
            
            Case Else
                'modeを設定し忘れた場合は、値0を返す
                Activation = 0

            End Select
        End If
    End Function


    Private Function ToString(x)
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


    Public Function Print(x)
        Msgbox ToString(x)
    End Function

End Class
```

使用例。

```vb
Dim nb
Set nb = New C_Num
nb.mode = "identity" '恒等関数
nb.Print nb.Activation(Array(0,0.5,1)) '>[0,0.5,1]
    
nb.mode = "step" 'ステップ関数
nb.Print nb.Activation(Array(0,0.5,1)) '>[0,1,1]
```

## ReLU関数

Rectified Linear Unitの略。「整流された線形ユニット」の意味になる。立ち上がり電圧分だけ横にずらした、ダイオードの電圧-電流特性に似ている。

注: ユニットが関数を意味しているので、ReLU関数と書くと「整流された線形**関数関数**」の意味になってしまうらしいですが、ここでは気にせずReLU関数と書きます。
{: .notice}

```vb
Function relu_function(x)
    If x > 0 Then
        relu_function = x 
    Else
        relu_function = 0
    End If
End Function
```

## シグモイド関数

シグモイド関数はニューラルネットワークの活性化関数として古くから使われてきたが、現在はReLU関数が使われることが多い（本の受け売り）。


```vb
Function sigmoid_function(x)
    sigmoid_function = 1/(1 + Exp(-x))
End Function
```

先ほど作ったクラスにReLU関数とシグモイド関数を追加する。

```vb
Class C_Num
    Private m_mode

    Public Property Let mode(val)
        m_mode = LCase(val) '文字列を小文字に置き換えて格納する
    End Property


    '値や配列に活性化関数を作用させる
    Public Function Activation(x)
        Dim buf()
        Dim i
        
        If IsArray(x) Then
            ReDim buf(UBound(x))
            For i = 0 To UBound(x)
                buf(i) = Activation(x(i))
            Next
            Activation = buf
        Else
            Select Case m_mode
            Case "identity" '恒等関数
                Activation = x

            Case "step" 'ステップ関数
                If x > 0 Then
                    Activation = 1 
                Else
                    Activation = 0
                End If

            Case "relu" 'ReLu関数
                If x > 0 Then
                    Activation = x
                Else
                    Activation = 0
                End If

            Case "sigmoid" 'シグモイド関数
                Activation = 1/(1 + Exp(-x))
            
            Case Else
                'modeを設定し忘れた場合は0を返す
                Activation = 0

            End Select
        End If
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
