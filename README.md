# redjinator-process-portfolio
# Process Portfolio
**Student:** *Reginald McPherson*<br>
**Course:** *DGL-104 Application Development Foundations*<br>
**Term:** *Winter 2022*


## Week 1

**RESEARCH: potential "textbook production" technologies that we could use for the purposes of our course textbook. Make a list of potential technologies (with links!). Closely examine at least one technology and write a pros and cons list as it relates to our potential use of the technology.**<br>

I looked at a few alternative avenues to the ones you described in your video including Designrr, Adobe inDesign, and Canva. I’ve focused on Canva here and I’ll start with the pros.

[Canva](https://www.canva.com) seems to have a relatively simple interface and is fairly popular for creating eBooks. You can add various types of media to projects with tools that do the heavy lifting for the layouts allowing more focus on the content rather than designing layouts. You can also choose from various design templates and access community content, as well as find support for team collaboration and commenting in real time.

There is also a whole education section [Canva for teachers and students](https://www.canva.com/education/)  that could be leveraged to our benefit. (edited in) It appears Canva is integrated into Slack as well, which could be another boon for us.

As for cons, it doesn’t look very optimized for things like coding examples and I’m doubting we could have any type of interactive examples. But it still looks like a pretty useful piece of software with a free plan so it might be something to look into for another project down the road, personal or otherwise.<br><br>

**REFLECT: on textbook that you've read for another course - what has worked, and what hasn't worked? What features would you like to see reproduced in our textbook? What should we avoid?**<br>

I’ll use my HTML & CSS Textbook. I liked how many examples there were in the context of a greater project, also there were many illustrations to help reinforce the topics at hand. I also found the case study at the end of each major chapter helped me recall the information one more time before changing subjects which I also thing helped immensely.
The first major thing I didn’t like about the textbook was manually flipping through pages. I found this to be a real bottleneck to how quickly I could find what I needed to solve a given problem in comparison to using Google, w3schools or stock overflow.

---

## Week 2

**RESEARCH: style guides. Find one for a programming language you are familiar with and give it  a careful read. What is new to you, or surprising? What is challenging, or difficult to understand?**

I’m went through the Kotlin style guide because it will also aid benefit me in DGL114. The first thing I noticed was there are a lot more “style preferences?” that I expected, but most seem to make a lot of sense and I can clearly see the advantages of becoming familiar with a style guide. I was surprised to find how many considerations there were, but they were so well documented it made it easy to comprehend the information.

Some of the topics that were new or surprising to me were;
* Tab characters not being used for indentation.
* Finding that there is a defined internet structure to code files.
* Filenames based on top-level class
* Intent to line wrap after 100 characters
* Classes are organized the same way as the code in a file
* Braces are not required for if statement bodies or when branches if they have only one else branch which fit on a single line.

I had my own quirk in style that I liked but I understand I’ll be switching styles and languages often enough that it’s always best to adopt whatever the team is using. Still I enjoyed my little style, it was clean (to me) and nice to look at. As an example, I would line up variable declaration statements after the assignment operator “=” up to tab positions so it was less of an eye sore.
```
Var myString = “this thing?”
Var myNumber = 4
Var theLostCityOfAtlantis = “Is definitely lost!”
```
I know now this adds tab characters and/or unnecessary whitespace and can cause issues with some languages.

**REFLECT: on your own coding practice. What have you done well, and where you could you improve in terms of style and best practices? Provide some examples from your own code.**

Looking back at some of my coding using pine script from the charting platform tradingview.com, I see much that could be improved. The following code is an old script of mine for a trading indicator called Relative Strength index, which i build my own version of to track multiple timeframes on a single chart.

At the time I wasn't even aware there was a [Pine script Style guide](https://www.tradingview.com/pine-script-docs/en/v5/writing/Style_guide.html), and as can be seen in the Visualization / Heat mapping features of my code below, I often repeated code or used lengthy nested if statements that could have been handled more appropriotely using use other tools such as lists and arrays..

![Image of a custom version of RSI I created translate values from 4 other timeframes and output a heat map](RSI%204xtf.png)

```
//@version=4
study(title="4x MTF RSI - by Redjinator ", precision=0)


// Timeframe/Length Adjustment
//==============================================================================
HTFLen(timeframe, length)=>
    (timeframe*length)/timeframe.multiplier



// Oversold / Overbought and Background
//==============================================================================
upLine = hline(70)
lowLine = hline(30)
fill(upLine, lowLine, color=color.purple, transp=90)



// Default RSI
//==============================================================================
src         = input(close, type=input.source, title="RSI #1: Source")
rsi_length  = input(14, title="RSI #1: Length")
rsi         = rsi(src, rsi_length)
plot(rsi, color=color.purple, transp=0)



// Alternate RSI Signals
//==============================================================================
showRSI2    = input(false, title="Display RSI #2?")
rsi2            = rsi(src, HTFLen(input(60, title="RSI #2: Timeframe"), input(14, minval=1, title="RSI #2: Length")))

showRSI3    = input(false, title="Display RSI #3?")
rsi3            = rsi(src, HTFLen(input(240, title="RSI #3: Timeframe"), input(14, minval=1, title="RSI #3: Length")))

showRSI4    = input(false, title="Display RSI #4?")
rsi4            = rsi(src, HTFLen(input(720, title="RSI #4: Timeframe"), input(14, minval=1, title="RSI #4: Length")))



// AVERAGE RSI
//==============================================================================
showAvgRSI      = input(false, title="Display Average of all 4 RSI?")
avgRSI          = avg(rsi, rsi2, rsi3, rsi4)
plot(showAvgRSI? avgRSI:na, title="RSI Average", style=plot.style_line, linewidth=2, color=color.fuchsia)



//Visualization / Heat mapping
//==============================================================================
Selected(rsiSelection) =>
    if rsiSelection == "RSI #1"
        rsi
    else
        if rsiSelection == "RSI #2"
            rsi2
        else
            if rsiSelection == "RSI #3"
                rsi3
            else
                if rsiSelection == "RSI #4"
                    rsi4
                else
                    if rsiSelection == "RSI Average"
                        avgRSI

Visualize(ind)=>
    colorBG = ind>=96? #E50100:ind<96 and ind>92? #E61400:ind<92 and ind>88? #E72700:ind<88 and ind>84? #E83B01:ind<84 and ind>80? #E94E01:ind<80 and ind>76? #EA6101:ind<76 and ind>72? #EB7502:ind<72 and ind>68? #EC8802:ind<68 and ind>64? #ED9C03:ind<64 and ind>60? #EEB003:ind<60 and ind>56? #EFC403:ind<56 and ind>52? #F0D804:ind<52 and ind>48? #F2EC04:ind<48 and ind>44? #E5F305:ind<44 and ind>40? #D2F405:ind<40 and ind>36? #C0F506:ind<36 and ind>32? #AEF606:ind<32 and ind>28? #9BF706:ind<28 and ind>24? #88F807:ind<24 and ind>20? #76F907:ind<20 and ind>16? #63FA08:ind<16 and ind>12? #50FB08:ind<12 and ind>8? #3DFC09:ind<8 and ind>4? #2AFD09:ind<4? #17FF09:na

useVisualizer   = input(false, title="Use RSI Heat-mapping?")

selection       = input(defval="RSI #1", options=["RSI #1", "RSI #2", "RSI #3", "RSI #4", "RSI Average"], title="RSI for Visualization")

rsi_4_visualizer = Selected(selection)



// Plots and Background Colors
//==============================================================================
plot(showRSI2? rsi2:na, color=color.blue,   transp=0, linewidth=1)
plot(showRSI3? rsi3:na, color=color.red,    transp=0, linewidth=1)
plot(showRSI4? rsi4:na, color=color.green,  transp=0, linewidth=1)

bgcolor(useVisualizer?  Visualize(rsi_4_visualizer):na, transp=50)
```
![User Interface for Indicator](rsi_4xtf_settings.png)

---

## Week 3

**RESEARCH: an API or library belonging to a programming language that you are familiar with.**

I started with the Bitfinex API for a cryptocurrency exchange and found that they supported Rest and web sockets as well as having a library built in multiple programming languages including the one I was looking for which was Python.

There are detailed examples on how to use the API and libraries, generally the examples will also give the type of errors returned in the event of a communication failure as well. At first, I didn’t realize why, but I realized shortly after that it was so I would know how to spot the errors and how to handle them.

Overall, the documentation is very well organized and clean looking. Finding what I need is pretty easy and I’ve started a project using the API already. (Later edit) I temporarily have ceased working on this project now until I’m done with Design patterns.

**REFLECT: on your own documentation efforts in past projects. What have you done well? What could be improved? What have you learned recently  about documentation that you hope to be able to take forward into future projects?**

I was never very good at documentation and I have found myself confused looking back on old projects quite a few times. I have a folder on my PC that has 100+ old scripts that I wrote, some are approaching 1000 lines and most of them I can’t remember what I was trying to accomplish but I keep them in case I become motivated to go through them and find any relevant code I want to keep.

Of course, back then I didn’t use any type of version control system and I ended up repeating much of my code across different files since Pine script did not support OOP. But I strive a lot more to comment my code better these days and I’m sure I’ll be able to leverage more good documentation techniques after our experience this semester.

---

## Week 4

**RESEARCH: find your community! Take some time to brainstorm your interests in the context of your personal goals. Do some searching around GitHub through open source repositories to find something that aligns with your interests. Remember to keep an eye on repository activity - make sure development is active! Does your community have a well-defined central hub (e.g. a Slack or Discord channel)? What are the key resources for your community? How will you go about finding more information as you need it? Who will you contact, or where will you look?**

I’ve finally found my community! I’m going to join the community at [TensorFlow](https://www.tensorflow.org/). I would really like to get into machine learning as it’s such a fascinating topic that can be applied to such a wide range of applications. The Github page has a detailed [contributing.md](https://github.com/tensorflow/tensorflow/blob/master/CONTRIBUTING.md), a [TensorFlow forum](https://discuss.tensorflow.org/), a [TensorFlow Blog](https://blog.tensorflow.org/), a [Youtube Channel](https://www.youtube.com/tensorflow), and many tutorials and related resources.

My plan is to start absorbing information from the website and using the resources mentioned above to start educating myself, learn the common lingo, and get myself started with a few simple examples.

Hopefully I haven’t bitten off more than I can chew here, but I’m going to go for it anyway. Plus the size of the community and the content geared towards new comers fills me with some confidence that I’ll be able to get the help I need as I more forward in complexity.

**REFLECT: on your community. After spending time engaging with your community (e.g. looking at example code, repositories, reading discussion threads and forum posts, reading PRs and comments, watching tutorials, etc.) What do you think the goals of your community are? What excites you most about your community? How do you hope to contribute to your chosen community?**

There is such a wide range of information is available it seems most of the community is using Tensorflow for research and related purposes, the online community is really robust and active. This is going to take me awhile.

---

## Week 5

**RESEARCH: tools in an IDE of your choice that supports debugging. The most obvious choice would be a linter, but other tools as demonstrated in lecture are possibilities too. Depending on your choice of IDE (Visual Studio Code vs. Android Studio, for example) your tool may be an extension / plugin, or it may already be integrated into your IDE. Look for documentation related to the tool - how easy is it to understand? Write some sample code and play around with the tool to better understand it. What have you learned from this process?**

### VS Code

I choose VS Code as it’s most comfortable to me and I love being able to add extensions. VS Code has build-in support for the Node.js runtime and can debug JavaScript natively and has extensions available to for debugging in other languages including Python which I also am also experimenting with a fair amount. While I was playing with the debugger, I used Python 3.10 and followed the general structure of the available documentation and took the following notes.

### Breakpoints

* **Conditional Breakpoints** – triggered when an expression evals to `True`, this was the most natural to understand.

* **Inline Breakpoints** – Single line multi-statement breakpoint, can target a statement amongst multiple statements chained together on 1 line. This one I suspect would have confused me had I run across it prior to reading this. I hadn’t considered how to debug multiple statements on a single line before. While I didn’t test it, I know it’s here now.

* **Function Breakpoints** – You can create breakpoints by specifying a function name directly. When in debug mode by hitting the + symbol. Though I haven’t been using it that way so far, rather I’ve been stepping into them using the control panel. I’ll have to try both to see which is more convenient in bigger projects as it wasn’t evident to me in my small test app.

* **Log points** – Logs messages to the console instead of breaking. I am in process of rewiring myself to use log points more frequently instead of printing to console to clean up my code. I’m pretty excited about this one 

### Launch Configurations

You can save configuration details in a .json file called `launch.json` located in a .vscode folder in
your project root folder or in your user/workspace settings. Configurations vary from debugger to
debugger and I personally haven’t specified any launch settings yet myself but I can use IntelliSense and
use (Ctrl+Space) when inside the `launch.json` code to show what options I have in the same way it does
when coding anything else.

It appears that there might be some potential for undesired boiler plate code generation in the json file
as the docs specifically state “Review all automatically generated values and make sure that they make
sense for your project and debugging environment.” This is particularly notable for me because I recall
having this file generated for me on multiple occasions and I didn’t know why. Sometimes I would
find I needed to delete it in order for my program to work properly again and was frustrating. I’ll keep
an eye on this.


There are 2 configurations types launch and attach:
* Launch configuration as a recipe for how to start your app in debug mode BEFORE VS Code attaches to it. I’m guessing this is how it works when I’m using Python automatically.
* 	Attach configuration is a recipe for how to connect VS Code’s debugger to an app or process that’s already running, like a browser.

### Debug Console REPL

Using the Debug Console REPL (Read-Eval-Print-Loop) was new, and it’s great! Being able to interact with my app from a console environment  like that is really cool. I can see this being a tool I revisit often.
You must be in a running debug session to use it, which makes sense. And I like how it can display colors, I often like organizing information this way.



**REFLECT: when has debugging been particularly challenging? Consider a project that you've worked on in the past where a bug was particularly difficult to solve. What techniques did you use? Knowing what you know now, would you be able to improve your process? How is what you've recently learned going to help you in the future? What debugging techniques will you adopt?**

When I used to write code in pinescript there was no debugger available for me and I had to be extremely careful not to introduct bugs because most of the time I was searching for patterns in price movements which could easily be thrown off and often was. Much of the time I was just making a change, saving, re-running to find/fix bugs, it was terrible. Knowning what I know now, I'm sure I could find a Pine script extension to VSCode or another IDE  where I can use debugging tools if I really tried however I think now I would likely try to build in something like python simply because of the tools available. Even just logging to console wasn't possible in pine script, there's no console to output, only a chart. (I would change elements on the chart like bg, candle colors, etc to verify changes as a rudimentry way to overcome this.)

---

## Week 6

**RESEARCH: code review documentation and best practices. Find some documentation for established companies or groups (perhaps from your community!) that discusses some aspect of code review (i.e. it doesn't have to be a full code review protocol, but it should outline some of the processes through which code is approved). How does this process fit with what has been proposed in lecture? Do you think there are any improvements to be made?**

For code review, I looked at [Google Engineering Practices - The Code Reviewer's Guide](https://google.github.io/eng-practices/review/reviewer/)

The primary purpose of code review here is to ensure the overall health of the code base and make sure it is improving over time. There are some key points I noted right away becaue they were less about identifying errors and more about providing an atmosphere that encouraged submissions from the developers which include.

1. Devs must be able to make progress on their tasks. If you never submit an improvment to the code base, then the code base never improves.
2. If a reviewer makes it difficult for change to happen, then developers are disincentivized to make improvements in the future.
3. The reviewer must make sure that each CL is of such quality that the overall code health of the codebase is not decreasing as time goes on. This can be tricky because often, codebases degrade through small decreases in code health over time, especially when a team is under significant time constraints and they feel that they have to take shortcuts in order to accomplish their goals.

I found this **senior principle** interesting. "In general, reviewers should favor approving a CL once it is in a state where it definitely improves the overall code health of the system being worked on, even if the CL isn’t perfect."

This definitly isn't what I expected, but it makes sense and a big portion of the information is reinforcing the idea improving code health and enabling developers to feel like they can submit code improvements without fear of being judged too harshely for code if it is not "perfect".


**REFLECT: on the code review process. Find a friend and perform a code review on one of their previous projects (and vice versa!) What worked well about the process? What could have been improved? How do you think this process could help on a real project? How do you think it could fail?**

To Do: I have not yet done a code review.

---

## Week 7

**RESEARCH: testing frameworks for a programming language and / or IDE that you are familiar with. Find some documentation related to the testing framework and read at least the basic principles. How easy is the documentation to understand? Could you use the tool right away? Try writing a simple test and deploying it? Did it work? What have you learned from this process?**

I decided to research [Pytest](https://docs.pytest.org/en/7.1.x/getting-started.html) for my testing framework. I downloaded the [Full pytest documentation](https://docs.pytest.org/en/6.2.x/contents.html) and referenced  To my suprise it didn't see overly complicated to use, at least on a small scale. The documentation gives some good  examples to start with when testing, and pytest will look for files you have starting with 'test_' and know they are testing related and run the tests in those files.

Many of the examples make use of assert for tests, which makes perfect sense.

An example (below) of assert in python:
```
assert <condition>, <option -message>
```

![Simple Test using pytest](my_test_of_pytest.png)

The test was a bit simple so I moved on to [Examples and customization tricks](https://docs.pytest.org/en/6.2.x/example/index.html) and [Good Integration Practices](https://docs.pytest.org/en/6.2.x/goodpractices.html#goodpractices) sections for more examples.

While I did find some information useful, I am not entirely sure I'm going to understand much of it until I've deployed some testing on one of my own projects and see the types of problems I'll need to solve during testing implementation.

On the other hand there is definitly still information I can infer from this at my level, and it's simple functions require simple testing. I easily understood the example test because it was a simple function with a simple test, so if I write in the same manner then I should produce healthy, testable and extensible code.
<br><br>

**REFLECT: when would testing have helped you most on past projects? Can you recall any past projects that would have benefitted from a test-first approach?  What is the most important learning you will take from this week (even if you don't feel comfortable using a testing framework, what can you take from the principles of testing and use in future projects)?**

Actually testing would have been a perfect fit when I was creating trading indicator scripts, however it wasn't available to me using that language. However I am hoping to create a python market simulator after this course which I think would really benefit from a test-first approach. There are a lot of modular parts that could easily be reused in an OOP language (Pine script was not OOP), and having tests for those modular parts would really help build confidence in my app and narrow potential places to find bugs.

---

## Week 8

**RESEARCH: all the tools! We've collectively looked a lot of tools and technologies up to this point, and many of them have been reported on in Slack. Take a read through the #weekly-research channel and identify the tool that interests you the most (it doesn't have to be one of yours!) Do your own research on the tool by examining the official documentation, reading third-party articles or completing tutorials. If you can, try the tool out yourself to get a sense of how it works.**

After reading through the weekly research I've decided three.js looks the most interesting.
The documentation I referenced was: [Three.js Documentation](https://threejs.org/docs/#manual/en/introduction)

Three.js can create 3d geometry on webpages and according to the docs can run locally in the broswer provided no textures are loaded.
It uses WebGL under the hood which is why it's able to perform so well in the browser. I have not yet found an opportunity to create anything yet. 

TO-DO: Create a 3d animation.

**REFLECT: on your chosen tool. What about it captured your interest in the first place? (Maybe it's purpose? Perhaps the student who reported on it wrote a really compelling discussion about it?) Write a pros and cons list that examines the use of the tool in a context you're familiar with (i.e. as it relates to a particular IDE or programming language, etc.) What have you learned from this process? Would you recommend the tool to a peer? Why?**

I found out about three.js from the weekly research mentioned by 2 of my peers. I was aware you could use HTML 5 Canvas for graphics but I was unaware there was a 3d tool for the level of graphics provided by three.js. I had not looked to see if there were any options to do this before because I did not think the browser could access the power required to do so.

Pros:
* Better looking graphics in the browser
* Desktop level of experience
* Browser games

---

## Week 9

**RESEARCH: UI design patterns. Feel free to use resources from other courses (i.e. especially DGL 111). Compile a list of useful UI pattern resources that are most helpful from a development perspective (i.e. resources that specify not just how a UI might be designed, but also how it might be coded). Did you learn anything new from this process?**

The first site I found with some clearly identified examples was [Interaction Design](https://www.interaction-design.org/literature/topics/ui-design-patterns), there was no real code implementation but the common pattern names were enough for me to find code examples in the language of my choice easily enough on whatever platform I wanted. Some of the patterns listed like 'Breadcrumbs' I've seen before without realizing it was a design pattern.

I also looked at [UI Design Patterns](https://ui-patterns.com/patterns) which has many examples of UI design patterns, but again no examples using code however the examples are detailed explaining the purpose of each pattern and best practices, do's and don't's, etc.

Taking the same approach we used during our app reviews in DGL-114 would help when using both sites, however [Material Design](https://material.io/components?platform=android) would be the best example I could think of which has extremely clean and clear coding examples for UI components.



**REFLECT: on the UI of an app that you use often. Feel free to make use of UI terminology and analysis that you've learned in other classes (e.g. especially DGL 111). Consider what works well with the UI and what doesn't, perhaps make a pros and cons list (consider all aspects, including navigation, menus, icons, device orientation, how the app interfaces with other apps or the system, etc.). Try to break the app: push the interface to its limits and try to find bugs - be intentional about this. Consider the UI from a developer's perspective: What would be difficult, based on what you know? What would be easy? Report on what you've learned.**

To Do:

---

## Week 10

 **RESEARCH: design patterns and anti-patterns. Take a look at the list provided in lecture, or do some researching online to find a resource that provides a collection of design patterns (and / or anti-patterns) in a programming language that you are familiar with. Choose one pattern and examine it closely. What is the pattern for? In what situations would you expect to find it? What are the key elements of the pattern? What have you learned from your close examination?**

 To Do:

**REFLECT: on a past coding project. Choose a project of significant length that you haven't touched in at least some months (it could be from a course you've already completed, or from work you've done outside of class). Examine the code carefully and consider the main themes we've discussed in this course to date (i.e. from best practices and style, documentation, debugging, through UI concerns and design patterns). What stands out to you as you examine your code? Do you understand the code and your intentions fully? What is the biggest thing you could do to improve the code? Explain why.**

To Do:

---

## Week 11

 **RESEARCH: either another software architecture pattern (e.g. MVP, MVI, VIPER, others), or a new resource that discusses one of the patterns discussed in lecture (i.e. MVC or MVVM). What did you learn from this resource? What will you be able to apply from what you've learned from this resource and from lecture?**

To Do:

**REFLECT: On the diagramatic way in which software architecture patterns are typically presented. Have these been helpful to you understanding? Have they hindered your understanding? Is there a better approach? If so, describe it; if not, explain what you think the strength of the diagramatic approach is.**

To Do: