# v-module的使用技巧	

**v-module解析**

```
<input v-model="sth" />
<input v-bind:value="sth" v-on:input="sth = $event.target.value" />
```

所以我们在使用组件的时候应该也是遵循这个规则

1. 先定义父组件的v-module
2. 使用defineProps接收modelValue
3. defineEmits(['update:modelValue']）触发这个值
4. 通过change变化的值把数据传递出去（update:modelValue这个传递的值就是父组件绑定的值）

父组件

```
<Parent v-model="parentData"></Parent>
```

子组件

```
<input v-module='son' @input="changeHandler"><input/>

const prop = defineProps({
  modelValue: String,
});
const emits = defineEmits(['update:modelValue']);

const changeHandler = () => {
  emits('update:modelValue', files.value);
};
```

总结：v-module 可以一直嵌套 每次去更新update:modelValue的数据就行