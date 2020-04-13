##### redux的逻辑，数据流单项循坏

###### store(存放状态)--->container(显示状态)--->reducer(处理动作)--->store

1. store是应用状态管理中心，保存着应用的state,状态更新时，触发视觉组件进行更新
2. container是视觉组件的容器，负责把传入的状态变量渲染成视觉组件，在浏览器显示出来
3. reducer是动作(action)的处理中心，负责处理各种动作并产生新的状态(state),返回给store

##### 一般的相关src目录

``` 
container 存放容器组件
components 存放普通显示组件
actions 存放可以发出的action
reducers 存放action的处理器reducers
services 存放经过封装的服务，如ajax,模拟数据服务
styles 存放应用的样式，如css,scss
images 存放图片资源
api 存放开发接口文档
```

Flux Standard Action---只允许多加3个属性:payload,error,meta

```
let FSA={
  type:"EAT_APPLE",
  payload:3,//负载是3，这里可以写成{id:3}
  meta:'this action'
}
一个action只是一个对象，需要根据时机被store的dispatch函数调用才会进行处理
```

广义的action是在中间件的支持下，dispatch函数可以调用的数据类型，常见的有thunk,promise

``` 
let pickAAction=(dispatch,getState)=>
{
  ajax({
    url:'/pick',
    method:'GET'
  }).done(data=>
  {
    //发射普通action
    dispatch({
      type:'DONE_PICK_APPLE',
      payload:data.weight //或者payload:{weight:data.weight}
    })
  }).fail(xhr=>
  {
    //发射普通action,其负载是一个error
    dispatch({
      type:'FAIL_PICK_APPLE',
      payload:new Error(xhr.responseText),
      error:true
    })
  })
}
定义好之后，我们可以直接调用这个thunk    dispatch(pickAAction)
比较简单的action
let eatA=(id)=>({
  type:'EAT_A',
  payload:id
})
调用---dispatch(eatA(3))
```

###### 创建action文件

``` 
const prefix='apple/'----命名空间

let actions=
{
  eatA:(id)=>({
    type:'apple/EAT_A',
    payload:id
  })
}
export default actions
调用：actions={{eatA:id=>dispatch(actions.eatApple(id))}}
```

<https://segmentfault.com/a/1190000005356568>