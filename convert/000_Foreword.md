### [Foreword](#contents)

"...then it began..."

In his introduction to this book, Michael Feathers uses that phrase to describe the start of his passion for software.

> 在这本书的引言中，Michael Feathers 用这句话来描述他对软件热情的开始。

"...then it began..."

Do you know that feeling? Can you point to a single moment in your life and say: "...then it began..."? Was there a single event that changed the course of your life and eventually led you to pick up this book and start reading this fore- word?

> 你知道那种感觉吗？你能指着你生命中的某个时刻说：“……然后它开始了……”吗？有没有一件事改变了你的人生历程，最终让你拿起这本书，开始阅读这本书？

I was in sixth grade when it happened to me. I was interested in science and space and all things technical. My mother found a plastic computer in a catalog and ordered it for me. It was called _Digi-Comp I_. Forty years later that little plastic computer holds a place of honor on my bookshelf. It was the catalyst that sparked my enduring passion for software. It gave me my first inkling of how joyful it is to write programs that solve problems for people. It was just three plastic S-R flip-flops and six plastic and-gates, but it was enough---it served. Then... for me... it began...

> 这件事发生在我六年级的时候，我对科学、太空和所有技术感兴趣。我妈妈在目录里找到一台塑料电脑，帮我订了一台，名叫“数码公司 I”。四十年后，那台小小的塑料电脑在我的书架上占据了一个荣誉的位置。正是这种催化剂激发了我对软件的持久热情。它让我第一次意识到为人们编写解决问题的程序是多么快乐。它只有三个塑料 S-R 触发器和六个塑料与门，但它已经足够了——它起作用了。然后对我来说…它开始了。。。

But the joy I felt soon became tempered by the realization that software sys- tems almost always degrade into a mess. What starts as a clean crystalline design in the minds of the programmers rots, over time, like a piece of bad meat. The nice little system we built last year turns into a horrible morass of tangled functions and variables next year.

> 但当我意识到软件系统几乎总是会退化成一团乱麻时，我的喜悦很快就缓和了。在程序员的脑海中，一开始是一个干净的水晶设计，随着时间的推移，它就像一块坏肉一样腐烂。我们去年建立的漂亮的小系统明年将变成一个由复杂的函数和变量组成的可怕的泥沼。

Why does this happen? Why do systems rot? Why can't they stay clean?

Sometimes we blame our customers. Sometimes we accuse them of changing the requirements. We comfort ourselves with the belief that if the customers had just been happy with what they said they needed, the design would have been fine. It's the customer's fault for changing the requirements on us.

> 有时我们会责怪我们的客户。有时我们指责他们改变了要求。我们相信，如果客户对他们所说的感到满意，设计就会很好，这让我们感到安慰。改变对我们的要求是客户的错。

Well, here's a news flash: _Requirements change_. Designs that cannot tolerate changing requirements are poor designs to begin with. It is the goal of every competent software developer to create designs that tolerate change.

> 好吧，这里有一个新闻快讯：_需求变化_。不能容忍不断变化的需求的设计从一开始就是糟糕的设计。**每一个有能力的软件开发人员的目标都是创建能够容忍更改的设计**。

This seems to be an intractably hard problem to solve. So hard, in fact, that nearly every system ever produced suffers from slow, debilitating rot. The rot is so pervasive that we've come up with a special name for rotten programs. We call them: **Legacy Code**.

> 这似乎是一个难以解决的难题。事实上，如此艰难，以至于几乎每一个产生的系统都会遭受缓慢而衰弱的腐烂。这种腐烂是如此普遍，以至于我们为腐烂的程序想出了一个特殊的名字。我们称之为：**遗留代码**。

Legacy code. The phrase strikes disgust in the hearts of programmers. It con- jures images of slogging through a murky swamp of tangled undergrowth with leaches beneath and stinging flies above. It conjures odors of murk, slime, stag- nancy, and offal. Although our first joy of programming may have been intense, the misery of dealing with legacy code is often sufficient to extinguish that flame.

> 旧代码。这句话引起了程序员的反感。它描绘了在一片杂乱的灌木丛中艰难前行的画面，下面是沥滤物，上面是刺痛的苍蝇。它会散发出臭味、黏液味、停滞味和内脏味。尽管我们编程的最初乐趣可能是强烈的，但处理遗留代码的痛苦往往足以扑灭火焰。

Many of us have tried to discover ways to _prevent_ code from becoming leg- acy. We've written books on principles, patterns, and practices that can help programmers keep their systems clean. But Michael Feathers had an insight that many of the rest of us missed. Prevention is imperfect. Even the most disciplined development team, knowing the best principles, using the best patterns, and fol- lowing the best practices will create messes from time to time. The rot still accu- mulates. It's not enough to try to prevent the rot---you have to be able to _reverse_ it.

> 我们中的许多人都试图找到防止代码失控的方法。我们写过一些关于原理、模式和实践的书，这些书可以帮助程序员保持系统的清洁。但迈克尔·费瑟斯有一个我们很多人都错过的洞察力。预防不完善。即使是最有纪律的开发团队，知道最好的原则，使用最好的模式，遵循最好的实践，也会时不时地制造混乱。腐败仍在滋生。**仅仅试图防止腐败是不够的——你必须能够“扭转”它**。

That's what this book is about. It's about reversing the rot. It's about taking a tangled, opaque, convoluted system and slowly, gradually, piece by piece, step by step, turning it into a simple, nicely structured, well-designed system. It's about reversing entropy.

> 这就是这本书的主题。这是关于扭转颓势。这是关于把一个混乱、不透明、错综复杂的系统慢慢地、逐渐地、一块一块地、一步一步地变成一个简单、结构良好、设计良好的系统。这是关于熵的反转。

Before you get too excited, I warn you; reversing rot is not easy, and it's not quick. The techniques, patterns, and tools that Michael presents in this book are effective, but they take work, time, endurance, and _care_. This book is not a magic bullet. It won't tell you how to eliminate all the accumulated rot in your systems overnight. Rather, this book describes a set of disciplines, concepts, and attitudes that you will carry with you for the rest of your career and that _will help you to turn systems that gradually degrade into systems that gradually improve_.

> 在你太激动之前，我警告你；扭转腐败并不容易，而且速度也不快。迈克尔在这本书中介绍的技术、模式和工具是有效的，但它们需要工作、时间、耐力和小心。这本书不是灵丹妙药。它不会告诉你如何在一夜之间消除系统中积累的所有腐败。相反，这本书描述了一系列你将在职业生涯的剩余时间里随身携带的纪律、概念和态度，它们将帮助你将逐渐退化的系统转变为逐渐改进的系统。

**_Robert C. Martin 2E June, 2004_**
