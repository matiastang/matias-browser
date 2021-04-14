<!--
 * @Author: tangdaoyong
 * @Date: 2021-04-14 11:57:33
 * @LastEditors: tangdaoyong
 * @LastEditTime: 2021-04-14 12:07:33
 * @Description: IE样式::after兼容
-->
# IE样式::after兼容

最近使用`React`开发用到`Antd`组件库，希望修改无数据时的样式。`scss`样式如下：

```scss
:global {
        // 接管空列表显示
        .ant-empty {
            padding: 48px 0;
            border-bottom: 1px solid #e8e8e8;
            @include flexCenter;
            &::after {
                content: '';
                width: 107px;
                height: 118px;
                background: url(./images/empty.png);
            }

            .ant-empty-image {
                display: none;
            }

            .ant-empty-description {
                display: none;
            }
        }

    }
```
通过`display: none`隐藏默认的无数据样式，`::after`设置自定义无数据样式。
结果发现：
1. 谷歌浏览器正常。
2. `IE`浏览器，`IE 11`也能正常显示。但是调试工具中显示的`::after`设置自定义样式，显示是无效的（横线画掉了），可界面却正常显示。
3. `IE`浏览器，`IE Edge`不能正常显示。

最后修改默认样式的容器，然后隐藏默认`svg`图片处理了该问题。

```scss
:global {
        // 接管空列表显示
        .ant-empty {
            padding: 48px 0;
            border-bottom: 1px solid #e8e8e8;
            @include flexCenter;

            .ant-empty-image {
                width: 107px;
                height: 118px;
                background: url(./images/empty.png);
                > svg {
                    display: none;
                }
            }

            .ant-empty-description {
                display: none;
            }
        }

    }
```
可能`::before`也有该兼容性问题。