---
layout: post
title: Difference between .ignore(…) and getline(…) for skipping lines in C++
---

Here I want to compare the difference between .ignore() and .getline() and see which one provides more restrictions. If we want to skip a few lines before reading the actual data, there are two main methods that could be used: `.ignore(...)` and `getline(...)`

<!--more-->

`inFile.open(FILE_NAME);`


## .ignore(..)

`inFile.ignore(LONG_MAX, '\\n');//Ignore first line`

Using ignore, the number of characters provided as the first argument will be ignored OR until the delimiter is reached ('\\n'). Here, we provided LONG_MAX as a very long number of characters.

In a modern compiler, having LONG_MAX characters in a single line is equal to more than 1 Million TeraBytes (TB)!


<img src="/images/media_ignore/image1.png" style="width:11.33333in;height:1.9375in" alt="Machine generated alternative text: main.cpp @ saving... *include &lt;iostream&gt; clang version 7 . O. .18 clang++—7 —pthread —std=c++17 . &#39;main Long limit: 9223372036854775807 .04.1 (tags/RE1_EASE 700/ - na —o mal n mal n. cpp *include int main() { std: : cout &quot;Long std: : cout &quot;Long bits)): \n&quot;&lt;&lt;(LONG limit: limit in Terabytes (2A4e characters TBS&quot;; (8 Long limit in Terabytes (2A40 characters (8 bits) ) : 1048575 " />

 

 

## getline(..)
`string junk;`
`getline(inFile, junk);`

Here, using getline, a junk string is created and used to STORE one line of input. String is in memory, so if there are n characters in the first line, we need at least n+1 bytes in memory.

# Comparison

It seems like getline() does not restrict the number of characters. Comparing with .ignore however, getline() tries to STORE the first line in memory! While .ignore() simply IGNORES the first n characters.

 

Let's see what happens in reality. If these assumptions are correct, for getline() to read LONG_MAX characters and store it in &lt;junk&gt; variable, we need more than 1 Million TBs of main memory!

 

## Creating test file (37 GB)

I only have 16GB of RAM in my computer. In the code below, I tried to create a 37GB file in my computer (two lines, first line being 37GB and the second containing a small string).

<img src="/images/media_ignore/image2.png" style="width:6.79167in;height:0.33333in" />

<img src="/images/media_ignore/image3.png" style="width:7.32292in;height:8.22917in" alt="Machine generated alternative text: ofstream outFi1e; //Create a 37GB file. outFi1e. open (FILE_NAME)•, for (long 1 = e; 1 &lt; leeeeeee; 1++) outFi1e &quot;C++C++C++C++C++C++C++C++C++C++C++C++C++C++C++ if (1 % leeeee e) { outFi1e.f1ush(); &quot;Flushed at cout outFi1e &lt;&lt; endl;//End of first line outFi1e.f1ush(); outFi1e. close(); for (long 1 outFi1e outFi1e outFi1e if (1 % = e; 1 &lt; le•, &quot;SecondLine leeeee e) { &quot;Flushed at outFi1e cout cout cout &quot;Before &quot;Done! &quot; ; endl;//End second line close. " />

The created file:

<img src="/images/media_ignore/image4.png" style="width:7.95833in;height:2.07292in" alt="This PC OS(C) IgnoreTest Name getline. txt Date modified 2020-04-07 10-49 PM Type Text Document Size 36.386719 KB " />

 

## Comparing both methods

Then I tried both methods:

<img src="/images/media_ignore/image5.png" style="width:10.22917in;height:6.6875in" alt="Machine generated alternative text: cout &lt;&lt; &quot;Openning file cout FILE NAME if (METHOD IGNORE) for reading... \n&quot;, file oppened succesfully for reading.. .\n&quot;, &lt;&lt; &quot;Before ignoring the first line using ignore(LONG_MAX, cout inFi1e.ignore(LLONG_MAX, &#39;\n&#39; ) ; // Ignore first line (N37GB) &lt;&lt; &quot;After ignoring the first line cout else if (METHOD - GETLINE) { &lt;&lt; &quot;Before ignoring the first line using get1ine(inFi1e, junk) cout string junk; get1ine(inFi1e, junk); &lt;&lt; &quot;After ignoring the first line using get line(inFi1e, junk) cout cout &lt;&lt; &quot;Before reading the second line\n&quot; • string secondLine; get1ine(inFi1e, secondLine); cout cout secondLine; &quot;All done! \n " />

 

# Results

## .ignore()

 

Using ignore, the computer used less than 1 MB of memory (920 KB) and finished in around 20 seconds and read the second file successfully. I shot this when the application was running using visual studio performance profiling.

<img src="/images/media_ignore/image6.png" style="width:8.67708in;height:6.03125in" alt="Machine generated alternative text: Report20200407-2329.diagsession O Stop Collection Output Diagnostics session: 12 seconds Memory (KB) 920 @ Take snapshot Force GC Take snapshot Test.cpp x Zoom In A Zoom Out Reset View IOS 40s C:\Users\erfan\OneDrive\@2020\University\CS 110\Test\Release\Test.exe Openning file for reading... C: \ IgnoreTest\get1ine.txt file oppened succesfully for reading... Before ignoring the first line using ignore(LONG_MAX, &#39;) (N37GB) " />



## getline()

getline method consumed computer memory quite quickly! In four seconds, it reached 1 GB of memory and then … it stopped reading. The result? Nothing! It did not read the second line at all! It just gave up it seems and the input buffer was probably broken.

<img src="/images/media_ignore/image7.png" style="width:11.21875in;height:8.27083in" alt="Machine generated alternative text: Report20200407-2328.diagsession Test.cpp x O Stop Collection Output Zoom In Zoom Out Reset View Diagnostics session: 4 seconds IOS Memory (GB) @ Take snapshot Force GC Take snapshot 40s 1:00min C:\Users\erfan\OneDrive\@2020\University\CS 1 Openning file for reading... C: \ IgnoreTest\get1ine.txt file oppened succesfully for reading... Before ignoring the first line using get1ine(inFi1e, junk) (N37GB) " />

<img src="/images/media_ignore/image8.png" style="width:18.3125in;height:3.84375in" alt="Machine generated alternative text: Test.cpp Report20200407-2328.diagsessiori x Output Reset Zoom Clear Selection Zoom In Diagnostics session: 8.817 seconds 500ms Memory (GB) Is 1.5s 2.5s 35 3.5s 45 4.5s 5.5s 6s A No snapshots were taken in the profiling session. In order to view application memory details, please start a new profiling session and take at least one snapshot using the &quot;Take snapshot&quot; button " />

# Conclusion

Although it may seem that .ignore() does not read very long files and has more restriction than .getline(), however, a real-world testing revealed that in practice .getline() cannot even ignore read a 37GB line in a computer with 16GB of memory and .ignore() is a more appropriate method for skipping very long lines up to 1 million Terabytes (in theory at least).

It makes sense from memory point of view as well. If we do not need a few lines of inputs, we should IGNORE them rather than STORING them in the memory using getline().

Copyright © 2020 Baran Erfani (baranerf.github.io)
