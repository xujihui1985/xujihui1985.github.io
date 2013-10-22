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

<iframe src="https://skydrive.live.com/embed?cid=9DA6FDBDFB12F7A9&resid=9DA6FDBDFB12F7A9%21356&authkey=ACNnXyzknF6QI3Y" width="319" height="140" frameborder="0" scrolling="no"> </iframe>

notice the `;` of the part
**(pos)#.##;(net)#.##;(zero number)**
