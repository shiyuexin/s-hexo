---
title: EasyUI工具类扩展
date: 2017-02-02
comments: true
categories: EasyUI
description: 这里封装了一些easyui常用的工具类，提高工作效率咯
---

### 【前言】

>如果对easyui还未有一定了解，引用参考[easyui 穿越>>](http://www.runoob.com/jeasyui/jqueryeasyui-tutorial.html)

### 【示例】

#### 1.扩展validate规则

>使用:`data-options="validType:'mobile'" //验证手机号`

```
$.extend($.fn.validatebox.defaults.rules, {
    //最大长度
    maxLength: {
        validator: function(value, param){
            return value.length <= param[0];
        },
        message: '您输入的文字太多了! '
    },
    //最小长度
    minLength: {
        validator: function(value, param){
            return value.length >= param[0];
        },
        message: '您输入的文字太少了! '
    },
    mobile: {
    	validator: function(value){
            return /^(13|14|15|17|18|19)\d{9}$/i.test(value);
        },
        message: '手机号码格式不正确! '
    },
    domainSeo: {
        validator: function(value){
            return /^[\w]+\.([\w]+\.)*((me)|(co)|(am)|(ca)|(com)|(net)|(org)|(gov\.cn)|(info)|(cc)|(com\.cn)|(net\.cn)|(org\.cn)|(name)|(biz)|(tv)|(cn)|(mobi)|(name)|(sh)|(ac)|(io)|(tw)|(com\.tw)|(hk)|(com\.hk)|(ws)|(travel)|(us)|(tm)|(la)|(me\.uk)|(org\.uk)|(ltd\.uk)|(plc\.uk)|(in)|(eu)|(it)|(jp))$/.test(value);
        },
        message: '请输入正确的域名! '
    },
    equals: {
        validator: function(value,param){
            return value == $(param[0]).val();
        },
        message: '两次输入的内容不匹配.'
    },
    xFloat: {
        validator: function(value,param){
            return /^[0-99]+(.[0-99]{1})?$/.test(value) && value<=100;
        },
        message: '请输入=>0且<=100的整数或一位小数'
    },
    xInt: {
        validator: function(value,param){
            return /^[0-9]\d*$/.test(value);
        },
        message: '请输入=>0的整数'
    },
    xBank: {
        validator: function(value,param){
            return /^(\d{16}|\d{19})$/.test(value);
        },
        message: '银行卡格式错误'
    }
});

```

#### 2.扩展datagrid方法

>使用:`$('#id').datagrid('getEditingRowIndexs'); //获取当前datagrid中在编辑状态的行编号列表`

```
$.extend($.fn.datagrid.methods, {
    //datagrid反选
    reverseSelect: function(jq){
        return jq.each(function(){
            var select_rows = $(this).datagrid('getSelections');
            var select_index;
            var now_index;
            var select_index_arr = [];
            for (var X in select_rows){
                select_index = $(this).datagrid('getRowIndex', select_rows[X]);
                select_index_arr.push(select_index);
            }
            var all_rows = $(this).datagrid('getRows');
            for (var X in all_rows){
                now_index = $(this).datagrid('getRowIndex', all_rows[X]);
                if ($.inArray(now_index, select_index_arr) == -1){
                    $(this).datagrid('selectRow', now_index);
                } else {
                    $(this).datagrid('unselectRow', now_index);
                }
            }
        })
	},
    //保持当前的选择状态
    keepSelect: function(jq){
        return jq.each(function(){
            var self = $(this);
            var select_rows = self.datagrid('getSelections');
            var select_index;
            var now_index;
            var select_index_arr = [];
            for (var X in select_rows){
                select_index = self.datagrid('getRowIndex', select_rows[X]);
                select_index_arr.push(select_index);
            }
            setTimeout(function(){
                self.datagrid('unselectAll');
                for (var Y in select_index_arr){
                    self.datagrid('selectRow', select_index_arr[Y]);
                }
            }, 100);
        })
	},
    //增加编辑器
    addEditor : function(jq, param) {
        if (param instanceof Array) {
            $.each(param, function(index, item) {
                var e = $(jq).datagrid('getColumnOption', item.field);
                e.editor = item.editor;
            });
        } else {
            var e = $(jq).datagrid('getColumnOption', param.field);
            e.editor = param.editor;
        }
    },
    //移除编辑器
    removeEditor : function(jq, param) {
    	if (param == null){
			param = $(jq).datagrid('getColumnFields');
    	}
        if (param instanceof Array) {
            $.each(param, function(index, item) {
                var e = $(jq).datagrid('getColumnOption', item);
                e.editor = {};
            });
        } else {
            var e = $(jq).datagrid('getColumnOption', param);
            e.editor = {};
        }
    },
    //获取编辑状态的行
    getEditingRowIndexs: function(jq) {
        var rows = $.data(jq[0], "datagrid").panel.find('.datagrid-row-editing');
        var indexs = [];
        rows.each(function(i, row) {
            var index = row.sectionRowIndex;
            if (indexs.indexOf(index) == -1) {
                indexs.push(index);
            }
        });
        return indexs;
    }
});

```

#### 3.扩展datagrid-editors行内编辑工具类

```
$.extend($.fn.datagrid.defaults.editors, {
    timespinner: {
        init: function(container, options){
            var input = $('<input class="easyui-timespinner">').appendTo(container);
            options.formatter = function(time){
                return new Date(time).format("hh:mm");
            };
            return input.timespinner(options);
        },
        getValue: function(target){
            return $(target).timespinner('getValue');
      	},
        setValue: function(target, value){
            $(target).timespinner('setValue', value);
        },
        resize: function(target, width){
        	$(target).timespinner('resize', width);
        }
    },
    combotree: {
    	init: function(container, options){
            var input = $('<input class="easyui-combotree">').appendTo(container);
            return input.combotree(options);
        },
        getValue: function(target){
        	var values = $(target).combotree('getValues');
        	var real_values = [];     	
        	for (var x = 0,len = values.length;x < len;x ++){
        		if (values[x] && values[x] != ''){
        			real_values.push(values[x]);
        		}
        	}
            return real_values.join(',');
      	},
        setValue: function(target, value){  	
            setTimeout(function(){
            	var values = value.split(',');
            	$(target).combotree('setValues', values);
            	var tree = $(target).combotree('tree');
	            var nodes = tree.tree('getChecked');
	            var parent;
	            for (var x = 0,len = nodes.length;x < len;x ++){
	            	parent = tree.tree('getParent', nodes[x]['target']);
	            	while(parent != null){
	            		tree.tree('expand', parent.target);
	            		parent = tree.tree('getParent', parent['target']);
	            	}      	
	            }
            }, 100);
        },
        resize: function(target, width){
        	$(target).combotree('resize', width);
        }
    },
    combobox: {
    	init: function(container, options){
            var input = $('<input class="easyui-combobox">').appendTo(container);
            return input.combobox(options);
        },
    	getValue : function(jq) {
	        var opts = $(jq).combobox('options');
	        if(opts.multiple){
	            var values = $(jq).combobox('getValues');
	            if(values.length>0){
	                if(values[0]==''||values[0]==' '){
	                    return values.join(',').substring(1);
	                }
	            }
	            return values.join(',');
	        }
	        else
	            return $(jq).combobox("getValue");
	    },
	    setValue : function(jq, value) {
	        var opts = $(jq).combobox('options');
	        if(opts.multiple&&value.indexOf(opts.separator)!=-1){//多选且不只一个值
	            var values = value.split(opts.separator);
	            $(jq).combobox("setValues", values);
	        }
	        else
	            $(jq).combobox("setValue", value);
	    },
        resize: function(target, width){
        	$(target).combobox('resize', width);
        }
    },
    datetimebox: {
    	init: function(container, options){
            var input = $('<input class="easyui-datetimebox">').appendTo(container);
            return input.datetimebox(options);
        },
    	getValue : function(jq) {
	        return $(jq).datetimebox("getValue");            
	    },
	    setValue : function(jq, value) {
	    	$(jq).datetimebox("setValue", value);
	    },
        resize: function(target, width){
        	$(target).datetimebox('resize', width);
        }
    },
    uploadbox: {
    	init: function(container, options){
            var input = $('<input class="easyui-validatebox" readonly=true>').appendTo(container);
            return input.validatebox(options);
        },
        destroy: function(target){
            $(target).remove();
        },
    	getValue : function(target) {
	        return $(target).val();            
	    },
	    setValue : function(target, value) {
	    	$(target).val(value);
	    },
        resize: function(target, width){
        	$(target)._outerWidth(width);
        }
    }
});

```

>这里只是easyui部分扩展组件,想了解更多js扩展请穿越->[js-util](/2017/02/20/js-util/)
