# my-blogs
一些博客

## 项目总结

### 校验

* DtoValidator 接口
```
public interface DtoValidator {

    /**
     * 校验当前对象，若校验失败，返回DtoValidateException;
     */
    void validate() throws DtoValidateException;
}
```
* DtoValidateException
```
dto对象校验异常。

必须包含校验错误的信息。Map<String, String>，key为属性名，value为错误信息。

 该异常须由spring mvc统一进行异常处理。
```

* 每个dto对象实现该接口，可以定义复杂的校验规则
* spring mvc 统一处理DtoValidateException异常
* 在service层，只调用dto.validate()方法即可完成自动校验
```
@Override
    public void createArticle(Long uid, AddArticleDto dto) throws HandleException {

        /* 1. 一些校验 */
        dto.validate();
        
        省略其他
    }
```

### 数据传输: `controller -> dto -> entity -> vo -> controller`
`dto` controller类生成，将其传入service层。service层校验后，生成相应的`entity`对象，调用dao方法持久化。service层也可生成`vo`对象返回给controller层。

在dto，entity，vo之间转换对象，可以写成静态方法。


* 不要为了复用这几个字段而强行继承（没有语义上的is-a关系）
* 为了项目结构的清楚尽量保持这些类独立，不要一个类充当多个角色，除非类中的字段完全相同。（如果需要修改字段，则要修改好几个类，但为了项目结构的清晰，这些修改是值得的。）

### service层与controller层之间的数据传输
原则：**成功返回结果，失败抛出异常**

有一些全局的异常，如：`数据库错误`，`文件存储错误`, `操作的对象不存在`, `权限错误`，`数据校验错误`这种任何一个业务都有可能发生的错误，抛出相应的异常，并使用spring mvc全局统一异常处理。

对于业务具体的异常，抛出类HandleException：
```
public class HandleException extends RuntimeException {

    private int code;
    private Object detail;
    
    ...
}
```
在service层规定code和错误的详细信息detail
在controller捕获HandleException做相应的处理。

### 使用实例测试jackson的细节

### spring mvc参数绑定实例

