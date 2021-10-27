**优雅更新props**
在子组件中不允许直接修改 props，因为这种做法不符合单向数据流的原则，在开发模式下还会报出警告。因此大多数人会通过 $emit 触发自定义事件，在父组件中接收该事件的传值来更新 prop。

```
        //child.vue
export defalut {
    props: {
        title: String
    },
    methods: {
        changeTitle(){
            this.$emit('change-title', 'hello')
        }
    }
}

```

```
        //parent.vue
        <child :title="title" @change-title="changeTitle"></child>

export default {
    data(){
        return {
            title: 'title'
        }
    },
    methods: {
        changeTitle(title){
            this.title = title
        }
    }
}

```
这种做法没有问题，就是单纯更新 props。没有其他操作。那么使用 .sync修饰符让做法变得简单

```
<child :title.sync="title"></child>

export defalut {
    props: {
        title: String
    },
    methods: {
        changeTitle(){
            this.$emit('update:title', 'hello')
        }
    }
}


```
只需要在绑定的属性上面添加 .sync 在子组件内部就可以触发 update:属性名 来更新 prop。可以看到这种手段确实简洁且优雅。TIPS: 但是据说在快要到来的vue3中将**不再支持** .sync修饰符。但是还有一个细节性的东西 **  this.$emit('update:title', 'hello')** update的 : 和title之间不能存在空格。