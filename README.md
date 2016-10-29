# 权限管理</br>
## 查询test1可以查看的页面:共19条记录  
查询语句：  
<pre>
select d.MenuNo,d.MenuName  
from sys_menu d  
where d.MenuID in  
	(select c.PrivilegeAccessKey  
from cf_privilege c  
where c.PrivilegeAccess='Sys_Menu'   
and c.PrivilegeMaster='CF_User'  
and c.PrivilegeMasterKey=  
			(select distinct a.UserID  
		from cf_user a  
		where a.LoginName='test1'))  
union   
select d.MenuNo,d.MenuName  
from sys_menu d  
where d.MenuID in  
	(select c.PrivilegeAccessKey  
from cf_privilege c  
where c.PrivilegeAccess='Sys_Menu'   
and c.PrivilegeMaster='CF_Role'  
and c.PrivilegeMasterKey=  
			(select distinct b.RoleID  
		from cf_userrole b  
		where b.UserID=  
					(select distinct a.UserID  
				from cf_user a  
				where a.LoginName='test1')))  
</pre>


![查询test1可以查看的页面](images/1.PNG)  
<p></p>
## 伪代码：
1.	根据用户的登录名test1在用户表里查对应的userid  
2.	根据userid去权限表里查对应的访问人类型为user、访问对象类型为Menu的对应的Menuid  
3.	根据Menuid去菜单（页面）表里查对应的菜单（页面）名称Menuname</br>
4.	 根据用户的登录名test1在用户表里查对应的userid</br>
5.	根据userid去用户角色表里查对应的roleid</br>
6.	根据roleid去权限表里查对应的访问人类型为role、访问对象类型为Menu的对应的Menuid</br>
7.	根据Menuid去菜单（页面）表里查对应的菜单（页面）名称Menuname</br>
8.	将第三步得到的菜单（页面）名称与第七步得到的菜单（页面）名称取并集</br>
<p></p>
<p></p>

## 查询test1可以对order页面进行的操作:共4条记录  
查询语句：
<pre>
select e.BtnID,e.BtnName,e.BtnNo,e.BtnIcon,e.MenuNo  
from sys_button e  
where e.MenuNo=  
(select d.MenuNo  
from sys_menu d  
where d.MenuName='订单' )  
and e.BtnID in  
(select c.PrivilegeAccessKey  
from cf_privilege c  
where c.PrivilegeAccess='Sys_button'  
and c.PrivilegeOperation='Permit'  
and c.PrivilegeMaster='CF_User'  
and c.PrivilegeMasterKey=  
			(select a.UserID  
			from cf_user a   
			where a.LoginName='test1'))  
UNION  
select e.BtnID,e.BtnName,e.BtnNo,e.BtnIcon,e.MenuNo  
from sys_button e  
where e.MenuNo=  
(select d.MenuNo  
from sys_menu d  
where d.MenuName='订单' )  
and e.BtnID in  
(select c.PrivilegeAccessKey  
from cf_privilege c  
where c.PrivilegeAccess='Sys_button'  
and c.PrivilegeMaster='CF_Role'  
and c.PrivilegeMasterKey=  
			(select b.RoleID  
			from cf_userrole b   
			where b.UserID=  
				(select distinct a.UserID  
				from cf_user a  
				where a.LoginName='test1')))  
</pre>


![查询test1可以对order页面进行的操作](images/2.PNG)
<p></p>

## 伪代码：  
1.	根据用户的登录名test1在用户表里查对应的userid  
2.	根据userid去权限表里查对应的访问人类型为user、访问对象类型为button的对应的buttonid</br>
3.	根据页面名称order去菜单（页面）表里查对应的MenuNo</br>
4.	根据MenuNo以及buttonid去按钮表里面查对应的操作名称buttonname</br>
5.	根据用户的登录名test1在用户表里查对应的userid</br>
6.	根据userid去用户角色表里查对应的roleid</br>
7.	根据roleid去权限表里查对应的访问人类型为role、访问对象类型为button的对应的buttonid</br>
8.	根据页面名称order去菜单（页面）表里查对应的MenuNo</br>
9.	根据MenuNo以及buttonid去按钮表里面查对应的操作名称buttonname</br>
10.	将第四步得到的操作名称与第九步得到的操作名称最终取并集</br>


