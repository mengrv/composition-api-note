### setup（new）



在组件内使用composition-api的入口方法，调用时间早于`befroeCreated`，对标vue2.x的`befroeCreated`和created（`befroeCreated`和`created`在3.x中被移除，使用`setup`替代）。



`setup`中如果返回`object`，那么`object`中的`properties`都将被合到vue实例`context`上。`setup`除了可以放回`object`外还可以返回`render`函数或者`jsx`来替代`template`。



`setup`拥有两个参数，第一个参数为`props`，`props`的定义方式和2.x基本一致，`props`值的变化是可以被即时更新和被`watchEffect`和`watch`监测到的。但是由于`props`是`reactive`的，所以如果需要在`tempalte`中使用并且即时更新，需要配合`toRef`或者`toRefs`来转换一下。

```js
export default {
  props: {
    name: String
  },
  setup(props) {
    watchEffect(() => {
      console.log(`name is: ` + props.name)
    })
  }
}
```

使用`props`的时候，不要尝试解构它，开发环境下会有提示。

```js
export default {
  props: {
    name: String
  },
  setup({ name }) {
    watchEffect(() => {
      console.log(`name is: ` + name) // Will not be reactive!
    })
  }
}
```



`setup`的第二个参数为`context`是一个包含了`emit`、`attr`、`slot`等的对象。

`setup`使用`this`时，需要注意的是`this`的指向不再是vue实例。



`setup`的类型定义

```js
interface Data {
  [key: string]: unknown
}

interface SetupContext {
  attrs: Data
  slots: Slots
  emit: (event: string, ...args: unknown[]) => void
}

function setup(props: Data, context: SetupContext): Data
```



### 总结

1. `befroeCreated`和`created`在3.x中被移除，使用`setup`替代。
2. `setup`可返回`object`、`render function`、`jsx`或者不返回任何内容。
3. `setup`拥有两个参数`props`、`context`。
4. 不要尝试解构`props`会破坏`reactive`，`props`如果需要在`template`中即时更新，需要配合`toRef`、`toRefs`使用。
5. `context`中包含了`emit`、`attr`、`slot`等，一定程度下相当于2.x的`this`。
6. `setup`中的`this`不再是2.x中的`this`。

