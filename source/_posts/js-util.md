---
title: javascript/jq工具类扩展
date: 2017-02-20
comments: true
categories: javascript
description: 主要是一些常用工具类封装整理，这样开发起来就高效些...持续更新中...
---

### 【javascript原生工具扩展】

#### Date工具类
```
/**
 * javascript原生工具扩展
 * ski
 */
Date.prototype.format = function(format) {
    var o = {
        "M+": this.getMonth() + 1, //month
        "d+": this.getDate(), //day
        "h+": this.getHours(), //hour
        "m+": this.getMinutes(), //minute
        "s+": this.getSeconds(), //second
        "q+": Math.floor((this.getMonth() + 3) / 3), //quarter
        "S": this.getMilliseconds() //millisecond
    }
    if (/(y+)/.test(format)) {
        format = format.replace(RegExp.$1, (this.getFullYear() + "").substr(4 - RegExp.$1.length));
    }
    for (var k in o) {
        if (new RegExp("(" + k + ")").test(format)) {
            format = format.replace(RegExp.$1, RegExp.$1.length == 1 ? o[k] : ("00" + o[k]).substr(("" + o[k]).length));
        }
    }
    return format;
};


/**
 * eg: new Date().toLocaleString();
 * @return 2011-01-01 12:00:00
 */
Date.prototype.toLocaleString = function() {
    if(this.toString()=="Invalid Date"){
        return '';
    } else {
        return this.format("yyyy-MM-dd hh:mm:ss");
    }
};

```

#### String工具类

```
/**
 * @description trim去掉字符串两边的指定字符,默去空格
 */
String.prototype.trim = function(tag) {
    if (!tag) { 
        tag = '\\s';
    }else { 
        if (tag == '\\') { 
        tag = '\\\\'; 
    } else if (tag == ',' || tag == '|' || tag == ';') { 
            tag = '\\' + tag; 
        }else { 
            tag = '\\s'; 
        } 
    }
    eval('var reg=/(^' + tag + '+)|(' + tag + '+$)/g;'); 
    return this.replace(reg, '');
};

/**
 * @description 字符串截取后面加入...
 */
String.prototype.interceptString = function(len) {
    if (this.length > len) {
        return this.substring(0, len) + "...";
    } else {
        return this;
    }
}


/**
 * @description 将一个字符串用给定的字符变成数组
 */
String.prototype.toArray = function(tag) {
    if (this.indexOf(tag) != -1) {
        return this.split(tag);
    }else {
        if (this != '') {
            return [this.toString()];
        }else {
            return [];
        }
    }
}

/**
 * @description 只留下数字(0123456789)
 */
String.prototype.toNumber= function() { 
    return this.replace(/\D/g, ""); 
}
/**
 * @description 保留中文
 */
String.prototype.toCN= function() {  
    var regEx = /[^\u4e00-\u9fa5\uf900-\ufa2d]/g;  
    return this.replace(regEx, '');  
}
/**
 * @description 是否是以XX开头
 */
String.prototype.startsWith= function(tag){
    return this.substring(0, tag.length) == tag;
}
/**
 * @description 是否已XX结尾
 */
String.prototype.endWith= function(tag){
    return this.substring(this.length - tag.length) == tag;
}

```

#### Array工具类

```
/**
 * @description 根据数据取得再数组中的索引
 */
Array.prototype.getIndex = function(obj){
    for (var i = 0; i < this.length; i++) {
        if (obj == this[i]) {
            return i;
        }
    }
    return -1;
}

/**
 * @description 移除数组中的某元素
 */
Array.prototype.remove= function (obj) {
    for (var i = 0; i < this.length; i++) {
        if (obj == this[i]) {
            this.splice(i, 1);
            break;
        }
    }
    return this;
}

/**
 * @description 判断元素是否在数组中
 */
Array.prototype.contains= function (obj) {
    for (var i = 0; i < this.length; i++) {
        if (obj == this[i]) {
            return true;
        }
    }
    return false;
}
```

#### 浏览器操作
```
//进入全屏模式,  判断各种浏览器，找到正确的方法
var launchFullScreen = function (element) {
    if(element.requestFullscreen) {
        element.requestFullscreen();
    } else if(element.mozRequestFullScreen) {
        element.mozRequestFullScreen();
    } else if(element.webkitRequestFullscreen) {
        element.webkitRequestFullscreen();
    } else if(element.msRequestFullscreen) {
        element.msRequestFullscreen();
    }
    return true;
}
//退出全屏模式
var exitFullScreen = function () {
    if(document.exitFullscreen) {
        document.exitFullscreen();
    } else if(document.mozCancelFullScreen) {
        document.mozCancelFullScreen();
    } else if(document.webkitExitFullscreen) {
        document.webkitExitFullscreen();
    }
    return false;
}
```


### 【jQuery工具扩展】

#### jquery公共方法

```
(function($) {
    //公用方法
    $.extend($, {

        /**
         * @description 判断变量是否存在,或者是否以某种类型存在
         * @param {object} o  判断变量是否存在
         * @param {object} type 数据类型  Number,Boolean等
         * @return {Boolean} nResult 返回结果 true或者false
         */
        isExists: function(o, type) {
            return o != undefined && o !== null && (type ? o.constructor == type : true);
        },
        
        /**
         * @description 把任意类型转成Boolean
         * @param {object} o  任意对象
         * @return {Boolean} nResult 返回结果 true或者false
         */
        parseBoolean: function(o) {
            var flag = !! o;
            return (flag && typeof(o) == "string" && (o
                .toLowerCase() == "false" || o.toLowerCase() == "null" || o.toLowerCase() == "undefined" || o == "0")) ? false : flag;
        },
        parseDateTime: function (str) {
            if (str) {
                return new Date(str.replace(/-/g, '/'));
            }
        },
        
        /**
         * @description 把字符串自动转换成它原来的类型
         * @param val o  任意对象
         * @return val any
         */
        parseAny: function(val) {
            if (typeof val == 'string') {
                if (val != "" && !isNaN(val)) {
                    val = val - 0;
                } else if (val.toLowerCase() == "true") {
                    val = true;
                } else if (val.toLowerCase() == "false") {
                    val = false;
                }
            }
            return val;
        },
        
        /**
         * @description 将?key1=value1&key2=value2&...转换成一个对象{key1:value1,key2:value2....}
         * @param {String} string  String字符串
         * @return {Object} obj 返回结果 {key1:value1,key2:value2....}
         */
        parseParam: function(str) {
            var obj = {};
            if (str == undefined || str == null) {
                return obj;
            }

            if (typeof str == 'object') {
                return str;
            }

            if (typeof str == 'string') {
                str = decodeURIComponent(str);
            }

            //踢出前缀#或者？
            str = str.replace(/^[\?\#]/, "");
            //分割参数
            var params = str.split("&");

            for (var i = 0; i < params.length; i++) {
                var item = params[i].split("=");
                var key = item[0];
                var val = item[1];

                if (!key) {
                    continue;
                }

                //类型转换
                if (val == undefined) {
                    val = true;
                } else {
                    val = this.parseAny(val);
                }
                obj[key] = val;
            }
            return obj;
        },
        
        /**
         * 获取url参数
         */
        getQueryString: function(name) {
            var reg = new RegExp("(^|&|)"+ name +"=([^&]*)(&|$)");
            var r = window.location.hash.substr(1).match(reg);
            if(r != null) return  unescape(r[2]); return null;
        },
        
        /**
         * @description 占位符格式化
         * @param {String} str 需要替换的模板
         * @param {Object} params 参数
         * @param {Boolean} isEncode 是否编码
         * @eg demo("http://www.baidu.com?name={name}&age={age}&chanelid={chanelid}",{name:"demo",age:23,chanelid:100},false)
         * @return {String} str 返回结果 http://www.baidu.com?name=demo&age=23&chanelid=100
         */
        format: function(str, params, isEncode) {
            if (typeof params == "object") {
                for (var key in params) {
                    if (!$.isExists(params[key]) || params[key] == "undefined" || params[key] == "null") {
                        params[key] = "";
                    }
                    str = str.replace(new RegExp("\\{" + key + "\\}", "ig"), isEncode ? encodeURIComponent(params[key]) : params[key]);
                }
            }
            return str.replace(/\{\w*\}/ig, "");
        },
        
        /**
         * 去掉字符串起始的制定字符
         * @method trimStart
         * @param <String> text 指定字符串
         * @param <String> trimStr 替换字符串
         */
        trimStart: function(text, trimStr) {
            return (text || "").replace(new RegExp("^" + trimStr + "*", "g"), "");
        },

        /**
         * 去掉字符串结尾的制定字符
         * @method trimEnd
         * @param <String> text 指定字符串
         * @param <String> trimStr 替换字符串
         */
        trimEnd: function(text, trimStr) {
            return (text || "").replace(new RegExp(trimStr + "*$", "g"), "");
        },
        startWith: function(s, d) {
            return new RegExp("^" + d).test(s);
        },
        
        /**
         * 获取标准表单数据，转换为JSON数据格式
         * @method getForm
         * @example $.getForm("#form");
         * isParse是否转化类型
         * @return <Object> data
         */
        getForm: function ($form, isParse) {
            var data = {};
            $form = $($form);
            if ($form.length == 0) {
                return data;
            }

            $("input", $form).each(function () {
                var item = $(this);
                var name = item.attr("name");
                if (!name || $.isExists(data[name])) {
                    return;
                }
                switch (item.attr("type")) {
                    case "radio":
                        // DOTO: 当 input[type="radio"][disabled="disabled"] 为禁用
                        if (!this.disabled && this.checked) {
                            data[name] = item.val();
                        }
                        break;
                    case "checkbox":
                        // DOTO: 当 input[type="checkbox"][disabled="disabled"] 为禁用
                        if (this.disabled) {
                            break;
                        }

                        data[name] = "";
                        $("input[type='checkbox'][name='" + name + "']:checked", $form).each(function () {
                            data[name] += this.value + ",";
                        })
                        data[name] = $.trimEnd(data[name], ',');
                        break;
                    case "button":
                    case "reset":
                    case "submit":
                        break;
                    default:
                        // DOTO: 当 input[type="text"][disabled="disabled"] 为禁用
                        if (!this.disabled) {
                            data[name] = $.trim(item.val());
                        }
                        break;
                }
                data[name] = isParse ? $.parseAny(data[name]) : data[name];
            });
            $("textarea", $form).each(function () {
                if (!this.name) {
                    return;
                }
                data[this.name] = $.parseAny(this.value);
            });
            $("select", $form).each(function () {
                if (!this.name) {
                    return;
                }
                data[this.name] = isParse ? $.parseAny(this.value) : this.value;
            });
            return data;
        },
        setForm: function ($form, data) {
            $form = $($form);
            if ($form.length == 0 || !$.isPlainObject(data)) {
                return;
            }
            var nodes = $form.get(0).elements;
            var i = 0,
                iLen = nodes.length;
            var j, jLen, item, name, type, select, value, existsVal;

            for (; i < iLen; i++) {
                item = nodes[i];
                name = item.name;
                type = item.type;
                value = data[name];

                if (!name || item.disabled || !$.isExists(value) || "file" == type) {
                    continue;
                }

                if ("radio" == type) {
                    item.checked = item.value == (value + "");
                } else if ("checkbox" == type || "select-multiple" == type) {
                    if ($.isArray(value)) {
                        //把值转化成字符串
                        for (var n = 0; n < value.length; n++) {
                            value[n] += "";
                        }
                    } else {
                        value = (value + "").split(',');
                    }

                    if ("checkbox" == type) {
                        item.checked = $.inArray(item.value, value) != -1;
                    } else {
                        select = item.options;
                        for (j = 0, jLen = select.length; j < jLen; j++) {
                            item = select[j];
                            item.selected = $.inArray(item.value, value) != -1;
                        }
                    }
                } else {
                    if (undefined != value) {
                        item.value = value;
                    }
                }
            }
        },
        
        /**
         * 重置表单
         * @method resetForm
         * @example $.resetForm("#myFormId");
         * @param formId #+表单Id
         */
        resetForm: function ($form) {
            $($form).get(0).reset();
        },
        
        /**
         * 加载并解析模板
         * @usage $.template('tpl-paotui-orderDetails', data) loads and compile '/static/manage/modules/paotui/batchDelivers.tpl'
         */
        template: function (id, data) {
            /**
             * @param id {String} starts with 'tpl'
             * @param data {Object}
             */
            var _templates = Namespace('DDXQ._templates');
            var render, tpl_base;
            tpl_base = site_root + '/static/manage/modules/';
            if (!id) {
                return;
            }

            if (!$.isPlainObject(data)) {
                data = {};
            }

            if (id.indexOf('tpl') != 0) {
                return template(id, data);
            } else {
                id = id.substr(4);
                if (id in _templates) {
                    return _templates[id](data);
                }
            }

            $.ajax({
                url: tpl_base + id.replace(/-/g, "/") + ".tpl",
                cache: false,
                async: false,
                dataType: 'html',
                success: function (html) {
                    render = template.compile(html);
                    _templates[id] = render;
                }
            });

            if (render) {
                return render(data);
            } else {
                console.log(id + '模板加载出错');
                return "";
            }
        },
        
        /**
         * 加载外部模版并解析
         * @usage $.artTemplate('test', data) loads and compile '/test.tpl'
         */
        artTemplate: function(name, data){
            var render = null;
            $.ajax({
                url: name + ".tpl",
                cache: false,
                async: false,
                dataType: 'html',
                success: function (tpl) {
                    render = template.compile(tpl);
                }
            });
            return render ? render(data) : '模版加载失败';
        },
        
        sendLog: function (key, msg) {
            /**
             * @param key {String} 日志key值，相当于路由
             * @param msg {Object} 一维键值对，用于传递需要打日志的数据
             * @usage $.sendLog('task/schedule/batchdispatch', {row_index: 2})
             */
            var data = $.extend(msg, {key: key});
            $.ajax({
                url: site_root + '/api/sendLog',
                method: 'post',
                data: data
            });
        },
        findByKey: function (key, value, collection) {
            /**
             * @param key {String} property key
             * @param value {Any} property value
             * @param collection {Array} data collection to be searching in
             */
            for (var i = 0, item; item = collection[i]; i++) {
                if (item[key] == value) {
                    return item;
                }
            }
            return null;
        },
        getProp: function (key, global) {
            /**
             * @param key {String} format: `a.b.c ...`
             * @param global {Object}
             */
            global = global || window;
            var segments = key.split('.');
            var segRefs = [];
            var length = segments.length;
            segRefs[0] = global[segments[0]];

            if (length == 1) {
                return segRefs[0];
            }

            for (var i = 1; i < segments.length; i++) {
                segRefs[i] = segRefs[i - 1][segments[i]];
                if (!segRefs[i]) {
                    return undefined;
                }
            }

            return segRefs[segments.length - 1];
        },
        uuid: function () {
            var d = new Date().getTime();
            if (window.performance && typeof window.performance.now === "function") {
                d += performance.now(); //use high-precision timer if available
            }
            var uuid = 'xxxxxxxx-xxxx-4xxx-yxxx-xxxxxxxxxxxx'.replace(/[xy]/g, function (c) {
                var r = (d + Math.random() * 16) % 16 | 0;
                d = Math.floor(d / 16);
                return (c == 'x' ? r : (r & 0x3 | 0x8)).toString(16);
            });
            return uuid;
        },
        makeComboboxData: function (textField, valueField, collection, hasAll) {
            var _data = [];
            hasAll && _data.push({text: '全部', value: ''});
            $.each(collection, function (index, item) {
                _data.push({
                    text: item[textField],
                    value: item[valueField]
                });
            });
            return _data;
        },
        makeComboboxFromMap: function (map, hasAll) {
            var _data = [];
            hasAll && _data.push({text: '全部', value: ''});
            $.each(map, function (key, val) {
                _data.push({
                    text: val,
                    value: key
                });
            });
            return _data;
        },
        
        /**
         * @param img
         * @param {Array} [size = [80, 80]]
         * @param {Number} [mode = 1]
         */
        qiniuImgCrop: function (img, size, mode) {
            // @see http://developer.qiniu.com/code/v6/api/kodo-api/image/imageview2.html
            if (typeof img !== 'string') {
                throw new TypeError('Invalid Param Type for `img`, URLString Expected.')
            }
            var defaults = {
                size: [80, 80],
                mode: 1
            }

            size = size || defaults.size;
            mode = mode || defaults.mode;

            var ext = 'imageView2/' + mode + '/w/' + size[0] + '/h/' + size[1]

            // `?` exist in img url
            if (img.indexOf('?') > -1) {
                return img.split('!')[0].split('?')[0] + '?' + ext
            }

            return img + '?' + ext
        }
    });
}($));


```

#### jquery cookie

```
(function($){
	$.extend({	
		cookie : function (key, value, options) {
		    // key and value given, set cookie...
		    if (arguments.length > 1 && (value === null || typeof value !== "object")) {
		        options = jQuery.extend({}, options);
		        if (value === null) {
		            options.expires = -1;
		        }
		        if (typeof options.expires === 'number') {
		            var days = options.expires, t = options.expires = new Date();
		            t.setDate(t.getDate() + days);
		        }
		        return (document.cookie = [
		            encodeURIComponent(key), '=',
		            options.raw ? String(value) : encodeURIComponent(String(value)),
		            options.expires ? '; expires=' + options.expires.toUTCString() : '', // use expires attribute, max-age is not supported by IE
		            options.path ? '; path=' + options.path : '',
		            options.domain ? '; domain=' + options.domain : '',
		            options.secure ? '; secure' : ''
		        ].join(''));
		    }
		    // key and possibly options given, get cookie...
		    options = value || {};
		    var result, decode = options.raw ? function (s) { return s; } : decodeURIComponent;
		    return (result = new RegExp('(?:^|; )' + encodeURIComponent(key) + '=([^;]*)').exec(document.cookie)) ? decode(result[1]) : null;
		}
	})
})(jQuery);
```