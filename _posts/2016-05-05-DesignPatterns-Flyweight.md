---
layout: post
title: Design Patterns - Flyweight
date: 2016-05-05
weather: rainy
categories: Design 
tags: [Design]
description: 
---

> [Design Patterns Category](http://raysxysun.github.io/categories/#Design)

> [Design Patterns Index](http://raysxysun.github.io/design/2016/04/18/DesignPatterns/)

# 11. Flyweight 

- **Intent**: 
	- Use sharing to support large numbers of fine-grained objects efficiently.	

	> What is the difference between coarse-grained and fine-grained?	
	
	> Granularity is the extent to which a system is broken down into small parts, either the system itself or its description or observation. It is the extent to which a larger entity is subdivided. For example, a yard broken into inches has finer granularity than a yard broken into feet.

	> Coarse-grained - larger components than fine-grained, large subcomponents. Simply wraps one or more fine-grained services together into a more coarse­-grained operation.
	
	> Fine-grained - smaller components of which the larger ones are composed, lower­level service

<img src="{{ site.url }}/assets/img/2016-04-18-DesignPatterns/Granularity.png" alt="{{ page.title }} at {{ site.title }}">

- **Applicability**:
	- An application uses a large number of objects.
	- Storage costs are high because of the sheer quantity of objects.
	- Most object state can be made extrinsic.
	- Many groups of objects may be replaced by relatively few shared objects once extrinsic state is removed.
	- The application doesn't depend on object identity. Since flyweight objects may be shared, identity tests will return true for conceptually distinct objects.

- **Structure**:	

<img src="{{ site.url }}/assets/img/2016-04-18-DesignPatterns/Flyweight1.png" alt="{{ page.title }} at {{ site.title }}">

<img src="{{ site.url }}/assets/img/2016-04-18-DesignPatterns/Flyweight2.png" alt="{{ page.title }} at {{ site.title }}">

- **Participants**:
	- Flyweight
		- declares an interface through which flyweights can receive and act on extrinsic state.
	- ConcreteFlyweight
		- implements the Flyweight interface and adds storage for intrinsic state, if any. A ConcreteFlyweight object must be sharable. Any state it stores must be intrinsic; that is, it must be independent of the ConcreteFlyweight object's context.
	- UnsharedConcreteFlyweight	
		- not all Flyweight subclasses need to be shared. The Flyweight interface enables sharing; it doesn't enforce it. It's common for UnsharedConcreteFlyweight objects to have ConcreteFlyweight objects as children at some level in the flyweight object structure
	- FlyweightFactory
		- creates and manages flyweight objects.
		- ensures that flyweights are shared properly. When a client requests a flyweight, the FlyweightFactory object supplies an existing instance or creates one, if none exists.
	- Client
		- maintains a reference to flyweight(s).
		- computes or stores the extrinsic state of flyweight(s).
- **Collaborations**
	- State that a flyweight needs to function must be characterized as either intrinsic or extrinsic. Intrinsic state is stored in the ConcreteFlyweight object; extrinsic state is stored or computed by Client objects. Clients pass this state to the flyweight when they invoke its operations.
	- Clients should not instantiate ConcreteFlyweights directly. Clients must obtain ConcreteFlyweight objects exclusively from the FlyweightFactory object to ensure they are shared properly.

- **Consequences**:
	- the reduction in the total number of instances that comes from sharing
	- the amount of intrinsic state per object
	- whether extrinsic state is computed or stored.

> Flyweights may introduce run-time costs associated with transferring, finding, and/or computing extrinsic state, especially if it was formerly stored as intrinsic state. However, such costs are offset by space savings, which increase as more flyweights are shared.


- **Related Patterns**:
	- The Flyweight pattern is often combined with the **Composite** pattern to implement a logically hierarchical structure in terms of a directed-acyclic graph with shared leaf nodes.
	- It's often best to implement **State** and **Strategy** objects as flyweights.

- Code Sample: Flyweight

<img src="{{ site.url }}/assets/img/2016-04-18-DesignPatterns/FlyweightSample1.png" alt="{{ page.title }} at {{ site.title }}">	

<img src="{{ site.url }}/assets/img/2016-04-18-DesignPatterns/FlyweightSample2.png" alt="{{ page.title }} at {{ site.title }}">	

		public class ConnectionPool {  
		      
		    private Vector<Connection> pool;  
		      
		    /*intrinsic state*/  
		    private String url = "jdbc:mysql://localhost:3306/test";  
		    private String username = "root";  
		    private String password = "root";  
		    private String driverClassName = "com.mysql.jdbc.Driver";  
		  
		    private int poolSize = 100;  
		    private static ConnectionPool instance = null;  
		    Connection conn = null;  
		  
		    /*constructor*/  
		    private ConnectionPool() {  
		        pool = new Vector<Connection>(poolSize);  
		  
		        for (int i = 0; i < poolSize; i++) {  
		            try {  
		                Class.forName(driverClassName);  
		                conn = DriverManager.getConnection(url, username, password);  
		                pool.add(conn);  
		            } catch (ClassNotFoundException e) {  
		                e.printStackTrace();  
		            } catch (SQLException e) {  
		                e.printStackTrace();  
		            }  
		        }  
		    }  
		  
		    /* destroy */  
		    public synchronized void release() {  
		        pool.add(conn);  
		    }  
		  
		    /* getter */  
		    public synchronized Connection getConnection() {  
		        if (pool.size() > 0) {  
		            Connection conn = pool.get(0);  
		            pool.remove(conn);  
		            return conn;  
		        } else {  
		            return null;  
		        }  
		    }  
		} 


> [Design Patterns Category](http://raysxysun.github.io/categories/#Design)

> [Design Patterns Index](http://raysxysun.github.io/design/2016/04/18/DesignPatterns/)