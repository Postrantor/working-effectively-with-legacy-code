[]{#_bookmark9 .anchor}[**Part I**](#contents)

# [The Mechanics of Change](#contents)

[]{#_bookmark10 .anchor}[**Chapter 1**](#contents)

### [Changing Software](#contents)

[]{#_bookmark12 .anchor}Changing code is great. It's what we do for a living. But there are ways of changing code that make life difficult, and there are ways that make it much easier. In the industry, we haven't spoken about that much. The closest we've gotten is the literature on refactoring. I think we can broaden the discussion a bit and talk about how to deal with code in the thorniest of situations. To do that, we have to dig deeper into the mechanics of change.

> 更改代码非常棒。这就是我们的谋生之道。但是，有些更改代码的方法会让生活变得困难，也有一些方法会让它变得更容易。在这个行业，我们还没有谈论那么多。我们得到的最接近的是关于重构的文献。我认为我们可以扩大讨论范围，讨论如何在最棘手的情况下处理代码。要做到这一点，我们必须深入研究变革的机制。

#### [Four Reasons to Change Software](#contents)

For simplicity's sake, let's look at four primary reasons to change software.

> 为了简单起见，让我们来看一下更改软件的四个主要原因。

1. Adding a feature
2. Fixing a bug
3. Improving the design
4. Optimizing resource usage

##### Adding Features and Fixing Bugs

Adding a feature seems like the most straightforward type of change to make. The software behaves one way, and users say that the system needs to do something else also.

> 添加功能似乎是最简单的更改类型。软件的行为是单向的，用户说系统还需要做一些其他的事情。

Suppose that we are working on a web-based application, and a manager tells us that she wants the company logo moved from the left side of a page to the right side. We talk to her about it and discover it isn't quite so simple. She wants to move the logo, but she wants other changes, too. She'd like to make it animated for the next release. Is this fixing a bug or adding a new feature? It depends on your point of view. From the point of view of the customer, she is definitely asking us to fix a problem. Maybe she saw the site and attended a meeting with people in her department, and they decided to change the logo placement and ask for a bit more functionality. From a developer's point of view, the change could be seen as a completely new feature. "If they just stopped changing their minds, we'd be done by now." But in some organizations the logo move is seen as just a bug fix, regardless of the fact that the team is going to have to do a lot of fresh work.

> 假设我们正在开发一个基于 web 的应用程序，一位经理告诉我们，她希望将公司徽标从页面的左侧移到右侧。我们和她聊了聊，发现事情并不是那么简单。她想移动标志，但她也想做其他改变。她想为下一次发行制作动画。**这是在修复 bug 还是添加新功能？这取决于你的观点**。从客户的角度来看，她肯定是在要求我们解决问题。也许她看到了这个网站并参加了与她所在部门的人员会面，他们决定更改徽标的位置，并要求提供更多功能。从开发人员的角度来看，这一变化可以被视为一个全新的功能。“如果他们停止改变主意，我们现在就完了。”但在一些组织中，标志的移动被视为只是一个错误修复，而不管团队将不得不做很多新的工作。

It is tempting to say that all of this is just subjective. You see it as a bug fix, and I see it as a feature, and that's the end of it. Sadly, though, in many organizations, bug fixes and features have to be tracked and accounted for separately because of contracts or quality initiatives. At the people level, we can go back and forth endlessly about whether we are adding features or fixing bugs, but it is all just changing code and other artifacts. Unfortunately, this talk about bugfixing and feature addition masks something that is much more important to us technically: behavioral change. There is a big difference between adding new behavior and changing old behavior.

> 人们很容易说，所有这些都只是主观的。你把它看作是一个 bug 修复，我把它看作一个功能，就这样结束了。然而，可悲的是，在许多组织中，由于**合同或质量计划的原因，bug 修复和功能必须单独跟踪和说明**。在人员级别，我们可以无休止地来回讨论是添加功能还是修复 bug，但这都只是更改代码和其他工件。不幸的是，这种关于 bug 修复和特性添加的讨论掩盖了对我们来说更重要的东西：**行为改变。添加新行为和更改旧行为之间有很大的区别**。

In the company logo example, are we adding behavior? Yes. After the change, the system will display a logo on the right side of the page. Are we getting rid of any behavior? Yes, there won't be a logo on the left side.

> 在公司徽标示例中，我们是否添加了行为？对更改后，系统将在页面右侧显示一个徽标。我们正在摆脱任何行为吗？是的，左边不会有标志。

Let's look at a harder case. Suppose that a customer wants to add a logo to the right side of a page, but there wasn't one on the left side to start with. Yes, we are adding behavior, but are we removing any? Was anything rendered in the place where the logo is about to be rendered?

> 让我们来看一个更难的案例。假设一个客户想在页面的右侧添加一个徽标，但左侧没有徽标。是的，我们正在添加行为，但我们是否删除了任何行为？在即将渲染徽标的地方是否渲染了任何内容？

Are we changing behavior, adding it, or both?

It turns out that, for us, we can draw a distinction that is more useful to us as programmers. If we have to modify code (and HTML kind of counts as code), we could be changing behavior. If we are only adding code and calling it, we are often adding behavior. Let's look at another example. Here is a method on a Java class:

> 事实证明，对我们来说，我们可以做出对我们程序员更有用的区分。如果我们必须修改代码(HTML 算是代码)，我们可能会改变行为。如果我们只是添加代码并调用它，那么我们通常是在添加行为。让我们看另一个例子。下面是 Java 类上的一个方法：

```java
public class CDPlayer {
  public void addTrackListing(Track track) { ... }
}
```

The class has a method that enables us to add track listings. Let's add another method that lets us replace track listings.

```java
public class CDPlayer {
  public void addTrackListing(Track track) { ... }
  public void replaceTrackListing(String name, Track track) { ... }
}
```

When we added that method, did we add new behavior to our application or change it? The answer is: neither. Adding a method doesn't change behavior unless the method is called somehow.

> 改变它？答案是：两者都不是。添加方法不会改变行为，除非以某种方式调用该方法。

Let's make another code change. Let's put a new button on the user interface for the CD player. The button lets users replace track listings. With that move, we're adding the behavior we specified in replaceTrackListing method, but we're also subtly changing behavior. The UI will render differently with that new button. Chances are, the UI will take about a microsecond longer to display. It seems nearly impossible to add behavior without changing it to some degree.

> 让我们再次更改代码。让我们在 CD 播放器的用户界面上添加一个新按钮。该按钮允许用户替换曲目列表。通过这个动作，我们添加了在 replaceTrackListing 方法中指定的行为，但我们也在微妙地改变行为。UI 将以不同的方式呈现新的 but-ton。UI 的显示时间可能会延长一微秒。如果不在某种程度上改变行为，添加行为似乎几乎是不可能的。

##### Improving Design

Design improvement is a different kind of software change. When we want to alter software's structure to make it more maintainable, generally we want to keep its behavior intact also. When we drop behavior in that process, we often call that a bug. One of the main reasons why many programmers don't attempt to improve design often is because it is relatively easy to lose behavior or create bad behavior in the process of doing it.

> 设计改进是一种不同类型的软件更改。当我们想要改变软件的结构以使其更易于维护时，通常我们也希望保持其行为的完整性。当我们在这个过程中放弃行为时，我们通常称之为 bug。许多程序员不经常尝试改进设计的主要原因之一是，在进行设计的过程中，相对容易失去行为或产生不良行为。

The act of improving design without changing its behavior is called _refactoring_. The idea behind refactoring is that we can make software more maintainable without changing behavior if we write tests to make sure that existing behavior doesn't change and take small steps to verify that all along the process. People have been cleaning up code in systems for years, but only in the last few years has refactoring taken off. Refactoring differs from general cleanup in that we aren't just doing low-risk things such as reformatting source code, or invasive and risky things such as rewriting chunks of it. Instead, we are making a series of small structural modifications, supported by tests to make the code easier to change. The key thing about refactoring from a change point of view is that there aren't supposed to be any functional changes when you refactor (although behavior can change somewhat because the structural changes that you make can alter performance, for better or worse).

> **在不改变其行为的情况下改进设计的行为称为重构。重构背后的理念是，如果我们编写测试以确保现有行为不会改变，并在整个过程中采取小步骤来验证这一点，我们就可以在不改变行为的情况下使软件更具可维护性。** 多年来，人们一直在清理系统中的代码，但直到最近几年才开始进行重构。重构与一般清理的不同之处在于，我们不仅仅是在做低风险的事情，比如重新格式化源代码，或者进行侵入性和风险性的事情，例如重写大块代码。相反，我们正在进行一系列小的结构修改，由测试支持，使代码更易于更改。从变化的角度来看，重构的关键是重构时不应该有任何功能性的变化(尽管行为可能会发生一些变化，因为你所做的结构变化可能会改变性能，无论好坏)。

##### Optimization

Optimization is like refactoring, but when we do it, we have a different goal. With both refactoring and optimization, we say, "We're going to keep functionality exactly the same when we make changes, but we are going to change something else." In refactoring, the "something else" is program structure; we want to make it easier to maintain. In optimization, the "something else" is some resource used by the program, usually time or memory.

> **优化就像重构**，但当我们这样做时，我们有一个不同的目标。对于重构和优化，我们说，“当我们做出改变时，我们会保持功能完全相同，但我们会改变其他东西。”在重构中，“其他东西”是程序结构；我们想让它更容易维护。在优化中，“其他东西”是程序使用的一些资源，通常是时间或内存。

##### Putting It All Together

It might seem strange that refactoring and optimization are kind of similar. They seem much closer to each other than adding features or fixing bugs. But is this really true? The thing that is common between refactoring and optimization is that we hold functionality invariant while we let something else change.

> 重构和优化有点相似，这可能看起来很奇怪。它们似乎比添加功能或修复 bug 更接近。但这真的是真的吗？**重构和优化之间的共同点是，当我们让其他东西改变时，我们保持功能不变**。

In general, three different things can change when we do work in a system: structure, functionality, and resource usage.

> 一般来说，当我们在一个系统中工作时，**有三件不同的事情可能会发生变化：结构、功能和资源使用**。

Let's look at what usually changes and what stays more or less the same when we make four different kinds of changes (yes, often all three change, but let's look at what is typical):

> 让我们看看当我们做出四种不同的改变时，通常会发生什么变化，以及什么或多或少保持不变(是的，通常三种都会发生变化，但让我们看看什么是典型的)：

+------------------+------------------------+--------------------+-------------------+------------------+
| | > **Adding a Feature** | > **Fixing a Bug** | > **Refactoring** | > **Optimizing** |
+==================+========================+====================+===================+==================+
| > Structure | > Changes | > Changes | > Changes | > --|
+------------------+------------------------+--------------------+-------------------+------------------+
| > Functionality | > Changes | > Changes | > --| > --|
+------------------+------------------------+--------------------+-------------------+------------------+
| > Resource Usage | > --| > --| > --| > Changes |
+------------------+------------------------+--------------------+-------------------+------------------+

Superficially, refactoring and optimization do look very similar. They hold functionality invariant. But what happens when we account for new functionality separately? When we add a feature often we are adding new functionality, but without changing existing functionality.

> 从表面上看，重构和优化看起来确实非常相似。它们保持功能不变。但是，当我们单独考虑新的功能性时，会发生什么呢？当我们添加一个功能时，通常是在添加新功能，但不更改现有功能。

+---------------------+------------------------+--------------------+-------------------+------------------+
| | > **Adding a Feature** | > **Fixing a Bug** | > **Refactoring** | > **Optimizing** |
+=====================+========================+====================+===================+==================+
| > Structure | > Changes | > Changes | > Changes | > --|
+---------------------+------------------------+--------------------+-------------------+------------------+
| > New Functionality | > Changes | > --| > --| > --|
+---------------------+------------------------+--------------------+-------------------+------------------+
| > Functionality | > --| > Changes | > --| > --|
+---------------------+------------------------+--------------------+-------------------+------------------+
| > Resource Usage | > --| > --| > --| > Changes |
+---------------------+------------------------+--------------------+-------------------+------------------+

[]{#_bookmark16 .anchor}invariant. In fact, if we scrutinize bug fixing, yes, it does change functionality, but the changes are often very small compared to the amount of existing functionality that is not altered.

> []｛#_bookmark16.anchor｝不变量。事实上，如果我们仔细检查 bug 修复，是的，它确实会更改功能，但与未更改的现有功能相比，更改通常非常小。

Feature addition and bug fixing are very much like refactoring and optimization. In all four cases, we want to change some functionality, some behavior, but we want to preserve much more (see Figure 1.1).

> 功能添加和错误修复非常像重构和优化。在这四种情况下，我们都希望更改一些功能和行为，但我们希望保留更多(见图 1.1)。

[]{#_bookmark17 .anchor}Existing Behavior New Behavior

**Figure 1.1** _Preserving behavior._

That's a nice view of what is supposed to happen when we make changes, but what does it mean for us practically? On the positive side, it seems to tell us what we have to concentrate on. We have to make sure that the small number of things that we change are changed correctly. On the negative side, well, that isn't the only thing we have to concentrate on. We have to figure out how to preserve the rest of the behavior. Unfortunately, preserving it involves more than just leaving the code alone. We have to know that the behavior isn't changing, and that can be tough. The amount of behavior that we have to preserve is usually very large, but that isn't the big deal. The big deal is that we often don't know how much of that behavior is at risk when we make our changes. If we knew, we could concentrate on that behavior and not care about the rest. Understanding is the key thing that we need to make changes safely.

> 这是一个很好的观点，说明当我们做出改变时应该发生什么，但实际上这对我们意味着什么？从积极的方面来看，它似乎告诉我们必须专注于什么。我们必须确保我们改变的少数事情得到正确的改变。从消极的方面来说，这并不是我们必须集中精力的唯一事情。我们必须想办法保护剩下的行为。不幸的是，保留它所涉及的不仅仅是代码本身。我们必须知道行为没有改变，这可能很艰难。我们必须预先发球的行为量通常很大，但这并不是什么大不了的。重要的是，当我们做出改变时，我们往往不知道这种行为有多大风险。如果我们知道，我们可以专注于这种行为，而不在乎其他的。理解是我们安全做出改变所需要的关键。

#### [Risky Change](#contents)

Preserving behavior is a large challenge. When we need to make changes and preserve behavior, it can involve considerable risk.

> 保护行为是一个巨大的挑战。当我们需要做出改变并保持行为时，可能会涉及相当大的风险。

[]{#_bookmark18 .anchor}To mitigate risk, we have to ask three questions:

1.  What changes do we have to make?

2.  How will we know that we've done them correctly?

3.  How will we know that we haven't broken anything? How much change can you afford if changes are risky?

> 3.我们怎么知道自己没有摔坏任何东西？如果改变有风险，你能负担得起多少改变？

Most teams that I've worked with have tried to manage risk in a very conservative way. They minimize the number of changes that they make to the code base. Sometimes this is a team policy: "If it's not broke, don't fix it." At other times, it isn't anything that anyone articulates. The developers are just very cautious when they make changes. "What? Create another method for that? No, I'll just put the lines of code right here in the method, where I can see them and the rest of the code. It involves less editing, and it's safer."

> 与我共事过的大多数团队都试图以一种非常保守的方式管理风险。它们最大限度地减少了对代码库所做的更改数量。有时这是一个团队政策：“如果它没有坏，就不要修理它。”在其他时候，这不是任何人所说的。开发人员在进行更改时非常谨慎。“什么？为此创建另一个方法？不，我只会把代码行放在方法的这里，在那里我可以看到它们和代码的其余部分。它需要更少的编辑，而且更安全。”

It's tempting to think that we can minimize software problems by avoiding them, but, unfortunately, it always catches up with us. When we avoid creating new classes and methods, the existing ones grow larger and harder to understand. When you make changes in any large system, you can expect to take a little time to get familiar with the area you are working with. The difference between good systems and bad ones is that, in the good ones, you feel pretty calm after you've done that learning, and you are confident in the change you are about to make. In poorly structured code, the move from figuring things out to making changes feels like jumping off a cliff to avoid a tiger. You hesitate and hesitate. "Am I ready to do it? Well, I guess I have to."

> 人们很容易认为，我们可以通过避免软件问题来最大限度地减少软件问题，但不幸的是，它总是困扰着我们。当我们避免创建新的类和方法时，现有的类和算法会变得更大，更难被低估。当你在任何大型系统中进行更改时，你可能需要花一点时间来熟悉你正在工作的领域。好的系统和坏的系统的区别在于，在好的系统中，你在完成学习后会感到非常平静，并且对即将做出的改变充满信心。在结构不佳的代码中，从弄清楚事情到做出更改，感觉就像是为了躲避老虎而跳下悬崖。你犹豫了又犹豫。“我准备好了吗？嗯，我想我必须这么做。”

Avoiding change has other bad consequences. When people don't make changes often they get rusty at it. Breaking down a big class into pieces can be pretty involved work unless you do it a couple of times a week. When you do, it becomes routine. You get better at figuring out what can break and what can't, and it is much easier to do.

> 避免改变还有其他不良后果。当人们不做出改变时，他们往往会对此感到生疏。除非你每周做几次，否则把一个大班分解成碎片可能是一项非常复杂的工作。当你这样做的时候，它就变成了例行公事。你会更好地弄清楚什么可以打破，什么不能打破，而且做起来容易得多。

The last consequence of avoiding change is fear. Unfortunately, many teams live with incredible fear of change and it gets worse every day. Often they aren't aware of how much fear they have until they learn better techniques and the fear starts to fade away.

> 避免改变的最后一个后果是恐惧。不幸的是，许多球队都生活在对变化的恐惧中，而且情况一天比一天糟糕。通常，直到他们学会了更好的技巧，恐惧开始消退，他们才意识到自己有多害怕。

We've talked about how avoiding change is a bad thing, but what is our alternative? One alternative is to just try harder. Maybe we can hire more people so that there is enough time for everyone to sit and analyze, to scrutinize all of the code and make changes the "right" way. Surely more time and scrutiny will make change safer. Or will it? After all of that scrutiny, will anyone know that they've gotten it right?

> 我们已经讨论过避免改变是一件坏事，但我们的替代方案是什么？一种选择是更加努力。也许我们可以雇佣更多的员工，这样每个人都有足够的时间坐下来分析，仔细审查所有代码，并以“正确”的方式进行更改。当然，更多的时间和审查将使变革更加安全。还是会这样？经过所有这些审查，有人知道他们做对了吗？

[[]{#_bookmark20 .anchor}]{#_bookmark19 .anchor}[**Chapter 2**](#contents)

> [[]{#_bookmark20.anchor}]{#_blaookmark19.anchor}[**第二章**](#contents)

### [Working with Feedback](#contents)

Changes in a system can be made in two primary ways. I like to call them _Edit and Pray_ and _Cover and Modify_. Unfortunately, _Edit and Pray_ is pretty much the industry standard. When you use _Edit and Pray_, you carefully plan the changes you are going to make, you make sure that you understand the code you are going to modify, and then you start to make the changes. When you're done, you run the system to see if the change was enabled, and then you poke around further to make sure that you didn't break anything. The poking around is essential. When you make your changes, you are hoping and praying that you'll get them right, and you take extra time when you are done to make sure that you did.

> 系统中的更改可以通过两种主要方式进行。我喜欢称它们为“编辑和祈祷”和“覆盖和修改”。不幸的是，“编辑和祈祷”几乎是行业标准。当你使用“编辑和祈祷”时，你要仔细计划你将要做的更改，确保你理解你将要修改的代码，然后你开始做更改。完成后，您运行系统以查看更改是否已启用，然后进一步查看以确保您没有破坏任何内容。四处窥探是必不可少的。当你做出改变时，你希望并祈祷你能把它们做好，当你完成时，你会花额外的时间来确保你做到了。

Superficially, _Edit and Pray_ seems like "working with care," a very professional thing to do. The "care" that you take is right there at the forefront, and you expend extra care when the changes are very invasive because much more can go wrong. But safety isn't solely a function of care. I don't think any of us would choose a surgeon who operated with a butter knife just because he worked with care. Effective software change, like effective surgery, really involves deeper skills. Working with care doesn't do much for you if you don't use the right tools and techniques.

> 从表面上看，“编辑和祈祷”似乎是“小心工作”，这是一件非常专业的事情。你所采取的“小心”是最重要的，当变化非常具有侵略性时，你会花费额外的小心，因为可能会出更多的问题。但安全不仅仅是一种关心的功能。我不认为我们中的任何人会选择一个用黄油刀做手术的外科医生，仅仅因为他工作认真。有效的软件更改，就像有效的手术一样，确实需要更深层次的技能。如果你不使用正确的工具和技术，谨慎工作对你没有多大帮助。

_Cover and Modify_ is a different way of making changes. The idea behind it is that it is possible to work with a _safety net_ when we change software. The safety net we use isn't something that we put underneath our tables to catch us if we fall out of our chairs. Instead, it's kind of like a cloak that we put over code we are working on to make sure that bad changes don't leak out and infect the rest of our software. Covering software means covering it with tests. When we have a good set of tests around a piece of code, we can make changes and find out very quickly whether the effects were good or bad. We still apply the same care, but with the feedback we get, we are able to make changes more carefully.

> *覆盖和修改*是一种不同的更改方式。它背后的想法是，当我们更换软件时，可以使用“安全网”。如果我们从椅子上摔下来，我们使用的安全网并不是放在桌子下面用来抓我们的。相反，它有点像一件斗篷，我们把它盖在正在处理的代码上，以确保糟糕的更改不会泄露出去并感染我们软件的其他部分。覆盖软件意味着用测试覆盖它。当我们对一段代码进行了一组好的测试时，我们可以进行更改，并很快发现效果是好是坏。我们仍然采取同样的谨慎态度，但有了反馈，我们能够更加谨慎地做出改变。

If you are not familiar with this use of tests, all of this is bound to sound a little bit odd. Traditionally, tests are written and executed after development. A

> 如果你不熟悉测试的这种用法，那么所有这些听起来一定有点奇怪。传统上，测试是在开发之后编写和执行的。A.

####### 9

[]{#_bookmark21 .anchor}group of programmers writes code and a team of testers runs tests against the code afterward to see if it meets some specification. In some very traditional development shops, this is just the way that software is developed. The team can get feedback, but the feedback loop is large. Work for a few weeks or months, and then people in another group will tell you whether you've gotten it right.

> []{#_bookmark21.anchor}一组程序员编写代码，然后一组测试人员对代码进行测试，看看它是否符合某些规范。在一些非常传统的开发商店中，这只是软件的开发方式。团队可以获得反馈，但反馈循环很大。工作几周或几个月，然后另一个小组的人会告诉你你是否做对了。

Testing done this way is really "testing to attempt to show correctness." Although that is a good goal, tests can also be used in a very different way. We can do "testing to detect change."

> 以这种方式进行的测试实际上是“尝试显示正确性的测试”。尽管这是一个很好的目标，但测试也可以以非常不同的方式使用。我们可以做“检测变化的测试”

In traditional terms, this is called regression testing. We periodically run tests that check for known good behavior to find out whether our software still works the way that it did in the past.

> 在传统的术语中，这被称为回归测试。我们定期运行测试，检查已知的良好行为，以了解我们的软件是否仍然像过去那样工作。

When you have tests around the areas in which you are going to make changes, they act as a software vise. You can keep most of the behavior fixed and know that you are changing only what you intend to.

> 当你对要进行更改的领域进行测试时，它们就像一把软件老虎钳。你可以固定大部分行为，并知道你只改变了你想要的。

Regression testing is a great idea. Why don't people do it more often? There is this little problem with regression testing. Often when people practice it, they do it at the application interface. It doesn't matter whether it is a web application, a command-line application, or a GUI-based application; regression testing has traditionally been seen as an application-level testing style. But this is unfortunate. The feedback we can get from it is very useful. It pays to do it at a finer-grained level.

> 回归测试是个好主意。为什么人们不经常这样做呢？回归测试有一个小问题。通常，当人们练习它时，他们会在应用程序界面上进行。不管是 web 应用程序、命令行应用程序还是基于 GUI 的应用程序；回归测试传统上被视为一种应用程序级测试风格。但这是不幸的。我们从中得到的反馈非常有用。在更细粒度的级别上做这件事是值得的。

Let's do a little thought experiment. We are stepping into a large function that contains a large amount of complicated logic. We analyze, we think, we talk to people who know more about that piece of code than we do, and then we make a change. We want to make sure that the change hasn't broken anything, but how can we do it? Luckily, we have a quality group that has a set of regression tests that it can run overnight. We call and ask them to schedule a run, and they say that, yes, they can run the tests overnight, but it is a good thing that we called early. Other groups usually try to schedule regression runs in the middle of the week, and if we'd waited any longer, there might not be a

> 让我们做一个小小的思想实验。我们正在进入一个包含大量复杂逻辑的大型函数。我们分析，我们思考，我们与比我们更了解这段代码的人交谈，然后我们做出改变。我们想确保这个改变没有破坏任何东西，但我们该怎么做呢？幸运的是，我们有一个质量小组，它有一组回归测试，可以在一夜之间运行。我们打电话让他们安排一次测试，他们说，是的，他们可以在一夜之间进行测试，但我们早点打电话是件好事。其他小组通常会尝试将回归运行安排在周中，如果我们再等一段时间，可能就不会有

[]{#_bookmark22 .anchor}timeslot and a machine available for us. We breathe a sigh of relief and then go back to work. We have about five more changes to make like the last one. All of them are in equally complicated areas. And we're not alone. We know that several other people are making changes, too.

> []{#_bookmark22.anchor}时隙和一台机器。我们松了一口气，然后回去工作。与上一次一样，我们还有大约五次更改要做。它们都处于同样复杂的领域。我们并不孤单。我们知道其他一些人也在做出改变。

The next morning, we get a phone call. Daiva over in testing tells us that tests AE1021 and AE1029 failed overnight. She's not sure whether it was our changes, but she is calling us because she knows we'll take care of it for her. We'll debug and see if the failures were because of one of our changes or someone else's.

> 第二天早上，我们接到一个电话。Daiva 在测试中告诉告诉，测试 AE1021 和 AE1029 在一夜之间失败。她不确定这是否是我们的改变，但她打电话给我们，因为她知道我们会帮她处理的。我们将进行调试，看看失败是因为我们的某个更改还是其他更改。

Does this sound real? Unfortunately, it is very real. Let's look at another scenario.

> 这听起来是真的吗？不幸的是，这是非常真实的。让我们看看另一个场景。

We need to make a change to a rather long, complicated function. Luckily, we find a set of unit tests in place for it. The last people who touched the code wrote a set of about 20 unit tests that thoroughly exercised it. We run them and discover that they all pass. Next we look through the tests to get a sense of what the code's actual behavior is.

> 我们需要对一个相当长而复杂的函数进行更改。幸运的是，我们为它找到了一组单元测试。最后一个接触代码的人写了一组大约 20 个单元测试，对它进行了彻底的测试。我们运行它们，发现它们都通过了。接下来，我们查看测试，了解代码的实际行为。

We get ready to make our change, but we realize that it is pretty hard to figure out how to change it. The code is unclear, and we'd really like to understand it better before making our change. The tests won't catch everything, so we want to make the code very clear so that we can have more confidence in our change. Aside from that, we don't want ourselves or anyone else to have to go through the work we are doing to try to understand it. What a waste of time!

> 我们已经准备好进行更改，但我们意识到很难弄清楚如何进行更改。代码尚不清楚，在进行更改之前，我们真的希望能更好地理解它。测试不会捕捉到所有内容，所以我们想让代码非常清晰，这样我们就可以对我们的更改更有信心。除此之外，我们不希望自己或其他人不得不经历我们正在做的工作来试图理解它。真是浪费时间！

We start to refactor the code a bit. We extract some methods and move some conditional logic. After every little change that we make, we run that little suite of unit tests. They pass almost every time that we run them. A few minutes ago, we made a mistake and inverted the logic on a condition, but a test failed and we recovered in about a minute. When we are done refactoring, the code is much clearer. We make the change we set out to make, and we are confident that it is right. We added some tests to verify the new behavior. The next programmers who work on this piece of code will have an easier time and will have tests that cover its functionality.

> 我们开始对代码进行一些重构。我们提取了一些方法，移动了一些条件逻辑。在我们做了每一个小小的更改之后，我们都会运行一小套单元测试。几乎每次我们运行它们时，它们都会通过。几分钟前，我们犯了一个错误，在一个条件下颠倒了逻辑，但测试失败了，我们在大约一分钟内恢复了状态。当我们完成重构时，代码会更加清晰。我们做出了我们打算做出的改变，我们相信这是正确的。我们添加了一些测试来验证新行为。下一个处理这段代码的程序员将有一段更轻松的时间，并将进行涵盖其功能的测试。

Do you want your feedback in a minute or overnight? Which scenario is more efficient?

> 你想在一分钟内还是一夜之间得到反馈？哪种方案更有效？

Unit testing is one of the most important components in legacy code work. System-level regression tests are great, but small, localized tests are invaluable. They can give you feedback as you develop and allow you to refactor with much more safety.

> 单元测试是遗留代码工作中最重要的组成部分之一。系统级的回归测试很棒，但小型的本地化测试是非常宝贵的。它们可以在你开发的过程中给你反馈，并允许你以更安全的方式进行重构。

#### [What Is Unit Testing?](#contents)

The term _unit test_ has a long history in software development. Common to most conceptions of unit tests is the idea that they are tests in isolation of individual components of software. What are components? The definition varies, but in unit testing, we are usually concerned with the most atomic behavioral units of a system. In procedural code, the units are often functions. In objectoriented code, the units are classes.

> “单元测试”一词在软件开发中有着悠久的历史。大多数单元测试概念的共同点是，它们是独立于软件单个组件的测试。什么是组件？定义各不相同，但在单元测试中，我们通常关注系统中最原子的行为单元。在程序代码中，单元通常是函数。在面向对象的代码中，单元是类。

Can we ever test only one function or one class? In procedural systems, it is often hard to test functions in isolation. Top-level functions call other functions, which call other functions, all the way down to the machine level. In object-oriented systems, it is a little easier to test classes in isolation, but the fact is, classes don't generally live in isolation. Think about all of the classes you've ever written that don't use other classes. They are pretty rare, aren't they? Usually they are little data classes or data structure classes such as stacks and queues (and even these might use other classes).

> 我们可以只测试一个函数或一个类吗？在过程系统中，通常很难单独测试函数。顶级函数调用其他函数，这些函数调用其他功能，一直到机器级别。在面向对象的系统中，隔离测试类稍微容易一些，但事实是，类通常不会隔离。想想你写过的所有不使用其他类的类。它们非常罕见，不是吗？通常，它们是小数据类或数据结构类，如堆栈和队列(甚至可能使用其他类)。

Testing in isolation is an important part of the definition of a unit test, but why is it important? After all, many errors are possible when pieces of software are integrated. Shouldn't large tests that cover broad functional areas of code be more important? Well, they are important, I won't deny that, but there are a few problems with large tests:

> 隔离测试是单元测试定义的重要组成部分，但为什么它很重要？毕竟，在集成软件时可能会出现许多错误。覆盖代码广泛功能领域的大型测试难道不应该更重要吗？嗯，它们很重要，我不会否认，但大型测试存在一些问题：

- **Error localization**---As tests get further from what they test, it is harder to determine what a test failure means. Often it takes considerable work to pinpoint the source of a test failure. You have to look at the test inputs, look at the failure, and determine where along the path from inputs to outputs the failure occurred. Yes, we have to do that for unit tests also, but often the work is trivial.

> -**错误定位**---随着测试离测试越来越远，很难确定测试失败意味着什么。通常需要大量的工作来确定测试失败的原因。您必须查看测试输入，查看故障，并确定从输入到输出的路径上发生故障的位置。是的，我们也必须为单元测试这样做，但通常工作是琐碎的。

- **Execution time**---Larger tests tend to take longer to execute. This tends to make test runs rather frustrating. Tests that take too long to run end up not being run.

> -**执行时间**---较大的测试往往需要更长的时间才能执行。这往往会使测试运行相当令人沮丧。运行时间过长的测试最终无法运行。

- []{#_bookmark25 .anchor}**Coverage**---It is hard to see the connection between a piece of code and the values that exercise it. We can usually find out whether a piece of code is exercised by a test using coverage tools, but when we add new code, we might have to do considerable work to create high-level tests that exercise the new code.

> -[]｛#_bookmark25.anchor｝**Coverage**---很难看出一段代码和执行它的值之间的联系。我们通常可以使用覆盖率工具来确定一段代码是否由测试执行，但当我们添加新代码时，我们可能需要做大量的工作来创建执行新代码的高级测试。

Unit tests fill in gaps that larger tests can't. We can test pieces of code independently; we can group tests so that we can run some under some conditions and others under other conditions. With them we can localize errors quickly. If we think there is an error in some particular piece of code and we can use it in a test harness, we can usually code up a test quickly to see if the error really is there.

> 单元测试填补了大型测试无法填补的空白。我们可以独立测试代码片段；我们可以对测试进行分组，这样我们就可以在某些条件下运行一些测试，而在其他条件下运行另一些测试。有了它们，我们可以快速定位错误。如果我们认为某段特定的代码中存在错误，并且我们可以在测试中使用它，我们通常可以快速编写测试代码，看看错误是否真的存在。

Here are qualities of good unit tests:

1.  They run fast.

2.  They help us localize problems.

In the industry, people often go back and forth about whether particular tests are unit tests. Is a test really a unit test if it uses another production class? I go back to the two qualities: Does the test run fast? Can it help us localize errors quickly? Naturally, there is a continuum. Some tests are larger, and they use several classes together. In fact, they may seem to be little integration tests. By themselves, they might seem to run fast, but what happens when you run them all together? When you have a test that exercises a class along with several of its collaborators, it tends to grow. If you haven't taken the time to make a class separately instantiable in a test harness, how easy will it be when you add more code? It never gets easier. People put it off. Over time, the test might end up taking as long as 1/10th of a second to execute.

> 在这个行业中，人们经常反复讨论特定的测试是否是单元测试。如果一个测试使用另一个生产类，那么它真的是单元测试吗？我回到两个品质：测试跑得快吗？它能帮助我们快速定位错误吗？自然，这是一个连续体。有些测试比较大，它们同时使用几个类。事实上，它们似乎只是一些小的集成测试。就它们自己而言，它们可能看起来跑得很快，但当你把它们一起跑时会发生什么？当你有一个测试与几个合作者一起练习一个类时，它往往会增长。如果您还没有花时间在测试工具中单独实例化一个类，那么添加更多代码会有多容易？它从未变得更容易。人们推迟了测试。随着时间的推移，测试可能需要十分之一秒的时间才能执行。

Yes, I'm serious. At the time that I'm writing this, 1/10th of a second is an eon for a unit test. Let's do the math. If you have a project with 3,000 classes and there are about 10 tests apiece, that is 30,000 tests. How long will it take to run all of the tests for that project if they take 1/10th of a second apiece? Close

> 是的，我是认真的。在我写这篇文章的时候，十分之一秒是单元测试的 eon。让我们计算一下。如果你有一个有 3000 个类的项目，每个类大约有 10 个测试，那就是 30000 个测试。如果每个项目需要 1/10 秒的时间，那么运行该项目的所有测试需要多长时间？关

[]{#_bookmark26 .anchor}to an hour. That is a long time to wait for feedback. You don't have 3,000 classes? Cut it in half. That is still a half an hour. On the other hand, what if the tests take 1/100th of a second apiece? Now we are talking about 5 to 10 minutes. When they take that long, I make sure that I use a subset to work with, but I don't mind running them all every couple of hours.

> []｛#_bookmark26.anchor｝到一个小时。等待反馈需要很长时间。你没有 3000 节课？把它切成两半。还有半个小时。另一方面，如果每次测试耗时 1/100 秒，该怎么办？现在我们讨论的是 5 到 10 分钟。当它们花那么长时间时，我会确保使用一个子集进行处理，但我不介意每隔几个小时运行一次。

With Moore's Law's help, I hope to see nearly instantaneous test feedback for even the largest systems in my lifetime. I suspect that working in those systems will be like working in code that can bite back. It will be capable of letting []{#_bookmark27 .anchor}us know when it is being changed in a bad way.

> 在摩尔定律的帮助下，即使是我一生中最大的系统，我也希望看到几乎即时的测试反馈。我怀疑在这些系统中工作就像在可以反噬的代码中工作一样。它将能够让[]｛#_bookmark27.anchor｝我们知道它何时以不好的方式被更改。

#### [Higher-Level Testing](#contents)

Unit tests are great, but there is a place for higher-level tests, tests that cover scenarios and interactions in an application. Higher-level tests can be used to pin down behavior for a set of classes at a time. When you are able to do that, often you can write tests for the individual classes more easily.

> 单元测试很好，但也有更高级别的测试，这些测试涵盖了应用程序中的场景和交互。更高级别的测试可以用于一次确定一组类的行为。当您能够做到这一点时，通常可以更容易地为各个类编写测试。

#### [Test Coverings](#contents)

So how do we start making changes in a legacy project? The first thing to notice is that, given a choice, it is always safer to have tests around the changes that we make. When we change code, we can introduce errors; after all, we're all

> 那么，我们如何开始对遗留项目进行更改呢？首先要注意的是，如果有选择，围绕我们所做的更改进行测试总是更安全的。当我们更改代码时，我们可能会引入错误；毕竟，我们都是

human. But when we cover our code with tests before we change it, we're more likely to catch any mistakes that we make.

> 人类但是，当我们在更改代码之前用测试覆盖代码时，我们更有可能发现我们犯的任何错误。

Figure 2.1 shows us a little set of classes. We want to make changes to the getResponseText method of InvoiceUpdateResponder and the getValue method of Invoice. Those methods are our change points. We can cover them by writing tests for the classes they reside in.

> 图 2.1 显示了一组小类。我们想更改 InvoiceUpdateResponder 的 getResponseText 方法和 Invoice 的 getValue 方法。这些方法是我们的转变点。我们可以通过为它们所在的类编写测试来覆盖它们。

To write and run tests we have to be able to create instances of InvoiceUpdateResponder and Invoice in a testing harness. Can we do that? Well, it looks like it should be easy enough to create an Invoice; it has a constructor that doesn't accept any arguments. InvoiceUpdateResponder might be tricky, though. It accepts a DBConnection, a real connection to a live database. How are we going to handle that in a test? Do we have to set up a database with data for our tests? That's a lot of work. Won't testing through the database be slow? We don't particularly care about the database right now anyway; we just want to cover our changes in InvoiceUpdateResponder and Invoice. We also have a bigger problem. The constructor for InvoiceUpdateResponder needs an InvoiceUpdateServlet as an argument. How easy will it be to create one of those? We could change the code so that it

> 为了编写和运行测试，我们必须能够在测试工具中创建 InvoiceUpdate-Respoder 和 Invoice 的实例。我们能做到吗？嗯，看起来创建发票应该很容易；它有一个不接受任何参数的构造函数。InvoiceUpdateResponder 可能很棘手。它接受 DBConnection，一个到活动数据库的真实连接。我们将如何在测试中处理这个问题？我们是否必须建立一个包含测试数据的数据库？这是一项艰巨的工作。通过数据库进行测试不会很慢吗？无论如何，我们现在并不特别关心数据库；我们只想涵盖 InvoiceUpdateResponder 和 Invoice 中的更改。我们还有一个更大的问题。InvoiceUpdateResponder 的结构需要 InvoiceUpdateServlet 作为参数。创建其中一个会有多容易？我们可以更改代码

+------------------------------------+---+-----------------------------------+
| > \+ getResponseText () : String | | |
+====================================+===+===================================+
| | | |
+------------------------------------+---+-----------------------------------+
| > Changing **getResponseText** and | | |
| > | | |
| > **getValue** | | |
+------------------------------------+---+-----------------------------------+
| | | |
+------------------------------------+---+-----------------------------------+

**Figure 2.1** _Invoice update classes._

[]{#_bookmark29 .anchor}doesn't take that servlet anymore. If the InvoiceUpdateResponder just needs a little bit of information from InvoiceUpdateServlet, we can pass it along instead of passing the whole servlet in, but shouldn't we have a test in place to make sure that we've made that change correctly?

> []｛#_bookmark29.anchor｝不再使用该 servlet。如果 InvoiceUpdateResponder 只需要来自 InvoiceUpdateServlet 的一点信息，我们可以传递它，而不是传递整个 servlet，但我们不应该有一个测试来确保我们正确地进行了更改吗？

All of these problems are dependency problems. When classes depend directly on things that are hard to use in a test, they are hard to modify and hard to work with.

> 所有这些问题都是依赖性问题。当类直接依赖于测试中难以使用的东西时，它们很难修改，也很难使用。

So, how do we do it? How do we get tests in place without changing code? The sad fact is that, in many cases, it isn't very practical. In some cases, it might even be impossible. In the example we just saw, we could attempt to get past the DBConnection issue by using a real database, but what about the servlet issue? Do we have to create a full servlet and pass it to the constructor of InvoiceUpdateResponder? Can we get it into the right state? It might be possible. What would we do if we were working in a GUI desktop application? We might not have any programmatic interface. The logic could be tied right into the GUI classes. What do we do then?

> 那么，我们该怎么做呢？我们如何在不更改代码的情况下进行测试？可悲的事实是，在许多情况下，这并不太实际。在某些情况下，这甚至可能是不可能的。在我们刚刚看到的示例中，我们可以尝试通过使用真实的数据库来解决 DBConnection 问题，但是 servlet 问题呢？我们是否必须创建一个完整的 servlet 并将其传递给 InvoiceUpdat-eResponder 的构造函数？我们能让它进入正确的状态吗？这也许是可能的。如果我们在 GUI 桌面应用程序中工作，我们会怎么做？我们可能没有任何编程接口。逻辑可以直接绑定到 GUI 类中。那我们该怎么办？

In the Invoice example we can try to test at a higher level. If it is hard to write tests without changing a particular class, sometimes testing a class that uses it is easier; regardless, we usually have to break dependencies between classes someplace. In this case, we can break the dependency on InvoiceUpdateServlet by passing the one thing that InvoiceUpdateResponder really needs. It needs the collection of invoice IDs that the InvoiceUpdateServlet holds. We can also break the dependency that InvoiceUpdateResponder has on DBConnection by introducing an interface (IDBConnection) and changing the InvoiceUpdateResponder so that it uses the interface instead. Figure 2.2 shows the state of these classes after the changes.

> 在 Invoice 示例中，我们可以尝试在更高级别进行测试。如果在不更改特定类的情况下编写测试很困难，那么有时测试使用它的类会更容易；不管怎样，我们通常必须在某个地方打破类之间的依赖关系。在这种情况下，我们可以通过传递 InvoiceUpdateResponder 真正需要的一件事来打破对 InvoiceUpdate-Serverlet 的依赖。它需要 InvoiceUpdateServlet 持有的发票 ID 的集合。我们还可以通过引入接口(IDBConnection)并更改 InvoiceUpdateResponder 以使其使用该接口来打破 InvoiceUpdateResponse 对 DBConnection 的依赖。图 2.2 显示了更改后这些类的状态。

+---------------------------------------------------------+----------------------------------------------------------------------------------------+
| > []{#_bookmark30 .anchor}**«interface» IDBConnection** | |
+=========================================================+========================================================================================+
| > \+ getInvoices(Criteria) : List | |
+---------------------------------------------------------+----------------------------------------------------------------------------------------+
| | ![](./media/image3.png){width="5.615485564304462e-2in" height="0.11718722659667541in"} |
+---------------------------------------------------------+----------------------------------------------------------------------------------------+
| > **DBConnection** | |
+---------------------------------------------------------+----------------------------------------------------------------------------------------+
| > \+ getInvoices(Criteria) : List | |
+---------------------------------------------------------+----------------------------------------------------------------------------------------+

**Figure 2.2** _Invoice update classes with dependencies broken._

Is this safe to do these refactorings without tests? It can be. These refactorings are named _Primitivize Parameter (385)_ and _Extract Interface (362)_, respectively. They are described in the dependency breaking techniques catalog at the end of the book. When we break dependencies, we can often write tests that make more invasive changes safer. The trick is to do these initial refactorings very conservatively.

> 在没有测试的情况下进行这些重构安全吗？可以。这些重构分别命名为*Primitialize Parameter(385)*和*Extract Interface(362)*。它们在本书末尾的依赖性打破技术目录中进行了描述。当我们打破依赖关系时，我们通常可以编写测试，使更具侵入性的更改更安全。诀窍是非常保守地进行这些初始重构。

Being conservative is the right thing to do when we can possibly introduce errors, but sometimes when we break dependencies to cover code, it doesn't turn out as nicely as what we did in the previous example. We might introduce parameters to methods that aren't strictly needed in production code, or we might break apart classes in odd ways just to be able to get tests in place. When we do that, we might end up making the code look a little poorer in that area. If we were being less conservative, we'd just fix it immediately. We can do that,

> 当我们可能会引入错误时，保守是正确的做法，但有时当我们打破依赖关系来覆盖代码时，结果并不像上一个例子那样好。我们可能会将参数引入到生产代码中不严格需要的方法中，或者我们可能会以奇怪的方式拆分类，以便能够进行适当的测试。当我们这样做的时候，我们可能会使代码在该领域看起来更差。如果我们不那么保守，我们会立即解决它。我们可以做到，

[[]{#_bookmark32 .anchor}]{#_bookmark31 .anchor}but it depends upon how much risk is involved. When errors are a big deal, and they usually are, it pays to be conservative.

> [[]{#_bookmark32.anchor}]{#_blaookmark31.anchor｝，但这取决于涉及的风险有多大。当错误是一件大事时，保守是值得的。

#### [The Legacy Code Change Algorithm](#contents)

When you have to make a change in a legacy code base, here is an algorithm you can use.

> 当你必须对遗留代码库进行更改时，这里有一个你可以使用的算法。

1.  Identify change points.

2.  Find test points.

3.  Break dependencies.

4.  Write tests.

5.  Make changes and refactor.

The day-to-day goal in legacy code is to make changes, but not just any changes. We want to make functional changes that deliver value while bringing more of the system under test. At the end of each programming episode, we should be able to point not only to code that provides some new feature, but also its tests. Over time, tested areas of the code base surface like islands rising out of the ocean. Work in these islands becomes much easier. Over time, the islands become large landmasses. Eventually, you'll be able to work in continents of test-covered code.

> 遗留代码的日常目标是进行更改，而不仅仅是任何更改。我们希望在对系统进行更多测试的同时，进行能够带来价值的功能更改。在每一集节目的结尾，我们不仅应该能够指出提供一些新功能的代码，还应该能够指出它的测试。随着时间的推移，代码库的测试区域会像从海洋中升起的岛屿一样浮出水面。在这些岛上工作变得容易多了。随着时间的推移，这些岛屿变成了大片的陆地。最终，您将能够在测试覆盖的代码中工作。

Let's look at each of these steps and how his book will help you with them.

> 让我们看看这些步骤中的每一个，以及他的书将如何帮助你。

##### Identify Change Points

The places where you need to make your changes depend sensitively on your architecture. If you don't know your design well enough to feel that you are making changes in the right place, take a look at Chapter 16, _I Don't Understand the Code Well Enough to Change It_, and Chapter 17, _My Application Has No Structure_.

> 您需要进行更改的地方敏感地取决于您的架构。如果你对自己的设计还不够了解，觉得自己在正确的地方进行了更改，请参阅第 16 章“我没有足够好地理解代码来更改它”和第 17 章“我的应用程序没有结构”。

##### Find Test Points

In some cases, finding places to write tests is easy, but in legacy code it can often be hard. Take a look at Chapter 11, _I Need to Make a Change. What Methods Should I Test?_, and Chapter 12, _I Need to Make Many Changes in One Area. Do I Have to Break Dependencies for All the Classes Involved?_ These chapters offer techniques that you can use to determine where you need to write your tests for particular changes.

> 在某些情况下，找到编写测试的地方很容易，但在遗留代码中，这通常很难。看看第 11 章，_我需要做出改变。我应该测试什么方法？_，第 12 章，*我需要在一个领域做出许多改变。我必须打破所有相关类的依赖关系吗？*这些章节提供了一些技术，您可以使用这些技术来确定需要在哪里编写针对特定更改的测试。

##### Break Dependencies

Dependencies are often the most obvious impediment to testing. The two ways this problem manifests itself are difficulty instantiating objects in test harnesses and difficulty running methods in test harnesses. Often in legacy code, you have to break dependencies to get tests in place. Ideally, we would have tests that tell us whether the things we do to break dependencies themselves caused problems, but often we don't. Take a look at Chapter 23, _How Do I Know That I'm Not Breaking Anything?_, to see some practices that can be used to make the first incisions in a system safer as you start to bring it under test. When you have done this, take a look at Chapter 9, _I Can't Get This Class into a Test Harness_, and Chapter 10, _I Can't Run This Method in a Test Harness_, for scenarios that show how to get past common dependency problems. These sections heavily reference the dependency breaking techniques catalog at the back of the book, but they don't cover all of the techniques. Take some time to look through the catalog for more ideas on how to break dependencies.

> 依赖性往往是测试中最明显的障碍。这个问题表现出来的两种方式是难以在测试工具中实例化对象和难以在测试程序中运行方法。通常在遗留代码中，您必须打破依赖关系才能进行测试。理想情况下，我们会有测试来告诉我们，我们为打破依赖关系所做的事情本身是否会导致问题，但通常我们不会。看看第 23 章，“我怎么知道我没有破坏任何东西？”，看看一些可以用来让系统中的第一个切口在测试时更安全的做法。当你完成这项工作后，看看第 9 章“我不能让这个类进入测试环境”和第 10 章“我无法在测试环境中运行这个方法”，了解如何克服常见依赖性问题的场景。这些部分大量引用了本书后面的依赖性打破技术目录，但它们并没有涵盖所有的技术。花些时间浏览目录，了解有关如何打破依赖关系的更多想法。

Dependencies also show up when we have an idea for a test but we can't write it easily. If you find that you can't write tests because of dependencies in large methods, see Chapter 22, _I Need to Change a Monster Method and I Can't Write Tests for It_. If you find that you can break dependencies, but it takes too long to build your tests, take a look at Chapter 7, _It Takes Forever to Make a Change._ That chapter describes additional dependency-breaking work that you can do to make your average build time faster.

> 当我们有一个测试的想法，但我们无法轻松编写时，依赖关系也会出现。如果您发现由于大型方法的依赖性而无法编写测试，请参阅第 22 章“我需要更改一个 Monster 方法，我无法为它编写测试”。如果你发现你可以打破依赖关系，但构建测试需要很长时间，请参阅第 7 章，*永远需要做出改变。*该章描述了你可以做的其他打破依赖关系的工作，以加快平均构建时间。

##### Write Tests

I find that the tests I write in legacy code are somewhat different from the tests I write for new code. Take a look at Chapter 13, _I Need to Make a Change but I Don't Know What Tests to Write_, to learn more about the role of tests in legacy code work.

> 我发现我在遗留代码中编写的测试与我为新代码编写的测试有些不同。看看第 13 章“我需要做出改变，但我不知道要写什么测试”，了解更多关于测试在遗留代码工作中的作用。

##### Make Changes and Refactor

I advocate using test-driven development (TDD) to add features in legacy code. There is a description of TDD and some other feature addition techniques in Chapter 8, _How Do I Add a Feature?_ After making changes in legacy code, we often are better versed with its problems, and the tests we've written to add features often give us some cover to do some refactoring. Chapter 20, _This Class Is Too Big and I Don't Want It to Get Any Bigger_; Chapter 22, _I Need to Change a Monster Method and I Can't Write Tests for It_; and Chapter 21, _I'm Changing the Same Code All Over the Place_ cover many of the techniques you can use to start to move your legacy code toward better structure. Remember that the things I describe in these chapters are "baby steps." They don't show you how to make your design ideal, clean, or pattern-enriched. Plenty of books show how to do those things, and when you have the opportunity to use those techniques, I encourage you to do so. These chapters show you how to make design better, where "better" is context dependent and often simply a few steps more maintainable than the design was before. But don't discount this work. Often the simplest things, such as breaking down a large class just to make it easier to work with, can make a significant difference in applications, despite being somewhat mechanical.

> 我主张使用测试驱动开发(TDD)在遗留代码中添加特性。第 8 章“如何添加特性？”中介绍了 TDD 和其他一些特性添加技术在对遗留代码进行更改后，我们通常会更好地了解其问题，并且我们为添加特性而编写的测试通常会为我们进行一些重构提供一些掩护。第 20 章，“这个班太大了，我不想它变得更大”；第 22 章，“我需要改变一个怪物的方法，但我不能为它写测试”；第 21 章“我到处都在使用相同的代码”涵盖了许多可以用来开始将遗留代码推向更好结构的技术。请记住，我在这些章节中描述的是“小步”。它们并没有向你展示如何使你的设计变得理想、干净或丰富。很多书都展示了如何做这些事情，当你有机会使用这些技术时，我鼓励你这样做。这些章节向你展示了如何让设计变得更好，其中“更好”取决于上下文，通常只是比以前的设计更容易维护几个步骤。但不要低估这项工作。通常，最简单的事情，比如分解一个大类以使其更容易使用，可以在应用程序中产生重大影响，尽管这有点机械化。

##### The Rest of This Book

The rest of this book shows you how to make changes in legacy code. The next two chapters contain some background material about three critical concepts in legacy work: sensing, separation, and seams.

> 本书的其余部分将向您展示如何对遗留代码进行更改。接下来的两章包含一些关于遗留工作中三个关键概念的背景材料：传感、分离和接缝。

[[]{#_bookmark36 .anchor}]{#_bookmark35 .anchor}[**Chapter 3**](#contents)

> [[]{#_bookmark36.anchor}]{#_blaookmark35.anchor}[**第三章**](#contents)

### [Sensing and Separation](#contents)

Ideally, we wouldn't have to do anything special to a class to start working with it. In an ideal system, we'd be able to create objects of any class in a test harness and start working. We'd be able to create objects, write tests for them, and then move on to other things. If it were that easy, there wouldn't be a need to write about any of this, but unfortunately, it is often hard. Dependencies among classes can make it very difficult to get particular clusters of objects under test. We might want to create an object of one class and ask it questions, but to create it, we need objects of another class, and those objects need objects of another class, and so on. Eventually, you end up with nearly the whole system in a harness. In some languages, this isn't a very big deal. In others, most notably C++, link time alone can make rapid turnaround nearly impossible if you don't break dependencies.

> 理想情况下，我们不必对一个类做任何特殊的事情就可以开始使用它。在理想的系统中，我们可以在测试工具中创建任何类的对象并开始工作。我们将能够创建对象，为它们编写测试，然后继续做其他事情。如果这很容易，就没有必要写这些了，但不幸的是，这通常很难。类之间的依赖关系可能会使获取测试中的特定对象集群变得非常困难。我们可能想创建一个类的对象并向它提问，但要创建它，我们需要另一个类，这些对象需要另一类的对象，以此类推。最终，你会发现几乎整个系统都在一个线束中。在某些语言中，这不是什么大事。在其他情况下，尤其是 C++，如果不打破依赖关系，仅连接时间就几乎不可能实现快速周转。

In systems that weren't developed concurrently with unit tests, we often have to break dependencies to get classes into a test harness, but that isn't the only reason to break dependencies. Sometimes the class we want to test has effects on other classes, and our tests need to know about them. Sometimes we can sense those effects through the interface of the other class. At other times, we can't. The only choice we have is to impersonate the other class so that we can sense the effects directly.

> 在没有与单元测试同时开发的系统中，我们通常必须打破依赖关系才能将类纳入测试工具，但这并不是打破依赖关系的唯一原因。有时，我们想要测试的类会对其他类产生影响，我们的测试需要了解它们。有时我们可以通过其他类的接口来感知这些效果。在其他时候，我们不能。我们唯一的选择是模拟另一个类，这样我们就可以直接感知效果。

Generally, when we want to get tests in place, there are two reasons to break dependencies: _sensing_ and _separation_.

> 通常，当我们想要进行适当的测试时，有两个原因可以打破依赖关系：*感知*和*分离*。

1.  **Sensing**---We break dependencies to _sense_ when we can't access values our code computes.

> 1.**感知\***——当我们无法访问代码计算的值时，我们会将依赖关系分解为*sense*。

2.  **Separation**---We break dependencies to _separate_ when we can't even get a piece of code into a test harness to run.

> 2.**Separation**---当我们甚至无法将一段代码放入测试工具中运行时，我们会将依赖关系分解为*Separation*。

####### 21

[]{#_bookmark37 .anchor}Here is an example. We have a class named NetworkBridge in a network-management application:

> []｛#_bookmark37.anchor｝下面是一个示例。我们在网络管理应用程序中有一个名为 NetworkBridge 的类：

public class NetworkBridge

{

public NetworkBridge(EndPoint \[\] endpoints) {

\...

}

public void formRouting(String sourceID, String destID) {

\...

}

\...

}

NetworkBridge accepts an array of EndPoints and manages their configuration

> NetworkBridge 接受端点阵列并管理其配置

using some local hardware. Users of NetworkBridge can use its methods to route traffic from one endpoint to another. NetworkBridge does this work by changing settings on the EndPoint class. Each instance of the EndPoint class opens a socket and communicates across the network to a particular device.

> 使用一些本地硬件。NetworkBridge 的用户可以使用其方法将流量从一个端点路由到另一个端点。NetworkBridge 通过更改 EndPoint 类上的设置来完成这项工作。EndPoint 类的每个实例都打开一个套接字，并通过网络与特定设备通信。

That was just a short description of what NetworkBridge does. We could go into more detail, but from a testing perspective, there are already some evident problems. If we want to write tests for NetworkBridge, how do we do it? The class could very well make some calls to real hardware when it is constructed. Do we need to have the hardware available to create an instance of the class? Worse than that, how in the world do we know what the bridge is doing to that hardware or the endpoints? From our point of view, the class is a closed box.

> 这只是对 NetworkBridge 功能的简短描述。我们可以更详细地介绍，但从测试的角度来看，已经存在一些明显的问题。如果我们想为 NetworkBridge 编写测试，我们该怎么做？该类在构造时可以很好地对实际硬件进行一些调用。我们需要有可用的硬件来创建类的实例吗？更糟糕的是，我们怎么知道这座桥对硬件或端点做了什么？从我们的角度来看，这个类是一个封闭的盒子。

It might not be too bad. Maybe we can write some code to sniff packets across the network. Maybe we can get some hardware for NetworkBridge to talk to so that at the very least it doesn't freeze when we try to make an instance of it. Maybe we can set up the wiring so that we can have a local cluster of endpoints and use them under test. Those solutions could work, but they are an awful lot of work. The logic that we want to change in NetworkBridge might not need any of those things; it's just that we can't get a hold of it. We can't run an object of that class and try it directly to see how it works.

> 这可能还不算太糟。也许我们可以写一些代码来嗅探网络中的数据包。也许我们可以找一些硬件让 NetworkBridge 与之对话，这样当我们尝试创建一个实例时，它至少不会冻结。也许我们可以设置布线，这样我们就可以拥有一个本地端点集群，并在测试中使用它们。这些解决方案可能会奏效，但工作量很大。我们想要在 NetworkBridge 中改变的逻辑可能不需要任何这些东西；只是我们无法掌握它。我们无法运行该类的对象并直接尝试它的工作方式。

This example illustrates both the sensing and separation problems. We can't sense the effect of our calls to methods on this class, and we can't run it separately from the rest of the application.

> 这个例子说明了传感和分离问题。我们无法感知对该类方法的调用的效果，也无法将其与应用程序的其他部分分开运行。

Which problem is tougher? Sensing or separation? There is no clear answer. Typically, we need them both, and they are both reasons why we break dependencies. One thing is clear, though: There are many ways to separate software. In fact, there is an entire catalog of those techniques in the back of this book on that topic, but there is one dominant technique for sensing.

> 哪个问题更难？感应还是分离？没有明确的答案。通常，我们都需要它们，它们都是我们打破依赖的原因。不过，有一点是明确的：有很多方法可以分离软件。事实上，在这本书的后面有一整套关于这个主题的技术，但有一种主要的传感技术。

#### [Faking Collaborators](#contents)

One of the big problems that we confront in legacy code work is dependency. If we want to execute a piece of code by itself and see what it does, often we have to break dependencies on other code. But it's hardly ever that simple. Often that other code is the only place we can easily sense the effects of our actions. If we can put some other code in its place and test through it, we can write our tests. In object orientation, these other pieces of code are often called _fake objects_.

> 我们在遗留代码工作中面临的一个大问题是依赖性。如果我们想自己执行一段代码，看看它能做什么，通常我们必须打破对其他代码的依赖。但事情从来没有这么简单。通常情况下，其他代码是我们唯一能轻易感知行动效果的地方。如果我们能把一些其他代码放在它的位置并通过它进行测试，我们就可以编写测试了。在面向对象方面，这些其他代码段通常被称为“伪对象”。

##### Fake Objects

A _fake object_ is an object that impersonates some collaborator of your class when it is being tested. Here is an example. In a point-of-sale system, we have a class called Sale (see Figure 3.1). It has a method called scan() that accepts a bar code for some item that a customer wants to buy. Whenever scan() is called, the Sale object needs to display the name of the item that was scanned, along with its price on a cash register display.

> 伪对象是在测试时模拟类的某个合作者的对象。下面是一个例子。在销售点系统中，我们有一个名为 sale 的类(见图 3.1)。它有一个称为 scan()的方法，接受客户想要购买的商品的条形码。每当调用 scan()时，Sale 对象都需要在收银机显示屏上显示扫描的商品的名称及其价格。

How can we test this to see if the right text shows up on the display? Well, if the calls to the cash register's display API are buried deep in the Sale class, it's going to be hard. It might not be easy to sense the effect on the display. But if we can find the place in the code where the display is updated, we can move to the design shown in Figure 3.2.

> 我们如何测试这一点，看看显示器上是否显示了正确的文本？好吧，如果对收银机显示 API 的调用被深深地埋在 Sale 类中，那就很难了。在显示器上感觉效果可能并不容易。但是，如果我们能在代码中找到更新显示的位置，我们就可以转到图 3.2 中所示的设计。

Here we've introduced a new class, ArtR56Display. That class contains all of the code needed to talk to the particular display device we're using. All we have to do is supply it with a line of text that contains what we want to display. We can move all of the display code in Sale over to ArtR56Display and have a system that does exactly the same thing that it did before. Does that get us anything? Well, once we've done that, we can move the a design shown in Figure 3.3.

> 在这里，我们介绍了一个新的类，ArtR56Display。该类包含与我们正在使用的特定显示设备对话所需的所有代码。我们所要做的就是为它提供一行文本，其中包含我们想要显示的内容。我们可以将 Sale 中的所有显示代码转移到 ArtR56Display，并拥有一个与以前完全相同的系统。这能给我们带来什么吗？好吧，一旦我们完成了，我们就可以移动如图 3.3 所示的设计。

**Figure 3.1** _Sale._

+-------------------------------+-----+-------------------------------+
| > **Sale** | | > **ArtR56Display** |
+===============================+=====+===============================+
| > \+ scan(barcode : String) | | > \+ showLine(line : String) |
+-------------------------------+-----+-------------------------------+

**Figure 3.2** _Sale communicating with a display class._

The Sale class can now hold on to either an ArtR56Display or something else, a FakeDisplay. The nice thing about having a fake display is that we can write tests against it to find out what the Sale does.

> Sale 类现在可以保留 ArtR56Display 或其他东西，FakeDisplay。有一个假显示器的好处是，我们可以针对它编写测试，以了解销售的作用。

How does this work? Well, Sale accepts a display, and a display is an object of any class that implements the Display interface.

> 这是怎么回事？Sale 接受一个 display，display 是实现 display 接口的任何类的对象。

public interface Display

{

void showLine(String line);

}

Both ArtR56Display and FakeDisplay implement Display.

A Sale object can accept a display through the constructor and hold on to it

> Sale 对象可以通过构造函数接受显示并保留它

internally:

public class Sale

{

private Display display;

public Sale(Display display) { this.display = display;

}

public void scan(String barcode) {

\...

String itemLine = item.name()

\+ \" \" + item.price().asDisplayText(); display.showLine(itemLine);

\...

}

}

![](./media/image3.png)

**Figure 3.3** _Sale with the display hierarchy._

[]{#_bookmark40 .anchor}In the scan method, the code calls the showLine method on the display variable. But what happens depends upon what kind of a display we gave the Sale object when we created it. If we gave it an ArtR56Display, it attempts to display on the real cash register hardware. If we gave it a FakeDisplay, it won't, but we will be able to see what would've been displayed. Here is a test we can use to see that:

> []｛#_bookmark40.anchor｝在扫描方法中，代码调用 display 变量上的 showLine 方法。但会发生什么取决于我们在创建 Sale 对象时给它什么样的显示器。如果我们给它一个 ArtR56 显示器，它会尝试在真正的收银机硬件上显示。如果我们给它一个假显示器，它不会，但我们可以看到会显示什么。以下是我们可以用来查看的测试：

import junit.framework.\*;

public class SaleTest extends TestCase

{

public void testDisplayAnItem() { FakeDisplay display = new FakeDisplay(); Sale sale = new Sale(display);

> public void testDisplayAnItem()｛FakeDisplay display=new FakeDisplay()；Sale Sale=new Sale(display)；

sale.scan(\"1\");

assertEquals(\"Milk \$3.99\", display.getLastLine());

}

}

The FakeDisplay class is a little peculiar. Let's look at it:

public class FakeDisplay implements Display

{

private String lastLine = \"\";

public void showLine(String line) { lastLine = line;

}

public String getLastLine() { return lastLine;

}

}

The showLine method accepts a line of text and assigns it to the lastLine vari-

> showLine 方法接受一行文本并将其分配给 lastLine 变量-

able. The getLastLine method returns that line of text whenever it is called. This is pretty slim behavior, but it helps us a lot. With the test we've written, we can find out whether the right text will be sent to the display when the Sale class is used.

> 能够的无论何时调用 getLastLine 方法，都会返回该行文本。这是一种相当苗条的行为，但它对我们帮助很大。通过我们编写的测试，我们可以发现当使用 Sale 类时，是否会向显示器发送正确的文本。

##### The Two Sides of a Fake Object

Fake objects can be confusing when you first see them. One of the oddest things about them is that they have two "sides," in a way. Let's take a look at the FakeDisplay class again, in Figure 3.4.

> 当你第一次看到假物体时，它们可能会让人感到困惑。他们最奇怪的一点是，在某种程度上，他们有两面性。让我们再次看看 Fake-Display 类，如图 3.4 所示。

The showLine method is needed on FakeDisplay because FakeDisplay implements Display. It is the only method on Display and the only one that Sale will see. The other method, getLastLine, is for the use of the test. That is why we declare display as a FakeDisplay, not a Display:

> 在 FakeDisplay 上需要 showLine 方法，因为 FakeDisplay 实现了 Display。这是 Display 上唯一的方法，也是 Sale 将看到的唯一方法。另一个方法 getLastLine 用于测试。这就是为什么我们将 dis-play 声明为 FakeDisplay，而不是 Display：

**Figure 3.4** _Two sides to a fake object._

[]{#_bookmark42 .anchor}import junit.framework.\*;

public class SaleTest extends TestCase

{

public void testDisplayAnItem() { **FakeDisplay** display = new FakeDisplay(); Sale sale = new Sale(display);

> public void testDisplayAnItem(){**FakeDisplay**display=new FakeDisplay()；Sale Sale=new Sale(display)；

sale.scan(\"1\");

assertEquals(\"Milk \$3.99\", **display.getLastLine()**);

}

}

The Sale class will see the fake display as Display, but in the test, we need to hold on to the object as FakeDisplay. If we don't, we won't be able to call getLastLine() to find out what the sale displays.

> Sale 类会将假显示显示显示为 display，但在测试中，我们需要将对象保留为 FakeDisplay。如果我们不这样做，我们将无法调用 getLastLine()来了解销售显示的内容。

##### Fakes Distilled

The example I've shown in this section is very simple, but it shows the central idea behind fakes. They can be implemented in a wide variety of ways. In OO languages, they are often implemented as simple classes like the FakeDisplay class in the previous example. In non-OO languages, we can implement a fake by defining an alternative function, one which records values in some global data structure that we can access in tests. See Chapter 19, _My Project is Not ObjectOriented. How Do I Make Safe Changes?_, for details.

> 我在本节中展示的例子非常简单，但它展示了赝品背后的核心思想。它们可以通过多种方式来实现。在 OO 语言中，它们通常被实现为简单的类，如前面示例中的 FakeDisplay 类。在非 OO 语言中，我们可以通过定义一个替代函数来实现 false，该函数在一些全局数据结构中记录值，我们可以在测试中访问这些值。请参阅第 19 章，_我的项目不是面向对象的。如何进行安全更改？_，详细信息。

##### Mock Objects

Fakes are easy to write and are a very valuable tool for sensing. If you have to write a lot of them, you might want to consider a more advanced type of fake called a _mock object_. Mock objects are fakes that perform assertions internally. Here is an example of a test using a mock object:

> 赝品很容易书写，是一种非常有价值的感知工具。如果你必须写很多，你可能想考虑一种更高级的假对象，称为*mock object*。Mock 对象是在内部执行断言的伪对象。下面是一个使用 mock 对象的测试示例：

import junit.framework.\*;

public class SaleTest extends TestCase

{

public void testDisplayAnItem() { MockDisplay display = new MockDisplay();

> public void testDisplayAnItem()｛MockDisplay display=new MockDisplay()；

display.setExpectation(\"showLine\", \"Milk \$3.99\"); Sale sale = new Sale(display);

> display.setExpected(\“showLine\”，\“Milk\$3.99\”)；销售=新销售(显示)；

sale.scan(\"1\"); display.verify();

}

}

[]{#_bookmark43 .anchor}In this test, we create a mock display object. The nice thing about mocks is that we can tell them what calls to expect, and then we tell them to check and see if they received those calls. That is precisely what happens in this test case. We tell the display to expect its showLine method to be called with an argument of \"Milk \$3.99". After the expectation has been set, we just go ahead and use the object inside the test. In this case, we call the method scan(). Afterward, we call the verify() method, which checks to see if all of the expectations have been met. If they haven't, it makes the test fail.

> []｛#_bookmark43.anchor｝在这个测试中，我们创建了一个模拟显示对象。mocks 的好处是，我们可以告诉他们要打什么电话，然后我们告诉他们检查一下，看看他们是否接到了这些电话。这正是这个测试用例中发生的情况。我们告诉显示器期望其 showLine 方法被调用，参数为\“Milk\$3.99”。在设置了期望值之后，我们只需继续在测试中使用该对象。在这种情况下，我们调用方法 scan()。之后，我们调用 verify()方法，该方法检查是否满足了所有期望。如果他们没有，测试就会失败。

Mocks are a powerful tool, and a wide variety of mock object frameworks are available. However, mock object frameworks are not available in all languages, and simple fake objects suffice in most situations.

> mock 是一个强大的工具，可以使用各种各样的 mock 对象框架。然而，mock 对象框架并不是在所有语言中都可用，在大多数情况下，简单的伪对象就足够了。

[[]{#_bookmark45 .anchor}]{#_bookmark44 .anchor}[**Chapter 4**](#contents)

> [[]{#_bookmark45.anchor}]{#_blaookmark44.anchor}[**第 4 章**](#contents)

### [The Seam Model](#contents)

[]{#_bookmark46 .anchor}One of the things that nearly everyone notices when they try to write tests for existing code is just how poorly suited code is to testing. It isn't just particular programs or languages. In general, programming languages just don't seem to support testing very well. It seems that the only ways to end up with an easily testable program are to write tests as you develop it or spend a bit of time trying to "design for testability." There is a lot of hope for the former approach, but if much of the code in the field is evidence, the latter hasn't been very successful.

> []｛#_bookmark46.anchor｝几乎每个人在尝试为现有代码编写测试时都会注意到的一件事是，代码不适合测试。这不仅仅是特定的程序或语言。一般来说，编程语言似乎不太支持测试。看来，获得一个易于测试的程序的唯一方法是在开发过程中编写测试，或者花一点时间尝试“可测试性设计”。前一种方法有很大的希望，但如果该领域的大部分代码都是证据，那么后一种方法就不太成功了。

One thing that I've noticed is that, in trying to get code under test, I've started to think about code in a rather different way. I could just consider this some private quirk, but I've found that this different way of looking at code helps me when I work in new and unfamiliar programming languages. Because I won't be able to cover every programming language in this book, I've decided to outline this view here in the hope that it helps you as well as it helps me.

> 我注意到的一件事是，在尝试测试代码时，我开始以一种完全不同的方式思考代码。我可以认为这是一种私人的怪癖，但我发现，当我使用新的和不熟悉的编程语言时，这种不同的看待代码的方式对我有帮助。因为我无法在这本书中涵盖每一种编程语言，所以我决定在这里概述这个观点，希望它对你和我都有帮助。

#### [A Huge Sheet of Text](#contents)

When I first started programming, I was lucky that I started late enough to have a machine of my own and a compiler to run on that machine; many of my friends starting programming in the punch-card days. When I decided to study programming in school, I started working on a terminal in a lab. We could compile our code remotely on a DEC VAX machine. There was a little accounting system in place. Each compile cost us money out of our account, and we had a fixed amount of machine time each term.

> 当我第一次开始编程时，我很幸运，我开始得足够晚，有了一台自己的机器和一个可以在那台机器上运行的编译器；我的许多朋友都是在打卡时代开始编程的。当我决定在学校学习编程时，我开始在实验室的终端上工作。我们可以在 DEC VAX 机器上远程编译代码。有一个小的账户系统。每一次编译都会从我们的账户中花费我们的钱，而且我们每学期都有固定的机器时间。

At that point in my life, a program was just a listing. Every couple of hours, I'd walk from the lab to the printer room, get a printout of my program and scrutinize it, trying to figure out what was right or wrong. I didn't know enough to care much about modularity. We had to write modular code to show that we could do it, but at that point I really cared more about whether the code was

> 在我生命的那个阶段，一个程序只是一个清单。每隔几个小时，我就会从实验室走到打印机室，打印出我的程序并仔细检查，试图弄清楚什么是对的或错的。我对模块化的了解不够多。我们必须编写模块化代码来证明我们可以做到这一点，但在那一点上，我真的更关心代码是否

####### 29

going to produce the right answers. When I got around to writing object-oriented code, the modularity was rather academic. I wasn't going to be swapping in one class for another in the course of a school assignment. When I got out in the industry, I started to care a lot about those things, but in school, a program was just a listing to me, a long set of functions that I had to write and understand one by one.

> 将产生正确的答案。当我开始编写面向对象的代码时，模块化是相当学术化的。我不会在学校作业的过程中换一个班。当我进入这个行业时，我开始非常关心这些事情，但在学校里，一个程序对我来说只是一个清单，一长串的函数，我必须一个接一个地写出来。

This view of a program as a listing seems accurate, at least if we look at how people behave in relation to programs that they write. If we knew nothing about what programming was and we saw a room full of programmers working, we might think that they were scholars inspecting and editing large impor[]{#_bookmark48 .anchor}tant documents. A program can seem like a large sheet of text. Changing a little text can cause the meaning of the whole document to change, so people make those changes carefully to avoid mistakes.

> 这种将程序视为列表的观点似乎是准确的，至少如果我们看看人们在编写程序时的行为方式。如果我们对编程一无所知，看到一屋子程序员在工作，我们可能会认为他们是检查和编辑大型重要文档的学者。一个程序可能看起来像一大块文本。更改一小部分文本会导致整个文档的含义发生变化，因此人们会谨慎地进行这些更改以避免出错。

Superficially, that is all true, but what about modularity? We are often told it is better to write programs that are made of small reusable pieces, but how often are small pieces reused independently? Not very often. Reuse is tough. Even when pieces of software look independent, they often depend upon each other in subtle ways.

> 从表面上看，这都是真的，但模块化呢？我们经常被告知，最好编写由可重复使用的小部件组成的程序，但小部件独立重复使用的频率有多高？不经常。重复使用很困难。即使软件看起来是独立的，它们也经常以微妙的方式相互依赖。

#### [Seams](#contents)

When you start to try to pull out individual classes for unit testing, often you have to break a lot of dependencies. Interestingly enough, you often have a lot of work to do, regardless of how "good" the design is. Pulling classes out of existing projects for testing really changes your idea of what "good" is with regard to design. It also leads you to think of software in a completely different way. The idea of a program as a sheet of text just doesn't cut it anymore. How should we look at it? Let's take a look at an example, a function in C++.

> 当您开始尝试提取单独的类进行单元测试时，通常需要打破许多依赖关系。有趣的是，无论设计有多“好”，你通常都有很多工作要做。从现有项目中提取类进行测试确实会改变你对设计“好”的看法。它也会让你以一种完全不同的方式来看待软件。一个程序作为一张文本的想法已经不能再切割它了。我们应该如何看待它？让我们来看一个例子，C++中的一个函数。

bool CAsyncSslRec::Init()

{

if (m_bSslInitialized) { return true;

}

m_smutex.Unlock(); m_nSslRefCount++;

m_bSslInitialized = true;

FreeLibrary(m_hSslDll1); m_hSslDll1=0; FreeLibrary(m_hSslDll2);

[]{#_bookmark49 .anchor}m_hSslDll2=0;

if (!m_bFailureSent) { m_bFailureSent=TRUE;

PostReceiveError(SOCKETCALLBACK, SSL_FAILURE);

}

CreateLibrary(m_hSslDll1,"syncesel1.dll"); CreateLibrary(m_hSslDll2,"syncesel2.dll");

> CreateLibrary(m_hSslDll1，“syncesel1.dll”)；CreateLibrary(m_hSslDll2，“syncesel2.dll”)；

m_hSslDll1-\>Init(); m_hSslDll2-\>Init();

return true;

}

It sure looks like just a sheet of text, doesn't it? Suppose that we want to run

> 它看起来确实只是一张纸，不是吗？假设我们想跑步

all of that method except for this line:

PostReceiveError(SOCKETCALLBACK, SSL_FAILURE);

How would we do that?

It's easy, right? All we have to do is go into the code and delete that line.

> 这很容易，对吧？我们所要做的就是进入代码并删除那一行。

Okay, let's constrain the problem a little more. We want to avoid executing that line of code because PostReceiveError is a global function that communicates with another subsystem, and that subsystem is a pain to work with under test. So the problem becomes, how do we execute the method without calling PostReceiveError under test? How do we do that and still allow the call to PostReceiveError in production?

> 好吧，让我们再把问题限制一下。我们希望避免执行这行代码，因为 PostReceiveError 是一个与另一个子系统通信的全局函数，而该子系统在测试中很难使用。因此，问题变成了，我们如何在不调用测试中的 PostReceiveError 的情况下执行该方法？我们如何做到这一点，并且仍然允许在生产中调用 PostReceiveError？

To me, that is a question with many possible answers, and it leads to the idea of a seam.

> 对我来说，这是一个有很多可能答案的问题，它导致了接缝的想法。

Here's the definition of a seam. Let's take a look at it and then some examples.

> 这是接缝的定义。让我们来看看它，然后举几个例子。

Is there a seam at the call to PostReceiveError? Yes. We can get rid of the behavior there in a couple of ways. Here is one of the most straightforward ones. PostReceiveError is a global function, it isn't part of the CAsynchSslRec class. What happens if we add a method with the exact same signature to the CAsynchSslRec class?

> 调用 PostReceiveError 时是否有接缝？对我们可以通过几种方式来消除那里的行为。这是最简单的一个。PostReceiveError 是一个全局函数，它不是 CAsynchSslRec 类的一部分。如果我们向 CAsynch-SslRec 类添加一个具有完全相同签名的方法，会发生什么？

class CAsyncSslRec

{

\...

virtual void PostReceiveError(UINT type, UINT errorcode);

\...

};

In the implementation file, we can add a body for it like this:

void CAsyncSslRec::PostReceiveError(UINT type, UINT errorcode)

{

::PostReceiveError(type, errorcode);

}

That change should preserve behavior. We are using this new method to dele-

> 这种改变应该能保护行为。我们正在使用这种新方法来删除-

gate to the global PostReceiveError function using C++'s scoping operator (::). We have a little indirection there, but we end up calling the same global function.

> 使用 C++的作用域运算符(：：)访问全局 PostReceiveError 函数。我们有一点间接性，但我们最终调用了相同的全局函数。

Okay, now what if we subclass the CAsyncSslRec class and override the

PostReceiveError method?

class TestingAsyncSslRec : public CAsyncSslRec

{

virtual void PostReceiveError(UINT type, UINT errorcode)

{

}

};

If we do that and go back to where we are creating our CAsyncSslRec and cre-

> 如果我们这样做，并回到我们创建 CAsyncSslRec 和 cre 的地方-

ate a TestingAsyncSslRec instead, we've effectively nulled out the behavior of the call to PostReceiveError in this code:

> 使用 TestingAsyncSslRec，我们在这段代码中有效地将调用 PostReceiveError 的行为为 null：

bool CAsyncSslRec::Init()

{

if (m_bSslInitialized) { return true;

}

m_smutex.Unlock(); m_nSslRefCount++;

m_bSslInitialized = true;

FreeLibrary(m_hSslDll1); m_hSslDll1=0; FreeLibrary(m_hSslDll2); m_hSslDll2=0;

> 自由图书馆(m_hSslDll1)；m_hSslDll1=0；自由库(m_hSslDll2)；m_hSslDll2=0；

if (!m_bFailureSent) { m_bFailureSent=TRUE;

**PostReceiveError(SOCKETCALLBACK, SSL_FAILURE);**

}

CreateLibrary(m_hSslDll1,\"syncesel1.dll\"); CreateLibrary(m_hSslDll2,\"syncesel2.dll\");

> CreateLibrary(m_hSslDll1，\“syncesel1.dll”)；CreateLibrary(m_hSslDll2，\“syncesel2.dll”)；

m_hSslDll1-\>Init(); m_hSslDll2-\>Init();

return true;

}

[[]{#_bookmark51 .anchor}]{#_bookmark50 .anchor}Now we can write tests for that code without the nasty side effect.

> [[]{#_bookmark51.anchor｝]{#_bookmark50.anchor}现在我们可以为该代码编写测试，而不会产生恶劣的副作用。

This seam is what I call an _object seam_. We were able to change the method that is called without changing the method that calls it. _Object seams_ are available in object-oriented languages, and they are only one of many different kinds of seams.

> 这个接缝就是我所说的“对象接缝”。我们能够在不改变调用它的方法的情况下更改被调用的方法。*对象接缝*在面向对象的语言中是可用的，它们只是许多不同类型接缝中的一种。

Why seams? What is this concept good for?

One of the biggest challenges in getting legacy code under test is breaking dependencies. When we are lucky, the dependencies that we have are small and localized; but in pathological cases, they are numerous and spread out throughout a code base. The seam view of software helps us see the opportunities that are already in the code base. If we can replace behavior at seams, we can selectively exclude dependencies in our tests. We can also run other code where those dependencies were if we want to sense conditions in the code and write tests against those conditions. Often this work can help us get just enough tests in place to support more aggressive work.

> 测试遗留代码的最大挑战之一是打破依赖关系。当我们幸运的时候，我们拥有的依赖关系很小，而且是本地化的；但在病理病例中，它们是大量的，并通过代码库传播开来。软件的 seam 视图帮助我们看到代码库中已经存在的机会。如果我们可以替换接缝处的行为，我们就可以选择性地排除测试中的依赖关系。如果我们想感知代码中的条件并根据这些条件编写测试，我们还可以在依赖关系所在的地方运行其他代码。通常，这项工作可以帮助我们进行足够的测试，以支持更积极的工作。

#### [Seam Types](#contents)

The types of seams available to us vary among programming languages. The best way to explore them is to look at all of the steps involved in turning the text of a program into running code on a machine. Each identifiable step exposes different kinds of seams.

> 我们可以使用的接缝类型因编程语言而异。探索它们的最佳方法是查看将程序的文本转换为机器上运行的代码所涉及的所有步骤。每个可识别的步骤都暴露出不同类型的接缝。

##### Preprocessing Seams

In most programming environments, program text is read by a compiler. The compiler then emits object code or bytecode instructions. Depending on the language, there can be later processing steps, but what about earlier steps?

> 在大多数编程环境中，程序文本由编译器读取。编译器然后发出目标代码或字节码指令。根据语言的不同，可能会有后面的处理步骤，但前面的步骤呢？

Only a couple of languages have a build stage before compilation. C and C++ are the most common of them.

> 只有少数几种语言在编译前有构建阶段。C 和 C++是它们中最常见的。

In C and C++, a macro preprocessor runs before the compiler. Over the years, the macro preprocessor has been cursed and derided incessantly. With it, we can take lines of text as innocuous looking as this:

> 在 C 和 C++中，宏预处理器在编译器之前运行。多年来，宏预处理器一直受到诅咒和嘲笑。有了它，我们可以将一行行看起来无害的文本如下：

TEST(getBalance,Account)

{

Account account;

LONGS_EQUAL(0, account.getBalance());

}

and have them appear like this to the compiler.

class AccountgetBalanceTest : public Test

{ public: AccountgetBalanceTest () : Test (\"getBalance\" \"Test\") {} void run (TestResult& result_); }

> ｛public:AccountgetBalanceTest()：Test(\“getBalance\”\“Test\”)｛｝无效运行(TestResult&result_)；｝

AccountgetBalanceInstance;

void AccountgetBalanceTest::run (TestResult& result_)

{

Account account;

{ result_.countCheck();

long actualTemp = (account.getBalance()); long expectedTemp = (0);

if ((expectedTemp) != (actualTemp))

{ result_.addFailure (Failure (name_, \"c:\\\\seamexample.cpp\", 24, StringFrom(expectedTemp),

> ｛result_.addFailure(Failure(name_，\“c:\\\\\seamexample.cpp\”，24，StringFrom(expectedTemp)，

StringFrom(actualTemp))); return; } }

}

We can also nest code in conditional compilation statements like this to sup-

> 我们还可以在这样的条件编译语句中嵌套代码，以支持-

port debugging and different platforms (aarrrgh!):

\...

m_pRtg-\>Adj(2.0);

#ifdef DEBUG #ifndef WINDOWS

{ FILE \*fp = fopen(TGLOGNAME,\"w\");

if (fp) { fprintf(fp,\"%s\", m_pRtg-\>pszState); fclose(fp); }} #endif

m_pTSRTable-\>p_nFlush \|= GF_FLOT; #endif

\...

It's not a good idea to use excessive preprocessing in production code

because it tends to decrease code clarity. The conditional compilation directives (#ifdef, #ifndef, #if, and so on) pretty much force you to maintain several different programs in the same source code. Macros (defined with #define) can be used to do some very good things, but they just do simple text replacement. It is easy to create macros that hide terribly obscure bugs.

> 因为它往往会降低代码的清晰度。条件编译指令(#ifdef、#ifndef、#if 等)几乎迫使您在同一源代码中维护几个不同的程序。宏(用#define 定义)可以用来做一些非常好的事情，但它们只是做简单的文本替换。创建宏可以很容易地隐藏非常模糊的错误。

These considerations aside, I'm actually glad that C and C++ have a preprocessor because the preprocessor gives us more seams. Here is an example. In a C program, we have dependencies on a library routine named db_update. The db_update function talks directly to a database. Unless we can substitute in another implementation of the routine, we can't sense the behavior of the function.

> 抛开这些考虑不谈，我真的很高兴 C 和 C++有一个预处理器，因为预处理器给了我们更多的接缝。下面是一个例子。在 C 程序中，我们依赖于一个名为 db_update 的库例程。db_update 函数直接与数据库对话。除非我们可以在例程的另一个实现中进行替换，否则我们无法感知函数的行为。

#include \<DFHLItem.h\> #include \<DHLSRecord.h\>

extern int db_update(int, struct DFHLItem \*);

void account_update(

int account_no, struct DHLSRecord \*record, int activated)

{

if (activated) {

if (record-\>dateStamped && record-\>quantity \> MAX_ITEMS) { db_update(account_no, record-\>item);

> if(record-\>dateStamped&&record-\>quantity\>MAX_ITEMS)｛db_update(account_no，record-\>1item)；

} else {

db_update(account_no, record-\>backup_item);

}

}

db_update(MASTER_ACCOUNT, record-\>item);

}

We can use preprocessing seams to replace the calls to db_update. To do this,

> 我们可以使用预处理接缝来替换对 db_update 的调用。为此，

we can introduce a header file called localdefs.h.

#include \<DFHLItem.h\> #include \<DHLSRecord.h\>

extern int db_update(int, struct DFHLItem \*);

**#include \"localdefs.h\"**

void account_update(

int account_no, struct DHLSRecord \*record, int activated)

{

if (activated) {

if (record-\>dateStamped && record-\>quantity \> MAX_ITEMS) { db_update(account_no, record-\>item);

> if(record-\>dateStamped&&record-\>quantity\>MAX_ITEMS)｛db_update(account_no，record-\>1item)；

} else {

db_update(account_no, record-\>backup_item);

}

}

db_update(MASTER_ACCOUNT, record-\>item);

}

Within it, we can provide a definition for db_update and some variables that

> 在其中，我们可以提供 db_update 的定义和一些变量

will be helpful for us:

#ifdef TESTING

\...

struct DFHLItem \*last_item = NULL; int last_account_no = -1;

#define db_update(account_no,item)\\

{last_item = (item); last_account_no = (account_no);}

\...

#endif

[]{#_bookmark52 .anchor}With this replacement of db_update in place, we can write tests to verify that db_update was called with the right parameters. We can do it because the #include directive of the C preprocessor gives us a seam that we can use to replace text before it is compiled.

> []｛#_bookmark52.anchor｝有了 db_update 的替换，我们可以编写测试来验证是否使用了正确的参数调用了 db_uupdate。我们之所以能做到这一点，是因为 C 预处理器的#include 指令给了我们一个接缝，我们可以在编译文本之前使用它来替换文本。

Preprocessing seams are pretty powerful. I don't think I'd really want a preprocessor for Java and other more modern languages, but it is nice to have this tool in C and C++ as compensation for some of the other testing obstacles they present.

> 预处理接缝功能非常强大。我不认为我真的想要一个用于 Java 和其他更现代语言的预处理器，但在 C 和 C++中有这个工具作为对它们所带来的其他一些测试障碍的补偿是很好的。

I didn't mention it earlier, but there is something else that is important to understand about seams: Every seam has an _enabling point_. Let's look at the definition of a seam again:

> 我之前没有提到这一点，但了解接缝还有一点很重要：每个接缝都有一个“使能点”。让我们再看看接缝的定义：

When you have a seam, you have a place where behavior can change. We can't really go to that place and change the code just to test it. The source code should be the same in both production and test. In the previous example, we wanted to change the behavior at the text of the db_update call. To exploit that seam, you have to make a change someplace else. In this case, the enabling point is a preprocessor define named TESTING. When TESTING is defined, the localdefs.h file defines macros that replace calls to db_update in the source file.

> 当你有接缝时，你就有了一个行为可以改变的地方。我们不能真的去那个地方仅仅为了测试而更改代码。生产和测试中的源代码都应该相同。在前面的示例中，我们希望更改 db_update 调用文本处的行为。要利用这条缝，你必须在其他地方做出改变。在这种情况下，启用点是一个名为 TESTING 的预处理器定义。定义 TESTING 时，本地-defs.h 文件定义宏，这些宏将替换源文件中对 db_update 的调用。

##### Link Seams

In many language systems, compilation isn't the last step of the build process. The compiler produces an intermediate representation of the code, and that representation contains calls to code in other files. Linkers combine these representations. They resolve each of the calls so that you can have a complete program at runtime.

> 在许多语言系统中，编译并不是构建过程的最后一步。编译器生成代码的中间表示，该表示包含对其他文件中代码的调用。链接器将这些表示组合在一起。它们解析每个调用，这样您就可以在运行时拥有一个完整的程序。

In languages such as C and C++, there really is a separate linker that does the operation I just described. In Java and similar languages, the compiler does the linking process behind the scenes. When a source file contains an import statement, the compiler checks to see if the imported class really has been compiled. If the class hasn't been compiled, it compiles it, if necessary, and then checks to see if all of its calls will really resolve correctly at runtime.

> 在像 C 和 C++这样的语言中，确实有一个单独的链接器来执行我刚才描述的操作。在 Java 和类似的语言中，编译器在后台执行链接过程。当源文件包含导入状态时，编译器会检查导入的类是否真的已编译。如果该类尚未编译，则在必要时对其进行编译，然后检查其所有调用是否在运行时都能正确解析。

[]{#_bookmark53 .anchor}Regardless of which scheme your language uses to resolve references, you can usually exploit it to substitute pieces of a program. Let's look at the Java case. Here is a little class called FitFilter:

> []｛#_bookmark53.anchor｝无论您的语言使用哪种方案来解析引用，您通常都可以利用它来替换程序片段。让我们来看看 Java 案例。下面是一个名为 FitFilter 的小类：

package fitnesse;

import fit.Parse; import fit.Fixture;

import java.io.\*; import java.util.Date;

import java.io.\*; import java.util.\*;

public class FitFilter {

public String input; public Parse tables;

public Fixture fixture = new Fixture(); public PrintWriter output;

public static void main (String argv\[\]) { new FitFilter().run(argv);

}

public void run (String argv\[\]) { args(argv);

process(); exit();

}

public void process() { try {

tables = new Parse(input); fixture.doTables(tables);

} catch (Exception e) { exception(e);

}

\...

}

}

tables.print(output);

In this file, we import fit.Parse and fit.Fixture. How do the compiler and the JVM find those classes? In Java, you can use a classpath environment variable to determine where the Java system looks to find those classes. You can actually create classes with the same names, put them into a different directory, and

> 在这个文件中，我们导入 fit.Parse 和 fit.Fixture。编译器和 JVM 如何找到这些类？在 Java 中，您可以使用类路径环境变量来确定 Java 系统查找这些类的位置。实际上，您可以创建具有相同名称的类，将它们放在不同的目录中，然后

alter the classpath to link to a different fit.Parse and fit.Fixture. Although it would be confusing to use this trick in production code, when you are testing, it can be a pretty handy way of breaking dependencies.

> 更改类路径以链接到不同的 fit.Parse 和 fit.Fixture。虽然在生产代码中使用这个技巧会让人困惑，但在测试时，它可能是打破依赖关系的一种非常方便的方法。

This sort of dynamic linking can be done in many languages. In most, there is some way to exploit link seams. But not all linking is dynamic. In many older languages, nearly all linking is static; it happens once after compilation.

> 这种动态链接可以用多种语言实现。在大多数情况下，有一些方法可以利用链接接缝。但并不是所有的链接都是动态的。在许多较老的语言中，几乎所有的链接都是静态的；编译后会发生一次。

Many C and C++ build systems perform static linking to create executables. Often the easiest way to use the link seam is to create a separate library for any classes or functions you want to replace. When you do that, you can alter your build scripts to link to those rather than the production ones when you are testing. This can be a bit of work, but it can pay off if you have a code base that is littered with calls to a third-party library. For instance, imagine a CAD application that contains a lot of embedded calls to a graphics library. Here is an example of some typical code:

> 许多 C 和 C++构建系统执行静态链接以创建可执行文件。通常，使用链接接缝的最简单方法是为要替换的任何类或函数创建一个单独的库。当你这样做的时候，你可以在测试时修改你的构建脚本，使其链接到那些而不是生产脚本。这可能需要一些工作，但如果你的代码库中充斥着对第三方库的调用，这可能会有回报。例如，假设一个 CAD 应用程序包含对图形库的大量嵌入式调用。以下是一些典型代码的示例：

void CrossPlaneFigure::rerender()

{

// draw the label

drawText(m_nX, m_nY, m_pchLabel, getClipLen()); drawLine(m_nX, m_nY, m_nX + getClipLen(), m_nY); drawLine(m_nX, m_nY, m_nX, m_nY + getDropLen()); if (!m_bShadowBox) {

> drawText(m_nX，m_nY，m_pchLabel，getClipLen())；drawLine(m_nX，m_nY，m_nX+getClipLen()，m_nY)；drawLine(m_nX、m_nY、m_nX 和 m_nY+getDropLen())；if(！m_bShadowBox){

drawLine(m_nX + getClipLen(), m_nY,

m_nX + getClipLen(), m_nY + getDropLen()); drawLine(m_nX, m_nY + getDropLen(),

> m_nX+getClipLen()、m_nY+getDropLen(())；drawLine(m_nX，m_nY+getDropLen()，

m_nX + getClipLen(), m_nY + getDropLen());

}

// draw the figure

for (int n = 0; n \< edges.size(); n++) {

\...

}

\...

}

This code makes many direct calls to a graphics library. Unfortunately, the

> 此代码对图形库进行了许多直接调用。不幸的是

only way to really verify that this code is doing what you want it to do is to

> 真正验证此代码是否正在执行您希望它执行的操作的唯一方法是

[]{#_bookmark54 .anchor}look at the computer screen when figures are redrawn. In complicated code, that is pretty error prone, not to mention tedious. An alternative is to use link seams. If all of the drawing functions are part of a particular library, you can create stub versions that link to the rest of the application. If you are interested in only separating out the dependency, they can be just empty functions:

> []｛#_bookmark54.anchor｝在重新绘制图形时查看计算机屏幕。在复杂的代码中，这很容易出错，更不用说乏味了。另一种选择是使用连接接缝。如果所有绘图函数都是特定库的一部分，则可以创建链接到应用程序其余部分的存根版本。如果您只想分离依赖项，它们可以只是空函数：

void drawText(int x, int y, char \*text, int textLength)

{

}

void drawLine(int firstX, int firstY, int secondX, int secondY)

{

}

If the functions return values, you have to return something. Often a code that indicates success or the default value of a type is a good choice:

> 如果函数返回值，则必须返回一些值。通常，指示成功的代码或类型的默认值是一个不错的选择：

int getStatus()

{

return FLAG_OKAY;

}

The case of a graphics library is a little atypical. One reason that it is a good

> 图形库的情况有点非典型。这是一个好的原因

candidate for this technique is that it is almost a pure "tell" interface. You issue calls to functions to tell them to do something, and you aren't asking for much information back. Asking for information is difficult because the defaults often aren't the right thing to return when you are trying to exercise your code.

> 这种技术的候选者是它几乎是一个纯粹的“tell”接口。您向函数发出调用，告诉它们做某事，并且不会要求返回太多信息。询问信息是很困难的，因为当您尝试执行代码时，默认值通常不是正确的返回值。

Separation is often a reason to use a link seam. You can do sensing also; it just requires a little more work. In the case of the graphics library we just faked, we could introduce some additional data structures to record calls:

> 分离通常是使用连接接缝的原因。你也可以做感应；它只需要多一点工作。在我们刚刚伪造的图形库的情况下，我们可以引入一些额外的数据结构来记录调用：

std::queue\<GraphicsAction\> actions;

void drawLine(int firstX, int firstY, int secondX, int secondY)

{

actions.push_back(GraphicsAction(LINE_DRAW, firstX, firstY, secondX, secondY);

> actions.push_back(GraphicsAction(LINE_DRAW，firstX，firstY，secondX，secondY)；

}

With these data structures, we can sense the effects of a function in a test:

> 通过这些数据结构，我们可以在测试中感知函数的效果：

TEST(simpleRender,Figure)

{

std::string text = \"simple\"; Figure figure(text, 0, 0);

figure.rerender(); LONGS_EQUAL(5, actions.size());

[]{#_bookmark55 .anchor}GraphicsAction action;

action = actions.pop_front(); LONGS_EQUAL(LABEL_DRAW, action.type);

action = actions.pop_front(); LONGS_EQUAL(0, action.firstX); LONGS_EQUAL(0, action.firstY); LONGS_EQUAL(text.size(), action.secondX);

> action=actions.pop_front()；LONGSEQUAL(0，action.firstX)；LONGSEQUAL(0，action.firstY)；LONGS_EQUAL(text.size()，action.secondX)；

}

The schemes that we can use to sense effects can grow rather complicated,

but it is best to start with a very simple scheme and allow it to get only as complicated as it needs to be to solve the current sensing needs.

> 但最好从一个非常简单的方案开始，并允许它只像解决当前传感需求所需的那样复杂。

The enabling point for a link seam is always outside the program text. Sometimes it is in a build or a deployment script. This makes the use of link seams somewhat hard to notice.

> 链接接缝的启用点总是在程序文本之外。有时它在构建或部署脚本中。这使得链接接缝的使用有些难以注意。

##### Object Seams

Object seams are pretty much the most useful seams available in object-oriented programming languages. The fundamental thing to recognize is that when we look at a call in an object-oriented program, it does not define which method will actually be executed. Let's look at a Java example:

> 对象接缝几乎是面向对象编程语言中最有用的接缝。要认识到的基本问题是，当我们在面向对象程序中查看调用时，它并没有定义实际执行的方法。让我们看一个 Java 示例：

cell.Recalculate();

When we look at this code, it seems that there has to be a method named Recalculate that will execute when we make that call. If the program is going to run, there has to be a method with that name; but the fact is, there can be more than one:

> 当我们看到这段代码时，似乎必须有一个名为 Recalculate 的方法，它将在我们进行该调用时执行。如果程序要运行，必须有一个具有该名称的方法；但事实是，可能不止一个：

**Figure 4.1** _Cell hierarchy._

Which method will be called in this line of code?

cell.Recalculate();

Without knowing what object cell points to, we just don't know. It could be the Recalculate method of ValueCell or the Recalculate method of FormulaCell. It could even be the Recalculate method of some other class that doesn't inherit from Cell (if that's the case, cell was a particularly cruel name to use for that variable!). If we can change which Recalculate is called in that line of code without changing the code around it, that call is a seam.

> 在不知道对象单元格指向什么的情况下，我们就是不知道。它可以是 ValueCell 的 Recalculate 方法或 FormulaCell 的重新计算方法。它甚至可能是其他类的 Recalculate 方法，该方法不是从 Cell 继承的(如果是这样的话，Cell 对该变量来说是一个特别残酷的名称！)。如果我们可以在不更改代码的情况下更改代码行中调用的 Recalculate，那么该调用就是一个接缝。

In object-oriented languages, not all method calls are seams. Here is an example of a call that isn't a seam:

> 在面向对象的语言中，并不是所有的方法调用都是接缝。以下是一个不是接缝的调用示例：

public class CustomSpreadsheet extends Spreadsheet

{

public Spreadsheet buildMartSheet() {

\...

Cell cell = new FormulaCell(this, \"A1\", \"=A2+A3\");

\...

**cell.Recalculate();**

\...

}

\...

}

In this code, we're creating a cell and then using it in the same method. Is the

> 在这段代码中，我们将创建一个单元格，然后以相同的方法使用它。是

call to Recalculate an object seam? No. There is no enabling point. We can't change which Recalculate method is called because the choice depends on the class of the cell. The class of the cell is decided when the object is created, and we can't change it without modifying the method.

> 调用以重新计算对象接缝？没有。没有任何使能点。我们无法更改调用的 Recalculate 方法，因为选择取决于单元格的类。单元格的类是在创建对象时决定的，如果不修改方法，我们就无法更改它。

What if the code looked like this?

[]{#_bookmark56 .anchor}public class CustomSpreadsheet extends Spreadsheet

> []｛#_bookmark56.anchor｝公共类 CustomSpreadsheet 扩展电子表格

{

public Spreadsheet buildMartSheet(**Cell cell**) {

\...

**cell.Recalculate();**

\...

}

\...

}

Is the call to cell.Recalculate in buildMartSheet a seam now? Yes. We can cre-

> 调用单元格。现在在 buildMartSheet 中重新计算接缝吗？对我们可以屠杀-

ate a CustomSpreadsheet in a test and call buildMartSheet with whatever kind of Cell we want to use. We'll have ended up varying what the call to cell.Recalculate does without changing the method that calls it.

> 在测试中使用 CustomSpreadsheet，并使用我们想要使用的任何类型的 Cell 调用 buildMartSheet。我们最终会改变对 cell.Recalcu-late 的调用，而不改变调用它的方法。

Where is the enabling point?

In this example, the enabling point is the argument list of buildMartSheet. We can decide what kind of an object to pass and change the behavior of Recalculate any way that we want to for testing.

> 在本例中，启用点是 buildMartSheet 的参数列表。我们可以决定传递什么样的对象，并以任何方式更改“重新计算”的行为以进行测试。

Okay, most object seams are pretty straightforward. Here is a tricky one. Is there an object seam at the call to Recalculate in this version of buildMartSheet?

> 好吧，大多数物体接缝都很简单。这是一个棘手的问题。在这个版本的 buildMartSheet 中，调用 Recalculate 时是否有对象接缝？

public class CustomSpreadsheet extends Spreadsheet

{

public Spreadsheet buildMartSheet(Cell cell) {

\...

**Recalculate(cell);**

\...

}

**private static** void Recalculate(Cell cell) {

\...

}

\...

}

The Recalculate method is a static method. Is the call to Recalculate in

buildMartSheet a seam? Yes. We don't have to edit buildMartSheet to change behavior at that call. If we delete the keyword static on Recalculate and make it a protected method instead of a private method, we can subclass and override it during test:

> buildMart 缝合？对我们不必编辑 buildMartSheet 来更改调用时的行为。如果我们在 Recalculate 上删除关键字 static，并使其成为受保护的方法而不是私有方法，我们可以在测试过程中对其进行子类化和覆盖：

public class CustomSpreadsheet extends Spreadsheet

{

public Spreadsheet buildMartSheet(Cell cell) {

\...

Recalculate(cell);

\...

}

**protected** void Recalculate(Cell cell) {

\...

}

\...

}

public class TestingCustomSpreadsheet extends CustomSpreadsheet { protected void Recalculate(Cell cell) {

> 公共类 TestingCustomSpreadsheet 扩展 CustomSpreadsheet｛protected void Recalculate(单元格){

\...

}

}

Isn't this all rather indirect? If we don't like a dependency, why don't we just

> 这不是很间接吗？如果我们不喜欢依赖，为什么不

go into the code and change it? Sometimes that works, but in particularly nasty legacy code, often the best approach is to do what you can to modify the code as little as possible when you are getting tests in place. If you know the seams that your language offers and how to use them, you can often get tests in place more safely than you could otherwise.

> 进入代码并更改它？有时这是有效的，但在特别讨厌的遗留代码中，通常最好的方法是在进行测试时尽可能少地修改代码。如果你知道你的语言提供的接缝以及如何使用它们，你通常可以比其他情况下更安全地进行测试。

The seams types I've shown are the major ones. You can find them in many programming languages. Let's take a look at the example that led off this chapter again and see what seams we can see:

> 我展示的接缝类型是主要的。你可以在许多编程语言中找到它们。让我们再来看看这个例子，看看我们能看到哪些接缝：

bool CAsyncSslRec::Init()

{

if (m_bSslInitialized) { return true;

}

m_smutex.Unlock(); m_nSslRefCount++;

m_bSslInitialized = true;

FreeLibrary(m_hSslDll1); m_hSslDll1=0; FreeLibrary(m_hSslDll2); m_hSslDll2=0;

> 自由图书馆(m_hSslDll1)；m_hSslDll1=0；自由库(m_hSslDll2)；m_hSslDll2=0；

if (!m_bFailureSent) { m_bFailureSent=TRUE;

**PostReceiveError(SOCKETCALLBACK, SSL_FAILURE);**

}

CreateLibrary(m_hSslDll1,\"syncesel1.dll\"); CreateLibrary(m_hSslDll2,\"syncesel2.dll\");

> CreateLibrary(m_hSslDll1，\“syncesel1.dll”)；CreateLibrary(m_hSslDll2，\“syncesel2.dll”)；

m_hSslDll1-\>Init(); m_hSslDll2-\>Init(); return true;

}

[]{#_bookmark57 .anchor}What seams are available at the PostReceiveError call? Let's list them.

> []｛#_bookmark57.anchor｝PostReceiveError 调用中有哪些接缝可用？让我们列出它们。

1.  PostReceiveError is a global function, so we can easily use the _link seam_ there. We can create a library with a stub function and link to it to get rid of the behavior. The enabling point would be our makefile or some setting in our IDE. We'd have to alter our build so that we would link to a testing library when we are testing and a production library when we want to build the real system.

> 1.PostReceiveError 是一个全局函数，所以我们可以很容易地使用那里的*link seam*。我们可以创建一个带有存根函数的库，并链接到它以消除这种行为。启用点将是我们的 makefile 或 IDE 中的一些设置。我们必须更改我们的构建，以便在测试时链接到测试库，在想要构建真实系统时链接到生产库。

2.  We could add a #include statement to the code and use the preprocessor to define a macro named PostReceiveError when we are testing. So, we have a _preprocessing seam_ there. Where is the _enabling point_? We can use a preprocessor define to turn the macro definition on or off.

> 2.我们可以在代码中添加#include 语句，并在测试时使用预处理器定义一个名为 PostReceiveError 的宏。所以，我们有一个预处理接缝。“使能点”在哪里？我们可以使用预处理器定义来打开或关闭宏定义。

3.  We could also declare a virtual function for PostRecieveError like we did at the beginning of this chapter, so we have an _object seam_ there also. Where is the enabling point? In this case, the enabling point is the place where we decide to create an object. We can create either an CAsyncSslRec object or an object of some testing subclass that overrides PostRecieveError.

> 3.我们也可以像本章开头那样为 PostReciveError 声明一个虚拟函数，所以我们也有一个*对象接缝*。使能点在哪里？在这种情况下，启用点是我们决定创建对象的地方。我们可以创建一个 CAsyncSsl-Rec 对象，也可以创建覆盖 PostRe-cieveError 的某个测试子类的对象。

It is actually kind of amazing that there are so many ways to replace the behavior at this call without editing the method:

> 实际上，令人惊讶的是，有这么多方法可以在不编辑方法的情况下替换此调用中的行为：

bool CAsyncSslRec::Init()

{

\...

if (!m_bFailureSent) { m_bFailureSent=TRUE;

**PostReceiveError(SOCKETCALLBACK, SSL_FAILURE);**

}

\...

return true;

}

It is important to choose the right type of seam when you want to get pieces

> 当你想要获得单品时，选择正确的接缝类型是很重要的

of code under test. In general, _object seams_ are the best choice in object-oriented languages. _Preprocessing seams_ and _link seams_ can be useful at times but they are not as explicit as _object seams._ In addition, tests that depend upon them can be hard to maintain. I like to reserve _preprocessing seams_ and _link seams_ for cases where dependencies are pervasive and there are no better alternatives.

> 的代码。一般来说，*对象接缝*是面向对象语言中的最佳选择*预处理接缝*和*链接接缝*有时会很有用，但它们不像*对象接缝那样明确。*此外，依赖它们的测试可能很难维护。我喜欢为依赖性普遍存在且没有更好的替代方案的情况保留*预处理接缝*和*链接接缝*。

When you get used to seeing code in terms of seams, it is easier to see how to test things and to see how to structure new code to make testing easier.

> 当你习惯于从接缝的角度来看待代码时，就更容易看到如何测试东西，以及如何构建新代码以使测试更容易。

[[[]{#_bookmark60 .anchor}]{#_bookmark59 .anchor}]{#_bookmark58 .anchor}[**Chapter 5**](#contents)

> [[]]｛#_bookmark60.anchor｝]｛#_bookmark59.anchor}]｛#bookmark58.anchor}[**第 5 章**](#contents)

### [Tools](#contents)

What tools do you need when you work with legacy code? You need an editor (or an IDE) and your build system, but you also need a testing framework. If there are refactoring tools for your language, they can be very helpful as well.

> 使用遗留代码时，您需要什么工具？您需要一个编辑器(或 IDE)和构建系统，但也需要一个测试框架。如果您的语言有重构工具，它们也会非常有用。

In this chapter, I describe some of the tools that are currently available and the role that they can play in your legacy code work.

> 在本章中，我将介绍一些当前可用的工具，以及它们在遗留代码工作中可以发挥的作用。

#### [Automated Refactoring Tools](#contents)

Refactoring by hand is fine, but when you have a tool that does some refactoring for you, you have a real time saver. In the 1990s, Bill Opdyke started work on a C++ refactoring tool as part of his thesis work on refactoring. Although it never became commercially available, to my knowledge, his work inspired many other efforts in other languages. One of the most significant was the Smalltalk refactoring browser developed by John Brant and Don Roberts at the University of Illinois. The Smalltalk refactoring browser supported a very large number of refactorings and has served as a state-of-the-art example of automated refactoring technology for a long while. Since then, there have been many attempts to add refactoring support to various languages in wider use. At the time of this writing, many Java refactoring tools are available; most are integrated into IDEs, but a few are not. There are also refactoring tools for Delphi and some relatively new ones for C++. Tools for C# refactoring are under active development at the time of this writing.

> 手工重构是可以的，但当你有一个工具可以为你做一些重构时，你就可以真正节省时间。在 20 世纪 90 年代，Bill Opdyke 开始研究 C++重构工具，这是他关于重构的论文工作的一部分。尽管它从未商业化，但据我所知，他的作品激发了许多其他语言的努力。其中最重要的是伊利诺伊大学的 John Brant 和 Don Roberts 开发的 Smalltalk 重构浏览器。Smalltalk 重构浏览器支持大量重构，并且在很长一段时间内一直是自动匹配重构技术的最先进的例子。从那时起，已经有许多尝试将重构支持添加到更广泛使用的各种语言中。在撰写本文时，已有许多 Java 重构工具可用；大多数集成到 IDE 中，但也有少数没有。还有一些用于 Del-phi 的重构工具和一些相对较新的用于 C++的重构工具。在撰写本文时，C#重构工具正在积极开发中。

With all of these, tools it seems that refactoring should be much easier. It is,

> 有了所有这些工具，重构似乎应该容易得多。是的，

in some environments. Unfortunately, the refactoring support in many of these tools varies. Let's remember what refactoring is again. Here is Martin Fowler's definition from _Refactoring: Improving the Design of Existing Code_ (AddisonWesley 1999):

> 在某些环境中。不幸的是，这些工具中的许多工具对重构的支持各不相同。让我们再次记住什么是重构。以下是 Martin Fowler 在《重构：改进现有代码的设计》(Addison-Wesley 1999)中的定义：

####### 45

[]{#_bookmark61 .anchor}A change is a refactoring only if it doesn't change behavior. Refactoring tools should verify that a change does not change behavior, and many of them do. This was a cardinal rule in the Smalltalk refactoring browser, Bill Opdyke's work, and many of the early Java refactoring tools. At the fringes, however, some tools don't really check---and if they don't check, you could be introducing subtle bugs when you refactor.

> []｛#_bookmark61.anchor｝只有在不改变行为的情况下，更改才是重构。重构工具应该验证一个改变不会改变行为，很多都会改变。这是 Smalltalk 重构浏览器、Bill Opdyke 的工作以及许多早期 Java 重构工具中的一条基本规则。然而，在边缘，有些工具并没有真正检查——如果他们不检查，那么在重构时可能会引入一些细微的错误。

It pays to choose your refactoring tools with care. Find out what the tool developers say about the safety of their tool. Run your own tests. When I encounter a new refactoring tool, I often run little sanity checks. When you attempt to extract a method and give it the name of a method that already exists in that class, does it flag that as an error? What if it is the name of a method in a base class---does the tool detect that? If it doesn't, you could mistakenly override a method and break code.

> 谨慎选择重构工具是值得的。了解工具开发人员对其工具安全性的评价。运行您自己的测试。当我遇到一个新的重构工具时，我经常进行一些健全性检查。当你试图提取一个方法并给它一个已经存在于该类中的方法的名称时，它会将其标记为错误吗？如果它是基类中方法的名称，该工具会检测到吗？如果没有，您可能会错误地重写一个方法并破坏代码。

In this book, I discuss work with and without automated refactoring support. In the examples, I mention whether I am assuming the availability of a refactoring tool.

> 在这本书中，我讨论了使用和不使用自动重构支持的工作。在示例中，我提到了我是否假设重构工具的可用性。

In all cases, I assume that the refactorings supplied by the tool preserve behavior. If you discover that the ones supplied by your tool don't preserve behavior, don't use the automated refactorings. Follow the advice for cases in which you don't have a refactoring tool---it will be safer.

> 在所有情况下，我都假设该工具提供的重构保留了行为。如果您发现工具提供的重构不能保持行为，请不要使用自动重构。在没有重构工具的情况下，请遵循建议——这样会更安全。

[]{#_bookmark62 .anchor}In at least two Java refactoring tools, we can use a refactoring to remove the v variable from doSomething. After the refactoring, the code looks like this:

> []｛#_bookmark62.anchor｝在至少两个 Java 重构工具中，我们可以使用重构来从 doSomething 中删除变量。重构后，代码如下所示：

public class A {

private int alpha = 0; private int getValue() {

alpha++; return 12;

}

public void doSomething() { int total = 0;

for (int n = 0; n \< 10; n++) { total += getValue();

[]{#_bookmark63 .anchor}}

}

}

See the problem? The variable was removed, but now the value of alpha is incremented 10 times rather than 1. This change clearly didn't preserve behavior.

> 看到问题了吗？变量被删除了，但现在 alpha 的值增加了 10 倍，而不是 1。这一变化显然没有保护行为。

It is a good idea to have tests around your code before you start to use automated refactorings. You can do some automated refactoring without tests, but you have to know what the tool is checking and what it isn't. When I start to use a new tool, the first thing that I do is put its support for extracting methods through its paces. If I can trust it well enough to use it without tests, I can get the code into a much more testable state.

> 在开始使用自动重构之前，最好先对代码进行测试。你可以在没有测试的情况下进行一些自动重构，但你必须知道工具在检查什么，它没有检查什么。当我开始使用一个新工具时，我要做的第一件事就是通过它的步伐来支持提取方法。如果我能够很好地信任它，在没有测试的情况下使用它，我就可以使代码进入一个更易于测试的状态。

#### [Mock Objects](#contents)

One of the big problems that we confront in legacy code work is dependency. If we want to execute a piece of code by itself and see what it does, often we have to break dependencies on other code. But it's hardly ever that simple. If we remove the other code, we need to have something in its place that supplies the right values when we are testing so that we can exercise our piece of code thoroughly. In object-oriented code, these are often called mock objects.

> 我们在遗留代码工作中面临的一个大问题是依赖性。如果我们想自己执行一段代码，看看它能做什么，通常我们必须打破对其他代码的依赖。但事情从来没有这么简单。如果我们删除了另一个代码，那么在测试时，我们需要在它的位置上提供正确的值，这样我们才能彻底地执行我们的代码。在面向对象的代码中，这些对象通常被称为 mock 对象。

Several mock object libraries are freely available. The web site [www](http://www.mockobjects.com/).mock[objects.com](http://www.mockobjects.com/) is a good place to find references for most of them.

> 有几个 mock 对象库是免费提供的。网站[www](http://www.mockobjects.com/).mock-[objects.com](http://www.mockobjects.com/)是一个很好的地方，可以为他们中的大多数人找到参考资料。

#### [Unit-Testing Harnesses](#contents)

Testing tools have a long and varied history. Not a year goes by that I don't run into four or five teams that have bought some expensive license-per-seat testing tool that ends up not living up to its price. In fairness to tool vendors, testing is a tough problem, and people are often seduced by the idea that they can test through a GUI or web interface without having to do anything special to their application. It can be done, but it is usually more work than anyone on a team is prepared to admit. In addition, a user interface often isn't the best place to write tests. UIs are often volatile and too far from the functionality being tested. When UI-based tests fail, it can be hard to figure out why. Regardless, people often spend considerable money trying to do all of their testing with those sorts of tools.

> 测试工具有着悠久而多样的历史。没有一年我会遇到四五个团队购买了一些昂贵的每个座位的许可证测试工具，但最终却达不到它的价格。公平地说，对于工具供应商来说，测试是一个棘手的问题，人们经常被这样一种想法所吸引，即他们可以通过 GUI 或 web 界面进行测试，而不必对他们的应用程序做任何特殊的事情。这是可以做到的，但这通常比团队中任何人都愿意承认的要多。此外，用户界面通常不是编写测试的最佳场所。UI 通常是不稳定的，并且离正在测试的功能太远。当基于 UI 的测试失败时，很难弄清楚原因。不管怎样，人们经常花大量的钱试图用这些工具进行所有的测试。

The most effective testing tools I've run across have been free. The first one is the xUnit testing framework. Originally written in Smalltalk by Kent Beck and then ported to Java by Kent Beck and Erich Gamma, xUnit is a small, powerful design for a unit-testing framework. Here are its key features:

> 我遇到的最有效的测试工具都是免费的。第一个是 xUnit 测试框架。xUnit 最初由 Kent Beck 用 Smalltalk 编写，然后由 Kent Bake 和 Erich Gamma 移植到 Java，它是一个小型、强大的单元测试框架设计。以下是它的主要功能：

- It lets programmers write tests in the language they are developing in.

- All tests run in isolation.

- Tests can be grouped into suites so that they can be run and rerun on demand.

The xUnit framework has been ported to most major languages and quite a few small, quirky ones.

> xUnit 框架已经移植到大多数主要语言和相当多的小而古怪的语言。

The most revolutionary thing about xUnit's design is its simplicity and focus. It allows us to write tests with little muss and fuss. Although it was originally designed for unit testing, you can use it to write larger tests because xUnit really doesn't care how large or small a test is. If the test can be written in the language you are using, xUnit can run it.

> xUnit 的设计最具革命性的地方在于它的简洁和专注。它允许我们在编写测试时不必大惊小怪。虽然它最初是为单元测试设计的，但你可以用它来编写更大的测试，因为 xUnit 真的不在乎测试有多大或有多小。如果测试可以用你正在使用的语言编写，xUnit 就可以运行它。

In this book, most of the examples are in Java and C++. In Java, JUnit is the preferred xUnit harness, and it looks very much like most of the other xUnits. In C++, I often use a testing harness I wrote named CppUnitLite. It looks quite a bit different, and I describe it in this chapter also. By the way, I'm not slighting the original author of CppUnit by using CppUnitLite. I was that guy a long time ago, and I discovered only after I released CppUnit that it could be quite a bit smaller, easier to use, and far more portable if it used some C idioms and only a bare subset of the C++ language.

> 在这本书中，大多数例子都是用 Java 和 C++编写的。在 Java 中，JUnit 是首选的 xUnit 工具，它看起来与大多数其他 xUnit 非常相似。在 C++中，我经常使用我编写的名为 CppUnitLite 的测试工具。它看起来有点不同，我也在这一章中描述了它。顺便说一句，我使用 CppUnitLite 并不是在轻视 CppUnit 的原作者。很久以前我就是那个人，直到我发布 CppUnit 后，我才发现，如果它使用一些 C 习惯用法，并且只使用 C++语言的一个子集，它可能会更小、更容易使用、更便携。

##### JUnit

In JUnit, you write tests by subclassing a class named TestCase.

import junit.framework.\*;

public class FormulaTest extends TestCase { public void testEmpty() {

assertEquals(0, new Formula(\"\").value());

}

public void testDigit() {

assertEquals(1, new Formula(\"1\").value());

}

}

Each method in a test class defines a test if it has a signature of this form:

> 测试类中的每个方法都定义了一个测试，如果它具有以下形式的签名：

void testXXX(), where XXX is the name you want to give the test. Each test method can contain code and assertions. In the previous testEmpty method, there is code to create a new Formula object and call its value method. There is also assertion code that checks to see if that value is equal to 0. If it is, the test passes. If it isn't, the test fails.

> void testXXX()，其中 XXX 是要为测试命名的名称。每个测试方法都可以包含代码和断言。在前面的 testEmpty 方法中，有一段代码用于创建一个新的 Formula 对象并调用其值方法。还有断言代码，用于检查该值是否等于 0。如果是，则测试通过。如果不是，则测试失败。

In a nutshell, here is what happens when you run JUnit tests. The JUnit test runner loads a test class like the one shown previously, and then it uses reflection to find all of the test methods. What it does next is kind of sneaky. It creates a completely separate object for each one of those test methods. From the previous code, it creates two of them: an object whose only job is to run the testEmpty method, and an object whose only job is to run the testDigit object. If you are wondering what the classes of the objects are, in both cases, it is the same: FormulaTest. Each object is configured to run exactly one of the test methods on FormulaTest. The key thing is that we have a completely separate object for each method. There is no way that they can affect each other. Here is an example.

> 简而言之，以下是运行 JUnit 测试时会发生的情况。JUnit 测试运行程序加载一个测试类，就像前面显示的那样，然后它使用 reflection 来查找所有的测试方法。它接下来要做的事情有点偷偷摸摸。它为每一种测试方法创建了一个完全独立的对象。从前面的代码中，它创建了其中两个：一个对象的唯一任务是运行 testEmpty 方法，另一个对象唯一任务是执行 testDigit 对象。如果你想知道对象的类是什么，在这两种情况下，都是一样的：FormulaTest。每个对象都被配置为在 FormulaTest 上只运行一种测试方法。关键是我们为每个方法都有一个完全独立的对象。它们不可能相互影响。下面是一个例子。

public class EmployeeTest extends TestCase { private Employee employee;

protected void setUp() {

employee = new Employee(\"Fred\", 0, 10);

TDate cardDate = new TDate(10, 10, 2000); employee.addTimeCard(new TimeCard(cardDate,40));

> TDate cardDate=新 TDate(102000)；employee.addTimeCard(新 TimeCard(cardDate，40))；

}

public void testOvertime() {

TDate newCardDate = new TDate(11, 10, 2000);

employee.addTimeCard(new TimeCard(newCardDate, 50)); assertTrue(employee.hasOvertimeFor(newCardDate));

> employee.addTimeCard(新 TimeCard(newCardDate，50))；assertTrue(employee.hasOvertimeFor(newCardDate))；

[]{#_bookmark67 .anchor}}

public void testNormalPay() { assertEquals(400, employee.getPay());

}

}

In the EmployeeTest class, we have a special method named setUp. The setUp

> 在 EmployeeTest 类中，我们有一个名为 setUp 的特殊方法。设置

method is defined in TestCase and is run in each test object before the test method is run. The setUp method allows us to create a set of objects that we'll use in a test. That set of objects is created the same way before each test's execution. In the object that runs testNormalPay, an employee created in setUp is checked to see if it calculates pay correctly for one timecard, the one added in setUp. In the object that runs testOvertime, an employee created in setUp for that object gets an additional timecard, and there is a check to verify that the second timecard triggers an overtime condition. The setUp method is called for each object of the class EmployeeTest, and each of those objects gets its own set of objects created via setUp. If you need to do anything special after a test finishes executing, you can override another method named tearDown, defined in TestCase. It runs after the test method for each object

> 方法在 TestCase 中定义，并在运行测试方法之前在每个测试对象中运行。setUp 方法允许我们创建一组将在测试中使用的对象。在执行每个测试之前，以相同的方式创建该组对象。在运行 testNormalPay 的对象中，将检查在 setUp 中创建的员工，以查看其是否正确计算了一张考勤卡(即在 setUp 添加的考勤卡)的工资。在运行 testOvertime 的对象中，在 setUp 中为该对象创建的员工将获得一个额外的时间卡，并进行检查以验证第二个时间卡是否触发加班条件。为类 EmployeeTest 的每个对象调用 setUp 方法，并且这些对象中的每个对象都有自己的一组通过 setUp 创建的对象。如果在测试执行完成后需要做任何特殊的事情，可以覆盖 TestCase 中定义的另一个名为 tearDown 的方法。它在每个对象的测试方法之后运行

When you first see an xUnit harness, it is bound to look a little strange. Why do test-case classes have setUp and tearDown at all? Why can't we just create the objects we need in the constructor? Well, we could, but remember what the test runner does with test case classes. It goes to each test case class and creates a set of objects, one for each test method. That is a large set of objects, but it isn't so bad if those objects haven't allocated what they need yet. By placing code in setUp to create what we need just when we need it, we save quite a bit on resources. In addition, by delaying setUp, we can also run it at a time when we can detect and report any problems that might happen during setup.

> 当你第一次看到 xUnit 线束时，它看起来一定有点奇怪。为什么测试用例类有 setUp 和 tearDown？为什么我们不能在构造函数中创建我们需要的对象？好吧，我们可以，但请记住测试运行器对测试用例类做了什么。它转到每个测试用例类，并创建一组对象，每个对象对应一个测试方法。这是一组很大的对象，但如果这些对象还没有分配他们需要的东西，那也没那么糟糕。通过在 setUp 中放置代码，在我们需要的时候创建我们需要的东西，我们节省了相当多的资源。此外，通过延迟 setUp，我们还可以在检测和报告安装过程中可能发生的任何问题时运行它。

##### CppUnitLite

When I did the initial port of CppUnit, I tried to keep it as close as I could to JUnit. I figured it would be easier for people who'd seen the xUnit architecture before, so it seemed to be the better thing to do. Almost immediately, I ran into a series of things that were hard or impossible to implement cleanly in C++ because of differences in C++ and Java's features. The primary issue was C++'s lack of reflection. In Java, you can hold on to a reference to a derived class's methods, find methods at runtime, and so on. In C++, you have to write code to register the method you want to access at runtime. As a result, CppUnit became a little bit harder to use and understand. You had to write your own suite function on a test class so that the test runner could run objects for individual methods.

> 当我完成 CppUnit 的初始端口时，我试图使它尽可能接近 JUnit。我认为对于以前看过 xUnit 体系结构的人来说，这会更容易，所以这似乎是更好的做法。几乎立刻，我就遇到了一系列很难或不可能在 C++中干净实现的事情，因为 C++和 Java 的特性不同。主要问题是 C++缺乏反思。在 Java 中，您可以保留对派生类方法的引用，在运行时查找方法，等等。在 C++中，您必须编写代码来注册要在运行时访问的方法。因此，CppUnit 变得有点难以使用和理解。您必须在测试类上编写自己的套件函数，以便测试运行程序可以为各个方法运行对象。

[]{#_bookmark68 .anchor}Test \*EmployeeTest::suite()

{

TestSuite \*suite = new TestSuite;

suite.addTest(new TestCaller\<EmployeeTest\>(\"testNormalPay\", testNormalPay));

> suite.addTest(new TestCaller\<EmployeeTest\>(\“testNormalPay\”，testNormalPay))；

suite.addTest(new TestCaller\<EmployeeTest\>(\"testOvertime\", testOvertime));

> suite.addTest(new TestCaller\<EmployeeTest\>(\“testOvertime”，testOvertime))；

return suite;

}

Needless to say, this gets pretty tedious. It is hard to maintain momentum

> 不用说，这会变得非常乏味。很难保持势头

writing tests when you have to declare test methods in a class header, define them in a source file, and register them in a suite method. A variety of macro schemes can be used to get past these issues, but I choose to start over. I ended up with a scheme in which someone could write a test just by writing this source file:

> 编写测试时，必须在类头中声明测试方法，在源文件中定义它们，并在套件方法中注册它们。可以使用各种宏观方案来克服这些问题，但我选择从头开始。我最终提出了一个方案，其中有人只需编写以下源文件就可以编写测试：

#include \"testharness.h\" #include \"employee.h\" #include \<memory\>

using namespace std; TEST(testNormalPay,Employee)

{

auto_ptr\<Employee\> employee(new Employee(\"Fred\", 0, 10)); LONGS_EQUALS(400, employee-\>getPay());

> auto_ptr\<Employee\>Employee(新员工(\“Fred\”，0，10))；LONGSEQUALS(400，employee-\>getPay())；

}

This test used a macro named LONGS_EQUAL that compares two long integers

for equality. It behaves the same way that assertEquals does in JUnit, but it's tailored for longs.

> 平等。它的行为与 assertEquals 在 JUnit 中的行为相同，但它被长期搁置。

The TEST macro does some nasty things behind the scenes. It creates a subclass of a testing class and names it by pasting the two arguments together (the name of the test and the name of the class being tested). Then it creates an instance of that subclass that is configured to run the code in braces. The instance is static; when the program loads, it adds itself to a static list of test objects. Later a test runner can rip through the list and run each of the tests.

> TEST 宏在幕后做了一些令人讨厌的事情。它创建一个测试类的子类，并通过将两个参数(测试的名称和要测试的类的名称)粘贴在一起来命名它。然后，它创建该子类的一个实例，该实例被配置为运行大括号中的代码。实例是静态的；当程序加载时，它将自己添加到测试对象的静态列表中。稍后，测试运行程序可以遍历列表并运行每个测试。

After I wrote this little framework, I decided not to release it because the code in the macro wasn't terribly clear, and I spend a lot of time convincing people to write clearer code. A friend of mine, Mike Hill, ran into some of the same issues before we met and created a Microsoft-specific testing framework called TestKit that handled registration the same way. Emboldened by Mike, I started to reduce the number of late C++ features used in my little framework, and then I released it. (Those issues had been a big issue in CppUnit. Nearly

> 在我写了这个小框架之后，我决定不发布它，因为宏中的代码不是很清楚，我花了很多时间说服人们写更清晰的代码。我的一个朋友 Mike Hill 在我们见面之前遇到了一些同样的问题，并创建了一个名为 TestKit 的微软特定测试框架，该框架以同样的方式处理注册。在 Mike 的鼓励下，我开始减少我的小框架中使用的后期 C++功能的数量，然后我发布了它

[]{#_bookmark69 .anchor}every day I received e-mail from people who couldn't use templates or the standard library, or who had exceptions with their C++ compiler.)

> []｛#_bookmark69.anchor｝我每天都会收到一些人发来的电子邮件，他们不能使用模板或标准库，或者他们的 C++编译器有异常。)

Both CppUnit and CppUnitLite are adequate as testing harnesses. Tests written using CppUnitLite are a little briefer, so I use it for the C++ examples in this book.

> CppUnit 和 CppUnitLite 都足以作为测试线束。使用 CppUnitLite 编写的测试有点简短，所以我在本书的 C++示例中使用了它。

##### NUnit

NUnit is a testing framework for the .NET languages. You can write tests for C# code, VB.NET code, or any other language that runs on the .NET platform. NUnit is very close in operation to JUnit. The one significant difference is that it uses attributes to mark test methods and test classes. The syntax of attributes depends upon the .NET language the tests are written in.

> NUnit 是一个用于.NET 语言的测试框架。您可以为 C#代码、VB.NET 代码或在.NET 平台上运行的任何其他语言编写测试。NUnit 在操作上与 JUnit 非常接近。一个显著的区别是它使用属性来标记测试方法和测试类。属性的语法取决于编写测试的.NET 语言。

Here is an NUnit test written in VB.NET:

Imports NUnit.Framework

\<TestFixture()\> Public Class LogOnTest Inherits Assertion

\<Test()\> Public Sub TestRunValid() Dim display As New MockDisplay() Dim reader As New MockATMReader()

> \<Test()\>Public Sub-TestRunValidat()Dim display As New MockDisplay(

Dim logon As New LogOn(display, reader) logon.Run()

AssertEquals(\"Please Enter Card\", display.LastDisplayedText) AssertEquals(\"MainMenu\",logon.GetNextTransaction().GetType.Name)

> AssertEquals(\“Please Enter Card”，display.LastDisplayedText)AssertEqual(\“MainMenu”，logon.GetNextTransaction().GetType.Name)

End Sub

End Class

\<TestFixture()\> and \<Test()\> are attributes that mark LogonTest as a test class and TestRunValid as a test method, respectively.

> \＜ TestFixture()\＞和＜ Test()\＜是分别将 LogonTest 标记为测试类和将 TestRunValid 标记为测试方法的属性。

##### Other xUnit Frameworks

There are many ports of xUnit to many different languages and platforms. In general, they support the specification, grouping, and running of unit tests. If you need to find an xUnit port for your platform or language, go to [www.xprogramming.com](http://www.xprogramming.com/) and look in the Downloads section. This site is run by Ron Jeffries, and it is the de facto repository for all of the xUnit ports.

> xUnit 有许多端口可以连接到许多不同的语言和平台。一般来说，它们支持单元测试的规范、分组和运行。如果您需要为您的平台或语言找到 xUnit 端口，请访问[www.xpprogramming.com](http://www.xprogramming.com/)并查看下载部分。这个站点由 Ron Jeffries 运行，它实际上是所有 xUnit 端口的存储库。

#### [General Test Harnesses](#contents)

The xUnit frameworks I described in the preceding section were designed to be used for unit testing. They can be used to test several classes at a time, but that sort of work is more properly the domain of FIT and Fitnesse.

> 我在上一节中描述的 xUnit 框架被设计用于单元测试。它们可以用来一次测试几个类，但这类工作更适合 FIT 和 Fitnesse 的领域。

##### Framework for Integrated Tests (FIT)

FIT is a concise and elegant testing framework that was developed by Ward Cunningham. The idea behind FIT is simple and powerful. If you can write documents about your system and embed tables within them that describe inputs and outputs for your system, and if those documents can be saved as HTML, the FIT framework can run them as tests.

> FIT 是一个简洁优雅的测试框架，由 Ward Cunningham 开发。FIT 背后的理念简单而强大。如果您可以编写有关系统的文档，并在其中嵌入描述系统输入和输出的表，并且这些文档可以保存为 HTML，则 FIT 框架可以将它们作为测试运行。

FIT accepts HTML, runs tests defined in HTML tables in it, and produces HTML output. The output looks the same as the input, and all text and tables are preserved. However, the cells in the tables are colored green to indicate values that made a test pass and red to indicate values that caused a test to fail. You also can use options to have test summary information placed in the resulting HTML.

> FIT 接受 HTML，在其中的 HTML 表中运行定义的测试，并生成 HTML 输出。输出看起来与输入相同，并且保留了所有文本和表。但是，表格中的单元格颜色为绿色，表示通过测试的值，红色表示导致测试失败的值。您还可以使用选项将测试摘要信息放置在结果 HTML 中。

The only thing you have to do to make this work is to customize some tablehandling code so that it knows how to run chunks of your code and retrieve results from them. Generally, this is rather easy because the framework provides code to support a number of different table types.

> 要做到这一点，唯一需要做的就是自定义一些表处理代码，这样它就知道如何运行代码块并从中检索结果。一般来说，这很容易，因为框架提供了支持多种不同表类型的代码。

One of the very powerful things about FIT is its capability to foster communication between people who write software and people who need to specify what it should do. The people who specify can write documents and embed actual tests within them. The tests will run, but they won't pass. Later developers can add in the features, and the tests will pass. Both users and developers can have a common and up-to-date view of the capabilities of the system.

> FIT 非常强大的一点是，它能够促进软件开发人员和需要指定软件应该做什么的人员之间的沟通。指定人员可以编写文档并在其中嵌入实际测试。测试会运行，但不会通过。以后的开发人员可以添加功能，测试就会通过。用户和开发人员都可以对系统的功能有一个通用的最新视图。

There is far more to FIT than I can describe here. There is more information about FIT at [http://fit.c2.com.](http://fit.c2.com/)

> FIT 的内容远远超出我在这里所能描述的范围。有关 FIT 的更多信息，请访问[http://fit.c2.com.](http://fit.c2.com/)

##### Fitnesse

Fitnesse is essentially FIT hosted in a wiki. Most of it was developed by Robert Martin and Micah Martin. I worked on a little bit of it, but I dropped out to concentrate on this book. I'm looking forward to getting back to work on it soon.

> Fitnesse 本质上是 FIT 托管在 wiki 中。其中大部分是由罗伯特·马丁和迈卡·马丁开发的。我写了一点，但我放弃了，专心写这本书。我期待着很快回去工作。

Fitnesse supports hierarchical web pages that define FIT tests. Pages of test tables can be run individually or in suites, and a multitude of different options make collaboration easy across a team. Fitnesse is available at [http://www.fitnesse.org.](http://www.fitnesse.org/) Like all of the other testing tools described in this chapter, it is free and supported by a community of developers.

> Fitnesse 支持定义 FIT 测试的分层网页。测试表页面可以单独运行，也可以在套件中运行，大量不同的选项使团队之间的协作变得容易。Fitnesse 可在[http://www.fitnesse.org.](http://www.fitnesse.org/)与本章中描述的所有其他测试工具一样，它是免费的，并得到开发人员社区的支持。
