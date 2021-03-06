## 保持技术精进

先得有方向，我用这个技术能给我带来什么回报？找到内在动力

1. 读书，学习视频课程

2. 去阅读源码，大的开源项目有新的技术、巧妙的设计、优良的架构，对自己写代码、架构的能力都有非常大的提升

3. 在项目中使用自己想用的技术，解决现实问题

4. 加入开源项目，和牛人一起工作，向牛人看齐

5. 加入高手的社群，与优秀的人在一起

----

## 如何明智地向程序员提问

From: https://z.codes/how-to-ask-computer-question/

### 简短版

我现在遇到一个问题X

我想到可能的原因是a, b, c

我排除了以下可能性d, e, f

我尝试过以下方案g, h, i

请问还有什么是我遗漏的?

### 首先你需要明白

* 程序员们只偏爱艰巨的任务，或者能激发他们思维的好问题

* 对方没有义务忍耐你的无知和懒惰

* 周全的思考，准备好你的问题，草率的发问只能得到草率的回答，或者根本得不到任何答案

### 提问之前

* 用中**英文**进行**Google**, 翻前两页的结果, 往往Stack Overflow网站上的答案就是正确答案. 如果没有找到, **更换可能的关键词多次尝试**

* 在FAQ/文档里找答案, 耐心读英文文档是基本素养

### 发问的形式

<ul>
<li><p>使用言简意赅，描述准确的标题</p>
</li>
<li><p>精确描述, 信息量大, 但是不啰嗦</p>
<ul>
<li><p>尽可能详细而明确的描述症状</p>
</li>
<li><p>提供问题发生的环境（机器配置、操作系统、应用程序以及别的什么）</p>
</li>
<li><p>说明你在提问前是怎样去研究和理解这个问题的</p>
</li>
<li><p>说明你在提问前采取了什么步骤去解决它</p>
</li>
<li><p>在自己的尝试中, 排除了哪些可能的原因</p>
</li>
<li><p>罗列最近做过什么可能有影响的硬件、软件变更</p>
</li>
<li><p>尽量想象一个程序员会怎样反问你，在提问的时候预先给他答案</p>
</li>
</ul>
</li>
<li><p>对每一个关键步骤截图, 如果有错误信息, **截图和文字版**连同产生问题的**代码**都要发给对方</p>
</li>
<li><p>给出自己出问题的代码, 必须是对方复制后就能立即运行, 并且复现问题的最简代码.  删去与问题无关的部分</p>
</li>
<li><p>别问应该自己解决的问题, 避免无意义的疑问</p>
</li>
</ul>

### 问题解决后

* 简短说明自己是如何解决的, 后续尝试的过程

* 如果别人对你有帮助, 感谢一下对方, 比如发个红包什么的

### 附加

![](assets/img/how_to_ask_question.jpg)

### 参考

电脑出现故障，如何正确地提问 [https://vjudge1.github.io/2015/07/01/how-to-ask.html](https://vjudge1.github.io/2015/07/01/how-to-ask.html)

你会问问题吗 [http://coolshell.cn/articles/3713.html](http://coolshell.cn/articles/3713.html)

《提问的艺术：如何快速获得答案》（精读版） [http://bbs.csdn.net/topics/390307835](http://bbs.csdn.net/topics/390307835)

### 本文的图片版

(方便在聊天工具里甩给对方):

![如何明智地向程序员提问](assets/img/how-to-ask-computer-question.png)

----

## 使用chrome缓存找到被删的qq空间的图片

看到有好友秀恩爱，然后就没有权限访问了，但打开过的图片有chrome缓存，于是便尝试从缓存找到图片url

chrome的缓存可以在这里找到：

```
chrome://cache/
```

然后随意点开一张qq空间的图片，发现其包含psb（毕竟右键保存的文件名默认就是psb），然后就是搜索咯

在点进去的缓存页面可以F12执行js，查看缓存图片：

代码来源：http://www.sensefulsolutions.com/2012/01/viewing-chrome-cache-easy-way.html

```
    (function() {
    var preTags = document.getElementsByTagName('pre');
    var preWithHeaderInfo = preTags[0];
    var preWithContent = preTags[2];

    var lines = preWithContent.textContent.split('\n');
 
    // get data about the formatting (changes between different versions of chrome)
    var rgx = /^(0{8}:\s+)([0-9a-f]{2}\s+)[0-9a-f]{2}/m;
    var match = rgx.exec(lines[0]);
 
    var text = '';
    for (var i = 0; i < lines.length; i++) {
        var line = lines[i];
        var firstIndex = match[1].length; // first index of the chars to match (e.g. where a '84' would start)
        var indexJump = match[2].length; // how much space is between each set of numbers
        var totalCharsPerLine = 16;
        index = firstIndex;
        for (var j = 0; j < totalCharsPerLine; j++) {
            var hexValAsStr = line.substr(index, 2);
            if (hexValAsStr == '  ') {
                // no more chars
                break;
            }

            var asciiVal = parseInt(hexValAsStr, 16);
            text += String.fromCharCode(asciiVal);

            index += indexJump;
        }
    }

    var headerText = preWithHeaderInfo.textContent;
    var elToInsertBefore = document.body.childNodes[0];
    var insertedDiv = document.createElement("div");
    document.body.insertBefore(insertedDiv, elToInsertBefore);

    // find the filename
    var nodes = [document.body];
    var filepath = '';
    while (true) {
        var node = nodes.pop();
        if (node.hasChildNodes()) {
            var children = node.childNodes;
            for (var i = children.length - 1; i >= 0; i--) {
                nodes.push(children[i]);
            }
        }

        if (node.nodeType === Node.TEXT_NODE && /\S/.test(node.nodeValue)) {
            // 1st depth-first text node (with non-whitespace chars) found
            filepath = node.nodeValue;
            break;
        }
    }
    
    outputResults(insertedDiv, convertToBase64(text), filepath, headerText);

    insertedDiv.appendChild(document.createElement('hr'));

    function outputResults(parentElement, fileContents, fileUrl, headerText) {
        // last updated 1/27/12
        var rgx = /.+\/([^\/]+)/;
        var filename = rgx.exec(fileUrl)[1];

        // get the content type
        rgx = /content-type: (.+)/i;
        var match = rgx.exec(headerText);
        var contentTypeFound = match != null;
        var contentType = "text/plain";
        if (contentTypeFound) {
            contentType = match[1];
        }

        var dataUri = "data:" + contentType + ";base64," + fileContents;

        // check for gzipped file
        var gZipRgx = /content-encoding: gzip/i;
        if (gZipRgx.test(headerText)) {
            filename += '.gz';
        }
        
        // check for image
        var imageRgx = /image/i;
        var isImage = imageRgx.test(contentType);
            
        // create link
        var aTag = document.createElement('a');
        aTag.textContent = "Left-click to download the cached file";
        aTag.setAttribute('href', dataUri);
        aTag.setAttribute('download', filename);
        parentElement.appendChild(aTag);
        parentElement.appendChild(document.createElement('br'));
    
        // create image
        if (isImage) {
            var imgTag = document.createElement('img');
            imgTag.setAttribute("src", dataUri);
            parentElement.appendChild(imgTag);
            parentElement.appendChild(document.createElement('br'));
        }
    
        // create warning
        if (!contentTypeFound) {
            var pTag = document.createElement('p');
            pTag.textContent = "WARNING: the type of file was not found in the headers... defaulting to text file.";
            parentElement.appendChild(pTag);
        }
    }

    function getBase64Char(base64Value) {
        if (base64Value < 0) {
            throw "Invalid number: " + base64Value;
        } else if (base64Value <= 25) {
            // A-Z
            return String.fromCharCode(base64Value + "A".charCodeAt(0));
        } else if (base64Value <= 51) {
            // a-z
            base64Value -= 26; // a
            return String.fromCharCode(base64Value + "a".charCodeAt(0));
        } else if (base64Value <= 61) {
            // 0-9
            base64Value -= 52; // 0
            return String.fromCharCode(base64Value + "0".charCodeAt(0));
        } else if (base64Value <= 62) {
            return '+';
        } else if (base64Value <= 63) {
            return '/';
        } else {
            throw "Invalid number: " + base64Value;
        }
    }

    function convertToBase64(input) {
        // http://en.wikipedia.org/wiki/Base64#Example
        var remainingBits;
        var result = "";
        var additionalCharsNeeded = 0;

        var charIndex = -1;
        var charAsciiValue;
        var advanceToNextChar = function() {
            charIndex++;
            charAsciiValue = input.charCodeAt(charIndex);
            return charIndex < input.length;
        };

        while (true) {
            var base64Char;

            // handle 1st char
            if (!advanceToNextChar()) break;
            base64Char = charAsciiValue >>> 2;
            remainingBits = charAsciiValue & 3; // 0000 0011
            result += getBase64Char(base64Char); // 1st char
            additionalCharsNeeded = 3;

            // handle 2nd char
            if (!advanceToNextChar()) break;
            base64Char = (remainingBits << 4) | (charAsciiValue >>> 4);
            remainingBits = charAsciiValue & 15; // 0000 1111
            result += getBase64Char(base64Char); // 2nd char
            additionalCharsNeeded = 2;

            // handle 3rd char
            if (!advanceToNextChar()) break;
            base64Char = (remainingBits << 2) | (charAsciiValue >>> 6);
            result += getBase64Char(base64Char); // 3rd char
            remainingBits = charAsciiValue & 63; // 0011 1111
            result += getBase64Char(remainingBits); // 4th char
            additionalCharsNeeded = 0;
        }

        // there may be an additional 2-3 chars that need to be added
        if (additionalCharsNeeded == 2) {
            remainingBits = remainingBits << 2; // 4 extra bits
            result += getBase64Char(remainingBits) + "=";
        } else if (additionalCharsNeeded == 3) {
            remainingBits = remainingBits << 4; // 2 extra bits
            result += getBase64Char(remainingBits) + "==";
        } else if (additionalCharsNeeded != 0) {
            throw "Unhandled number of additional chars needed: " + additionalCharsNeeded;
        }

        return result;
    }
    })()
```

例如找到http://a3.qpic.cn/psb?/V12C1bLj2DcCgb/f9hTWn5wbxt3dZd5MlUCHX6tA9oqVOudgT2rqARLltk!/a/dI4BAAAAAAAA

但这样只是一张小图，我们当然希望有大图，比对大图的url发现只要将上述url的/a/替换为/b/即可

所以总结一下就是打开缓存页面chrome://cache/，查找psb字符串，找到想要的图片，如果是小图就改一下url得到大图