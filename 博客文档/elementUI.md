## elementUI form 校验

基本使用：

- prop 对应 model中ruleForm 下的字段

- rules 对应 prop 字段

**使用默认的校验规则**

```vue
<el-form :model="ruleForm" :rules="rules" ref="ruleForm" label-width="100px" class="demo-ruleForm">
  <el-form-item label="活动名称" prop="name">
    <el-input v-model="ruleForm.name"></el-input>
  </el-form-item>
<el-form/>
 <script>
  export default {
    data() {
      return {
        ruleForm: {
          name: '',
        },
        rules: {
          name: [
            { required: true, message: '请输入活动名称', trigger: 'blur' },
            { min: 3, max: 5, message: '长度在 3 到 5 个字符', trigger: 'blur' }
          ],
        }
      };
    },
 <script/>

```



**自定义校验规则错误提示**

```js
password(rule, value, callback) { 
	if (/*条件*/) return callback(new Error('错误提示'))
	callback()
}
```



**手动触发校验以及重置清除**

```js
this.$refs['formRefName'].validate() //对整个表单进行校验的方法
this.$refs['formRefName'].validateField('formItemProp') //对部分表单字段进行校验的方法
this.$refs['formRefName'].resetFields() //所有字段值重置为初始值并移除校验结果
this.$refs['formRefName'].clearValidate('formItemProp') //移除表单项的校验结果。不传值为移除所有
```

## elementUI 弹窗dialog

弹窗未加载弹出时候，调用其ref报错。

解决：

在下次 DOM 更新循环结束之后执行延迟回调。

```vue
this.$nextTick(()=>{
	this.$refs['dialogRef'].methodName()
})
```



