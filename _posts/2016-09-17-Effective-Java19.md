---
layout: post
title: Effective Java - Item 19 Use interfaces only to define types
date: 2016-09-17
weather: sunny
categories: Java
tags: [Java]
description:
---

> Chapeter 4 Classes and Interfaces


# Item 19: Use interfaces only to define types

> When a class implements an interface, the interface serves as a type that can be used to refer to instances of the class.

- ### 1.constant interface: fails this test
	- [Conclustion] The constant interface pattern is a poor use of interfaces.
	- [What is it?] It contains no methods and consists solely of static final fields.
	- [Consequence] causes this implementation detail to leak into the class’s exported API
	- [Java platform libraries] such as java.io.ObjectStreamConstants
	- [Best Solution] If the constants are strongly tied to an existing class or interface, you should add them to the class or interface. **Example: export MIN_VALUE and MAX_VALUE constants**.
		- (1) export constants with enum type (item 30)
		- (2) export constants with a noninstantiable utility class (item 4)
						
						===========================Constant Interface===========================
						// Constant interface antipattern - do not use!
						public interface PhysicalConstants {
							// Avogadro's number (1/mol)
							static final double AVOGADROS_NUMBER = 6.02214199e23;
							
							// Boltzmann constant (J/K)
							static final double BOLTZMANN_CONSTANT = 1.3806503e-23;
							
							// Mass of the electron (kg)
							static final double ELECTRON_MASS = 9.10938188e-31;
						}
						==========================Constant utility class========================
						// Constant utility class
						package com.effectivejava.science;
						public class PhysicalConstants {
							private PhysicalConstants() { }
							// Prevents instantiation
							public static final double AVOGADROS_NUMBER = 6.02214199e23;
							public static final double BOLTZMANN_CONSTANT = 1.3806503e-23;
							public static final double ELECTRON_MASS = 9.10938188e-31;
						}
						-----------------------using constant utility class----------------------
						// a client should qualify constant names with a class name like 
						// PhysicalConstants.AVOGADROS_NUMBER .

						// [Alternative] as of release 1.5, using static import
						// Use of static import to avoid qualifying constants
						import static com.effectivejava.science.PhysicalConstants.*;

						public class Test {
							double atoms(double mols) {
								return AVOGADROS_NUMBER * mols;
							}
							...
							// Many more uses of PhysicalConstants justify static import
						}

---

- ### Conclusion
	- interfaces should be used only to define types. They should not be used to export constants.
	