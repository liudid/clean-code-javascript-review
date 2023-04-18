# JavaScript场景功能实现code review
打造实用场景中的good code，用于他人参考或学习

## 注意
某个业务场景中的某个功能的设计或具体实现 ✅
单一示例的代码片段 ❌

## 发起
type-1.您在code review自身团队代码中或其它代码中认为存在或不可取的bad code，并且您认为有good code / best code去实现它。
  1. 标题:[主要阐述]
  2. 来源:[您从何处看到的-bad code]
  3. 场景:[什么样的场景下的什么功能]
  4. 坏处:[指出它为什么是-bad code]
  5. 好的实现:[贴出您认为的-good code / best code]
  6. 为何示是好的:[说明它为什么是good code / best code]
  7. 避免:[如何避免此类的bad code]
  

<br><br>
type-2.您在实现某个场景中的功能中认为自己的代码是bad code，但您不知道如何变成best code，那么请您贴出您认为的bad code，可由他人帮助您变成best code。
  1. 您认为它是bad code的理由
  2. 您理想中的设计/实现应该是怎样的？

<br><br><br>

## example [type-1]<br>
#### 标题: 在项目中，我经常在业务代码中看到这样不好的代码
#### 来源: 我们项目中的代码
#### 场景: 一个关于Tab切换的功能
#### 坏处: 阅读成本高、隐式、不方便维护
#### 好的实现: 参考example
#### 为何是好的: 参考example
#### 如何避免: 参考example

**Bad:**
```javascript
// 每个地方都用value去做判断，导致Tabs中的value一旦发生变化时，就要更改每一处
// 使用changeTab('1')可以进行隐式传值
const Tabs = [
  { label: '未开始', value: '1'},
  { label: '进行中', value: '2'},
  { label: '已结束', value: '3'},
]

let activeTab = 1

function changeTab(status) {
  activeTab = status
  switch (activeTab) {
    case '1':
      //...
      break
    case '2':
      //...
      break
    case '3':
      //...
      break
  }
}

function onTabChange(status){
  changeTab(status)
}
```

**Good:**
```javascript
// 用类似枚举的方式将不同状态进行置顶声明，改变value时，无需更改其它位置
// 好处：消除了“魔法字符串”（Eliminate the magic string）
// 弊端：使用changeTab('1')可以进行隐式传值
const TAB_STATUS = {
  NotStart: '1',
  Processing: '2',
  Finished: '3',
}

const Tabs = [
  { label: '未开始', value: TAB_STATUS.NotStart},
  { label: '进行中', value: TAB_STATUS.Processing},
  { label: '已结束', value: TAB_STATUS.Finished},
]

let activeTab = TAB_STATUS.NotStart

function changeTab(status) {
  activeTab = status
  switch (activeTab) {
    case TAB_STATUS.NotStart:
      //...
      break
    case TAB_STATUS.Processing:
      //...
      break
    case TAB_STATUS.Finished:
      //...
      break
  }
}

function onTabChange(status){
  changeTab(status)
}
```

**Best:**
```javascript
// 对Good进行了优化字符串'1','2','3'没有任何含义，使用Symbol()代替
// 好处：不可以使用changeTab('xx')进行隐式传值
const TAB_STATUS = {
  NotStart: Symbol(),
  Processing: Symbol(),
  Finished: Symbol(),
}

const Tabs = [
  { label: '未开始', value: TAB_STATUS.NotStart},
  { label: '进行中', value: TAB_STATUS.Processing},
  { label: '已结束', value: TAB_STATUS.Finished},
]

let activeTab = TAB_STATUS.NotStart

function changeTab(status) {
  activeTab = status
  switch (activeTab) {
    case TAB_STATUS.NotStart:
      //...
      break
    case TAB_STATUS.Processing:
      //...
      break
    case TAB_STATUS.Finished:
      //...
      break
  }
}

function onTabChange(status){
  changeTab(status)
}
```
