### 限制输入框只能输入正整数
```html
    function positiveInteger(obj) {
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
/* 限制输入框只能输入正整数 */
function posit(obj) {
    //禁止录入整数部分两位以上，但首位为0
    obj.value = obj.value.replace(/^([1-9]\d*(\.[\d]{0,2})?|0(\.[\d]{0,2})?)[\d.]*/g, "$1");
    //输入0-9的整数，其他的除外
    obj.value = obj.value.replace(/[^0-9]/g, '')
}
```