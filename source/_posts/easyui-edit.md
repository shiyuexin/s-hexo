---
title: EasyUI DataGrid行内编辑
date: 2017-01-06
comments: true
categories: EasyUI
description: EasyUI 是一个基于 jQuery 的框架，集成了各种用户界面插件。简单易用是他最大的特点，现在仍有很多公司应用。这里主要介绍 EasyUI 在 dataGrid 中的行内编辑功能
---

### 【前言】

>如果对easyui还未有一定了解，引用参考[easyui 穿越>>](http://www.runoob.com/jeasyui/jqueryeasyui-tutorial.html)

### 【示例】

#### 1.datagrid数据网格

>`editor` `type` `options` 是表格行内编辑的主要属性
>`beginEdit` `endEdit` `cancelEdit` 是行内编辑主要方法(很语意化的)
>`row.editing` 判断是否已经开始编辑

```
this.$keyGrid = $('#key_grid');
this.$keyGrid.datagrid({
    title: 'xxx',
    url: 'xxxxxxxxx',
    height: that.config.win.winHeight-20,
    singleSelect: true,
    fitColumns: true,
    pagination: true, //设置true将在数据表格底部显示分页工具栏。
    pagePosition: "bottom",
    pageSize: 30,
    tools:[{
        iconCls:'icon-add',
        handler:function(){
       
        }
    }],
    toolbar: '#toolbar',
    columns:[[
        {field:'pinyinsuoxie',title:'拼音缩写',width:10,align:'center',
            editor: "text",
            editor:{
                type:'validatebox',
                options: { required: true}
            }
        },
        {field:'business_typess',title:'类型',width:10,align:'center',
            editor:{
                type:'combobox',
                options:{
                    data: $.makeComboboxFromMap(ENUMS.KEY_STATUS),
                    valueField: 'value',
                    textField: 'text',
                    editable: true,
                    required: true
                }
            }
        },
        {field:'update_time',title:'更新时间',width:10,align:'center',
            formatter:function(value,row,index){
                if(value)
                return new Date(value*1000).format('yyyy-MM-dd hh:mm')
            }
        },
        {field:'action',title:'操作',width:10,align:'center',
            formatter:function(value,row,index){
                if (row.editing){
                    var s = '<a href="javascript:;" class="btn btn-success k-save" data-id='+ row._id +'>保存</a> ';
                    var c = '<a href="javascript:;" class="btn btn-primary k-cancel" data-id='+ row._id +'>取消</a>';
                    return s+c;
                } else {
                    var e = '<a href="javascript:;" class="btn btn-primary k-edit" data-id='+ row._id +'>编辑</a> ';
                    if(row.status == 1){
                        var d = '<a href="javascript:;" class="btn btn-primary k-delete" data-id='+ row._id +'>删除</a> ';
                    }else{
                        var d = '<a href="javascript:;" class="btn btn-primary k-recover" data-id='+ row._id +'>恢复</a> ';
                    }
                    return e+d;
                }
            }
        }
    ]],
    onBeforeEdit:function(index,row){
        row.editing = true;
        that.updateActions(index);
    },
    onAfterEdit:function(index,row){
        row.editing = false;
        that.updateActions(index);
    },
    onCancelEdit:function(index,row){
        row.editing = false;
        that.updateActions(index);
    }
})

//本地更新表格
updateActions: function(index){
    this.$keyGrid.datagrid('updateRow',{
        index: index,
        row:{}
    })
}

//获取行索引
getRowIndex: function (){
    var that = this;
    that.config.data.row = that.$keyGrid.datagrid('getSelected');
    var rowIndex = that.$keyGrid.datagrid('getRowIndex', that.config.data.row);
    console.log('rowindex:',rowIndex)
    that.config.filter.old_index = rowIndex;
    return rowIndex;
}
```
#### 2.events事件
```
//编辑
this.el.$keyContent.on('click','.k-edit',function(){
    //标记当前点击行,点击其他行时恢复按钮状态
    that.config.filter.old_index >= '0'
        ? that.$keyGrid.datagrid('endEdit', that.config.filter.old_index)
        : '';
    //更新行内编辑状态'beginEdit'
    that.$keyGrid.datagrid('beginEdit', that.getRowIndex());
})
//取消
this.el.$keyContent.on('click','.k-cancel',function(){
    //更新行内编辑状态'cancelEdit'
    that.$keyGrid.datagrid('cancelEdit', that.getRowIndex());
})
```

#### 3.添加一行

```
//添加列
addKey: function(){
    //添加时先判断是否有开启编辑的行，如果有则把开户编辑的那行结束编辑
    this.$keyGrid.datagrid("insertRow", {
        index: 0, // index start with 0
        row: {}
    });
    //将新插入的那一行开户编辑状态
    this.$keyGrid.datagrid("beginEdit", 0);
    //给当前编辑的行赋值
}
```

#### 4.行内表单验证

```
//保存

this.el.$keyContent.on('click','.k-save',function(e){
    var $target = $(e.currentTarget);
    that.$keyGrid.datagrid('endEdit', that.getRowIndex());
    //获取当前选中行表单
    var isValid = $('.datagrid-row-selected').form('validate')
    if(!isValid){
        $.messager.show({
            title:'提示',
            msg:'请补全表单',
            timeout:3000,
            showType:'slide'
        });
        return;
    }
    that.saveKey();     //执行保存操作
})
```

>这里只演示了textbox和combobox组件行内编辑，其他组件请自行验证。还有一点需要说明的是这里的每一块都是写在一个对象中的，直接copy无法使用，请仔细阅读，选择性copy^_^
