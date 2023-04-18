# clean-code-javascript-review
打造实用场景中的good code，用于他人参考或学习


对于某个业务场景 / 某个功能设计或具体实现

1.您在code review自身团队代码中或其它代码中认为存在或不可取的bad code，并且您认为有good code / best code。请您先贴出bad code，并贴出您认为的best code。
  1. 你从哪里看到的
  2. 指出它为什么是bad code
  3. 贴出您认为的good code / best code
  4. 说明它的好处
  5. 针对于您给出的bad code，如何去避免

<br><br>
2.您在实现某个场景中的功能中认为自己的代码是bad code，但您不知道如何变成best code，那么请您贴出您认为的bad code，可由他人帮助您变成best code。

<br><br><br>

## example<br>
*标题*: 不应去字符串去做判断<br>
*说明*: 在项目中，我经常在业务代码中看到这样不好的代码<br>
*场景/功能*: 切换不同类型的Tab<br>

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
