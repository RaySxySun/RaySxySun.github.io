---
layout: post
title: Design Patterns - Composite
date: 2016-04-26
weather: sunny
categories: Design 
tags: [Design]
description: 
---

> [Design Patterns Index](http://raysxysun.github.io/categories/#Design)

# 8. Composite 

- **Intent**: 
	- Compose objects into tree structures to represent part-whole hierarchies. Composite lets clients treat individual objects and compositions of objects uniformly.
- **Applicability**:
	- you want to represent part-whole hierarchies of objects.
	- you want clients to be able to ignore the difference between compositions of objects and individual objects. Clients will treat all objects in the composite structure uniformly.

- **Structure**:	

<img src="{{ site.url }}/assets/img/2016-04-18-DesignPatterns/Composite.png" alt="{{ page.title }} at {{ site.title }}">
<img src="{{ site.url }}/assets/img/2016-04-18-DesignPatterns/CompositeTypical.png" alt="{{ page.title }} at {{ site.title }}">

- **Participants**:
	- Component 
		- declares the interface for objects in the composition.
		- implements default behavior for the interface common to all classes, as appropriate.
		- declares an interface for accessing and managing its child components.
		- (optional) defines an interface for accessing a component's parent in the recursive structure, and implements it if that's appropriate.
	- Leaf
		- represents leaf objects in the composition. A leaf has no children.
		- defines behavior for primitive objects in the composition.
	- Composite 
		- defines behavior for components having children.
		- stores child components.
		- implements child-related operations in the Component interface.
	- Client
		- manipulates objects in the composition through the Component interface.

- **Collaborations**
	- Clients use the Component class interface to interact with objects in the composite structure. If the recipient is a Leaf, then the request is handled directly. If the recipient is a Composite, then it usually forwards requests to its child components, possibly performing additional operations before and/or after forwarding.

- **Consequences**:
	- 1.defines class hierarchies consisting of primitive objects and composite objects.
	- 2.makes the client simple.
	- 3.makes it easier to add new kinds of components.
	- 4.can make your design overly general.

- **Related Patterns**:
	- Often the component-parent link is used for a Chain of Responsibility
	- Decorator is often used with Composite.
		- When decorators and composites are used together, they will usually have a common parent class. So decorators will have to support the Component interface with operations like Add, Remove, and GetChild.
	- Flyweight lets you share components, but they can no longer refer to their parents.
	- Iterator can be used to traverse composites.
	- Visitor localizes operations and behavior that would otherwise be distributed across Composite and Leaf classes.

- Code Sample: Composite

<img src="{{ site.url }}/assets/img/2016-04-18-DesignPatterns/Compositeample.png" alt="{{ page.title }} at {{ site.title }}">	

		public class TreeNode {  
		      
		    private String name;  
		    private TreeNode parent;  
		    private Vector<TreeNode> children = new Vector<TreeNode>();  
		      
		    public TreeNode(String name){  
		        this.name = name;  
		    }  
		  
		    public String getName() {  
		        return name;  
		    }  
		  
		    public void setName(String name) {  
		        this.name = name;  
		    }  
		  
		    public TreeNode getParent() {  
		        return parent;  
		    }  
		  
		    public void setParent(TreeNode parent) {  
		        this.parent = parent;  
		    }  
		      
		    public void add(TreeNode node){  
		        children.add(node);  
		    }  
		      
		    public void remove(TreeNode node){  
		        children.remove(node);  
		    }  
		      
		    public Enumeration<TreeNode> getChildren(){  
		        return children.elements();  
		    }  
		}  

		public class Tree {  
		  
		    TreeNode root = null;  
		  
		    public Tree(String name) {  
		        root = new TreeNode(name);  
		    }  
		  
		    public static void main(String[] args) {  
		        Tree tree = new Tree("A");  
		        TreeNode nodeB = new TreeNode("B");  
		        TreeNode nodeC = new TreeNode("C");  
		          
		        nodeB.add(nodeC);  
		        tree.root.add(nodeB);  
		        System.out.println("build the tree finished!");  
		    }  
		}  

> [Design Patterns Index](http://raysxysun.github.io/categories/#Design)