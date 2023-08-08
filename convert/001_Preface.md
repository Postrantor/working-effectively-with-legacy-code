### [Preface](#contents)

Do you remember the first program you wrote? I remember mine. It was a little graphics program I wrote on an early PC. I started programming later than most of my friends. Sure, I'd seen computers when I was a kid. I remember being really impressed by a minicomputer I once saw in an office, but for years I never had a chance to even sit at a computer. Later, when I was a teenager, some friends of mine bought a couple of the first TRS-80s. I was interested, but I was actually a bit apprehensive, too. I knew that if I started to play with computers, I'd get sucked into it. It just looked too cool. I don't know why I knew myself so well, but I held back. Later, in college, a roommate of mine had a computer, and I bought a C compiler so that I could teach myself programming. Then it began. I stayed up night after night trying things out, poring through the source code of the emacs editor that came with the compiler. It was addictive, it was challenging, and I loved it.

> 你还记得你写的第一个程序吗？我记得我的。这是我在早期的电脑上写的一个小图形程序。我比大多数朋友都晚开始编程。当然，我小时候见过电脑。我记得有一次在办公室里看到的一台小型计算机给我留下了深刻的印象，但多年来，我甚至没有机会坐在电脑前。后来，当我十几岁的时候，我的一些朋友买了几辆第一辆 TRS-80。我很感兴趣，但实际上我也有点担心。我知道，如果我开始玩电脑，我会被它吸引。它看起来太酷了。我不知道为什么我这么了解自己，但我忍住了。后来，在大学里，我的一个室友有了一台电脑，我买了一个 C 编译器，这样我就可以自学编程了。然后它开始了。我彻夜未眠，仔细研究编译器附带的 emacs 编辑器的源代码。它令人上瘾，充满挑战，我喜欢它。

I hope you've had experiences like this---just the raw joy of making things work on a computer. Nearly every programmer I ask has. That joy is part of what got us into this work, but where is it day to day?

> 我希望你有过这样的经历——只是在电脑上工作的原始乐趣。我问过的几乎每个程序员都有。这种快乐是我们从事这项工作的部分原因，但它在哪里？

A few years ago, I gave my friend Erik Meade a call after I'd finished work one night. I knew that Erik had just started a consulting gig with a new team, so I asked him, "How are they doing?" He said, "They're writing legacy code, man." That was one of the few times in my life when I was sucker-punched by a coworker's statement. I felt it right in my gut. Erik had given words to the precise feeling that I often get when I visit teams for the first time. They are trying very hard, but at the end of the day, because of schedule pressure, the weight of history, or a lack of any better code to compare their efforts to, many people are writing legacy code.

> 几年前，一天晚上我下班后，给我的朋友 Erik Meade 打了个电话。我知道 Erik 刚刚在一个新团队开始了一项咨询工作，所以我问他，“他们过得怎么样？”他说，“他们在写遗产代码，伙计。”这是我一生中为数不多的几次被同事的声明打得鼻青脸肿。我感觉到了。埃里克用语言表达了我第一次访问球队时经常有的赛前感觉。他们正在努力，但最终，由于日程压力、历史的重量，或者缺乏更好的代码来与他们的努力进行比较，许多人正在编写遗留代码。

What is legacy code? I've used the term without defining it. Let's look at the strict definition: Legacy code is code that we've gotten from someone else. Maybe our company acquired code from another company; maybe people on the original team moved on to other projects. Legacy code is somebody else's code. But in programmer-speak, the term means much more than that. The term _legacy code_ has taken on more shades of meaning and more weight over time.

> 什么是遗留代码？我使用了这个术语，但没有定义它。让我们看看严格的定义：遗留代码是我们从其他人那里得到的代码。也许我们公司从另一家公司获得了代码；也许最初团队中的人转到了其他项目。遗留代码是别人的代码。但从程序员的角度来说，这个词的意义远不止于此。随着时间的推移，“遗留代码”一词有了更多的含义和分量。

What do you think about when you hear the term _legacy code_? If you are at all like me, you think of tangled, unintelligible structure, code that you have to change but don't really understand. You think of sleepless nights trying to add in features that should be easy to add, and you think of demoralization, the sense that everyone on the team is so sick of a code base that it seems beyond care, the sort of code that you just wish would die. Part of you feels bad for even thinking about making it better. It seems unworthy of your efforts. That definition of legacy code has nothing to do with who wrote it. Code can degrade in many ways, and many of them have nothing to do with whether the code came from another team.

> 当你听到“遗留代码”这个词时，你会怎么想？如果你和我一样，你会想到复杂、难以理解的结构，你必须更改但并不真正理解的代码。你会想到失眠的夜晚，试图添加本应易于添加的功能，你会想到士气低落，团队中的每个人都厌倦了代码库，以至于它似乎无法照顾，你只希望这种代码会消亡。你的一部分甚至因为想让它变得更好而感到难过。这似乎不值得你努力。遗留代码的定义与谁写的无关。代码可以在很多方面退化，其中许多与代码是否来自另一个团队无关。

In the industry, _legacy code_ is often used as a slang term for difficult-to-change code that we don't understand. But over years of working with teams, helping them get past serious code problems, I've arrived at a different definition.

> 在行业中，“遗留代码”经常被用作俚语，表示我们不理解的难以更改的代码。但经过多年与团队的合作，帮助他们克服严重的代码问题，我得出了不同的定义。

To me, _legacy code_ is simply code without tests. I've gotten some grief for this definition. What do tests have to do with whether code is bad? To me, the answer is straightforward, and it is a point that I elaborate throughout the book:

> 对我来说，遗留代码只是没有测试的代码。我为这个定义感到有些悲伤。测试与代码是否坏有什么关系？对我来说，答案很简单，这是我在整本书中阐述的一点：

You might think that this is severe. What about clean code? If a code base is very clean and well structured, isn't that enough? Well, make no mistake. I love clean code. I love it more than most people I know, but while clean code is good, it's not enough. Teams take serious chances when they try to make large changes without tests. It is like doing aerial gymnastics without a net. It requires incredible skill and a clear understanding of what can happen at every step. Knowing precisely what will happen if you change a couple of variables is often like knowing whether another gymnast is going to catch your arms after you come out of a somersault. If you are on a team with code that clear, you are in a better position than most programmers. In my work, I've noticed that teams with that degree of clarity in all of their code are rare. They seem like a statistical anomaly. And, you know what? If they don't have supporting tests, their code changes still appear to be slower than those of teams that do.

> 你可能会认为这很严重。那么干净的代码呢？如果一个代码库非常干净且结构良好，那还不够吗？好吧，别搞错了。我喜欢干净的代码。我比我认识的大多数人都更喜欢它，但尽管干净的代码很好，但这还不够。当团队试图在没有测试的情况下做出大的改变时，他们会抓住很大的机会。这就像在没有网的情况下做空中体操。这需要令人难以置信的技能和对每一步可能发生的事情的清晰理解。准确地知道如果你改变几个变量会发生什么，通常就像知道你翻筋斗后另一名体操运动员是否会抓住你的手臂一样。如果你所在的团队的代码如此清晰，那么你的处境就比大多数程序员要好。在我的工作中，我注意到在所有代码中都有这种清晰程度的团队是罕见的。它们看起来像是统计上的反常现象。你知道吗？如果他们没有支持测试，他们的代码更改似乎仍然比有支持测试的团队慢。

Yes, teams do get better and start to write clearer code, but it takes a long time for older code to get clearer. In many cases, it will never happen completely. Because of this, I have no problem defining legacy code as code without tests. It is a good working definition, and it points to a solution.

> 是的，团队确实变得更好，开始编写更清晰的代码，但旧代码需要很长时间才能变得更清晰。在许多情况下，它永远不会完全发生。正因为如此，我可以将遗留代码定义为无需测试的代码。这是一个很好的工作定义，它指向了一个解决方案。

I've been talking about tests quite a bit so far, but this book is not about testing. This book is about being able to confidently make changes in any code

> 到目前为止，我一直在谈论测试，但这本书不是关于测试的。这本书是关于能够自信地对任何代码进行更改

base. In the following chapters, I describe techniques that you can use to understand code, get it under test, refactor it, and add features.

> 基础在接下来的几章中，我将介绍一些技术，您可以使用这些技术来对代码进行测试、重构和添加特性。

One thing that you will notice as you read this book is that it is not a book about pretty code. The examples that I use in the book are fabricated because I work under nondisclosure agreements with clients. But in many of the examples, I've tried to preserve the spirit of code that I've seen in the field. I won't say that the examples are always representative. There certainly are oases of great code out there, but, frankly, there are also pieces of code that are far worse than anything I can use as an example in this book. Aside from client confidentiality, I simply couldn't put code like that in this book without boring you to tears and burying important points in a morass of detail. As a result, many of the examples are relatively brief. If you look at one of them and think "No, he doesn't understand---my methods are much larger than that and much worse," please look at the advice that I am giving at face value and see if it applies, even if the example seems simpler.

> 当你读这本书时，你会注意到一件事，那就是它不是一本关于漂亮代码的书。我在书中使用的例子是捏造的，因为我与客户签订了保密协议。但在许多考试中，我都试图保留我在该领域看到的代码精神。我不会说这些例子总是有代表性的。当然有很多很棒的代码，但坦率地说，也有一些代码比我在这本书中举的任何例子都糟糕得多。除了对客户保密之外，我在这本书中写下这样的代码时，会让你厌烦得流泪，并将重要的点埋在细节的泥沼中。因此，许多例子都比较简短。如果你看着其中一个，然后想“不，他不明白——我的方法比这个大得多，而且更糟”，请从表面上看我给出的建议，看看它是否适用，即使这个例子看起来更简单。

The techniques here have been tested on substantially large pieces of code. It is just a limitation of the book format that makes examples smaller. In particular, when you see ellipses (...) in a code fragment like this, you can read them as "insert 500 lines of ugly code here":

> 这里的技术已经在相当大的代码块上进行了测试。这只是书籍格式的限制，使例子变得更小。特别是，当你在这样的代码片段中看到省略号（…）时，你可以把它们读成“在这里插入 500 行丑陋的代码”：

```code
m_pDispatcher->register(listener);
...
m_nMargins++;
```

If this book is not about pretty code, it is even less about pretty design. Good design should be a goal for all of us, but in legacy code, it is something that we arrive at in discrete steps. In some of the chapters, I describe ways of adding new code to existing code bases and show how to add it with good design principles in mind. You can start to grow areas of very good high-quality code in legacy code bases, but don't be surprised if some of the steps you take to make changes involve making some code slightly uglier. This work is like surgery. We have to make incisions, and we have to move through the guts and suspend some aesthetic judgment. Could this patient's major organs and viscera be better than they are? Yes. So do we just forget about his immediate problem, sew him up again, and tell him to eat right and train for a marathon? We could, but what we really need to do is take the patient as he is, fix what's wrong, and move him to a healthier state. He might never become an Olympic athlete, but we can't let "best" be the enemy of "better." Code bases can become healthier and easier to work in. When a patient feels a little better, often that is the time when you can help him make commitments to a healthier life style. That is what we are shooting for with legacy code. We are trying to get to the point at which we are used to ease; we expect it and actively attempt to make code change easier. When we can sustain that sense on a team, design gets better.

> 如果这本书不是关于漂亮的代码，那么它就更不关于漂亮的设计了。好的设计应该是我们所有人的目标，但在遗留代码中，这是我们通过离散步骤实现的。在其中的一些章节中，我描述了向现有代码库添加新代码的方法，并展示了如何在牢记良好设计原则的情况下添加新代码。您可以开始在遗留代码库中增加非常好的高质量代码区域，但如果您为进行更改而采取的一些步骤涉及使某些代码变得稍微丑陋，也不要感到惊讶。这项工作就像外科手术。我们必须做出切口，我们必须穿过内脏，暂停一些审美判断。这个病人的主要器官和内脏会比现在更好吗？对那么，我们是不是忘记了他眼前的问题，再次给他缝合伤口，让他好好吃饭，训练马拉松？我们可以，但我们真正需要做的是让病人保持原样，解决问题，让他处于更健康的状态。他可能永远不会成为奥运会运动员，但我们不能让“最好”成为“更好”的敌人。代码库可以变得更健康，更容易使用。当患者感觉好一点时，通常是你可以帮助他做出更健康生活方式的承诺的时候。这就是我们对遗留代码的追求。我们正试图在习惯于放松；我们期待它，并积极尝试使代码更改更容易。当我们能够在团队中保持这种感觉时，设计就会变得更好。

The techniques I describe are ones that I've discovered and learned with coworkers and clients over the course of years working with clients to try to establish control over unruly code bases. I got into this legacy code emphasis accidentally. When I first started working with Object Mentor, the bulk of my work involved helping teams with serious problems develop their skills and interactions to the point that they could regularly deliver quality code. We often used Extreme Programming practices to help teams take control of their work, collaborate intensively, and deliver. I often feel that Extreme Programming is less a way to develop software than it is a way to make a well-jelled work team that just happens to deliver great software every two weeks.

> 我所描述的技术是我在多年与客户合作的过程中发现并与同事和客户一起学习的，试图建立对不规则代码库的控制。我无意中进入了这种遗留代码强调。当我刚开始使用 Object Mentor 时，我的大部分工作都涉及帮助有严重问题的团队发展他们的技能和互动，使他们能够定期交付高质量的代码。我们经常使用极限编程实践来帮助团队控制他们的工作、集中协作和交付。我经常觉得极限编程与其说是开发软件的一种方式，不如说是组建一个团队的一种方法，恰好每两周就能交付一次优秀的软件。

From the beginning, though, there was a problem. Many of the first XP projects were "greenfield" projects. The clients I was seeing had significantly large code bases, and they were in trouble. They needed some way to get control of their work and start to deliver. Over time, I found that I was doing the same things over and over again with clients. This sense culminated in some work I was doing with a team in the financial industry. Before I'd arrived, they'd realized that unit testing was a great thing, but the tests that they were executing were full scenario tests that made multiple trips to a database and exercised large chunks of code. The tests were hard to write, and the team didn't run them very often because they took so long to run. As I sat down with them to break dependencies and get smaller chunks of code under test, I had a terrible sense of déjà vu. It seemed that I was doing this sort of work with every team I met, and it was the sort of thing that no one really wanted to think about. It was just the grunge work that you do when you want to start working with your code in a controlled way, if you know how to do it. I decided then that it was worth really reflecting on how we were solving these problems and writing them down so that teams could get a leg up and start to make their code bases easier to live in.

> 然而，从一开始就存在问题。许多最初的 XP 项目都是“绿地”项目。我看到的客户有相当大的代码库，他们遇到了麻烦。他们需要一些方法来控制他们的工作并开始交付。随着时间的推移，我发现我和客户一次又一次地做同样的事情。这种感觉在我与金融行业的一个团队一起做的一些工作中达到了顶峰。在我到达之前，他们已经意识到单元测试是一件很棒的事情，但他们正在执行的测试是全场景测试，需要多次访问数据库并运行大量代码。测试很难编写，团队也不经常运行，因为运行时间太长。当我和他们坐下来打破依赖关系，测试更小的代码块时，**我有一种可怕的似曾相识的感觉。我似乎和我遇到的每一个团队都在做这种工作，这是没有人真正想去想的事情**。当你想开始以可控的方式处理代码时，如果你知道如何做的话，这只是你所做的垃圾工作。当时我决定，值得真正反思一下我们是如何解决这些问题的，并把它们写下来，这样团队就可以站起来，开始让他们的代码库更容易使用。

A note about the examples: I've used examples in several different programming languages. The bulk of the examples are written in Java, C++, and C. I picked Java because it is a very common language, and I included C++ because it presents some special challenges in a legacy environment. I picked C because it highlights many of the problems that come up in procedural legacy code. Among them, these languages cover much of the spectrum of concerns that arise in legacy code. However, if the languages you use are not covered in the examples, take a look at them anyway. Many of the techniques that I cover can be used in other languages, such as Delphi, Visual Basic, COBOL, and FORTRAN.

> 关于示例的说明：我已经在几种不同的程序设计语言中使用了示例。大部分示例都是用 Java、C++和 C 编写的。我选择 Java 是因为它是一种非常常见的语言，而我选择 C++是因为它在遗留环境中带来了一些特殊的挑战。我选择 C 是因为它突出了过程遗留代码中出现的许多问题。其中，这些语言涵盖了 legacy 代码中出现的许多问题。但是，如果您使用的语言没有包含在示例中，那么无论如何都要查看它们。我介绍的许多技术可以用于其他语言，如 Delphi、Visual Basic、COBOL 和 FORTRAN。

I hope that you find the techniques in this book helpful and that they allow you to get back to what is fun about programming. Programming can be very rewarding and enjoyable work. If you don't feel that in your day-to-day work, I hope that the techniques I offer you in this book help you find it and grow it on your team.

> 我希望你能发现这本书中的技术很有帮助，它们能让你回到编程的乐趣所在。编程可以是一项非常有收获和愉快的工作。如果你在日常工作中没有这种感觉，我希望我在这本书中为你提供的技巧能帮助你找到它，并在你的团队中成长。
