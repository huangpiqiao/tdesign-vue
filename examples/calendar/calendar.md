:: BASE_DOC ::

### 功能示例

#### 切换模式 日历\月历
组件默认以日历的形式展示，并提供了“月”（日历）\“年”（月历）两种模式切换按钮，在组件外开发者可以通过修改 `mode` 属性切换模式。
::: demo demos/mode
:::

#### 指定第一列的星期
组件默认第一列为“周一”，可以通过 `firstDayOfWeek` 属性设置成其他星期(只对mode为"month"有效)。
::: demo demos/firstDayOfWeek
:::

#### 自定义日历范围
组件默认情况下，年份可以选择1970~∞，月份没有限制。可以通过 `range` 属性来设置日历的可选范围。
::: demo demos/range
:::

#### 控件相关配置
通过 `controllerConfig` 属性，开发者可以对日历组件右上角的组件进行一些个性化的控制（该属性结构比较复杂，详见文档“Calendar Props”中该字段的说明）。
::: demo demos/controllerConfig
:::


### 事件示例

#### 各种事件
组件提供提供了一些事件，开发者可以使用这些事件去实现一些更加定制化的功能（详见文档“Calendar Events”的说明）。
::: demo demos/events
:::

### 插槽示例

####  头部插槽（组件左上角）
::: demo demos/head
:::

#### 单元格插槽-追加内容
默认情况下日历单元格中会显示当前日期，如果还需要额外显示其他信息，可以通过slot `cellAppend` 来实现。
``` 
💡 请注意：不同模式下使用的是同一个slot，可以通过slot-scope的data.mode来判断当前是哪种模式，然后做出不同渲染 :)
```
::: demo demos/cellAppend
:::

#### 单元格插槽-自定义内容
和slot `cellAppend` 不同，slot `cell` 可以完全自定义单元格内容。
``` 
💡 请注意：不同模式下使用的是同一个slot，可以通过slot-scope的data.mode来判断当前是哪种模式，然后做出不同渲染 :)
```
::: demo demos/cell
:::

#### 卡片风格下的单元格插槽
卡片风格下单元格的空间非常有限，可以slot-scope的data.theme来判断当前是那种风格，然后做出不同渲染 :)
::: demo demos/cardCell
:::

### Calendar Props
| 属性 | 类型 | 默认值 | 必传 | 说明 |
|-----|-----|-----|-----|-----|
|value|Date|new Date()|N|指定高亮日期|
|firstDayOfWeek|number|1|N|周起始日（可以设置第一列显示周几，其他列就顺延下去），mode为“month”的时候有效；传入的值得必须是整数1到7其中之一|
|theme|string|"full"|N|风格，传入值必须是"full"或"card"|
|mode|string|"month"|N|模式，传入值必须是"month"或"year"|
|range|{from:Date, to:Date}|-|N|自定义日历的年月份显示范围（包含from和to）|
|preventCellContextmenu|boolean|false|N|是否禁用单元格右键默认系统菜单|
|isShowWeekendDefault|boolean|true|N|默认是否显示周末|
|controllerConfig|object|[见下方]|-|右上角控件组的相关配置|

```
controllerConfig 属性的结构示例如下：
{
  visible: true,	// 是否显示（全部控件）
  disabled: false,	// 是否禁用（全部控件）
  // 模式切换单选组件设置
  mode: {
    visible: true,	// 是否显示
    radioGroupProps: {},	// 用于透传props给该radioGroup组件
  },
  // 年份选择框组件相关设置
  year: {
    visible: true,	// 是否显示
    selecteProps: {},	// 用于透传props给该select组件
  },
  // 年份选择框组件相关设置
  month: {
    visible: true,	// 是否显示（“year”模式下本身是不显示该组件的）
    selecteProps: {},	// 用于透传props给该select组件
  },
  // 隐藏\显示周末按钮组件相关设置
  weekendToggle: {
    visible: true,	// 是否显示
    showWeekendButtonProps: {},	// 用于透传props给显示周末按钮组件
    hideWeekendButtonProps: {},	// 用于透传props给隐藏周末按钮组件
  },
  // “今天\本月”按钮组件相关设置
  current: {
    visible: true,	// 是否显示
    currentDayButtonProps: {},	// 用于透传props给“今天”钮组件（“month”模式下有效）
    currentMonthButtonProps: {},	// 用于透传props给“本月”按钮组件（“year”模式下有效）
  },
}
```


### Calendar Events
| 事件名 | 说明 | 说明 |
|-----|-----|-----|
|onCellClick|日历单元格点击事件（左键点击）|({ date: Date, filterDate: Date, mode: String }) => {}|
|onCellDoubleClick|日历单元格双击事件|({ date: Date, filterDate: Date, mode: String }) => {}|
|onCellRightClick|日历单元格点击事件（右键点击）|({ date: Date, filterDate: Date, mode: String }) => {}|
|controllerChange|右上角控件组选中值有变化的时候触发该事件|({ isShowWeekend: Boolen, filterDate: Date, mode: String }) => {}|

### Calendar Slots
| 插槽名称 | 参数 | 说明 |
|-----|-----|-----|
|head| - |头部插槽（左上角处，默认不显示任何内容）|
|cell| { data: Object } |单元格插槽（替换默认内容）|
|cellAppend| { data: Object } |单元格插槽（在原来的内容之后追加）|
```
slot cell\cellAppend 的data包含字段说明：
{
  mode:string,    // 当前的模式("month"或"year")
  date:Date,      // 单元格对应的日期
  year:number,    // 单元格对应的年份
  month:number,   // 单元格对应的月份

  // 当mode为"month"（日历）时，有以下其他字段
  belongTo:number,      // 值为0表示是当前日历显示的月份中的日期，为-1表示是上个月的，为1表示是下个月的
  dateDiaplay:string,   // 单元格默认显示的内容
  day:number,           // 单元格对应的星期（1~7，表示周一到周日）
  isCurDate:boolean,    // 是否当前日期（高亮）
  isWeekend:boolean,    // 是否是周末
  weekNum:number,       // 对应本月的第几周
  
  // 当mode为"year"（月历）时，有以下其他字段
  monthDiaplay:string,  // 单元格默认显示的内容
  isCurMon:boolean,     // 是否当前月份（对应高亮月份）
  isCurYear:boolean,    // 是否当年份（对应高亮月份）
}
```