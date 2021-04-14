<!--
 * @Author: tangdaoyong
 * @Date: 2021-04-14 13:34:45
 * @LastEditors: tangdaoyong
 * @LastEditTime: 2021-04-14 13:42:23
 * @Description: IE input file 兼容
-->
# IE input file 兼容

```jsx

/*
* 选择文件回调
* @params e { Obj } 事件对象
* */
selectFile = (e) => {
    if (e.target.files.length <= 0) {
        /*
        * IE浏览器this.fileInput.value = '';之后再次调用onchange事件处理
        */
        return;
    }
    // 清空原来选中的文件
    this.allFiles = {};
    let files = [];
    const { change, isImmediate } = this.props;
    let fileName = '';
    let fileAccept = '';
    if (this.isIE9Version) { // ie中获取文件，ie中只能单个选择文件，不能一次选择多个
        let file = e.target;
        file.select();
        file.blur();
        let path = document.selection.createRange().text.split('\\');
        fileName = [path[path.length - 1]];
        fileAccept = this.filterFile(fileName);
    } else {
        files = [...e.target.files];
        fileAccept = this.filterFile(files);
    }

    if (!fileAccept) { // 判断上传文件与规定的文件是否是同类型
        files.map(item => this.allFiles[item.name] = item);// 去重处理
        let filesPara = this.isIE9Version ? fileName : Object.keys(this.allFiles);
        change && change(filesPara);
        isImmediate && this.submit1();// 用户选择文件 立即上传
    } else {
        this.props.error();
    }
    this.fileInput.value = '';
}
<input
    type="file"
    name="file"
    accept={accept}
    multiple={multiple}
    className="upload_input"
    onChange={this.selectFile}
    ref={(item) => this.fileInput = item}
/>
```
`input`为`type="file"`时，选择了文件需要设置`this.fileInput.value = ''`，清空选择。
谷歌等浏览器正常，但`IE`浏览器，在使用了`this.fileInput.value = ''`之后会再次调用`onChange`事件。
为此，我们通过：
```js
if (e.target.files.length <= 0) {
    /*
    * IE浏览器this.fileInput.value = '';之后再次调用onchange事件处理
    */
    return;
}
```
来判断是否选择了文件，丢弃掉`IE`浏览器因`this.fileInput.value = ''`导致的触发。