

�������̣�
1.��ȷ����������ݿ��ṹ
2.������˽ӿ�
3.����ǰ��UI���
4.����ǰ��
5.����
6.����ʹ����Աʹ��
7.�����Ŀ�ĵ����ύ��������ר����

Ŀǰ���У�2.������˽ӿ�


���SpringBoot2.7����Swagger����
������Ϣ��
```
org.springframework.context.ApplicationContextException: Failed to start bean 'documentationPluginsBootstrapper'; nested exception is java.lang.NullPointerException
```
�����ʽ����`application.properties`���������
```properties
spring.mvc.pathmatch.matching-strategy=ant_path_matcher
```
������Ŀ������������ַ�鿴�ӿ��ĵ���
http://localhost:8888/swagger-ui/index.html



###### SpringBoot properties����
```properties
//������˹���
spring.mvc.static-path-pattern=/images/**
//���徲̬��Դ·��
spring.web.resources.static-locations=classpath:/static/
```

###### �ļ��ϴ�
����ʽѡ��form-data 
