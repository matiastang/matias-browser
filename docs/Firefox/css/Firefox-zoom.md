<!--
 * @Author: tangdaoyong
 * @Date: 2021-04-14 14:38:37
 * @LastEditors: tangdaoyong
 * @LastEditTime: 2021-04-14 14:46:01
 * @Description: Firefox-zoom
-->
# Firefox-zoom

`Firefox`浏览器不支持`zoom`。

判断浏览器是否支持摸个属性如下：
```js
/**
 * 是否是支持摸个Css属性
 */
supportCss3(style) { 
    var prefix = ['webkit', 'Moz', 'ms', 'o'],
        i, 
        humpString = [], 
        htmlStyle = document.documentElement.style, 
        _toHumb = function(string) { 
            return string.replace(/-(\w)/g, function($0, $1) { 
                return $1.toUpperCase(); 
            }); 
        }; 
        
    for (i in prefix) {
        humpString.push(_toHumb(prefix[i] + '-' + style));
    } 
        
    humpString.push(_toHumb(style)); 
        
    for (i in humpString) {
        if (humpString[i] in htmlStyle) {
            return true;
        }
    }
        
    return false; 
}
```
**注意**：如果支持的话, 会输出 ""，如果不支持的话, 会输出 undefined，新版本的浏览器不用判断前缀了, 老版本的浏览器还是需要判断前缀。
当浏览器不支持`zoom`时，可以使用`transform: scale(zoom)`代替，但需要注意的是，`transform`不会改变实际的大小，而`zoom`会改变组件的大小为缩放之后的。所以需要特殊处理实际的大小，可以使用`margin`为负数来达到效果。如下：
```jsx
/**
 * 获取显示尺寸及缩放
 * @param {string} style 
 * @returns 
 */
getZoomStyle(style) {
    // 判断是否支持zoom属性
    if (this.supportCss3('zoom')) {
        return Object.assign({
            zoom: 270 / style.width
        }, style);
    }
    let zoom = 270 / style.width;
    let widthValue = style.width * (1 - zoom) / 2;
    let heightValue = style.height * (1 - zoom) / 2;
    return Object.assign({
        marginTop: `-${heightValue}px`,
        marginBottom: `-${heightValue}px`,
        marginLeft: `-${widthValue}px`,
        transform: `scale(${zoom})`
    }, style);
}
```