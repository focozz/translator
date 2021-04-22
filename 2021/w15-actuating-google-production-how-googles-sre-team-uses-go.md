#Go 在 Google SRE 的工程实践
谷歌线上跑着一些巨型服务，这些服务由全球基础设施提供支持，涵盖了开发者所需的所有组件：存储系统、负载均衡器、网络、日志记录、监控等等。然而，它并不是一个静态系统，它也不可能是。随着架构的演化，新产品和新想法的不断产生，新版本的发布，配置的推送，数据库模式的更新等等，最终需要我们会以每秒数十次的速度将这些更改部署到系统中。

由于规模过于庞大，以及对可靠性的迫切需求，谷歌开创了网站可靠性工程（SRE）的先河，此后许多其他公司都开始采用了这一角色。
> “SRE是当你把操作当作软件问题来对待时得到的结果。我们的使命是保护、提供和改进谷歌所有公共服务背后的软件和系统，时刻关注它们的可用性、延迟、性能和容量。”-[网站可靠性工程（SRE）](https://sre.google/)。

![Credit to Renee French for the Go Gopher](../static/images/w15-actuating-google-production-how-googles-sre-team-uses-go/gosreheader.png
)
Credit to Renee French for the Go Gopher


2013-2014年，谷歌的SRE团队开始意识到，我们的生产管理方法在很多方面都不再像之前那么有效。我们的运维自动化已经非常领先了，但是我们这样的规模有太多的变动部分和复杂性，所以我们需要一种新的方法来保障它。我们最终确定我们需要朝着生产声明模型（称为“ `Prodspec`”）的方向前进，用它来驱动一个专用的控制平面（称为“`Annealing`”）。

当我们开始开发这些项目时，`Go`刚刚成为Google提供关键服务的可行选择之一。大多数工程师更熟悉`Python`和`C++`，这两者都是有效的选择。尽管如此，`Go`还是引起了我们的兴趣。好奇心当然是一个因素，但是，更重要的是，`Go`承诺了其他语言都无法提供的在性能和可读性之间的最佳平衡点。我们开始了一个小实验， 用Go实现了`Annealing`和`Prodspec`的一些初始部分。随着项目的进展，最初用`Go`编写的部分成为了核心。我们对`Go`很满意——它的简单性越来越好，性能也很好，并且并发原语很难被替代。

尽管我们从来没有收到使用`Go`的命令或要求，但是我们已经不想回去使用`Python`或`C++`。`Go`在`Annealing`和`Prodspec`逐步被应用。现在回过头来看，这是相当正确的选择。现在，大部分谷歌产品是由我们用Go编写的系统管理和维护的。

在这些项目中拥有一门简单语言的力量是难以言表的。尽管`Go`在有些情况下确实缺少了某些特性，如在代码中强制某些复杂结构不发生改变的能力。但是对于大多数情况，毫无疑问，在多数情况下，这种简单性起到了帮助作用。

举个例子，`Annealing`影响各种各样的团队和服务，这意味着我们的发展严重依赖于整个公司的贡献。`Go`的简单性使我们团队之外的人能够看到为什么某些部分或其他部分不适合他们，并经常自己提供修复或特性，这让我们得以迅速成长。

`Prodspec`和`Annealing`负责一些非常关键的组件。Go的简单性意味着这些代码无论是在审核期间发现错误还是在尝试确定服务中断期间到底发生了什么时，都很容易去跟踪。

`Go`的高性能和对并发性支持也是帮助我们工作的关键。由于我们的产品模型是声明式的，所以我们需要操作大量的结构化数据，这些数据主要用来描述了产品是什么以及它应该是什么。加上我们拥有大量的服务，所以数据量变得巨大无比，这种情况下如果使用纯粹的顺序执行处理就显得不是那么高效。

我们在很多地方用很多方式操纵这些数据，与其让一个聪明人想出我们算法的并行版本的问题，不如在遇到一个偶然的并行性问题时，找到瓶颈并将这段相关代码进行并行化，`Go`可以轻易地做到这一点。

归功于我们在`Go`上取得了成功，我们现在将`Go`应用于`Prodspec`和`Annealing`的每个新开发中。除了SRE团队，谷歌的工程团队也在他们的开发过程中采用了`Go`。点击了解[Core Data Solutions](https://go.dev/solutions/google/coredata/)，[Firebase Hosting](https://go.dev/solutions/google/firebase/)和[Chrome](https://go.dev/solutions/google/chrome/)团队如何使用`Go`来大规模构建快速，可靠和高效的软件。


## References
1. 网站可靠性工程（SRE）https://sre.google/
2. Core Data Solutions https://go.dev/solutions/google/coredata/
3. Firebase Hosting https://go.dev/solutions/google/firebase/
4. Chrome https://go.dev/solutions/google/chrome/