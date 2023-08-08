### [Introduction](#contents)

#### How to Use This Book

I tried several different formats before settling on the current one for this book. Many of the different techniques and practices that are useful when working with legacy code are hard to explain in isolation. The simplest changes often go easier if you can find seams, make fake objects, and break dependencies using a couple of dependency-breaking techniques. I decided that the easiest way to make the book approachable and handy would be to organize the bulk of it (_Part II, Changing Software_) in FAQ (frequently asked questions) format. Because specific techniques often require the use of other techniques, the FAQ chapters are heavily interlinked. In nearly every chapter, you'll find references, along with page numbers, for other chapters and sections that describe particular techniques and refactorings. I apologize if this causes you to flip wildly through the book as you attempt to find answers to your questions, but I assumed that you'd rather do that than read the book cover to cover, trying to understand how all the techniques operate.

> 我尝试了几种不同的格式，然后为这本书选择了目前的格式。许多在使用遗留代码时有用的不同技术和实践很难单独解释。如果您能够找到接缝、制作假对象并使用两种依赖关系打破技术打破依赖关系，那么最简单的更改通常会变得更容易。我决定，让这本书变得平易近人和方便的最简单方法是以 FAQ（常见问题）格式组织大部分内容（_第二部分，更改软件_）。由于特定的技术通常需要使用其他技术，常见问题解答章节之间有很大的联系。在几乎每一章中，你都会发现其他章节的参考资料和页码，这些章节描述了特定的技术和重构。如果这导致你在试图找到问题的答案时疯狂地翻阅这本书，我很抱歉，但我认为你宁愿这样做，也不愿一本接一本地阅读，试图了解所有技术是如何运作的。

In _Changing Software_, I've tried to address very common questions that come up in legacy code work. Each of the chapters is named after a specific problem. This does make the chapter titles rather long, but hopefully, they will allow you to quickly find a section that helps you with the particular problems you are having.

> 在“更改软件”中，我试图解决遗留代码工作中出现的非常常见的问题。每一章都以一个特定的问题命名。这确实使章节标题相当长，但希望它们能让你快速找到一个章节来帮助你解决你遇到的特定问题。

_Changing Software_ is bookended by a set of introductory chapters (_Part I, The Mechanics of Change_) and a catalog of refactorings, which are very useful in legacy code work (_Part III, Dependency-Breaking Techniques_). Please read the introductory chapters, particularly Chapter 4, _The Seam Model_. These chapters provide the context and nomenclature for all the techniques that follow. In addition, if you find a term that isn't described in context, look for it in the Glossary.

> *更改软件* 由一组介绍性章节（_第一部分，更改机制_）和一系列重构组成，这些重构在遗留代码工作中非常有用（_第三部分，依赖性打破技术_）。请阅读介绍性章节，特别是第 4 章“接缝模型”。这些章节提供了以下所有技术的上下文和命名。此外，如果您发现上下文中没有描述的术语，请在词汇表中查找。

The refactorings in _Dependency-Breaking Techniques_ are special in that they are meant to be done without tests, in the service of putting tests in place. I encourage you to read each of them so that you can see more possibilities as you start to tame your legacy code.

> *Dependency Breaking Techniques*中的重构是特别的，因为它们是在没有测试的情况下进行的，是为了将测试放在适当的位置。我鼓励您阅读其中的每一个，以便在开始驯服遗留代码时看到更多的可能性。
