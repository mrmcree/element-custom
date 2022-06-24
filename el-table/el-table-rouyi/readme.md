```
 <ElTableRouyi
        :table-config="tableConfig"
        :column="tableColumn"
        @pagination="getList"
        :action-config="  {
           operations:[
             {
            operationType : 'add',
            type          : 'text',
            actionBtnName : '新增',
            permissionCode:'system:user:add',
            handleClick   : handleAdd
          }
          ]
        }"

        :pagination="this.tablepagination"
        :tableList="userList"

        >
```



```
  tableColumn:[
        { prop: 'userId', label: `用户编号`, format: ()=>{},},
        { prop: 'userName', label: `用户名称`, format: ()=>{}},
        { prop: 'nickName', label: `用户昵称`, format: ()=>{}},
        { prop: 'deptName', label: `部门`, format: ()=>{}},
        { prop: 'phonenumber', label: `手机号码`, format: ()=>{}},
        { prop: 'status', label: `状态`, format: ()=>{},slotName:'status'},
        { prop: 'createTime', label: `创建时间`, format: ()=>{}},

      ],
```
