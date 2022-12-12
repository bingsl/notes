```
ldapsearch -x -b "cn=bing,ou=Group,dc=pheicloud,dc=com"

ldapdelete -x -D "cn=Manager,dc=pheicloud,dc=com" -W "cn=bing,ou=Group,dc=pheicloud,dc=com"

ldapadd -x  -D "cn=Manager,dc=pheicloud,dc=com" -W -f /root/base1.ldif
```

```
参数说明：
x：指定使用简单授权
D：指定LDAP的管理区b
W:提示输入管理密码
最后的内容是指定删除的用户信息

```

```
LDAP简称对应
o– organization（组织-公司）
ou – organization unit（组织单元/部门）
c - countryName（国家）
dc - domainComponent（域名组件）
sn – suer name（真实名称）
cn - common name（常用名称）
dn - distinguished name（专有名称）
```





```
uid   唯一标识
cn: 名
sn: 姓
userPassword: 密码
telephoneNumber： 电话号码
facsimileTelephoneNumber 传真
postalAddress：邮寄地址
postalCode： 邮编
homePhone
homePostalAddress
givenName
jpegPhoto
mail
mobile
o
photo
roomNumber
```



创建数据库

* base.ldif

```
# replace to your own domain name for "dc=***,dc=***" section
dn: dc=pheicloud,dc=com
objectClass: top
objectClass: dcObject
objectClass: organization
o: School
dc: pheicloud

dn: ou=People,dc=pheicloud,dc=com
objectClass: top
objectClass: organizationalUnit
ou: People

dn: ou=Group,dc=pheicloud,dc=com
objectClass: top
objectClass: organizationalUnit
ou: Group


ldapadd -x -D "cn=Manager,dc=pheicloud,dc=com" -W -f ~/base.ldif
```

* 创建group soup.ldif

```
#create group
dn: cn=Teacher,ou=Group,dc=pheicloud,dc=com
objectClass: posixGroup
gidNumber: 1001
cn: Teacher

dn: cn=Student,ou=Group,dc=pheicloud,dc=com
objectClass: posixGroup
gidNumber: 1000
cn: Student

dn: cn=Manager,ou=Group,dc=pheicloud,dc=com
objectClass: posixGroup
gidNumber: 1002
cn: Manager
```

* 添加用户 user.ldif

```
# add user
dn: uid=bing2,ou=People,dc=pheicloud,dc=com
objectClass: top
objectClass: person
objectClass: b
objectClass: inetOrgPerson
cn: shuaibing2
sn: zheng2
uid: bing2
userPassword: 654321
mobile: 18358336400
mail: bing@pheicloud.com
```

* addusertogroup.ldif

```
#add user to group
dn: cn=Student,ou=Group,dc=pheicloud,dc=com
changetype: modify
add: memberuid
memberuid: bing2
```

