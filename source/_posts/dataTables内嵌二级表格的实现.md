---
title: dataTables内嵌二级表格的实现
date: 2017-05-23 11:41:08
tags: [插件]
categories: 插件
---

之前在写angular时有用过ui-grid写过内嵌展开二级表格，怎么说呢，ui-grid很强大，功能挺多的，好像也是angular团队开发的，例子足，文档好(对那些英文阅读能力比较好的人来说)，不过他的有些功能不稳定，其中就包括表格中内嵌表格的实现[Expandable grid](http://ui-grid.info/docs/#/tutorial/216_expandable_grid)。

之后在写别的项目时也遇到了相同的需求，这次准备使用datatables，在此记录下过程。

直接上代码:

# html部分 #

    //这个div用来存放查询条件
    <div>
        <div class="float-left m-b-md"><label>账户名:</label><input id="accountName" type="text"></div>
        <div class="float-left m-b-md"><label>银行账号:</label><input id="bankAccount" type="text"></div>
        <div class="float-left m-b-md"><label>账户ID:</label><input id="accountId" type="text"></div>
        
        <button id="searchBtn" class="m-b-md">查询</button>
    </div>
    //以下是table的主体
    <table id="example" class="display" cellspacing="0" width="100%">
        <thead>
            <tr>
                <th></th>
                <th>账户ID</th>
                <th>账户名</th>
                <th>账户状态</th>
                <th>账户类型</th>
                <th>账户余额</th>
                <th>银行账号</th>
                <th>冻结金额</th>
                <th>在途金额</th>
                <th>最后交易时间</th>
                
                
            </tr>
        </thead>
    </table>

html部分分为搜索部分和table主体部分，挺简单的没啥说的

# js部分 #

        //1.初始化部分
        //1.1禁用表格自带的搜索和排序
        $.extend($.fn.dataTable.defaults, {
            searching: false,
            ordering: false
        });
        //1.2初始化表格主体
        var table = $('#example').DataTable({
        //配置相应部分的中文显示(废话，不然就显示英文了)
            "language": {
                "lengthMenu": "每页 _MENU_ 条记录",
                "zeroRecords": "没有找到记录",
                "info": "第 _PAGE_ 页 ( 总共 _PAGES_ 页 )",
                "infoEmpty": "无记录",
                "infoFiltered": "(从 _MAX_ 条记录过滤)",
                "paginate": {
                    "first": "First",
                    "last": "Last",
                    "next": "后一页",
                    "previous": "上一页"
                },
            },
        
            "pageLength": 10,           //默认每页条数
            "lengthChange": false,      //禁掉表格自带的选择每页条数的下拉框
            "processing": true,         //是否显示进度遮罩
            "serverSide": true,         //开启服务端传输数据
            "ajax": {                   //ajax
                url: "${ctx}/client/account/accountListData",
                type: "POST",
                data: function(d) {
                    //获取查询条件的值
                    var accountName  = $("#accountName").val();
                    var bankAccount  = $("#bankAccount").val();
                    var accountId  = $("#accountId").val();
                    //添加额外的参数传给服务器  
                     d.accountName = accountName;
                     d.bankAccountNo = bankAccount;
                     d.accountId = accountId;
                },
                
            },
            //这里虽然没用到，但这个属性很有用，可以初始datatable的data(还是去看看官网吧)，因为插件对返回值的格式要求严格
            /* "dataSrc": function(json){
                   
                   return json.data[0].body.records;
                }, */
            //配置各列
            "columns": [{
                className: 'details-control',   //就是给这列添加的类名
                orderable: false,               //禁用掉这列的排序功能
                data: null,                  //data为空，因为这列只有展开icon
                defaultContent: ''          //没数据的话为空
                },{
                data:'accountId'
                    /* "targets": 0,
                    "data": "body",
                    "render": function ( data, type, full, meta ) {
                      return data.records.memberId;
                    } */
                },{
                data: 'accountName'
                },{
                data: 'accountStatusdic'
                },{
                data: 'accounTypedic'
                },{
                data: 'availableBalance'
                },{
                data: 'bankAccount'
                },{
                data: 'freezeBalance'
                },{
                data: 'undeterminedBalance'
                },/* {
                data: 'memberId',
                },{
                data: 'memberStatusDic',  
                }, */{
                 data: 'lastTranTime'
                }
                ],
            "columnDefs": [
                {
                    targets: [5, 7, 8 ],  //这个用于对列的批量处理，里面为需要处理的列的下标
                    //render很有用，这里主要是过滤用，好像在columns中也可以写render
                    render: function(data,type,row,meta){
                        return ("￥"+data.toFixed(2));
                    }
                },
                
             ] 
        });
        //点击查询，触发表格的渲染
        $("#searchBtn").click(function() {
            table.ajax.reload();
        });
        //二级table的模板
        function format(table_id) {
            return '<table class="table table-striped" id="opiniondt_' + table_id + '">' +
            '<thead>'+
            '<tr>'+
            '<th>账户ID</th><th>账户名</th><th>账户状态</th>'+
            '<th>账户类型</th><th>可用余额</th><th>冻结金额 </th>'+
            '<th>最后交易时间</th><th>锁定金额</th><th>在途金额</th>'+
            '</tr>'+
            '</thead>' +
            '<tr>' +
            '<td>Full name:</td>' +
            '<td>Extension number:</td>' +
            '<td>Extra info:</td>' +
            '</tr>' +
            '<tr>' +
            '<td>Full name:</td>' +
            '<td>Extension number:</td>' +
            '<td>Extra info:</td>' +
            '</tr>' +
            '</table>';
        }
        //先初始化一个变量，用于动态给展开的二级table赋值
        var iTableCounter = 1;
        var oInnerTable;
        //展开icon的点击事件
        $('#example tbody').on('click', 'td.details-control', function() {
            //获得最近的tr老爸
            var tr = $(this).closest('tr');
            //遍历老铁们
            var td = $(this).siblings("td");
            //获取第一个老铁的内容
            var acId=td[0].innerHTML;
            
            //把选择的这一行变成datatable的行，这样就可以使用一些方法了
            //dataTables默认给所有的行设置了他们的子显示区
            //可通过row().child.show()和row().child.hide()显示隐藏
            var row = table.row(tr);
            
            var data;
            if (row.child.isShown()) {
                //  This row is already open - close it
                row.child.hide();
                //这个shown是为了控制展开icon和收缩icon的切换显示
                tr.removeClass('shown');
            } else {
                //ajax开始传参给后台
                  $.ajax({
                    url:"${ctx}/client/account/getSlaveAccount",
                    type:"POST",
                    data:{accountId:acId},
                }).done(function(res){
                    data = JSON.parse(res);
                    //这里是请求后台字典接口，进行过滤数据，记住设置为同步请求
                    data.slaveAccounts.forEach(function(e){
                        (function(){
                        $.ajax({
                            url:"${ctx}/client/dict/getDicValue",
                            type:"post",
                            async:false,
                            data:{
                                type:"accountType",
                                value:e.accountType,
                                defaultValue:""
                            }
                        }).done(function(res1){
                            e.accountType=res1;
                        });
                    }());
                        //处理accountStatus
                        (function(){
                        $.ajax({
                            url:"${ctx}/client/dict/getDicValue",
                            type:"post",
                            async:false,
                            data:{
                                type:"accountStatus",
                                value:e.accountStatus,
                                defaultValue:""
                            }
                        }).done(function(res2){
                            e.accountStatus=res2;
                        });
                        }());
                    })
                    var lastData=data.slaveAccounts;
                    // Open this row
                    //child()内接收二级table的模板
                    row.child(format(iTableCounter)).show();
                    //显示出收缩icon
                    tr.addClass('shown');
                    // try datatable stuff
                    //为所有的二级列表配置
                    oInnerTable = $('#opiniondt_' + iTableCounter).dataTable({
                        data: lastData,
                        autoWidth: true,
                        deferRender: true,
                        info: false,
                        lengthChange: false,
                        ordering: false,
                        paging: false,
                        scrollX: false,
                        scrollY: false,
                        searching: false,
                        columns: [{
                            data: 'accountId'
                        }, {
                            data: 'accountName'
                        }, {
                            data: 'accountStatus'
                        }, {
                            data: 'accountType'
                        }, {
                            data: 'availableBalance'
                        }, {
                            data: 'freezeBalance'
                        }, {
                            data: 'lastTranTime'
                        }, {
                            data: 'lockBalance'
                        }, {
                            data: 'undeterminedBalance'
                        }],
                        columnDefs: [{
                            targets: [4,5, 7, 8 ],
                            render: function(data,type,row,meta){
                                        return ("￥"+data.toFixed(2));
                                    }
                        }] 
                    });
                    //+1后下一个展开的二级table的id加1
                    iTableCounter = iTableCounter + 1;
                })  
            }
        })

嗯，想说的过程全变成注释了，就酱吧。