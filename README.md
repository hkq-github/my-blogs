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

