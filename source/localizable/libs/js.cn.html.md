---
layout: libs.cn
section: js
toc: article
title: Javascript 库
---

## 总述

Javascript 库为网站的搜索框提供搜索功能。直接使用该库能帮助开发者方便快捷地使用微搜索的各项功能。所有源码托管在 GitHub， 开发者们可前往查看 [最新版本][github]。源码同时附带 [例子][examples]，方便用户学习使用。

## 准备

### 依赖

本库分别提供基于 jQuery 和 Zepto 的 TinySou 插件，移动端默认使用基于 Zepto 的轻量级 Javascript 库，PC 端使用基于 jQuery 库的插件。

### `engine_key`

使用 Javascript 库之前，你需要有一个 `engine_key`。

在 TinySou 控制台 Engines-> 概述 里，你可以找到你的 `engine_key`；如果你没有账号，请先前往 [注册][setup]。

## 使用

### 基本用法

 复制粘贴下面的代码到 HTML 中的 `script` 标签中，并填写你的 `engine_key`。简单添加后，输入框将具有搜索功能。

```html
<form><input type='text' id='ts-search-input'></form>
```

```javascript
var option = {
  engineKey: 'YOUT_ENGINE_KEY'
};
(function(w,d,t,u,n,s,e){
  s = d.createElement(t);
  s.src = u;
  s.async = 1;
  w[n] = function(r){
    w[n].opts = r;
  };
  e = d.getElementsByTagName(t)[0];
  e.parentNode.insertBefore(s, e);
})(window,document,'script','//tinysou-cdn.b0.upaiyun.com/ts.js','_ts');
_ts(option);
```

### Option

你可以通过传入特定的 `option` 值，来方便定制搜索行为。下面是默认的 `option` 列表：

参数         | 默认值      | 说明
----------- | ----------- | -----------
searchInputElement | '#ts-search-input' | 指定搜索框。`#ts-search-input` 指定 `id` 为 `ts-search-input` 的输入框。
attachTo | undefined | 自动补全列表默认将位于搜索框的下面。
collection | 'page' | 默认搜索的 `collection` 是 `page`。
filters | undefined | 可以通过 filter 参数来限制搜索的范围。更多内容可以查看 [filter][doc-filter]。
engineKey `必需` | undefined | 搜索引擎的 `engine_key`，需要指定这个 field。
searchFields | undefined | 默认情况下，会对 document 的所有 string 和 text 类型的 field 进行搜索。如果想只搜索特定的 field，可以利用 search_fields 进行指定。例如： ["title", "body"] 表示只搜索每个 document 的 'title’, 'body' 两个 field。field 可以搜索的类型包括：string，text，enum，integer，float。
sort | undefined | 默认情况下，根据搜索结果中每个 document 的 score，对搜索结果进行排序。如果你需要按照特定 field 进行排序，可通过 sort 参数实现。例如，sort 指定为 price，表示按照 'price' 升序(从小到大)排序，当'price'是个array时，取平均值作为排序依据。
fetchFields | ['title', 'url', 'sections'] | 默认情况下，搜索结果中的每个 document，会包含 title，url 和 sections。
renderStyle| undefined | 支持三种显示搜索结果的方式：弹出，嵌入页面和新页面，其对应值为 undefined(未指定)，'inline' 和 'new_page'，默认为弹出方式。
resultPageURL| undefined | 当 renderStyle 为 'new_page' 时，通过 'resultPageURL' 来指定新页面的 URL。注意，此处为页面地址的路径部分，即 URL 的 pathname。例如如果新页面的 URL 为 'http://tinysou.com/result.html'，'resulePageURL' 应设为 '/result.html'。
resultContainingElement| undefined | 搜索结果将显示在其指定的元素中，当 renderStyle 设为 'inline' 或 'new_page' 时，需要指定这个 field。
preRenderFunction| undefined | 指定渲染搜索结果前调用的方法。
postRenderFunction| undefined | 指定渲染搜索结果后调用的方法。
loadingFunction| defaultLoadingFunction | 加载搜索结果时的方法，默认值见下。
renderResultsFunction| defaultRenderResultsFunction | 渲染搜索结果的方法，默认值见下。
renderFunction| defaultRenderFunction | 渲染每条搜索结果的方法。
perPage| 10 | 每页显示搜索结果的数量。
activeItemClass| 'active' | 自动补全列表中，选中某个条目时添加的 CSS class。
onComplete| defaultOnComplete | 点击自动补全结果中的条目后调用的方法，默认值见下。
renderActResultsFunction| defaultRenderActResultsFunction | 渲染自动补全结果的方法，默认值见下。
renderActFunction| defaultRenderActFunction | 渲染每条自动补全条目的方法，默认值见下。
dropdownStylesFunction| defaultDropdownStylesFunction | 指定生成自动补全下拉框样式的方法，默认值见下。
resultLimit| 5 | 自动补全时补全条目的数量。
autocompleteListType| 'ul' | 指定自动补全列表的 HTML 标签，默然为 ul。
autocompleteListClass| 'autocomplete' | 自动补全列表默认的 CSS class 为 'autocomplete', 这将使用 TinySou 提供的补全样式，如果想定义另一套补全样式，可以指定这个 class。
resultListSelector| 'li' | 自动补全结果中每个条目的 HTML 标签。
setWidth| true | 是否按照 `attachTo` 设定的元素来设置补全列表的宽度。
typingDelay| 80 | 搜索框输入搜索内容后，进行自动补全的延迟时间，默认为 80ms。
disableAutocomplete| false | 是否禁用自动补全，默认开启自动补全，值为 true 时禁用自动补全。
autocompleteContainingElement| 'body' | 自动补全部分的 HTML 将默认添加到 body 元素中。

####默认的 JS 方法

* `defaultLoadingFunction`

```javascript
var defaultLoadingFunction = function(query, $resultContainer) {
    $resultContainer.html('<p class="ts-loading-message">loading...</p>');
  };
```

* `defaultRenderResultsFunction`

```javascript
var defaultRenderResultsFunction = function (ctx, data) {
  var $resultContainer = ctx.resultContainer,
    config = ctx.config;

  $resultContainer.html('');

  $.each(data.records, function (idx, item) {
    ctx.registerResult($(config.renderFunction(item)).appendTo($resultContainer), item);
  });

  renderPagination(ctx, data.info);
  if (!config.renderStyle) {
    $('#ts-results-container').appendTo('body').ts_modal({zIndex: 9999});
  } else if (config.renderStyle == 'new_page') {
    var url = config.resultPageURL + window.location.hash;
    window.location.replace(url);
  }
};
```

* `defaultRenderFunction`

```javascript
var defaultRenderFunction = function (item) {
  var title = item['document']['title'];
  var url = item['document']['url'];
  var body = (item.highlight && item.highlight['body']) || item['document']['sections'].join(',');
  return '<div class="ts-result"><h3 class="title"><a href='+ url + ' class="ts-search-result-link">' + title + '</a></h3><div class="ts-metadata"><span class="ts-snippet">' + body + '</span></div></div>';
};
```

* `defaultOnComplete`

```javascript
var defaultOnComplete = function(item, prefix) {
  window.location = item['document']['url'];
};
```

* `defaultRenderActResultsFunction`

```javascript
var defaultRenderActResultsFunction = function(ctx, results) {
  var $list = ctx.list,
    config = ctx.config;

  $.each(results, function(idx, item) {
    ctx.registerActResult($('<li>' + config.renderActFunction(item) + '</li>').appendTo($list), item);
  });
};
```

* `defaultRenderActFunction`

```javascript
var defaultRenderActFunction = function(item) {
  return '<p class="title">' + TinySou.htmlEscape(item['document']['title']) + '</p>';
};
```

* `defaultDropdownStylesFunction`

```javascript
var defaultDropdownStylesFunction = function($this) {
  var config = $this.data('tinysou-config-autocomplete');
  var $attachEl = config.attachTo ? $(config.attachTo) : $this;
  var offset = $attachEl.offset();
  var styles = {
    'position': 'absolute',
    'z-index': 9999,
    'top': offset.top + $attachEl.outerHeight() + 1,
    'left': offset.left
  };

  if (config.setWidth) {
    styles['width'] = $attachEl.outerWidth() - 2;
  }
  return styles;
};
```

## 参与

1. Fork 我们的项目 ( [https://github.com/tinysou/tinysou-search.js/fork][fork] )
2. 创建你的新分支 (`git checkout -b my-new-feature`)
3. 提交你的修改 (`git commit -am 'Add some feature'`)
4. 推送到新分支 (`git push origin my-new-feature`)
5. 创建一个合并请求


[github]:https://github.com/tinysou/tinysou-search.js
[setup]:http://dashboard.tinysou.com/signup
[fork]:https://github.com/tinysou/tinysou-search.js/fork
[examples]:https://github.com/tinysou/tinysou-search.js/tree/master/examples
[doc-filter]:http://doc.tinysou.com/v1/searching.html#3-7-filter
