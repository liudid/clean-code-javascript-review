# JavaScript功能实现code review
实践场景中的Good code，用于他人参考或学习


## 可能对您的帮助
 1. 我学习了一些设模式或看到了一些clean code，我想把其中的“Good”应用在我们的实际开发场景中，并熟练掌握它
 2. 我看到了一些Bad code，但我不知如何把它变成Good code，我想参考一些他人的设计
3.我在团队中缺少code review环境，我想提高自己的code水平

## 输出
 * PR：输出您在code review时认为“经常容易”出现的Bad code，并输出您认为的Good code
 * Issues：输出您认为的Bad code，并求助/讨论如何变成Good code

## 注意
 * 某个业务场景中的功能的设计或具体实现 ✅
 * 示例性质的代码片段 ❌
 * 您可以在不影响原实现的表达情况下而减少代码。但不可以是一个单纯的示例 💡

## 格式
**Type-1**———输出您在code review时认为“经常容易”出现的Bad code，并输出您认为的Good code
  1. 标题 <您的主要阐述>
  2. 来源 <您从何处看到的-Bad code>
  3. 场景 <什么样的场景下的什么功能>
  4. 坏处 <为什么您认为它是-Bad code>
  5. 好的实现 <你认为的-Good code / The best code>
  6. 为何示是好的 <说明它为什么是-Good code / The best code>
  7. 避免 <如何避免此类的Bad code>
  

<br><br>

**Type-2**———输出您认为的Bad code，并求助/讨论如何变成Good code
  1. 标题 <您的主要阐述>
  2. 场景 <什么样的场景下的什么功能>
  3. 想法 <您认为哪里做的不好>
  4. 目标 <应该达到某种状态>

<br><br><br>

## example type-1
  * 标题: 在项目中，我经常在业务代码中看到这样不好的代码
  * 来源: 我们项目中的代码
  * 场景: 通过点击菜单中的不同选项来达到不同的目的
  * 坏处: 阅读成本高、隐式、不方便维护
  * 好的实现: 参考example
  * 为何是好的: 参考example
  * 如何避免: 参考example

**Bad:**
```javascript
// bad:每个地方都用value去做判断，导致menus中的value一旦发生变化时，就要更改每一处
// bad:使用clickMenu('edit')可以进行隐式传值
let menus = [
  { label: '编辑', value: 'edit'},
  { label: '详情', value: 'detail'},
  { label: '更多', value: 'more'},
  { label: '删除', value: 'delete'},
]

function clickMenu(value) {
  switch (value) {
    case 'edit':
      toEdit()
      //...
      break
    case 'detail':
      toDetail()
      //...
      break
    case 'more':
      toMore()
      //...
      break
    case 'delete':
      deleteItem()
      //...
      break
    default:
      console.log('not menu')
      break
  }
}

clickMenu('edit')
```

**Good:**
```javascript
// 用类似枚举的方式将类型进行置顶声明，改变value时，无需更改其它位置
// 好处：消除了“魔法字符串”（Eliminate the magic string）
// 弊端：使用clickMenu('edit')可以进行隐式传值
const MENUS_TYPE = {
  Edit: '1',
  Detail: '2',
  More: '3',
  Delete: '4'
}

let menus = [
  { label: '编辑', value: MENUS_TYPE.Edit},
  { label: '详情', value: MENUS_TYPE.Detail},
  { label: '更多', value: MENUS_TYPE.More},
  { label: '删除', value: MENUS_TYPE.Delete}
]

function clickMenu(value) {
  switch (value) {
    case MENUS_TYPE.Edit:
      toEdit()
      //...
      break
    case MENUS_TYPE.Detail:
      toDetail()
      //...
      break
    case MENUS_TYPE.More:
      toMore()
      //...
      break
    case MENUS_TYPE.Delete:
      deleteItem()
      //...
      break
    default:
      console.log('not menu')
      break
  }
}

clickMenu(MENUS_TYPE.Edit)
clickMenu('1')

```

**Best:**
```javascript
// 对Good进行了优化，字符串'1234'没有任何含义，使用Symbol()代替
// 好处：不可以使用changeTab('xx')进行隐式传值
const MENUS_TYPE = {
  Edit: symbol(),
  Detail: symbol(),
  More: symbol(),
  Delete: symbol()
}

let menus = [
  { label: '编辑', value: MENUS_TYPE.Edit},
  { label: '详情', value: MENUS_TYPE.Detail},
  { label: '更多', value: MENUS_TYPE.More},
  { label: '删除', value: MENUS_TYPE.Delete}
]

function clickMenu(value) {
  switch (value) {
    case MENUS_TYPE.Edit:
      toEdit()
      //...
      break
    case MENUS_TYPE.Detail:
      toDetail()
      //...
      break
    case MENUS_TYPE.More:
      toMore()
      //...
      break
    case MENUS_TYPE.Delete:
      deleteItem()
      //...
      break
    default:
      console.log('not menu')
      break
  }
}

clickMenu(MENUS_TYPE.Edit)
```
