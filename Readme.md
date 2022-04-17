## SHooks
- yarn add shooks -S
- import { useDebounce } from 'shooks'

### useRem 辅助使用rem
- 不是使用rem
- 动态修改html的font-size
- 监听onResize
- 参数 options? 配置 选填;每baseWidth-->baseFontSize
  - baseWidth: number; 选填 默认为375
  - baseFontSize: number;选填 默认为16px
  - maxWidth: number;选填 默认无
- @return fontSize:number
  - fontSize:number,返回font-size

```tsx
// const fontSize = useRem({maxWidth:800})
const fontSize = useRem()
```
### useCookie 设置/获取cookie
- 参数:{key,options}
  - key:cookie的键
  - options 可选
    - defaultValue?: string | number; 默认值
    - expires?: Date | number;过期时间/有效时长(ms)
    - path?: string;可用的路径
    - domain?: string;可用的域
    - secure?: boolean;https 安全协议
    - ameSite?: "strict" | "lax" | "none";跨域请求
- @return [data,update]
  - data:string 取出的值
  - update:(value:string)=>void 更新
```tsx
  const [data,update] =useCookie('a',{
      // defaultValue:210,
      path:'/',
      // expires:new Date(2023,0,1),
      expires:1000*60,
      domain:'localhost',
      secure:true,
      sameSite:'strict'
  });
```
### useUUID 获取随机uuid
- 支持自定义长度
- 默认为 [8, 4, 4, 4, 12] // 如 `902e95e8-5101-0c54-374e-dfd89dc98523`
- 参数:
  - len?:number | number[] 选填,纯数字或数字数组,用数组会用-隔开
- @return  [uuid, refresh]
  - uuid 结果
  - refresh 更新
```tsx
const [uuid, refresh] = useUUID()
// const [uuid, refresh] = useUUID(80)
// const [uuid, refresh] = useUUID([1,2,3,4])
return <>
    <div onClick={refresh}>{uuid}</div>
</>
```
### useParams 解析params
- 解析params或location.search
- 支持重复key
- 参数 (params)
  - params:string 选填,默认为location.search
- @return {}: Record<string, string | string[]>
  - 返回对象,如果key重复,返回数组
```tsx
const [params,setParams] = useState(undefined as string)//init use location.search
const data= useParams(params)
return <>
    <div onClick={()=>setParams('a=1&a=2&a=1')}>update</div>
</>
```
### useSetState `setState`
- 参数 object
- @return [state,setState]
  - state 最新的对象
  - setState 更新,类似Class的
```tsx
const [state,setState] = useSetState({a:1,b:2})
return <>
    <div onClick={()=>setState({a:state.a+1})}>{JSON.stringify(state)}</div>
    <div onClick={()=>setState({c:1})}>add c</div>
    <div onClick={()=>setState(prev=>({
        a:prev.a-1
    }))}>update</div>
</>
```
### useRandom 随机获取
- 参数 [...args]
  - args:任意参数
- @return [data,update]
  - data 取出的随机值
  - update 更新随机值

### useAdd 大数相加
- 不支持负数
- 参数 [...nums]
  - 数字/字符串表示的数字
- @return [data,add]
  - 计算结果
  - add:累加
```tsx
const [data,add] = useAdd('99999999999999999999999999999','9999999999999999999999999999999999','9999999999999999999999999999999999')
return <div onClick={()=>add('99999999999999999999999999999')}>{data}</div>/
```

### useCountDown 标准时间倒计时
- 以秒为单位的倒计时
- 参数 [time,options:{interval,callback}]
  - time:number,倒计时(s)
  - options
    - interval?:number,可选,触发间隔,单位秒
    - callback?:function,可选,倒计时归0后触发
  - @return [remain,{stop,wait,start}]
    - remain 剩余时间,单位秒
    - stop 停止
    - wait 暂停
    - start 开始/继续

```tsx
 const [remain,{
      stop,
      wait,
      start
  }] = useCountDown(10,{
      interval:2,
      callback:()=>{
          console.log('done')
      }
  })

  return <>
      <div>{remain}</div>
      <span onClick={wait}>wait</span>
      <span onClick={stop}>stop</span>
      <span onClick={start}>start</span>
  </>
```

### useScroll
- 监听元素的滚动位置
  - 支持ref,未绑定返回0
  - document
- @return [left,top] 
  - left:number,坐边距
  - top:number,上边距

### useSize
- 基于ResizeObserver
- 初始化参数
  - ref/Element
- @return
  - [width,height]
```tsx
const ref = useRef()
const [width,height] = useSize(ref)//正常的ref

const refNull = useRef()
const [wNull] = useSize(refNull)//空ref 返回0

const [wApp] = useSize(document.getElementById('app'))// document
const [nullApp] = useSize(document.getElementById('app1'))// 不存在的DOM 返回0

return <>
    <div ref={ref}>{width}</div>
    <div>{wNull}</div>
    <div>{wApp}</div>
    <div>{nullApp}</div>
</>
```

### useFullScreen 全屏
- 支持全屏的浏览器才有效果
- 参数 element
  - element:ref,()=>dom 非必填,默认document
- 返回 [isFullScreen, {enterFullscreen, exitFullscreen, toggleFullscreen,isEnabled}]
  - isFullScreen 现在是否是全屏
  - enterFullscreen 进入全屏
  - exitFullscreen 退出全屏
  - toggleFullscreen 切换全屏
  - isEnabled 是否支持全屏
```tsx
const ref = useRef()
const [isFullScreen, {
    enterFullscreen, exitFullscreen, toggleFullscreen,isEnabled
// }] = useFullScreen(ref)
}] = useFullScreen(()=>document.querySelector('img'))

return <>
    {`${isFullScreen}`}
    <img ref={ref} src="https://www.baidu.com/favicon.ico" alt=""/>
    <div onClick={enterFullscreen}>enterFullscreen</div>
    <div onClick={exitFullscreen}>exitFullscreen</div>
    <div onClick={toggleFullscreen}>toggleFullscreen</div>
</>
```
### useUpdate 强制刷新
```tsx
const update= useUpdate()
return <>
    <div onClick={update}>update</div>
</>
```
### useFavicon 更改网站的favicon
- 参数
  - url:string 初始化favicon
- @return
  - url:string 现在的favicon
  - update:(newUrl:string)=>void 更新
```tsx
const [url,update] = useFavicon('xx')
  return <>
      <div onClick={update.bind(null,'xxx')}>update</div>
  </>
```
### useTitle 修改修改title
```tsx
useTitle('shooks')
```

### useDebounceValue 值防抖
- 数值改变后nms后响应
- 初始化参数
  - 初始化的值
  - timeout:number,防抖数值 默认1000ms
- @return
  - value:最新的值

```tsx
  const [num,setNum] = useState(1)
  const value = useDebounceValue(num)

  function add(){
      setNum(num+1)
  }
  return <>
      <div onClick={add}>{num}</div>
      <div>{value}</div>
  </>
```
### useThrottleValue 值节流
> useDebounceValue,获得节流效果
### useDebounce 防抖
- 初始化参数
  - 初始化的值
  - timeout:number,防抖数值 默认1000ms
- @return
  - value:最新的值
    - fn:设置最新的结果
      - fn(value)
```tsx
  const [num,setNum] = useDebounce(1)
  const [age,setAge] = useDebounce(1,10)
  function add(){
      setNum(num+1)
      setAge(age+1)
  }
  return <>
      <div onClick={add}>{num}</div>
      <div>{age}</div>
  </>
```
### useThrottle 节流
> 用法同useDebounce,获得节流效果

### useLocalStorage
- 参数
  - key:string 必填 
  - options 选填
    - defaultValue:初始值 选填
    - force:boolean;覆盖老数据,如果没有defaultValue,则设置local为'''
```tsx
const [data,{set,reset}] = useLocalStorage('store',{
    defaultValue:{
        age:10,
    },
    force:true
})
const [name,upName] = useLocalStorage('reset',{
    force:true
})

function add(){
    set({
        age:data.age+1
    })
    upName.set('reset')
}


return <>
    <div onClick={add}>{data.age}</div>
    <div onClick={reset}>{name}</div>
    </>
```
### useSessionStorage
> 同 `useLocalStorage`

### useMap
```
import { useMap } from 'shooks'

const [value, { set, remove, clear, reset, has, size }] = useMap([
    ["a", "b"],
]);
```
### useSet
```
import { useSet } from 'shooks'

const [value, { set, remove, clear, reset, has, size }] = useSet(1);
```

### useBoolean
- defaultValue:false
- toggle:切换
- setTrue:设为true
- setFalse:设为false
```tsx
const [data, {toggle,setTrue,setFalse}] = useBoolean(true)
```