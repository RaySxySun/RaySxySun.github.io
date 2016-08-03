---
layout: post
title: Effective Java - Item 2 Consider a builder
date: 2016-08-01
weather: rainy
categories: Java
tags: [Java]
description:
---

# Item 2: Consider a builder when faced with many constructor parameters

> Static factories and constructors **share a limitation**: they do not scale well to large numbers of optional parameters.

- **Problem**: the telescoping constructor pattern works, but it is hard to write client code when there are many parameters, and harder still to read it.


	- programmers have used the telescoping constructor pattern, in which you provide a constructor with only the required parameters, another with a single optional parameter, a third with two optional parameters, and so on.

				// Telescoping constructor pattern - does not scale well!
				public class NutritionFacts {
					private final int servingSize; // (mL) required
					private final int servings; // (per container) required
					private final int calories; //optional
					private final int fat; // (g) optional
					private final int sodium; // (mg) optional
					private final int carbohydrate; // (g) optional

					public NutritionFacts(int servingSize, int servings) {
						this(servingSize, servings, 0);
					}
					public NutritionFacts(int servingSize, int servings,
						int calories) {
						this(servingSize, servings, calories, 0);
					}
					public NutritionFacts(int servingSize, int servings, int calories, int fat) {
						this(servingSize, servings, calories, fat, 0);
					}
					public NutritionFacts(int servingSize, int servings, int calories, int fat, int sodium) {
						this(servingSize, servings, calories, fat, sodium, 0);
					}
					public NutritionFacts(int servingSize, int servings, int calories, int fat, int sodium, int carbohydrate) {
						this.servingSize = servingSize;
						this.servings = servings;
						this.calories = calories;
						this.fat = fat;
						this.sodium = sodium;
					this.carbohydrate = carbohydrate;
					}
				}

				NutritionFacts cocaCola = new NutritionFacts(240, 8, 100, 0, 35, 27);
	
	- Typically this constructor invocation will require many parameters that you don’t want to set, but you’re forced to pass a value for them anyway.


	- An alternative is the JavaBeans pattern
		- [implement] call a parameterless constructor to create the object and then call setter methods to set each required parameter and each optional parameter of interest
		- [+] This pattern has none of the disadvantages of the telescoping constructor pattern.
			- easy & wordy
		- [-] an inconsistent state
		- [-] the JavaBeans pattern precludes the possibility of making a class immutable


	- The third alternative that combines (a form of Builder pattern)
		- 1.calls a constructor (or static factory) with all of the required parameters and gets a builder object
		- 2.calls setter-like methods on the builder object to set each optional parameter of interest.
		- 3.calls a parameterless build method to generate the object, which is immutable

					// Builder Pattern
					public class NutritionFacts {
						private final int servingSize;
						private final int servings;
						private final int calories;
						private final int fat;
						private final int sodium;
						private final int carbohydrate;
						
						public static class Builder {
							// Required parameters
							private final int servingSize;
							private final int servings;
							// Optional parameters - initialized to default values
							private int calories = 0;
							private int fat = 0;
							private int carbohydrate = 0;
							private int sodium = 0;
							
							
							public Builder(int servingSize, int servings) {
								this.servingSize = servingSize;
								this.servings = servings;
							}

							public Builder calories(int val)
							{ calories = val; return this;}

							public Builder fat(int val)
							{ fat = val; return this; }
							
							public Builder carbohydrate(int val)
							{ carbohydrate = val; return this;}
							
							public Builder sodium(int val)
							{ sodium = val; return this;}

							public NutritionFacts build() {
								return new NutritionFacts(this);
							}
						}
						
						private NutritionFacts(Builder builder) {
							servingSize = builder.servingSize;
							servings = builder.servings;
							calories = builder.calories;
							fat = builder.fat;
							sodium = builder.sodium;
							carbohydrate = builder.carbohydrate;
						}
					}

					NutritionFacts cocaCola = new NutritionFacts.Builder(240, 8).calories(100).sodium(35).carbohydrate(27).build();

	
					----------------------------------------

					// a single generic type

					// A builder for objects of type T
					public interface Builder<T> {
						public T build();
					}

					//bounded wildcard type

					Tree buildTree(Builder<? extends Node> nodeBuilder) { ... }


	- Disadvantages (a form of Builder pattern)
		- [-] In order to create an object, you must first create its builder
		- [-] more verbose than the telescoping constructor pattern

	- Exceptions: You don’t get a compile-time error if the class has no accessible parameterless constructor.
		- [runtime]InstantiationException
		- [runtime]IllegalAccessException
		- IllegalStateException (build method should throw)
		- IllegalArgumentException (setter methods take entire groups of parameters)



> In summary, the Builder pattern is a good choice when designing classes whose constructors or static factories would have more than a handful of parameters, especially if most of those parameters are optional. Client code is much easier to read and write with builders than with the traditional telescoping constructor pattern, and builders are much safer than JavaBeans.