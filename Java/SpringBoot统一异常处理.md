---
title: SpringBoot统一异常处理
date: 2022-04-08
tags: 
- Java
- SpringBoot
---

### 使用@ControllerAdvice实现异常统一处理

下面的代码对三类异常进行全局异常处理：400参数异常、自定义异常、其他异常

```java
/**
 * Global exception handler.
 *
 * @author pdai
 */
@Slf4j
@RestControllerAdvice
public class GlobalExceptionHandler {

    /**
     * exception handler for bad request.
     *
     * @param e
     *            exception
     * @return ResponseResult
     */
    @ResponseBody
    @ResponseStatus(code = HttpStatus.BAD_REQUEST)
    @ExceptionHandler(value = { BindException.class, ValidationException.class, MethodArgumentNotValidException.class })
    public ResponseResult<ExceptionData> handleParameterVerificationException(@NonNull Exception e) {
        ExceptionData.ExceptionDataBuilder exceptionDataBuilder = ExceptionData.builder();
        log.warn("Exception: {}", e.getMessage());
        if (e instanceof BindException) {
            BindingResult bindingResult = ((MethodArgumentNotValidException) e).getBindingResult();
            bindingResult.getAllErrors().stream().map(DefaultMessageSourceResolvable::getDefaultMessage)
                    .forEach(exceptionDataBuilder::error);
        } else if (e instanceof ConstraintViolationException) {
            if (e.getMessage() != null) {
                exceptionDataBuilder.error(e.getMessage());
            }
        } else {
            exceptionDataBuilder.error("invalid parameter");
        }
        return ResponseResultEntity.fail(exceptionDataBuilder.build(), "invalid parameter");
    }

}


    /**
     * handle business exception.
     *
     * @param businessException
     *            business exception
     * @return ResponseResult
     */
    @ResponseBody
    @ExceptionHandler(BusinessException.class)
    public ResponseResult<BusinessException> processBusinessException(BusinessException businessException) {
    log.error(businessException.getLocalizedMessage(), businessException);
    // 这里可以屏蔽掉后台的异常栈信息，直接返回"business error"
    return ResponseResultEntity.fail(businessException, businessException.getLocalizedMessage());
}


  /**
   * handle other exception.
   *
   * @param exception
   *            exception
   * @return ResponseResult
   */
  @ResponseBody
  @ExceptionHandler(Exception.class)
  public ResponseResult<Exception> processException(Exception exception) {
      log.error(exception.getLocalizedMessage(), exception);
      // 这里可以屏蔽掉后台的异常栈信息，直接返回"server error"
      return ResponseResultEntity.fail(exception, exception.getLocalizedMessage());
  }

```

不需要在controller接口做异常处理。
