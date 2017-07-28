---
title: JavaScript高级程序设计里的functions
date: 2017-07-17 17:34:27
tags:
- javascript
---
因为比较无聊，然后本包包又是个比较喜欢做笔记的人

整理了一下JavaScript高级程序设计里面出现过的function并标了注释

大抵摘自第9章自第14章




{% asset_img syobo.jpg 魔力小包包 %}



话说昨天睡前喝了几口咖啡一直亢奋到现在

而且就一直梦见炒黄豆炒了一晚上QAQ


```javascript
//代码整理♪(^∇^*)啦啦 客户端检查
var client = function () {
    var engine = {
        ie: 0,
        gecko: 0,
        webkit: 0,
        khtml: 0,
        opera: 0,
        ver: null
    }
    var browser = {
        ie: 0,
        firefox: 0,
        safari: 0,
        konq: 0,
        opera: 0,
        chrome: 0,
        ver: null
    }
    var system = {
        win: false,
        mac: false,
        x11: false,
        iphone: false,
        ipod: false,
        ipad: false,
        ios: false,
        android: false,
        nokiaN: false,
        winMobile: false,
        wii: false,
        ps: false
    }
    var ua = navigator.userAgent;
    /*
    * opera 5及更高版本中都有opera对象，用于保存浏览器相关的表示信息等 opera
    * 7.6及更高版本中，需要调用version()方法以返回一个表示浏览器版本的字符串
    */
    if (window.opera) {
        engine.ver = browser.ver = window
            .opera
            .version();
        engine.opera = browser.opera = parseFloat(engine.ver);
    } else if (/AppleWebKit\/(\S+)/.test(ua)) {
        engine.ver = RegExp["$1"];
        engine.webkit = parseFloat(engine.ver);
        //确定safari还是chrome
        if (/Chrome\/(\S+)/.test(ua)) {
            /*
        * chrome版本号信息格式
        * Mozilla/5.0 (平台;加密类型;操作系统或CPU;语言)
        * AppleWebKit/AppleWebKit版本号 (KHTML, like Gecko) Chrome/Chrome版本号 Safari/Safari版本
        * 实例：Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_5) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/59.0.3071.115 Safari/537.36
        */
            browser.ver = RegExp["$1"];
            browser.chrome = parseFloat(browser.ver);
        } else if (/Version\/(\S+)/.test(ua)) {
            /*
        * safari版本号信息格式(3及以后)
        * Mozilla/5.0 (平台;加密类型;操作系统或CPU;语言)
        * AppleWebKit/AppleWebKit版本号 (KHTML, like Gecko) Version/Safari实际版本号  Safari/Safari版本号
        * 实例：Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_5) AppleWebKit/603.2.4 (KHTML, like Gecko) Version/10.1.1 Safari/603.2.4
        */
            browser.ver = RegExp["$1"];
            browser.safari = parseFloat(browser.ver);
        } else {
            //确定相近版本号
            var safariVersion = 1;
            if (engine.webkit < 100) {
                safariVersion = 1;
            } else if (engine.webkit < 312) {
                safariVersion = 1.2;
            } else if (engine.webkit < 412) {
                safariVersion = 1.3;
            } else {
                safariVersion = 2;
            }
            browser.safari = browser.ver = safariVersion;
        }
    } else if (/KHTML\/(\S+)/.test(ua) || /Konqueror\/([^;]+)/.test(ua)) {
        /*
        * Konqueror版本信息格式
        * Mozilla/5.0 (compatible; Konqueror/版本号;操作系统或CPU)
        * konqueror 3.2之后
        * Mozilla/5.0 (compatible; Konqueror/版本号;操作系统或CPU) KHTML/KHTML 版 本号 (like Gecko)
        */
        engine.ver = browser.ver = RegExp["$1"];
        engine.khtml = browser.konq = parseFloat(engine.ver);
    } else if (/rv:([^\)]+)\) Gecko\/\d{8}/.test(ua)) {
        /*
        * Gecko版本信息格式
        * Mozilla/Mozilla版本号 (平台;加密类型;操作系统或CPU;语言;预先发行版本)
        * Gecko/Gecko版本号 应用程序或产品/应用程序或产品版本号
        * 实例(火狐)：Mozilla/5.0 (Macintosh; Intel Mac OS X 10.12; rv:47.0) Gecko/20100101 Firefox/47.0
        */
        engine.ver = RegExp["$1"];
        engine.gecko = parseFloat(engine.ver);
        //确定是不是Firefox
        if (/Firefox\/(\S+)/.test(ua)) {
            browser.ver = RegExp["$1"];
            browser.firefox = parseFloat(browser.ver);
        }
    } else if (/MSIE ([^;]+)/.test(ua)) {
        //ie
        engine.ver = browser.ver = RegExp["$ 1"];
        engine.ie = browser.ie = parseFloat(engine.ver);
    }

    //监测浏览器
    browser.ie = engine.ie;
    browser.opera = engine.opera;

    //监测平台
    var p = navigator.platform;
    system.win = p.indexOf('Win') == 0;
    system.mac = p.indexOf('Mac') == 0;
    system.x11 = (p == "X11") || (p.indexOf("Linux") == 0);

    //检查Windows的操作系统
    if (system.win) {
        /*
        * 匹配Windows95和Windows98这两个字符串。对这两个字符串，只有Gecko与其他浏览器不同，
        * 即没有"dows"，而且"Win"与版本号之间没有空格 <- Win95和Win98
        * Gecko在表示Windows NT时会在末尾添加"4.0"，因此在最后匹配一下小数
        */
        if (/Win(?:dows )?([^do]{2}) \s?(\d+\.\d+)?/.test(ua)) {
            if (RegExp["$1"] === "NT") {
                switch (RegExp["$2"]) {
                    case "5.0":
                        system.win = "2000";
                        break;
                    case "5.1":
                        system.win = "XP";
                        break;
                    case "6.0":
                        system.win = "Vista";
                        break;
                    case "6.1":
                        system.win = "7";
                        break;
                    default:
                        system.win = "NT";
                        break;
                }
            } else if (RegExp["$1"] === "9x") {
                system.win = "ME";
            } else {
                system.win = RegExp["$1"];
            }
        }
    }
    //移动设备
    system.iphone = ua.indexOf("iPhone") > -1;
    system.ipad = ua.indexOf("iPad") > -1;
    system.ipod = ua.indexOf("iPod") > -1;
    system.nokiaN = ua.indexOf("NokiaN") > -1;
    //windows mobile
    if (system.win === "CE") {
        system.winMobile = system.win;
    } else if (system.win === "Ph") {
        if (/Windows Phone OS (\d+.\d+)/.test(ua)) {
            system.win = "Phone";
            system.winMobile = parseFloat(RegExp["$1"]);
        }
    }
    //监测iOS版本
    if (system.mac && ua.indexOf("Mobile") > -1) {
        if (/CPU (?:iPhone)?OS (\d+_\d+)/.test(ua)) {
            system.ios = parseFloat(RegExp["$1"].replace("_", "."));
        } else {
            system.ios = 2;
        }
    }
    //监测安卓
    if (/Android (\d+\.\d+)/.test(ua)) {
        system.android = parseFloat(RegExp["$1"]);
    }
    //游戏系统
    system.wii = ua.indexOf("Wii") > -1;
    system.ps = /playstation/i.test(ua);

    return {engine, browser, system};
}()

//NodeList转换为数组
function convertToArray(nodes) {
    var array = null;
    try {
        array = []
            .slice
            .call(nodes, 0);
    } catch (ex) {
        //ie8及更早版本中NodeList是COM对象，所以需要手动枚举
        array = [];
        for (var i = 0; i < nodes.length; i++) {
            array.push(nodes[i]);
        }
    }
    return array;
}

//序列化attribute属性
function outputAttributes(element) {
    var pairs = [],
        attrName,
        attrValue,
        i,
        len;
    for (i = 0, len = element.attributes.length; i < len; i++) {
        attrName = element.attributes[i].nodeName;
        attrValue = element.attributes[i].nodeValue;
        if (element.attributes[i].specified) {
            //ie7的问题(返回一大堆)，因此指定specified，使其返回html中指定的相应特性
            pairs.push(`${attrName}=${attrValue}`);
        }
    }
    return pairs.join(" ");
}

//动态脚本 然而并没有方法知道脚本有米有加载完
function loadScript(url) {
    var script = document.createElement("script");
    script.type = "text/javascript";
    script.src = url;
    document
        .body
        .appendChild(script);
}

//动态样式
function loadStyles(url) {
    var link = document.createElement("link");
    link.rel = "stylesheet";
    link.type = "text/css";
    link.href = url;
    var head = document.getElementsByTagName("head")[0];
    head.appendChild(link);
}

//动态插入行内样式
function loadStyleString(css) {
    var style = document.createElement("style");
    style.type = "text/css";
    try {
        style.appendChild(document.createTextNode(css));
    } catch (ex) {
        //ie将style视为一个特殊节点，不能直接访问其子节点。重用同一个style元素并重设cssText属性时，可能会导致浏览器崩溃
        style.styleSheet.cssText = css;
    }
    var head = document.getElementsByTagName("head")[0];
    head.appendChild(style);
}

//检查元素是否与选择符匹配
function matchesSelector(el, selector) {
    if (el.matchesSelector) {
        return el.matchesSelector(selector);
    } else if (el.msMatchesSelector) {
        return el.msMatchesSelector(selector);
    } else if (el.mozMatchesSelector) {
        return el.mozMatchesSelector(selector);
    } else if (el.webkitMatchesSelector) {
        return el.webkitMatchesSelector(selector);
    } else {
        throw Error('Not Support!');
    }
}

//包含
function contains(refNode, otherNode) {
    if (typeof refNode.contains === "function") {
        //是后代节点则返回true，支持的浏览器有IE、FireFox9+、Safari、Opera和Chrome
        return refNode.contains(otherNode);
    } else if (typeof refNode.compareDocumentPosition === "function") {
        //1-无关，2-居前，4-居后，8-包含，16-被包含
        return !!(refNode.compareDocumentPosition & 16);
    } else {
        var node = otherNode.parentNode;
        while (node !== null) {
            if (node === refNode) {
                return true;
            } else {
                node = node.parentNode;
            }
        }
        return false;
    }
}

//获取样式表
function getStyleSheet(el) {
    //ie不支持sheet属性
    return el.sheet || el.styleSheet;
}

//向样式表插入规则
function insertRule(sheet, selectorText, cssText, position) {
    if (sheet.insertRule) {
        //FireFox,Safari,Opera和Chrome支持
        sheet.insertRule(selectorText + "(" + cssText + ")", position);
    } else if (sheet.addRule) {
        //ie8及更早，最多添加4095条样式
        sheet.addRule(selectorText, cssText, position);
    }
}

//删除规则
function deleteRule(sheet, index) {
    if (sheet.deleteRule) {
        sheet.deleteRule(index);
    } else if (sheet.removeRule) {
        //ie
        sheet.removeRule(index);
    }
}

//获取某元素在页面上的偏移量(左)
function getElementLeft(el) {
    var actualLeft = el.offsetLeft;
    var current = el.offsetParent;
    while (current !== null) {
        actualLeft += current.offsetLeft;
        current = current.offsetParent;
    }
    return actualLeft;
}

//获取某元素在页面上的偏移量(上)
function getElementTop(el) {
    var actualTop = el.offsetTop;
    var current = el.offsetParent;
    while (current !== null) {
        actualTop += current.offsetTop;
        current = current.offsetParent;
    }
    return actualTop;
}

//获取视窗大小
function getViewport() {
    //clientWidth-元素内容区宽度+内边距宽度
    if (document.compatMode === "BackCompat") {
        //ie7及之前
        return {width: document.body.clientWidth, height: document.body.clientHeight}
    } else {
        return {width: document.documentElement.clientWidth, height: document.documentElement.clientHeight}
    }
}

//获取元素大小和位置
function getBoundingClientRect(element) {
    //然而严格模式下不允许使用calleeQAQ;ie8及更早以(2,2)为坐标七点，因此需要检查一下元素的位置
    var scrollTop = document.body.scrollTop;
    var scrollLeft = document.body.scrollLeft;
    if (element.getBoundingClientRect) {
        if (typeof arguments.callee.offset !== "number") {
            var temp = document.createElement("div");
            temp.style.cssText = "position:absolute;left:0;top:0;";
            document
                .body
                .appendChild(temp);
            arguments.callee.offset = -temp
                .getBoundingClientRect()
                .top - scrollTop;
            document
                .body
                .removeChild(temp);
            temp = null;
        }
        var rect = element.getBoundingClientRect();
        var offset = arguments.callee.offset;

        return {
            left: rect.left + offset,
            right: rect.right + offset,
            top: rect.top + offset,
            bottom: rect.bottom + offset
        }
    } else {
        var actualTop = getElementTop(element);
        var actualLeft = getElementLeft(element);
        return {
            left: actualLeft - scrollLeft,
            right: actualLeft + element.offsetWidth - scrollLeft,
            top: actualTop - scrollTop,
            bottom: actualTop + element.offsetHeight - scrollTop
        }
    }
}

var EventUtil = {
    addHandler(element, type, handler) {
        if (element.addEventListener) {
            //false-冒泡阶段调用，true-捕获阶段调用
            element.addEventListener(type, handler, false);
        } else if (element.attachEvent) {
            element.attachEvent("on" + type, handler);
        } else {
            element["on" + type] = handler;
        }
    },
    removeHandler(element, type, handler) {
        if (element.removeEventListener) {
            element.removeEventListener(type, handler, false);
        } else if (element.attachEvent) {
            element.attachEvent("on" + type, handler)
        } else {
            element["on" + type] = null;
        }
    },
    getEvent(e) {
        //在ie中，event参数是未定义的，因此就会返回window.event
        return e || window.event;
    },
    getTarget(e) {
        // e.srcElement是e.target的一个别名，只有ie有；load事件target会被设置成document，ie则不会设置scrElement
        // script标签在ie9+、Safari3+及其他浏览器有onload事件，ie&opera的link有onload事件
        return e.target || e.srcElement;
    },
    preventDefault(e) {
        if (e.preventDefault) {
            e.preventDefault();
        } else {
            //ie用，不标准，ff没有这个属性
            e.returnValue = false;
        }
    },
    stopPropagation(e) {
        if (e.stopPropagation) {
            e.stopPropagation();
        } else {
            e.cancelBubble = true;
        }
    },
    getRelatedTarget(e) {
        //只有mouseover和mouseout事件才有该值，ie8及之前版本不支持relatedTarget
        if (e.relatedTarget) {
            return e.relatedTarget;
        } else if (e.toElement) {
            //mouseout事件触发
            return e.toElement;
        } else if (e.fromElement) {
            //mouseover事件触发
            return e.fromElement;
        } else {
            return null;
        }
    },
    getButton(e) {
        //mousedown&mouseup事件，则在其event时间中存在一个button属性
        if (document.implementation.hasFeature("MouseEvents", "2,0")) {
            //常规0-主鼠标，1-中间鼠标，2-次鼠标触发
            return e.button;
        } else {
            //ie8及之前
            switch (e.button) {
                case 0: //没有按下
                case 1: //按下主鼠标
                case 3: //同时按下主次
                case 5: //按下主和中
                case 7: //按下三个
                    return 0;
                    break;
                case 2: //按下次鼠标
                case 6:
                    return 2;
                    break;
                case 4: //按下中间
                    return 1;
                    break;
            }
        }
    },
    getWheelDelta(e) {
        //mousewheel事件
        if (e.wheelDelta) {
            //在opera9.5之前是正负号电脑的，一般是120的倍数
            return (client.engine.opera && client.engine.opera < 9.5)
                ? -e.wheelDelta
                : e.wheelDelta;
        } else {
            //ff是3的倍数, 实际看来我用的chrome虽然是放在wheelDelta里面的但是也是3的倍数
            return -e.detail * 40
        }
    },
    getCharCode(e) {
        // 代表一个与键盘对应的码
        // ie9+,ff,chrome,safari在keypress的时候有charCode属性，此时的keyCode可能是0or其他所按键的键码
        // 因为keypress是按下自附件出发，keydown是按下所有键触发
        if (typeof e.charCode === "number") {
            return e.charCode
        } else {
            return e.keyCode
        }
    },
    getKey(e) {
        // dom3新增，键盘事件不再包含charCode，而包含key、char属性，key代表按下的字符文本，char在非字符的时候是null
        // ie9支持key不支持char，safari5&chrome支持keyIdentifier(对于字符键返回表示unicode值)
        // 因为跨浏览器问题，不推荐（我使用的chrome没有keyIndentifier但又key和char）
        return e.key || e.char || e.keyIdentifier
    },
    getLocation(e) {
        //0-默认键盘，1-左侧位置，2-右侧位置，3-数字小键盘，4-虚拟键盘，5-手柄
        return e.location || e.keyLocation;
    },
    getClipboardText(e) {
        var clipboardData = (e.clipboardData || window.clipboardData);
        //safari和chrome中接受的参数是MIMEtype，不过可以用text代表text/plain
        return clipboardData.getData("text");
    },
    setClipboardText(e, value) {
        if (e.clipboardData) {
            return e
                .clipboardData
                .setData("text/plain", value);
        } else if (window.clipboardData) {
            return window
                .clipboardData
                .setData("text", value);
        }
    }
}

//取得选中的文本
function getSelectedText(textbox) {
    if (typeof textbox.selectionStart === "number") {
        return textbox
            .value
            .substring(textbox.selectionStart, textbox.selectionEnd);
    } else if (document.selection) {
        //兼容ie8
        return document
            .selection
            .createRange()
            .text;
    }
}

//选择部分文本
function selectText(textbox, startIndex, stopIndex) {
    if (textbox.setSelectionRange) {
        //ie9+及其他，要看到选择的文本需要焦点
        textbox.setSelectionRange(startIndex, stopIndex);
    } else if (textbox.createTextRange) {
        //创建范围
        var range = textbox.createTextRange();
        //将范围止跌到文本框开始位置
        range.collapse(true);
        //移动
        range.moveStart("character", startIndex);
        range.moveEnd("character", stopIndex - startIndex);
        //选择文本
        range.select();
    }
    textbox.focus();
}

//对于选择多项的选择框，selectedIndex只返回选中项的第一项
function getSelectedOptions(selectbox) {
    var result = [];
    var option = null;
    for (var i = 0, len = selectbox.options.length; i < len; i++) {
        option = selectbox.options[i];
        if (option.selected) {
            result.push(option);
        }
    }
    return result;
}
```