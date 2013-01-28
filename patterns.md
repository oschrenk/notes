# Patterns #

## DAO (Data Access Object) ##

(Business) Applications (almost) always have to persists data. This data can be stored and accessed in multiple ways - in case of an address book for instance you might use an LDAP or CalDav connection. There are multiple mechanisms, protocols and APIs.

DAOs abstract from these different data sources. A DAO manages the connection and hides the implementation details. A DAO does not only map one object to one table, it also allows for more complex mappings between complex data structures and tables depending on your needs.

### JAVA Example ###

	public interface CustomerDAO
	{

	  public void insert(Customer customer)
	   throws CustomerDAOException;

	  public void update(CustomerPK pk, Customer customer)
	   throws CustomerDAOException;

	  public void delete(CustomerPK pk)
	   throws CustomerDAOException;

	  public Customer[] findAll()
	   throws CustomerDAOException;

	  public Customer findByPrimaryKey(String email)
	   throws CustomerDAOException;

	  public Customer[] findByCompany(int companyId)
	   throws CustomerDAOException;
	}
