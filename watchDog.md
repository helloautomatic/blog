# 看门狗
看门狗就是一个比较特殊的定时器，通常它的时钟源与系统的时钟源是相互独立的。看门狗定时器的定时时间不是那么的准确。但是它不需要像系统那样精确。

喂狗操作，可以在定时器中断中进行，也可以在主程序中进行。在开启看门狗的情况下，在规定时间（<=某个时间）内进行喂狗操作，如果不进行喂狗操作，看门狗认为程序出现了异常，将会进行复位操作。

## 定时器中断中喂狗和主程序中喂狗哪个比较安全？

问：在定时器中喂狗，能解决程序陷入无限循环的问题嘛？

答：在一定程度上能

问：我是这样理解的，如果程序陷入无限循环，比如`while(1);`，但是，看门狗的定时器中断仍然可以产生啊！只要能产生中断，就能喂狗，就不会触发复位，那么在这种情况下，看门狗不就失效了么？

答：你的理解是正确的。如果程序陷入无限循环（比如 while(1);），而且看门狗的喂狗操作也在该循环之后的某个地方执行，那么看门狗定时器中断的确会产生，喂狗的操作也会被执行，阻止了看门狗复位的触发。在这种情况下，看门狗失去了监督程序是否正常运行的效果。
这也强调了看门狗的使用需要注意喂狗的时机。如果看门狗的喂狗操作位于一个可能导致程序陷入无限循环的代码之后，看门狗的作用将受到限制，因为程序无法从无限循环中跳出去执行喂狗操作。
为了确保看门狗的有效性，通常喂狗的操作应该分布在程序的各个关键点，而不是集中在某个可能陷入无限循环的地方。这样即使程序的某一部分陷入了问题，其他部分仍然能够继续执行喂狗操作，保持看门狗的监督作用。
总之，看门狗的喂狗操作需要谨慎设计，确保分布在程序的合适位置，以充分发挥看门狗的监督和保护作用。

