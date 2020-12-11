## 下拉框操作和选中
```html
    <select id="dome">
        <option value="0" >选项一</option>
        <option value="1">选项二</option>
    </select>
```
> js逻辑,前提引入jQuery
```js
    $(function () {

        $('#dome').val(1)//动态控制下拉框选中项


        $(`#dome`).change(function () {
            let val = $(this).val()
            console.log(val);
        })
    })
```