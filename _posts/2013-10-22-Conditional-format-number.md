---
layout: post
title: "Conditional formatting for number"
description: ""
category: "Csharp"
tags: [Number]
---
{% include JB/setup %}

        [Test]
        public void ConditionalFormattingNumber()
        {
            const decimal positiveNumber = 100;
            const decimal negNumber = -100;
            const decimal zero = decimal.Zero;

            const string threePartFormat = "(pos)#.##;(net)#.##;(zero number)";
            Console.WriteLine(positiveNumber.ToString(threePartFormat));
            Console.WriteLine(negNumber.ToString(threePartFormat));
            Console.WriteLine(zero.ToString(threePartFormat));
        }

![Result](https://gpbz0a.blu.livefilestore.com/y2pQND6_3kySxaU6770oi3PkhTcSF1PIrmXFqhRXL_OQRn4TCX50DeJthhbyi0XzyvVBSmzqwWc3mgtaQR66TR1rhKabGh_kB-Epw5t1Ct8waU/Conditional-formatting-number.PNG?psid=1)

notice the `;` of the part
**(pos)#.##;(net)#.##;(zero number)**