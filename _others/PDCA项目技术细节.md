## PDCA项目技术细节

```
后台 Rails
前台 Vue、axios、element-ui、actioncable、intro.js、underscore、vue-axios、vue-echarts、vue-moment、vue-router、vuex
```

```
ElementUI表单输入框验证：
data(){
	# 写validate验证方法
	var validatePass = (rule, value, callback) => {
        if (value === '') {
          return callback(new Error('密码不能为空'))
        } else if (value.match(/([a-z])+/) === null || value.match(/([0-9])+/) === null || value.match(/([A-Z])+/) === null) {
          setTimeout(() => {
            callback(new Error('密码规则为大小写和数字组合'))
          }, 1000)
        } else {
          callback()
        }
      }
      return {
        ruleForm: {
          pass: '',
          checkPass: ''
        },
        rules: {
          pass: [
            { validator: validatePass, trigger: 'blur' },
            { min: 6, max: 10, message: '长度在 6 到 10 个字符', trigger: 'blur' }
          ],
          checkPass: [
            { validator: validatePass2, trigger: 'blur' }
          ]
        }
      }
}
methods: {
	submitForm(formName) {
		this.$refs[formName].validate((valid) => {
		  if (valid) {
			# 验证通过调用方法
		  } else {
		    return false
		  }
		})
	}
}
```

### Linux命令

```
$ tail -5000 log.log | grep -C 5 'Error'    #查询log.log文件的后5000行，并且匹配Error错误的上下5行（-A是前五行，-B是后五行）
```

### Vue碰到问题

1. Vue数据对象监听的时候，没有监听到怎么办？

```
没有监听到是因为对象的该字段没有初始化，也就是在定义该对象的时候，没有为这个字段赋值，而是后来添加上的。
两种方案：
1、对象初始化的时候赋值。
2、使用这种方法this.$set(object, 'field', newValue)
```












