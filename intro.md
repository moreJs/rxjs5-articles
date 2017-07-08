# Rxjs v5 新手必备

> 前言: Rxjs 是前端目前为止响应式编程的最佳实践。很不幸的是我们已经用传统方式开发很多年了，“evething is stream” 的思想对我们来说不再是顺其自然，甚至会有一点蹩脚，尤其是初入响应式的坑。因此，我们需要给大家推荐三款可视化的神器，帮助大家对响应式进行感性的了解。

## rxviz
  
  [github](https://github.com/moroshko/rxviz), [onlineDemo](https://github.com/moroshko/rxviz)

  这款可视化工具是由 facebook 的 Misha Moroshko 开发。

  rxviz 可以简洁的可视化给定的 Observable. 你提供的流式代码会被执行，如果最后一个表达式是 Observable， 一个
  带着动画的可视化会出现在眼前。

  同时，你可以通过修改时间窗口来控制动画的速率，也可以将可视化svg copy下来用于你想用的地方，你同样可以将可视化分享给其他人。

  总之，rxviz 是一款很适合 Rxjs 初学者的可视化工具。

## rxvision

  [github](https://github.com/jaredly/rxvision), [onlineDemo](https://jaredforsyth.com/rxvision/examples/gh-follow/)  

  推荐这款 rxvision 可视化的工具时，我的内心是纠结的。个人来讲，我非常喜欢它，但是，尴尬的是作者已经不维护了，擦。
  但是，它还有一个不得不推荐的理由。请容我慢慢道来。

  相信[这篇文章](https://gist.github.com/staltz/868e7e9bc2a7b8c1f754 )是所有前端响应式的殿堂级入门文章，中文也有人翻译再加工过。文章中的例子，也是经典，详细阐述了如何用“响应式”的思想构建业务逻辑，而 rxvision 对这个例子进行了可视化，没错，是这个样子的。

  所以，我们可以结合这篇文章和 rxvision 更好的理解响应式开发模式。

## rxmarbles

  [github](https://github.com/staltz/rxmarbles) [onlineDemo](http://www.rxmarbles.com) 

  这个库不得不推荐啊，这是响应式大神 staltz 的作品。和前面库最大的不同是, Observable 的每个 item 是可交互的，你可以拖拽，然后整个 Observable 都会做出相应的改变。


> 结论:我们可以在学习 Rxjs 的时候，结合这些可视化的，相信会达到事半功倍的效果。


