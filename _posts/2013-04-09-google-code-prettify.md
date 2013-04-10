---
layout: post
category : Code
tagline: "Supporting tagline"
tags : [google-code-prettify]
---
{% include JB/setup %}

这个博客，现在用的这个是google-code-prettfy，效果也相当不错。最重要的是，非常小，加载速度比SyntaxHighlighter快得多，而且，可以直接使用markdown的语法去写代码。

首先需要两个文件，prettify.js和prettify.css，自己去官网去下。把这两个放到模板中，如下

    <pre class="prettyPrint">
    <link href="/assets/themes//google-code-prettify/desert.css" rel="stylesheet" type="text/css" media="all" />
    <script type="text/javascript" src="/assets/themes//google-code-prettify/prettify.js">
    $(function() {
    $('pre').addClass('prettyprint linenums').attr('style', 'overflow:auto');
    });
    </script>
    </pre>

<!--more-->
for examples

就可以使用pre标签代码高亮
    


    <pre class="prettyPrint">
     // code here
    </pre>

    <pre class="prettyPrint">
    <style type="text/css">
        pre {
            white-space: pre-wrap; /* css-3 */
            white-space: -moz-pre-wrap; /* Mozilla, since 1999 */
            white-space: -pre-wrap; /* Opera 4-6 */
            white-space: -o-pre-wrap; /* Opera 7 */
            word-wrap: break-word; /* Internet Explorer 5.5+ */
        }
    </style>
    </pre>
