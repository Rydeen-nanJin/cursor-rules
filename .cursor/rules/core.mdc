---
description: 项目通用核心规则
alwaysApply: true
---

# 项目通用核心规则

## 一、强制规则

### Entity 到 VO/DTO 转换
- **必须使用 MapStruct** 进行 Entity 到 VO/DTO 的转换，禁止手动使用 set/get 方法
- 转换接口应放在 `convert` 或 `mapstruct` 包下，命名规范：`*Convert`
- 示例：
  ```java
  // ❌ 禁止：手动 set/get
  UserVO vo = new UserVO();
  vo.setName(entity.getName());
  vo.setAge(entity.getAge());
  
  // ✅ 正确：使用 MapStruct
  @Mapper(componentModel = "spring")
  public interface UserConvert {
      UserVO toVO(UserEntity entity);
  }
  ```

### 枚举类规范
- 创建枚举类时必须遵循标准结构和命名规范
- 枚举类应包含：
  - 私有字段（code、desc 等业务字段）
  - **必须使用 Lombok `@Getter` 注解**，自动生成 getter 方法
  - **推荐使用 Lombok `@RequiredArgsConstructor` 注解**，自动生成构造函数
  - 静态方法 `of(code)` 或 `getByCode(code)` 用于根据 code 查找
- **MyBatis-Plus 集成**：
  - 当枚举用于 MyBatis-Plus 实体类时，**必须在 code 字段上标记 `@EnumValue` 注解**
  - `@EnumValue` 用于指定枚举中哪个字段作为数据库存储值
- 命名规范：
  - 枚举类名使用大驼峰，通常以业务含义 + `Enum` 结尾（如 `OrderStatusEnum`）
  - 枚举值使用全大写下划线分隔（如 `PENDING_PAYMENT`）
- 示例：
  ```java
  // ✅ 标准枚举结构（使用 Lombok + MyBatis-Plus）
  @Getter
  @RequiredArgsConstructor
  public enum OrderStatusEnum {
      PENDING_PAYMENT(1, "待支付"),
      PAID(2, "已支付"),
      CANCELLED(3, "已取消");
      
      @EnumValue
      private final Integer code;
      private final String desc;
      
      public static OrderStatusEnum getByCode(Integer code) {
          for (OrderStatusEnum status : values()) {
              if (status.getCode().equals(code)) {
                  return status;
              }
          }
          return null;
      }
  }
  ```

### 类命名规范
- **VO（View Object）**：使用大驼峰命名，以 `VO` 结尾（如 `UserVO`、`OrderVO`）
- **DTO（Data Transfer Object）**：使用大驼峰命名，以 `DTO` 结尾（如 `UserDTO`、`CreateOrderDTO`）
- **Service 接口**：使用大驼峰命名，以 `Service` 结尾（如 `UserService`、`OrderService`）
- **Service 实现类**：使用大驼峰命名，以 `ServiceImpl` 结尾（如 `UserServiceImpl`、`OrderServiceImpl`）
- **Controller**：使用大驼峰命名，以 `Controller` 结尾（如 `UserController`、`OrderController`）
- **Mapper 接口**：使用大驼峰命名，以 `Mapper` 结尾（如 `UserMapper`、`OrderMapper`）
- **Convert 转换接口**：使用大驼峰命名，以 `Convert` 结尾（如 `UserConvert`、`OrderConvert`）
- **Config 配置类**：使用大驼峰命名，以 `Config` 结尾（如 `RedisConfig`、`MybatisConfig`）
- **枚举类**：使用大驼峰命名，以 `Enum` 结尾（如 `OrderStatusEnum`、`UserTypeEnum`）
- **异常类**：使用大驼峰命名，以 `Exception` 结尾（如 `BusinessException`、`ValidationException`）
- **常量类**：使用大驼峰命名，以 `Constants` 结尾（如 `SystemConstants`、`ErrorCodeConstants`）

## 二、禁止事项

以下规则适用于全部代码，必须严格遵守：

1. **禁止在枚举类中定义业务逻辑**
   - 禁止在枚举类中操作缓存、数据库等业务逻辑
   - 枚举类应保持为纯数据结构，业务逻辑应在 Service 层处理
   - 示例：
     ```java
     // ❌ 禁止：在枚举中操作缓存
     public enum OrderStatusEnum {
         PAID(2, "已支付");
         
         public void updateCache() {  // 禁止
             // 缓存操作
         }
     }
     
     // ✅ 正确：枚举保持纯数据结构
     public enum OrderStatusEnum {
         PAID(2, "已支付");
     }
     // 业务逻辑在 Service 层处理
     ```

2. **禁止使用 `@AllArgsConstructor` 替代 `@RequiredArgsConstructor`**
   - 除非有特殊需求，禁止使用 `@AllArgsConstructor`
   - `@RequiredArgsConstructor` 只为 final 字段生成构造函数，更加安全
   - 适用于所有使用 Lombok 的类（包括枚举类、实体类等）
   - 示例：
     ```java
     // ❌ 禁止：使用 @AllArgsConstructor
     @AllArgsConstructor
     public class User {
         private String name;
         private Integer age;
     }
     
     // ✅ 正确：使用 @RequiredArgsConstructor
     @RequiredArgsConstructor
     public class User {
         private final String name;
         private final Integer age;
     }
     ```

