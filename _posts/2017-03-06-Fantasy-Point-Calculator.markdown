---
layout: post
title:  "Fantasy Point Calculator"
date:   2017-03-06 15:40:56
tags: Java, JComponent, Fantasy Basketball, Calculator
---

This is my first program ever! It is built usisng Java.

In my free time, I play fantasy basketball sometimes, a game that compares the statistic of real-life players, particularly daily fantasy basketball site like DraftKing.com. Every day, you have a budget of 22k to assemble your own team with 10 players. Star players who perform well consistently tend to have high salaries, whereas bench players who rarely play cost you only the minimum price 3k. 

However, some players, underrated by the system, can be relatively cheap but perform almost as well as the star players.

To identify these players, I created an easy calculator that allows inputs of player's name, salary, and average fantasy points and provides outputs of fantasy points per salary. In addition, the calculator is going to store all the results. 

{: .center}
![Fantasy Point Calculator]({{ site.url }}/img/posts/DFP_cal.png)

The whole program is made of JComponent of Java class. I created a lot of JTextField for all the input and a JButton for the resulting output. Then, I appended all the JTextField and JButton into a JPanel and then to a JFrame. Eventually, I created the scrollable JPanel to store the result and set the JFrame to visible. You can check out the code on my [github page].

[github page]: https://github.com/edward0414/FantasyPoint_calculator
