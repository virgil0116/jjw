# Kanban项目开发

```
axios请求自定义headers(上传文件时候，默认的content-type是json，改为multipart)
const config = { headers: { 'Content-Type': 'multipart/form-data' } };
let fd = new FormData();
fd.append('file',files[0])
return axios.post("http://localhost:5000/upload", fd, config)
```

```
js获取后缀或者后几位

let index = this.item.url.lastIndexOf('.')
let length = this.item.url.length
let suffix = this.item.url.substring(index, length)

let idnex = 'adfasdfasdf.xlsx'.lastIndexOf(".")
```

