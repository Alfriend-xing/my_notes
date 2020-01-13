# spring data jpa

## 实体类 

`AccountInfo.java`

```java
@Entity 
@Table(name = "t_accountinfo") 
public class AccountInfo implements Serializable { 
@Id
GeneratedValue(strategy = GenerationType.AUTO)
private Long accountId; 

@Column(name = "name", columnDefinition = "varchar(255) not null")
private String name;

@Column(unique = true)
private String userName;
// 此处省略 getter 和 setter 方法。
}
```
- @Entity 是一个必选的注解，声明这个类对应了一个数据库表。
- @Table(name = "t_accountinfo") 是一个可选的注解。声明了数据库实体对应的表信息。包括表名称、索引信息等。这里声明这个实体类对应的表名是 t_accountinfo。如果没有指定，则表名和实体的名称保持一致。
- @Id 标注用于声明一个实体类的属性映射为数据库的主键列。该属性通常置于属性声明语句之前，可与声明语句同行，也可写在单独行上。 
- @GeneratedValue 用于标注主键的生成策略。@GeneratedValue注解有两个属性,分别是strategy和generator。(strategy表示主键生成策略，generator声明了该主键生成器的名称)。strategy 属性默认情况下，JPA 自动选择一个最适合底层数据库的主键生成策略：SqlServer对应identity，MySQL 对应 auto increment。 (GenerationType.IDENTITY/GenerationType.AUTO(默认))
- @Column 用来声明实体属性的表字段的定义。默认的实体每个属性都对应了表的一个字段。字段的名称默认和属性名称保持一致（并不一定相等）。字段的类型根据实体属性类型自动推断。这里主要是声明了字符字段的长度。如果不这么声明，则系统会采用 255 作为该字段的长度
- [@Table注解介绍](https://fanlychie.github.io/post/jpa-table-annotation.html)
- [@Column注解介绍](https://fanlychie.github.io/post/jpa-column-annotation.html)
- [索引优化](https://blog.csdn.net/Abysscarry/article/details/80792876)


## 业务层接口 

`UserService.java`


```java
public interface UserDao extends Repository<AccountInfo, Long> { 
 
public AccountInfo save(AccountInfo accountInfo); 

// 增加新的持久层业务，查询给出 ID 的 AccountInfo 对象
public AccountInfo findByAccountId(Long accountId); 
}
```
- 该接口不需要实现，spring会自动实现
- 声明持久层的接口，该接口继承 Repository，Repository 是一个标记型接口，它不包含任何方法，在接口中声明需要的业务方法。Spring Data 将根据给定的策略来为其生成实现代码。
- CrudRepository也是一个可供继承的持久层接口，与Repository不同的是他自带十个增删改查方法，不用自行定义，缺点是一旦继承该接口，所有数据的常用操作都会暴露出来
- PagingAndSortingRepository 是另一个可供继承的持久层接口，他继承自CrudRepository，特点是自带分页查询和排序方法，自带方法很少直接使用，而是在自己声明的方法参数列表最后增加一个 Pageable 或 Sort 类型的参数，用于指定分页或排序信息。想要实现分页功能需要继承PagingAndSortingRepository或JpaRepository接口
- JpaRepository继承自PagingAndSortingRepository ，提供了 flush()，saveAndFlush()，deleteInBatch() 等方法


## Repository配置

@EnableJpaRepositories注解用于Srping JPA的代码配置，用于取代xml形式的配置文件

简单配置

- @EnableJpaRepositories("com.spr.repository")
- @EnableJpaRepositories({"com.cshtong.sample.repository", "com.cshtong.tower.repository"})

```java
@EnableJpaRepositories(
    basePackages = {},
    basePackageClasses = {},
    includeFilters = {},
    excludeFilters = {},
    repositoryImplementationPostfix = "Impl",
    namedQueriesLocation = "",//META-INF/jpa-named-queries.properties
    queryLookupStrategy=QueryLookupStrategy.Key.CREATE_IF_NOT_FOUND, //QueryLookupStrategy.Key.x
    repositoryFactoryBeanClass=JpaRepositoryFactoryBean.class, //class
    entityManagerFactoryRef="entityManagerFactory",
    transactionManagerRef="transactionManager",
    considerNestedRepositories=false, 
    enableDefaultTransactions=true
)
```
- basePackage 用于配置扫描Repositories所在的package及子package。
- basePackageClasses 指定 Repository 类。 例@EnableJpaRepositories(basePackageClasses = BookRepository.class)。配置包类的一个Repositories类，该包内其他Repositores也会被加载


## 数据库配置

```properties
#通用数据源配置
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.url=jdbc:mysql://10.110.2.56:3306/springboot_jpa?charset=utf8mb4&useSSL=false
spring.datasource.username=springboot
spring.datasource.password=springboot
# JPA 相关配置
spring.jpa.database-platform=org.hibernate.dialect.MySQL5InnoDBDialect
spring.jpa.show-sql=true
spring.jpa.hibernate.ddl-auto=create
```
- jpa的hibernate.ddl-auto的几个属性
  - create 每次加载hibernate时都会删除上一次的生成的表，然后根据你的model类再重新来生成新表
  - create-drop 每次加载hibernate时根据model类生成表，但是sessionFactory一关闭,表就自动删除
  - update 最常用的属性，第一次加载hibernate时根据model类会自动建立起表的结构（前提是先建立好数据库），以后加载hibernate时根据 model类自动更新表结构，即使表结构改变了但表中的行仍然存在不会删除以前的行。要注意的是当部署到服务器后，表结构是不会被马上建立起来的，是要等 应用第一次运行起来后才会。 
  - validate 每次加载hibernate时，验证创建数据库表结构，只会和数据库中的表进行比较，不会创建新表，但是会插入新值。




## 三种创建查询的方式

### 通过解析方法名创建查询

框架在进行方法名解析时，会先把方法名多余的前缀截取掉，比如 find、findBy、read、readBy、get、getBy，然后对剩下部分进行解析。并且如果方法的最后一个参数是 Sort 或者 Pageable 类型，也会提取相关的信息，以便按规则进行排序或者分页查询。

比如 findByUserAddressZip ()。框架在解析该方法时，首先剔除 findBy，然后对剩下的属性进行解析，详细规则如下（此处假设该方法针对的域对象为 AccountInfo 类型）：

- 先判断 userAddressZip （根据 POJO 规范，首字母变为小写，下同）是否为 AccountInfo 的一个属性，如果是，则表示根据该属性进行查询；如果没有该属性，继续第二步；
- 从右往左截取第一个大写字母开头的字符串（此处为 Zip），然后检查剩下的字符串是否为 AccountInfo 的一个属性，如果是，则表示根据该属性进行查询；如果没有该属性，则重复第二步，继续从右往左截取；最后假设 user 为 AccountInfo 的一个属性；
- 接着处理剩下部分（ AddressZip ），先判断 user 所对应的类型是否有 addressZip 属性，如果有，则表示该方法最终是根据 "AccountInfo.user.addressZip" 的取值进行查询；否则继续按照步骤 2 的规则从右往左截取，最终表示根据 "AccountInfo.user.address.zip" 的值进行查询。

可能会存在一种特殊情况，比如 AccountInfo 包含一个 user 的属性，也有一个 userAddress 属性，此时会存在混淆。读者可以明确在属性之间加上 "_" 以显式表达意图，比如 "findByUser_AddressZip()" 或者 "findByUserAddress_Zip()"。

在查询时，通常需要同时根据多个属性进行查询，且查询的条件也格式各样（大于某个值、在某个范围等等），Spring Data JPA 为此提供了一些表达条件查询的关键字，大致如下：

- And --- 等价于 SQL 中的 and 关键字，比如 findByUsernameAndPassword(String user, Striang pwd)；
- Or --- 等价于 SQL 中的 or 关键字，比如 findByUsernameOrAddress(String user, String addr)；
- Between --- 等价于 SQL 中的 between 关键字，比如 findBySalaryBetween(int max, int min)；
- LessThan --- 等价于 SQL 中的 "<"，比如 findBySalaryLessThan(int max)；
- GreaterThan --- 等价于 SQL 中的">"，比如 findBySalaryGreaterThan(int min)；
- IsNull --- 等价于 SQL 中的 "is null"，比如 findByUsernameIsNull()；
- IsNotNull --- 等价于 SQL 中的 "is not null"，比如 findByUsernameIsNotNull()；
- NotNull --- 与 IsNotNull 等价；
- Like --- 等价于 SQL 中的 "like"，比如 findByUsernameLike(String user)；
- NotLike --- 等价于 SQL 中的 "not like"，比如 findByUsernameNotLike(String user)；
- OrderBy --- 等价于 SQL 中的 "order by"，比如 findByUsernameOrderBySalaryAsc(String user)；
- Not --- 等价于 SQL 中的 "！ ="，比如 findByUsernameNot(String user)；
- In --- 等价于 SQL 中的 "in"，比如 findByUsernameIn(Collection<String> userList) ，方法的参数可以是 Collection 类型，也可以是数组或者不定长参数；
- NotIn --- 等价于 SQL 中的 "not in"，比如 findByUsernameNotIn(Collection<String> userList) ，方法的参数可以是 Collection 类型，也可以是数组或者不定长参数；

### 使用 @Query 创建查询

@Query 注解的使用非常简单，在声明的方法上面标注该注解，同时提供一个 JPQL 查询语句即可

```java
public interface UserDao extends Repository<AccountInfo, Long> { 
 
@Query("select a from AccountInfo a where a.accountId = ?1") 
public AccountInfo findByAccountId(Long accountId); 
 
   @Query("select a from AccountInfo a where a.balance > ?1") 
public Page<AccountInfo> findByBalanceGreaterThan( 
Integer balance,Pageable pageable); 
}
```

使用命名参数来代替位置编号

```java
public interface UserDao extends Repository<AccountInfo, Long> { 
 
public AccountInfo save(AccountInfo accountInfo); 
 
@Query("from AccountInfo a where a.accountId = :id") 
public AccountInfo findByAccountId(@Param("id")Long accountId); 
 
  @Query("from AccountInfo a where a.balance > :balance") 
  public Page<AccountInfo> findByBalanceGreaterThan( 
@Param("balance")Integer balance,Pageable pageable); 
}
```

使用 @Query 来执行一个更新操作，在使用 @Query 的同时，用 @Modifying 来将该操作标识为修改查询，这样框架最终会生成一个更新的操作，而非查询。

```java
@Modifying 
@Query("update AccountInfo a set a.salary = ?1 where a.salary < ?2") 
public int increaseSalary(int after, int before);
```

### 通过调用 JPA 命名查询语句创建查询

- [JPA之使用JPQL语句进行增删改查](https://www.jianshu.com/p/06beda1a0831)

命名查询是 JPA 提供的一种将查询语句从方法体中独立出来，以供多个方法共用的功能。在代码中使用 @NamedQuery（或 @NamedNativeQuery）定义好查询语句，唯一要做的就是为该语句命名时，需要满足”DomainClass.methodName()”的命名规则。

```java
@Entity
@Table(name="t_user")
@NamedQueries({
        @NamedQuery(name="findAllUser",query="SELECT u FROM User u"),
        @NamedQuery(name="findUserWithId",query="SELECT u FROM User u WHERE u.id = ?1"),
        @NamedQuery(name="findUserWithName",query="SELECT u FROM User u WHERE u.name = :name")
         
})
public class User{}
```

```java
public interface UserDao extends Repository<AccountInfo, Long> { 
 
...... 
   
public List<AccountInfo> findTop5(); 
}
```
- 如果希望为 findTop5() 创建命名查询，并与之关联，我们只需要在适当的位置定义命名查询语句，并将其命名为 "AccountInfo.findTop5"，框架在创建代理类的过程中，解析到该方法时，优先查找名为 "AccountInfo.findTop5" 的命名查询定义，如果没有找到，则尝试解析方法名，根据方法名字创建查询。

### 创建查询的顺序

pass

### CrudRepository自带方法

```java
// 保存实体对象
<S extends T> S save(S entity)

// 保存所有给定的实体对象
<S extends T> Iterable<S> saveAll(Iterable<S> entities)

// 按照id查询，无结果返回空
Optional<T> findById(ID id)

// 判断id是否存在，返回true false
boolean existsById(ID id)

// 返回所有实体对象
Iterable<T> findAll()

// 指定多个id查询
Iterable<T> findAllById(Iterable<ID> ids)

// 返回可获取的实体数量
long count()

// 删除指定id的实体
void deleteById(ID id)

// 删除指定实体
void delete(T entity)

// 删除指定的多个实体
void deleteAll(Iterable<? extends T> entities)

// 删除表中所有数据
void deleteAll()
```

### Repository 定义方法参考


```java
// 自定义方法中的find，get，read是等效的，在源码中这样匹配
private static final String QUERY_PATTERN = "find|read|get|query|stream";

// remove和delete也是等效的，同理
private static final String DELETE_PATTERN = "delete|remove";
```

```java
// 多条件查询
findByUsernameAndPassword(String user, Striang pwd)；findByUsernameOrAddress(String user, String addr)；
findByUsernameNot(String user)；

// 多值查询，方法的参数可以是 Collection 类型，也可以是数组或者不定长参数；
findByUsernameIn(Collection userList);
findByUsernameNotIn(Collection userList);

// 查询字段所有不同的值
List<People> findDistinctByNameNotIn(List<String> names);

@Query("SELECT DISTINCT a.city FROM Address a")
List<String> findDistinctCity();

// 查询指定字段
String findNamebyId(Integer id);

// 更新
int updateUserNameById(String name,Integer id);

// 删除
void deleteByKey(String key);

// 范围
findBySalaryBetween(int max, int min)；

// 比较
findBySalaryLessThan(int max)；
findBySalaryGreaterThan(int min)；

// 统计
long countByName(String name);

// 查询空值和非空值
findByUsernameIsNull()；
findByUsernameIsNotNull()；

// 查询字段存在
boolean existsByName(String name);

// 模糊查询
findByUsernameLike(String user)；
findByUsernameNotLike(String user)；

// 排序
findByUsernameOrderBySalaryAsc(String user)；
findByUsernameOrderBySalaryDesc(String user)；
findAllByOrderByIdAsc();  // ByOrderBy而不是OrderBy
findTop10ByOrderByLevelDesc();
findByOrderByProgDateAscStartTimeAsc();
// 调用时使用排序参数排序
repository.findAll(Sort.by(Sort.Direction.ASC, "seatNumber"));

// 分页查询
// 想要实现分页功能需要继承PagingAndSortingRepository或JpaRepository接口
// 首先是接口自带的方法
findAll(Pageable pageable)
findAll(Sort sort)
// 自定义方法
List<Product> findAllByPrice(double price, Pageable pageable);

// 分页排序
Page<Passenger> page = repository.findAll(PageRequest.of(0, 1, Sort.by(Sort.Direction.ASC, "seatNumber")));



```

### 分页查询

需要两步
- 创建或获取一个PageRequest对象，他是Pageable接口的实现
- 将PageRequest对象作为参数传入至继承了`PagingAndSortingRepository` 或 `JpaRepository` 接口的repository方法中，我们可以通过传入页面编号和页面信息条数来创建PageRequest对象。

```java
Pageable firstPageWithTwoElements = PageRequest.of(0, 2);
 
Pageable secondPageWithFiveElements = PageRequest.of(1, 5);

PageRequest.of(0, 1, Sort.by(Sort.Direction.ASC, "seatNumber"));

Page<Product> allProducts = productRepository.findAll(firstPageWithTwoElements);
 
List<Product> allTenDollarProducts = 
  productRepository.findAllByPrice(10, secondPageWithFiveElements);

```
- 通过将Pageable类型的实例传递给repository方法，实现分页查询。
- 自定义的repository方法可以指定返回的类型为`Page<T>`,  `Slice<T>` 或 `List<T>`
- 自带的findAll默认返回 `Page<T>` 对象 findAll(Pageable pageable) 
- 返回的`Page<T>`实例会统计总页数，为了减去统计页数的开销，可以指定用`Slice<T>` 或 `List<T>`返回结果;使用Slice会自动查询是否有下一页，但至少不会统计总页数。

```java
// 分页排序

// 只排序
Page<Product> allProductsSortedByName = productRepository.findAll(Sort.by("name"));

// 将排序细节传入PageRequest对象即可实现分页排序
Pageable sortedByName = 
  PageRequest.of(0, 3, Sort.by("name"));
 
Pageable sortedByPriceDesc = 
  PageRequest.of(0, 3, Sort.by("price").descending());
 
// 多字段排序方法
Pageable sortedByPriceDescNameAsc = 
  PageRequest.of(0, 5, Sort.by("price").descending().and(Sort.by("name")));

```

参考地址

- https://www.ibm.com/developerworks/cn/opensource/os-cn-spring-jpa/index.html
- https://blog.csdn.net/qq_36666651/article/details/80719259
- https://stackoverflow.com/questions/39869707/what-is-the-difference-between-query-methods-find-by-read-by-query-by-and-get
- https://docs.spring.io/spring-data/commons/docs/current/api/org/springframework/data/repository/CrudRepository.html
- https://www.baeldung.com/spring-data-sorting
- https://www.baeldung.com/spring-data-jpa-pagination-sorting




