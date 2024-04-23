## Resouces
-[1��㶨SpringBoot+Vueȫջ����](https://www.bilibili.com/video/BV1nV4y1s7ZN/)

## Spring Boot Full Stack

### Spring Boot

###### IDEAʵ��������
-TO BE DONE


###### ʹ��Swagger ����Restful���API
1.�������
swagger2  swagger-ui
2.Apioperation��''��
3.������
4.����http://localhost:8080/swagger-ui.html

###### ORM
ORM(Object Relational Mapping)�����ϵӳ��
����������͹�ϵ�����ݿ�֮��Ĳ�ƥ������
MyBatis ���ݳ־ò�Ŀ��

������
1.MyBatis
2.Mysql
3.druid

###### Mybatis-plus
�����ѯ�����ݿ��ﲻ���ڸ��ֶΣ������ע�⣺
@TableField(exist = false)

��������ѯ������ʹ�ã�
[����ѯ](E:\MyGithubWeb\SpringBoot-FullStack\����ѯ.png)

���û��Ͷ���Ϊ����
һ���û���Ӧ���������һ��������ӦΨһһ���û���

���û���ʵ�����У���orders�������ֶΣ��������ݿ��в�û�С�����ͨ��
���û��в�ѯ���û������ж�����������UserMapper������д:
��ѯ�����û������ж���
@Select("select * from t_user)
@Results(
    {
        @Result(column="id", property="id"),
        @Result(column="username", property="username"),
        @Result(password="password", property="password"),
        @Result(birthday="birthday", property="birthday"),
        @Result(column="id", property="orders",
            many=@Many(select="com.example.demo.mapper.OrderMapper.selectByuid")
        )
    }
)
List<User> selectAllUserAndOrders();
����OrderMapper�е�selectByuid�����������û���id�����ظ��û������ж�����

��������OrderMapper
@Select("select * from t_order where uid=#{uid}")
List<Order> selectByuid(int uid);
��ѯ������ֵ�ύ�������orders���ԣ���ɸ�ֵ��

�ܽ�һ�£���ѯ�����û��������Լ��Ķ�����������Userʵ�����м�������orders��Ȼ����UserMapper��дһ����ѯ�����û������ǵĶ����ķ���������OrderMapper��дһ����ѯĳ���û������ж����ķ�����

������ѯ QueryWrapper
@GETMapping("/user/find")
public List<User> findByCond(){
    QueryWrapper<User> queryWrapper= new QueryWrapper();
    queryWrapper.eq("username","zhangsan");
    return userMapper.selectList(queryWrapper);
}

��ҳ��ѯ��
1.
@GETMapping("/user/findByPage")
public IPage findByPage(){
    Page<User> page = new Page<>(0,2);
    IPage ipage = userMapper.selectPage(page,null);
    return ipage;
}
2.����������
��ӷ�ҳ������

