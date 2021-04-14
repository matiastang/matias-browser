<!--
 * @Author: tangdaoyong
 * @Date: 2021-04-14 13:46:45
 * @LastEditors: tangdaoyong
 * @LastEditTime: 2021-04-14 13:49:38
 * @Description: IE-input-readonly失效
-->
# IE-input-readonly失效

`readonly`可以让`input`只读，在谷歌上没问题。但`IE`浏览器不行。
```html
<input class="name "  readonly/>
```
`IE`浏览器需要加上`unselectable='on'`。
```html
<input class="name "  readonly  unselectable='on' />
```