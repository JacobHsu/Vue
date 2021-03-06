---
title: Watch
---

`Watch` 可讓我們監視某個值，當這個值變動的時候，就去做某些事情。

例，如果 userName 發生變化，就去執行 _valid 這個 method。使用者輸入了合法的字串。

https://jsbin.com/zesijokaju/edit?html,output

```html
 <div id="app">
    <p>請輸入使用者名稱。</p>
    <input type="text" v-model="userName">
    <p>${ errMsg }</p>
  </div>
```

```js
<script>
    var vm = new Vue({
    el: '#app',
    delimiters: ['${', '}'],
    data: {
    userName: '',
    errMsg: ''
    },
    watch: {
    userName: function(value) {
        if(this._valid(value)) {
        this.errMsg = '使用者名稱開頭不可為數字。'
        } else {
        this.errMsg = '合法的使用者名稱。';
        }
    }
    },
    methods: {
    _valid: function(name) {
        return /^[0-9]/.test(name);
    }
    }
});
</script>
```

Vue 提供了一種更通用的方式來觀察和響應 Vue 實例上的數據變動：`偵聽屬性`。當你有一些數據需要隨著其它數據變動而變動時，你很容易濫用 watch——特別是如果你之前使用過 AngularJS。然而，通常更好的做法是使用`計算屬性`而不是命令式的 watch 回調。

雖然計算屬性在大多數情況下更合適，但有時也需要一個自定義的偵聽器。這就是為什麼 Vue 通過 watch 選項提供了一個更通用的方法，來響應數據的變化。當需要**在數據變化時執行異步或開銷較大的操作**時，這個方式是最有用的。


### computed和watch用法異同

相同：`computed`和`watch`都起到監聽/依賴一個數據，並進行處理的作用

異同：它們其實都是vue對監聽器的實現，只不過
`computed`主要用於對同步數據的處理，  
`watch`則主要用於觀測某個值的變化去完成一段開銷較大的複雜業務邏輯。  

能用computed的時候優先用computed，避免了多個數據影響其中某個數據時多次調用watch的尷尬情況。


## watch 的高级用法

[Vue.js中 watch 的高级用法](https://juejin.im/post/5ae91fa76fb9a07aa7677543)

handler方法和immediate属性

```js
watch: {
  firstName: {
    handler(newName, oldName) {
      this.fullName = newName + ' ' + this.lastName;
    },
    // 代表在wacth里声明了firstName这个方法之后立即先去执行handler方法
    immediate: true
  }
}
```

watch 里面还有一个属性 deep，默认值是 false，代表是否深度监听，
如果我们需要监听obj里的属性a的值呢？这时候deep属性就派上用场了
deep的意思就是深入观察，监听器会一层层的往下遍历，给对象的所有属性都加上这个监听器，但是这样性能开销就会非常大了，任何修改obj里面任何一个属性都会触发这个监听器里的 handler。
优化，我们可以是使用字符串形式监听。

```js
watch: {
  'obj.a': {
    handler(newName, oldName) {
      console.log('obj.a changed');
    },
    immediate: true,
    // deep: true
  }
} 
```

#### References

[Vue.js: Watch](https://cythilya.github.io/2017/04/15/vue-watch/)  
[Computed Properties and Watchers](https://vuejs.org/v2/guide/computed.html)  