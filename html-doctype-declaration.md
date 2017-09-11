<style>
  td .marked {
    color: #e80000;
  }
</style>

## HTML `<!DOCTYPE>` 声明

### 定义和用法

`<!DOCTYPE>` 声明必须位于 HTML 文档中最开始的位置，在 `<html>` 标签之前。`<!DOCTYPE>` 声明并不是一个 HTML 标签，它告诉 Web 浏览器这个页面使用的 HTML 的版本。

在 HTML 4.01 中，`<!DOCTYPE>` 声明必须提供一个 DTD，因为 HTML 4.01 是基于 SGML 的。这个 DTD 文件确定了标记语言的规则，这样浏览器才能正确地将内容渲染出来。HTML5 并不基于 SGML，所以也就不需要引用 DTD。

**提示：**总是在你的 HTML 文档中添加 `<!DOCTYPE>` 声明，这样浏览器才能知道这是一个怎样的文档。

### 浏览器支持

所有的主流浏览器都支持 `<!DOCTYPE>`。

### HTML 4.01 和 HTML 5 的差别

在 HTML 4.01 中有 3 中不同的 `<!DOCTYPE>` 声明，在 HTML 5 中只有一中。

```html
<!DOCTYPE html>
```

### HTML 元素和文档声明

下面这张表中显示了不同类型的文档声明中可以出现的文素。

<table>
  <tbody>
    <tr>
      <th rowspan="2" style="background-color:#ffffff;">Tag</th>
      <th rowspan="2" style="background-color:#ffffff;">HTML5</th>
      <th colspan="3" style="text-align:center;">HTML 4.01 / XHTML 1.0</th>
      <th rowspan="2" style="background-color:#ffffff;">XHTML 1.1</th>
    </tr>
    <tr>
      <th style="width:15%">Transitional</th>
      <th style="width:15%">Strict</th>
      <th style="width:15%">Frameset</th>
    </tr>
    <tr>
      <td>&lt;a&gt;</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
    </tr>
    <tr>
      <td>&lt;abbr&gt;</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
    </tr>
    <tr>
      <td>&lt;acronym&gt;</td>
      <td><span class="marked">No</span></td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
    </tr>
    <tr>
      <td>&lt;address&gt;</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
    </tr>
    <tr>
      <td>&lt;applet&gt;</td>
      <td><span class="marked">No</span></td>
      <td>Yes</td>
      <td><span class="marked">No</span></td>
      <td>Yes</td>
      <td><span class="marked">No</span></td>
    </tr>
    <tr>
      <td>&lt;area&gt;</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td><span class="marked">No</span></td>
    </tr>
    <tr>
      <td>&lt;article&gt;</td>
      <td>Yes</td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
    </tr>
    <tr>
      <td>&lt;aside&gt;</td>
      <td>Yes</td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
    </tr>
    <tr>
      <td>&lt;audio&gt;</td>
      <td>Yes</td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
    </tr>
    <tr>
      <td>&lt;b&gt;</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
    </tr>
    <tr>
      <td>&lt;base&gt;</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
    </tr>
    <tr>
      <td>&lt;basefont&gt;</td>
      <td><span class="marked">No</span></td>
      <td>Yes</td>
      <td><span class="marked">No</span></td>
      <td>Yes</td>
      <td><span class="marked">No</span></td>
    </tr>
    <tr>
      <td>&lt;bdi&gt;</td>
      <td>Yes</td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
    </tr>
    <tr>
      <td>&lt;bdo&gt;</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td><span class="marked">No</span></td>
    </tr>
    <tr>
      <td>&lt;big&gt;</td>
      <td><span class="marked">No</span></td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
    </tr>
    <tr>
      <td>&lt;blockquote&gt;</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
    </tr>
    <tr>
      <td>&lt;body&gt;</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
    </tr>
    <tr>
      <td>&lt;br&gt;</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
    </tr>
    <tr>
      <td>&lt;button&gt;</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
    </tr>
    <tr>
      <td>&lt;canvas&gt;</td>
      <td>Yes</td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
    </tr>
    <tr>
      <td>&lt;caption&gt;</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
    </tr>
    <tr>
      <td>&lt;center&gt;</td>
      <td><span class="marked">No</span></td>
      <td>Yes</td>
      <td><span class="marked">No</span></td>
      <td>Yes</td>
      <td><span class="marked">No</span></td>
    </tr>
    <tr>
      <td>&lt;cite&gt;</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
    </tr>
    <tr>
      <td>&lt;code&gt;</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
    </tr>
    <tr>
      <td>&lt;col&gt;</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td><span class="marked">No</span></td>
    </tr>
    <tr>
      <td>&lt;colgroup&gt;</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td><span class="marked">No</span></td>
    </tr>
    <tr>
      <td>&lt;datalist&gt;</td>
      <td>Yes</td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
    </tr>
    <tr>
      <td>&lt;dd&gt;</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
    </tr>
    <tr>
      <td>&lt;del&gt;</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td><span class="marked">No</span></td>
    </tr>
    <tr>
      <td>&lt;details&gt;</td>
      <td>Yes</td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
    </tr>
    <tr>
      <td>&lt;dfn&gt;</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
    </tr>
    <tr>
      <td>&lt;dialog&gt;</td>
      <td>Yes</td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
    </tr>
    <tr>
      <td>&lt;dir&gt;</td>
      <td><span class="marked">No</span></td>
      <td>Yes</td>
      <td><span class="marked">No</span></td>
      <td>Yes</td>
      <td><span class="marked">No</span></td>
    </tr>
    <tr>
      <td>&lt;div&gt;</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
    </tr>
    <tr>
      <td>&lt;dl&gt;</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
    </tr>
    <tr>
      <td>&lt;dt&gt;</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
    </tr>
    <tr>
      <td>&lt;em&gt;</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
    </tr>
    <tr>
      <td>&lt;embed&gt;</td>
      <td>Yes</td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
    </tr>
    <tr>
      <td>&lt;fieldset&gt;</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
    </tr>
    <tr>
      <td>&lt;figcaption&gt;</td>
      <td>Yes</td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
    </tr>
    <tr>
      <td>&lt;figure&gt;</td>
      <td>Yes</td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
    </tr>
    <tr>
      <td>&lt;font&gt;</td>
      <td><span class="marked">No</span></td>
      <td>Yes</td>
      <td><span class="marked">No</span></td>
      <td>Yes</td>
      <td><span class="marked">No</span></td>
    </tr>
    <tr>
      <td>&lt;footer&gt;</td>
      <td>Yes</td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
    </tr>
    <tr>
      <td>&lt;form&gt;</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
    </tr>
    <tr>
      <td>&lt;frame&gt;</td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
      <td>Yes</td>
      <td><span class="marked">No</span></td>
    </tr>
    <tr>
      <td>&lt;frameset&gt;</td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
      <td>Yes</td>
      <td><span class="marked">No</span></td>
    </tr>
    <tr>
      <td>&lt;h1&gt; to &lt;h6&gt;</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
    </tr>
    <tr>
      <td>&lt;head&gt;</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
    </tr>
    <tr>
      <td>&lt;header&gt;</td>
      <td>Yes</td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
    </tr>
    <tr>
      <td>&lt;hr&gt;</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
    </tr>
    <tr>
      <td>&lt;html&gt;</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
    </tr>
    <tr>
      <td>&lt;i&gt;</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
    </tr>
    <tr>
      <td>&lt;iframe&gt;</td>
      <td>Yes</td>
      <td>Yes</td>
      <td><span class="marked">No</span></td>
      <td>Yes</td>
      <td><span class="marked">No</span></td>
    </tr>
    <tr>
      <td>&lt;img&gt;</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
    </tr>
    <tr>
      <td>&lt;input&gt;</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
    </tr>
    <tr>
      <td>&lt;ins&gt;</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td><span class="marked">No</span></td>
    </tr>
    <tr>
      <td>&lt;kbd&gt;</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
    </tr>
    <tr>
      <td>&lt;keygen&gt;</td>
      <td>Yes</td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
    </tr>
    <tr>
      <td>&lt;label&gt;</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
    </tr>
    <tr>
      <td>&lt;legend&gt;</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
    </tr>
    <tr>
      <td>&lt;li&gt;</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
    </tr>
    <tr>
      <td>&lt;link&gt;</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
    </tr>
    <tr>
      <td>&lt;main&gt;</td>
      <td>Yes</td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
    </tr>
    <tr>
      <td>&lt;map&gt;</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td><span class="marked">No</span></td>
    </tr>
    <tr>
      <td>&lt;mark&gt;</td>
      <td>Yes</td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
    </tr>
    <tr>
      <td>&lt;menu&gt;</td>
      <td>Yes</td>
      <td>Yes</td>
      <td><span class="marked">No</span></td>
      <td>Yes</td>
      <td><span class="marked">No</span></td>
    </tr>
    <tr>
      <td>&lt;menuitem&gt;</td>
      <td>Yes</td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
    </tr>
    <tr>
      <td>&lt;meta&gt;</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
    </tr>
    <tr>
      <td>&lt;meter&gt;</td>
      <td>Yes</td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
    </tr>
    <tr>
      <td>&lt;nav&gt;</td>
      <td>Yes</td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
    </tr>
    <tr>
      <td>&lt;noframes&gt;</td>
      <td><span class="marked">No</span></td>
      <td>Yes</td>
      <td><span class="marked">No</span></td>
      <td>Yes</td>
      <td><span class="marked">No</span></td>
    </tr>
    <tr>
      <td>&lt;noscript&gt;</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
    </tr>
    <tr>
      <td>&lt;object&gt;</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
    </tr>
    <tr>
      <td>&lt;ol&gt;</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
    </tr>
    <tr>
      <td>&lt;optgroup&gt;</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
    </tr>
    <tr>
      <td>&lt;option&gt;</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
    </tr>
    <tr>
      <td>&lt;output&gt;</td>
      <td>Yes</td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
    </tr>
    <tr>
      <td>&lt;p&gt;</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
    </tr>
    <tr>
      <td>&lt;param&gt;</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
    </tr>
    <tr>
      <td>&lt;pre&gt;</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
    </tr>
    <tr>
      <td>&lt;progress&gt;</td>
      <td>Yes</td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
    </tr>
    <tr>
      <td>&lt;q&gt;</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
    </tr>
    <tr>
      <td>&lt;rp&gt;</td>
      <td>Yes</td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
    </tr>
    <tr>
      <td>&lt;rt&gt;</td>
      <td>Yes</td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
    </tr>
    <tr>
      <td>&lt;ruby&gt;</td>
      <td>Yes</td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
    </tr>
    <tr>
      <td>&lt;s&gt;</td>
      <td>Yes</td>
      <td>Yes</td>
      <td><span class="marked">No</span></td>
      <td>Yes</td>
      <td><span class="marked">No</span></td>
    </tr>
    <tr>
      <td>&lt;samp&gt;</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
    </tr>
    <tr>
      <td>&lt;script&gt;</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
    </tr>
    <tr>
      <td>&lt;section&gt;</td>
      <td>Yes</td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
    </tr>
    <tr>
      <td>&lt;select&gt;</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
    </tr>
    <tr>
      <td>&lt;small&gt;</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
    </tr>
    <tr>
      <td>&lt;source&gt;</td>
      <td>Yes</td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
    </tr>
    <tr>
      <td>&lt;span&gt;</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
    </tr>
    <tr>
      <td>&lt;strike&gt;</td>
      <td><span class="marked">No</span></td>
      <td>Yes</td>
      <td><span class="marked">No</span></td>
      <td>Yes</td>
      <td><span class="marked">No</span></td>
    </tr>
    <tr>
      <td>&lt;strong&gt;</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
    </tr>
    <tr>
      <td>&lt;style&gt;</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
    </tr>
    <tr>
      <td>&lt;sub&gt;</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
    </tr>
    <tr>
      <td>&lt;summary&gt;</td>
      <td>Yes</td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
    </tr>
    <tr>
      <td>&lt;sup&gt;</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
    </tr>
    <tr>
      <td>&lt;table&gt;</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
    </tr>
    <tr>
      <td>&lt;tbody&gt;</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td><span class="marked">No</span></td>
    </tr>
    <tr>
      <td>&lt;td&gt;</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
    </tr>
    <tr>
      <td>&lt;textarea&gt;</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
    </tr>
    <tr>
      <td>&lt;tfoot&gt;</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td><span class="marked">No</span></td>
    </tr>
    <tr>
      <td>&lt;th&gt;</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
    </tr>
    <tr>
      <td>&lt;thead&gt;</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td><span class="marked">No</span></td>
    </tr>
    <tr>
      <td>&lt;time&gt;</td>
      <td>Yes</td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
    </tr>
    <tr>
      <td>&lt;title&gt;</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
    </tr>
    <tr>
      <td>&lt;tr&gt;</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
    </tr>
    <tr>
      <td>&lt;track&gt;</td>
      <td>Yes</td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
    </tr>
    <tr>
      <td>&lt;tt&gt;</td>
      <td><span class="marked">No</span></td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
    </tr>
    <tr>
      <td>&lt;u&gt;</td>
      <td>Yes</td>
      <td>Yes</td>
      <td><span class="marked">No</span></td>
      <td>Yes</td>
      <td><span class="marked">No</span></td>
    </tr>
    <tr>
      <td>&lt;ul&gt;</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
    </tr>
    <tr>
      <td>&lt;var&gt;</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
    </tr>
    <tr>
      <td>&lt;video&gt;</td>
      <td>Yes</td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
    </tr>
    <tr>
      <td>&lt;wbr&gt;</td>
      <td>Yes</td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
      <td><span class="marked">No</span></td>
    </tr>
  </tbody>
</table>

### 提示和注意

**提示：**`<!DOCTYPE>` 声明对大小写不敏感。
**提示：**如果想要检查你的 Web 文档是否是有效的，前往 [W3C 验证服务](http://validator.w3.org/)。

### 常用的文档类型声明

#### HTML 5

```html
<!DOCTYPE html>
```

#### HTML 4.01 Strict

这个 DTD 文件中包含了所有的 HTML 元素和属性，但是不包括描述性或弃用的元素（比如 `font`），同时 Framesets 也是不被允许的。

```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">
```

#### HTML 4.01 Transitional

这个 DTD 文件中包含了所有的 HTML 元素和属性，也包括描述性或弃用的元素（比如 `font`），但是 Framesets 也是不被允许的。

```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
```

#### HTML 4.01 Frameset

这个 DTD 文件与 HTML 4.01 Transitional 相同，但是允许使用 Frameset。

```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Frameset//EN" "http://www.w3.org/TR/html4/frameset.dtd">
```

#### XHTML 1.0 Strict

这个 DTD 文件中包含了所有的 HTML 元素和属性，但是不包括描述性或弃用的元素（比如 `font`），同时 Framesets 也是不被允许的。所有的标签都必须以规格化的 XML 格式书写。

```html
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
```

#### XHTML 1.0 Transitional

这个 DTD 文件中包含了所有的 HTML 元素和属性，也包括描述性或弃用的元素（比如 `font`），但是 Framesets 也是不被允许的。所有的标签都必须以规格化的 XML 格式书写。

```html
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
```

#### XHTML 1.0 Frameset

这个 DTD 文件与 XHTML 1.0 Transitional 相同，但是允许使用 Frameset。所有的标签都必须以规格化的 XML 格式书写。
```html
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Frameset//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-frameset.dtd">
```

#### XHTML 1.1

这个 DTD 文件与 XHTML 1.0 Strict 相同，但是允许你添加模型。所有的标签都必须以规格化的 XML 格式书写。

```html
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.1//EN" "http://www.w3.org/TR/xhtml11/DTD/xhtml11.dtd">
```
