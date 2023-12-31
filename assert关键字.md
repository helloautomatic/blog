在运行阶段，如果启用了 assert，它会进行运行时检查。当检查的条件表达式为真时，assert 不会产生任何影响，程序会继续正常执行。但如果条件表达式为假，assert 将触发一个断言失败，输出错误信息，并通常终止程序的执行。

在运行阶段，如果禁用了 assert（通过定义宏 NDEBUG），那么所有的 assert 语句都将被预处理器移除，不会对程序的执行产生任何影响，包括断言检查和相关的错误信息输出。

所以，assert 主要在开发和调试阶段起作用，帮助开发者发现和修复潜在的问题。在生产环境中，为了提高性能和减少不必要的开销，通常会禁用 assert。

assert(布尔表达式)

比如：

```
void RCC_APB2PeriphClockCmd(u32 RCC_APB2Periph, FunctionalState NewState)
{
  /* Check the parameters */
  assert(IS_RCC_APB2_PERIPH(RCC_APB2Periph));
  assert(IS_FUNCTIONAL_STATE(NewState));

  if (NewState != DISABLE)
  {
    RCC->APB2ENR |= RCC_APB2Periph;
  }
  else
  {
    RCC->APB2ENR &= ~RCC_APB2Periph;
  }
}
```
