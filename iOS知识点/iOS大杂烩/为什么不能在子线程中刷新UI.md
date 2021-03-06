## 为什么不能在子线程中刷新UI 

1.一切有关UI的操作，涉及操作程序用户界面的类，都需要在主线程执行，因为UIKit（包括绘图）非线程安全，如果两个子线程同时操作同一个图片，将会导致APP崩溃，而且释放也是会成为一个问题，同一个视图对象只能被释放一次，所以要在主线程中执行；

2.在操作UI时，比如改变了 Frame、更新了 UIView/CALayer 的层次时，或者手动调用了 UIView/CALayer 的 setNeedsLayout/setNeedsDisplay方法后，这个 UIView/CALayer 会先被标记为待处理，并被提交到一个全局的容器去。之后通过Observer监听事件调用函数再遍历所有待处理的 UIView/CAlayer 以执行实际的绘制和调整，并更新 UI 界面。所以，子线程会导致出现延时提交或者无法提交，而放在主线程就不会有如此影响用户交互的情况出现。
 
