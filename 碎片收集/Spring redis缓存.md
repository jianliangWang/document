# Spring redis缓存

名称    解释
Cache    缓存接口，定义缓存操作。实现有：RedisCache、EhCacheCache、ConcurrentMapCache等
CacheManager    缓存管理器，管理各种缓存（cache）组件
@Cacheable    主要针对方法配置，能够根据方法的请求参数对其进行缓存
@CacheEvict    清空缓存
@CachePut    保证方法被调用，又希望结果被缓存与@Cacheable区别在于是否每次都调用方法，常用于更新
@EnableCaching    开启基于注解的缓存
keyGenerator    缓存数据时key生成策略
serialize    缓存数据时value序列化策略
@CacheConfig    统一配置本类的缓存注解的属性

```java
@CachePut(cacheNames = "dept",key = "#result.id")
    @Override
    public Dept addDept(Dept dept) {
        deptMapper.addDept(dept);

        return dept;
    }

    @CacheEvict(cacheNames = "dept")
    @Override
    public Integer deleteById(Integer id) {
        Integer result=deptMapper.deleteById(id);
        return result;
    }
    @CachePut(cacheNames = "dept",key = "#result.id")
    @Override
    public Dept updateDeptById(Dept dept) {
        deptMapper.updateDeptById(dept);
        return dept;
    }

    @Cacheable(cacheNames = "dept",key = "#id")
    @Override
    public Dept findAllDept(Integer id) {
        return deptMapper.findAllDept(id);
    }
```
