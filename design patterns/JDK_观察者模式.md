## JDK对观察者模式的支持

![](assets/image32.jpeg)

- Observer：抽象观察者
update(Observable o, Object arg); 观察者对目标状态修改的响应

- Observable：观察目标
addObserver(Observer o) 新增观察者
deleteObserver (Observer o) 删除观察者