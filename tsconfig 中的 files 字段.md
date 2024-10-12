在 tsconfig.json 中，`"files": ["src/app.ts"]` 用于明确指定要包含在编译中的具体文件，它告诉 TypeScript 编译器只编译 src 目录下的 app.ts 文件。

作用：

单个文件编译：当你只想编译某几个特定的文件时，可以使用 files 字段。编译器将只处理这些列出的文件，而忽略其他文件和目录。

快速定位：如果项目规模较小且只需编译特定文件，这样的配置可以加快编译速度。

注意：

如果在 `tsconfig.json` 中同时使用了 `files` 和 `include`，则 `files` 字段中的文件会优先编译，而 `include` 中的路径会被忽略。

如果你想编译整个 src 目录中的所有 TypeScript 文件，建议只使用 include 字段，而不使用 files 字段，如下所示：

```json
"include": [
  "src/**/*"
]
```

这样，所有位于 src 目录及其子目录下的 .ts 文件都会被编译。
