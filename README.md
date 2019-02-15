# ReactiveCocoa学习
* 本文意在方便以后方便**回顾**和**复习**使用,**思维导图**估计就自己看得懂了
## 话不多说,上代码
### RACDynamicSignal
```
RACDisposable * (^didSubscribe)(id<RACSubscriber> subscriber) = ^RACDisposable * _Nullable(id<RACSubscriber>  _Nonnull subscriber) {

self.subscriber = subscriber;
//didSubscribe调用:只要一个信号被订阅就会被调用
//didSubscribe作用:发送数据
NSLog(@"信号被订阅");
[subscriber sendNext:@"zmy"];

//        [subscriber sendError:<#(nullable NSError *)#>]
//        [subscriber sendCompleted];
return [RACDisposable disposableWithBlock:^{
//只要信号被取消订阅就回来这里
//清空资源
NSLog(@"信号被取消订阅了");
}];
};

RACSignal *signal = [RACSignal createSignal:didSubscribe];
RACDisposable *disposable = [signal subscribeNext:^(id  _Nullable x) {
//nextBlock调用:只要订阅者发送数据就会调用
//nextBlock作用:处理数据，展示到UI上面
NSLog(@"来了:%@",x);
}];
[disposable dispose];
```
### RACSubject
```
//1.创建信号
RACSubject *subject = [RACSubject subject];
//2.订阅信号
//不同信号订阅的方式不一样
//RACSubject处理订阅:仅仅是保存订阅者 (1.RACSubscriber *o创建，2.[subscribers addObject:o];)
[subject subscribeNext:^(id  _Nullable x) {
NSLog(@"接收到数据:%@",x);
}];
[subject subscribeNext:^(id  _Nullable x) {
NSLog(@"第二个订阅者:%@",x);
}];
//3.发送数据
//取出数组里的subscribers 每一个订阅者， 发送 sendNext，调用RACSubscriber 的 nextBlock
[subject sendNext:@"zmy"];
//    [subject sendNext:@"asd"];
```
### RACReplaySubject
```
//1.创建信号
RACReplaySubject *subject = [RACReplaySubject subject];

//3.发送信号
//3.1 保存发送的值
//3.2遍历所有的订阅者，发送数据
[subject sendNext:@"zmy"];
[subject sendNext:@"zmy1"];

//2.订阅信号
[subject subscribeNext:^(id  _Nullable x) {
NSLog(@"接收到数据:%@",x);
}];
//RACReplaySubject特点:可以先发送信号，再订阅信号
```
## 思维导图
![Image text](https://github.com/iOS-fei/ReactiveCocoa/blob/master/img-folder/Snip20190215_1.png)
