
# 核心概念

### entry（入口）
指示webpack应该使用哪个模块，来作为构建其内部依赖图的开始。

1.单个入口
  ~~~
    const config = {
      entry: {
        main: './path/to/my/entry/file.js'
      }
    }
  ~~~
