# caoxicheng.github.io

## 操作说明

### 新建文章

```
hexo new [layout] <title>
```

### list
```
$ hexo list <type>
```
列出网站数据

### generate

```bash
$ hexo generate
```

生成静态文件。

|        选项         |    描述                                                                                                  |
|:-----------------:|:-----------------------------------------------------------------------------------------------------|
|   -d, --deploy    |    文件生成后立即部署网站                                                                                         |
|    -w, --watch    |    监视文件变动                                                                                              |
|    -b, --bail     | 生成过程中如果发生任何未处理的异常则抛出异常                                                                               |
|    -f, --force    |    强制重新生成文件 Hexo 引入了差分机制，如果 public 目录存在，那么 hexo g 只会重新生成改动的文件。使用该参数的效果接近 `hexo clean && hexo generate` |
| -c, --concurrency |    最大同时生成文件的数量，默认无限制                                                                                   |

### publish

发布草稿

```
hexo publish [layout] <filename>
```

### service

```
hexo server
```

启动服务器。默认情况下，访问网址为： `http://localhost:4000/`

|          选项           | 描述       |
|:---------------------:|:---------|
|      -p, --port       |    重设端口    |
| -s, --static |    只使用静态文件 |
|-l, --log    | 启动日记记录，使用覆盖记录格式|

