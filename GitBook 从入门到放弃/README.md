
## 未验证
`Linux`系统上部署`gitbook`服务，
`gitbook serve` 命令占用cpu非常多，建议直接用 `gitbook build` 然后放到任何支持静态文件的 `web server` 上。

```bash
gitbook build cd _book python -m SimpleHTTPServer 4000
```
