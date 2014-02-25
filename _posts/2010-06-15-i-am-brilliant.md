---
layout: post
title: "I'm Brilliant"
---

My daughter had some maths homework based around the alphabet with each letter representing a number i.e. a = 1, b = 2 etc. One of the questions was to 
find the Scottish Football team that added up to 100 using the letter values.

My wife and daughter started to do this manually but after a while gave up. I thought I'd get creative and use some of my basic programming skills to 
come up with the answer. Using Excel I produced a macro which worked out the answer.

{% highlight vbnet linenos %}
Sub Macro1()
	Dim t As Integer ''number of teams - 42 in this case
	Dim i As Integer ''counter for length of team name
	Dim z As String ''counter for each letter in the team name
	Dim a As Integer ''counter for number of elements in the arrays
	Dim l As Variant ''letter array
	Dim v As Variant ''value array
	Dim c As Integer ''the counter for the value of the letters
	nl = Array("a", "b", "c", "d", "e", "f", "g", "h", "i", "j", "k", "l", "m", "n", "o", "p", "q", "r", "s", "t", "u", "v", "w", "x", "y", "z")
	v = Array(1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26)
	For t = 1 To 42
		Range("A" & t).Select
		c = 0
		For i = 1 To Len(ActiveCell.Value)
			z = Mid(ActiveCell.Value, i, 1)
			For a = 0 To 25
				If StrComp(z, l(a)) = 0 Then
					c = c + v(a)
				End If
			Next a
		Next i
		Range("B" & t).Select
		ActiveCell.Value = c
	Next t
End Sub
{% endhighlight %}

By the way, the answer is Cowdenbeath.