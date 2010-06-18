# Aspect Oriented Programming with AspectJ

	<aop:config>
		<aop:advisor advice-ref="txAdvice" pointcut="execution(* com.acme.ServiceImpl.*(..))" order="1"/>
	</aop:config>
	
## Pointcuts

A pointcut is a set of join points defined with a query. Whenever the program execution reaches such a join point, a piece of code, the *advice* is executed.

	* com.acme.ServiceImpl.*(..)
	
Every method with every access modifier (public, private, ..) on the class `de.q2web.cmdb.soa.CrudServiceImpl` with every parameter.
