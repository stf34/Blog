### 限制输入框只能输入正整数
```html
    <input maxlength="3" style="width:70px;height:32px;" type="text" id="btStime" placeholder="限制输入框只能输入正整数" oninput="positive(this)">


```

> js逻辑

```js
    function positive(obj) {
        if(obj.value.length == 1) {
            obj.value = obj.value.replace(/[^1-9]/g, '')
        } else {
            obj.value = obj.value.replace(/\D/g, '')
        }
        return obj.value;
    }
```
### 输入整数并保留两位小数
```html
    <input maxlength="3" style="width:70px;height:32px;" type="text" id="btStime" placeholder="输入整数并保留两位小数" oninput="inpnumers(this)">

```
> js逻辑

```js
    /* 价格限制 */
    function inpnumers(obj) {
      //禁止录入整数部分两位以上，但首位为0
      obj.value = obj.value.replace(/^([1-9]\d*(\.[\d]{0,2})?|0(\.[\d]{0,2})?)[\d.]*/g, "$1");
      //先把非数字的都替换掉，除了数字和.
      obj.value = obj.value.replace(/[^\d\.]/g, "");
      //必须保证第一个为数字而不是.
      obj.value = obj.value.replace(/^\./g, "0.");
      //保证只有出现一个.而没有多个.
      obj.value = obj.value.replace(/\.{2,}/g, ".");
      //保证.只出现一次，而不能出现两次以上
      obj.value = obj.value.replace(".", "$#$").replace(/\./g, "").replace("$#$", ".");
      //只能输入两个小数
      obj.value = obj.value.replace(/^(\-)*(\d+)\.(\d\d).*$/, "$1$2.$3");
      return obj.value;
    }
```
### input只能输入正整数，且第一个不能为0，不能输入小数点

```html
    <input maxlength="3" style="width:70px;height:32px;" type="text" id="btStime" placeholder="只能输入正整数，且第一个不能为0，不能输入小数点" oninput="posit(this)">
```

> js逻辑

```js
    /* 限制输入框只能输入正整数 */
    function posit(obj) {
        //禁止录入整数部分两位以上，但首位为0
        obj.value = obj.value.replace(/^([1-9]\d*(\.[\d]{0,2})?|0(\.[\d]{0,2})?)[\d.]*/g, "$1");
        //输入0-9的整数，其他的除外
        obj.value = obj.value.replace(/[^0-9]/g, '')
    }
```

### input验证在文本框输入的只能是数字和字母

```html
    <input
        id="amount"
        maxlength="12"
        type="text"
        placeholder="请输入12位激活密码"
        style="width:70px;height:32px;"
        oninput="amountInit(this)"
    />
```

> js逻辑

```js
    /* 限制输入框只能是数字和字母 */
    function amountInit(obj) {
        //输入数字和字母，其他的除外
        obj.value = obj.value.replace(/[\W]/g, "")
    }
```