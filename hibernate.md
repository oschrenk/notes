# Hibernate #

## Bean Conventions and Immutability ##

One of my pet peeves with Hibernate is that it needs setter methods to fill the Java object with content. As I'm very much for immutability, this was just a no-go for me. It turns out it was somewhat of misconception. You still need setter (and getter methods), but they access can be set to private, making them at least immutable on an API level

For some more details, have a look at this [article](http://www.javalobby.org/java/forums/t49288.html) 

## Using native queries ##

Issuing native sql queries

	sess.createSQLQuery("SELECT * FROM CATS").list();
	
will return a List of Object arrays (`Object[]`) with scalar values for each column. 

Hibernate will use `ResultSetMetadata` to deduce the actual order and types of the returned scalar values. To avoid that or to simply be more explicit in what is returned:

	sess.createSQLQuery("SELECT * FROM CATS")
	 .addScalar("ID", Hibernate.LONG)
	 .addScalar("NAME", Hibernate.STRING)
	 .addScalar("BIRTHDATE", Hibernate.DATE)
	This query specified:

This will return `Object` arrays, but now it will not use `ResultSetMetadata` but will instead explicitly get the `ID`, `NAME` and `BIRTHDATE` column as respectively a `Long`, `String` and a `Short` from the underlying resultset. This also means that only these three columns will be returned, even though the query is using `*` and could return more than the three listed columns.
