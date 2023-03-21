# params引用方法

- task中的 params.name字段的值 == pipeline中的params.name字段的值
- pipeline中的params.name对应的value字段的值 == $(params.VALUE),其中VALUE就是pipelinerun中params.name字段的值，这样就可以引用到name对应的value的值。


对于yaml中
```yaml
params:
- name: repo-url
  value: github.com/xxx/xxx
```
**其实就是key，value对。其中key就是params.name字段的值；value就是对应value字段的值**。
