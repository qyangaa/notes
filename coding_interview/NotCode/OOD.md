### Object-Oriented Concepts

- Class 
- Object (instance) 
- Abstract
  - Create model for real world objects.
- Encapsulation
  - **Hiding data implemtation**, by restricting access 
- Inheritance
  - Share attributes/behaviors
  - call the class directly (`ChampionAhri ahri = new ChampionAhri() `)
- Polymorphism
  - Implement similar behaviors in different ways
  - Initiate parent class pointer but current class new object (`Bird bird = new Parrot()`)
  - equivalent to multiple polymorphic functions in one class
- Abstract class v.s. interface
  - Cannot create an instance for both
  - There can be non-abstract functions in abstract class, but **all functions in interface are abstract** 
  - Non-static variables can be declared in abstract class, but all variables in interface should be **static and final**
- Final: not changeable
  - Final class, final method, final variable
- Static: shared across objects of the same class
  - Static variable, static function