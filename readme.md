## rxjs5 随想录

### 起源
- rxjs5是“革命派”，不是“革新派”。当我们还习惯于使用基于“pull”的离散孤立数据逻辑单元组织解构复杂的业务和交互逻辑时,rxjs5已经悄无声息的把基于"push"的连续(stream)可轻易相互转化并任意组合的流式响应式数据层解决方案带给了前端同学们。从此，当我们面对复杂的糅合了同步和异步，并且有协作或者竞争等等逻辑关系时，可以微微一笑，心中默念 “rxjs 在手，天下我有”。

### 形式化
- 我们都喜欢形式化，好像只有可以形式化的东西，才是科学的，可以被信任的东西。
- react给我们做了一个很好的榜样，view = f(state), 即针对不同的输入state，返回不同的 view。因此，当我们用 react 解决 view 层的问题时，时时刻刻都在想的是如何抽象 state。
- 同样的，当我们开始用rxjs的视角在数据层建模时，我们需要遵循的真理是:
```js
   result_stream = origin_stream.transform1()
                                .transform2()
                                ...;
```
- 稍微简单的介绍下，由rxjs构造的世界里基本单元是 stream(流)，流是一个二维的概念，除了值之外自动的帮你添加了时间的维度，即 stream_value = stream(stream_time)。tranform1 和 tranform2 被称为操作符，将一个流转化为另外一个流，即 new_stream = operator(old_stream)。所以，上面那一段不太好理解的代码就可以转化为文字：当我们使用rxjs给数据层建模时，实际上就是经历了：从初始流不断转化或者组合变成目标流的样子。
- 是不是有点难以理解，没关系下文我们会通过一个实际的例子将这种方法论进行详细的说明。

### 实际例子背景介绍(整数旅行器)
    - 我们需要开发一款整数旅行器产品，用户输入想要到达的终点整数，旅行器需要从上一个整数(初始为0)出发，以1为单位，向上或者向下逼近并到达终点整数. 
    - 文字有点晦涩，让我们把玩把玩 [demo](http://jsbin.com/jojucaqiki/1/edit?html,js,output) 

**在继续探索后面的内容之前，请确保对整数旅行器理解透彻**

### 让我们来看看rxjs是如何思考的
    - 首先，我们要准确的识别出源头流，在本例中为:input 输入流。随着时间的流逝，用户可以输入多个数值，即源头流发出多个数据。
    - 其次，我们要准确的识别出目标流，在本例中为:在特定时间内，从上一个数据状态逐步变化为从input拿到的值。
    - 那么，我们该怎么一步一步的将源头流转化为目标流呢？
        1: 创建源头流
        ```js
           const origin_stream = create_stream_from_input_event();
        ```
        2: 源头流会发出每个input事件对象，而我们只关心用户的输入值，因此，转化为发出用户输入数据的流。如下图，我们利用了操作符map达到了这个目的。此时此刻，我们拥有了一个只发出用户输入数据的中间流。
        ```js
            const with_input_value_stream = orign_stream.map(event => event.value);
        ```
        3: 只拿到用户的数据不行，我们还需要继续前进。当我们拿到用户数据的输入数据时，我们需要产出从上一个数据状态到用户输入值的所有整数，才能完成整数旅行器的职能。很明显，我们需要针对每个用户输入值创建一个从上一个状态到用户输入值的流，
            3.1 从哪开始呢？首先要引入时间，因为数据是在不同的时间发出。这个流代表从现在开始，每隔20ms发出从0开始自增的数据。
            ```js
                const timer_stream = create_stream_from_interval_timer(0, 20)
            ```
            3.2 等等，我们不是要从上一个状态开始嘛？这个流从0开始，肯定不对啊。别着急，转化下即可。将流转换为从上个数据状态开始。
            ```js
                const timer_stream_start_with_old_state = 
                    timer_stream.startwith(old_state);
            ```
            3.3 到目前为止，我们得到了发出从一个数据状态开始的递增流，这还不满足的我们的需求，我们希望得到一个发出从上一个状态到用户输入的流，怎么办？很简单，我们只需要得到方向(正一活着负一)，然后运用累加函数即可。依然还是流变化。
            ```js
                const timer_stream_start_with_old_state_with_direction = timer_stream_start_with_old_state.mapto(() => start > end ? -1 : 1)

                const timer_stream_start_with_old_state_with_direction_reducer = timer_stream_start_with_old_state_with_direction.reducer((dir, accu) => dir + accu)
            ```
            3.4 这样，我们就得到了一个从上一个数据状态，以-1或者1为方向，一直前进不停止的数据流，还差一点点，我们需要数据流要停止，怎么办？很简单，还是进行变换，流变化。通过给上面的流，添加 takeutil 操作符，就可以了。
            ```js
                const finnal_stream = timer_stream_start_with_old_state_with_direction_reducer.takeutil(value => value == input);
            ```


### 结论

 这篇文章的目的只有一个：让大家对 rxjs 的方法论： 流 + 变化（操作符），有一定的感官认识。

 在大家对 rxjs 的方法论，有一定的感官认识之后，后续的文章将会深入 rxjs 的内部。


 # one more thing

     http://cn.rx.js.org/， rxjs 中文社区成立之后做的第一个重要事情，rxjs 中文官方api 的上线，希望得到大家的支持，我们一起努力，
     rx evething.
