---
layout: post
title: Design Patterns - Abstract Factory
date: 2016-04-19
weather: cloudy
categories: Design 
tags: [Design]
description: 
---

> [Design Patterns Index](http://raysxysun.github.io/categories/#Design)

# 1. Abstract Factory

<a name="Abstract_Factory"></a>

### 1. Abstract Factory

- **Intent**: 
	- Provide an interface for creating families of related or dependent objects without specifying their concrete classes.

- **Also Known As**:
	- InterViews uses the "Kit" suffix to denote AbstractFactory classes. It defines WidgetKit and DialogKit abstract factories for generating look-and-feel-specific user interface objects.

- **Applicability**:
	- a system should be independent of how its products are created, composed, and represented.
	- a system should be configured with one of multiple families of products.
	- a family of related product objects is designed to be used together, and you need to enforce this constraint.
	- you want to provide a class library of products, and you want to reveal just their interfaces, not their implementations.
- **Related Patterns**:
	- Factory Method
	- Prototype
	- Singleton

<img src="{{ site.url }}/assets/img/2016-04-18-DesignPatterns/AbsFactory.png" alt="{{ page.title }} at {{ site.title }}">

- **Participants**:
	- AbstractFactory (WidgetFactory)
		- declares an interface for operations that create abstract product objects.
	- ConcreteFactory (MotifWidgetFactory, PMWidgetFactory)
		- implements the operations to create concrete product objects.
	- AbstractProduct (Window, ScrollBar)
		- declares an interface for a type of product object.
	- ConcreteProduct (MotifWindow, MotifScrollBar)
		- defines a product object to be created by the corresponding concrete factory.
		- implements the AbstractProduct interface.
	- Client
		- uses only interfaces declared by AbstractFactory and AbstractProduct classes.

- **Consequences**:
	- It isolates concrete classes.
	- It makes exchanging product families easy.
	- It promotes consistency among products.
	- Supporting new kinds of products is difficult.

- **Sample**: 

> We'll apply the Abstract Factory pattern to creating the mazes

		//Class MazeFactory can create components of mazes. It builds rooms, walls, and
	doors between rooms.

		class MazeFactory {
		public:
			MazeFactory();

			virtual Maze* MakeMaze() const
			{ return new Maze; }
			virtual Wall* MakeWall() const
			{ return new Wall; }
			virtual Room* MakeRoom(int n) const
			{ return new Room(n); }
			virtual Door* MakeDoor(Room* r1, Room* r2) const
			{ return new Door(r1, r2); }
		};

		//CreateMaze hard-codes the class names, making it difficult to create mazes with different components.	
		//Here's a version of CreateMaze that remedies that shortcoming by taking a MazeFactory as a parameter:

		Maze* MazeGame::CreateMaze (MazeFactory& factory) {
			Maze* aMaze = factory.MakeMaze();
			Room* r1 = factory.MakeRoom(1);
			Room* r2 = factory.MakeRoom(2);
			Door* aDoor = factory.MakeDoor(r1, r2);
			aMaze->AddRoom(r1);
			aMaze->AddRoom(r2);
			r1->SetSide(North, factory.MakeWall());
			r1->SetSide(East, aDoor);
			r1->SetSide(South, factory.MakeWall());
			r1->SetSide(West, factory.MakeWall());
			r2->SetSide(North, factory.MakeWall());
			r2->SetSide(East, factory.MakeWall());
			r2->SetSide(South, factory.MakeWall());
			r2->SetSide(West, aDoor);
			return aMaze;
		}

		//We can also create EnchantedMazeFactory, a factory for enchanted mazes, by subclassing MazeFactory.
		class EnchantedMazeFactory : public MazeFactory {
			public:
			EnchantedMazeFactory();

			virtual Room* MakeRoom(int n) const
			{ return new EnchantedRoom(n, CastSpell()); }

			virtual Door* MakeDoor(Room* r1, Room* r2) const
			{ return new DoorNeedingSpell(r1, r2); }

			protected:
			Spell* CastSpell() const;
		};

		//suppose we want to make a maze game in which a room can have a bomb set in it.
		class BombedMazeFactory : public MazeFactory {
			public:
			BombedMazeFactory();

			Wall* BombedMazeFactory::MakeWall () const 
			{ return new BombedWall;}

			Room* BombedMazeFactory::MakeRoom(int n) const 
			{return new RoomWithABomb(n);}
			
			protected:
			Spell* CastSpell() const;
		};

		//To build a simple maze that can contain bombs
		MazeGame game;
		BombedMazeFactory factory;
		game.CreateMaze(factory);


> Java Implementation:

		// Interface
		public interface AbstractFactory {  
		    public ProductA factoryA();  
		    public ProductB factoryB();  
		}  

		public interface ProductA {  
			...
		} 

		public interface ProductB {  
			...
		}  

		//Concrete Classes 
		public class ConcreateFactory1 implements AbstractFactory {  
	  
		    @Override  
		    public ProductA factoryA() {   
		        return new ConcreateProductA1();  
		    }  
		      
		    @Override  
		    public ProductB factoryB() {  
		        return new ConcreateProductB1();  
		    }  
	  
		}  

		public class ConcreateFactory2 implements AbstractFactory {  
	  
		    @Override  
		    public ProductA factoryA() {  
		        return new ConcreateProductA2();  
		    }  
		   
		    @Override  
		    public ProductB factoryB() {  
		        return new ConcreateProductB2();  
		    }  
		} 

		public class ConcreateProductA1 implements ProductA {  
			...
		} 
		public class ConcreateProductA2 implements ProductA {  
			...
		}  
		public class ConcreateProductB1 implements ProductB {  
			...
		} 
		public class ConcreateProductB2 implements ProductB {  
			...
		}  

> [Design Patterns Index](http://raysxysun.github.io/categories/#Design)