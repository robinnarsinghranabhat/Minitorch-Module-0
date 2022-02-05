## OOP : 
- `self` is the instance itself.
    Example : 
    ```
    class A:
        def __init__(self,..):
            print(self)
            self.a = 4
            self.b = 5

        def other_method(self):
            ...    

    a = A()
    print(a)

    ## ID's in both the PRINTS WILL REFER TO SAME THING
    ```
- A child class will search for methods inside itself, then along the parent classes. For Multiple parent classes having same attributes, it figures out throguth `method_order_resolution`

    ```
    class P:
        def method(self):
            pass
    
    class C(P):
        pass
    ```
    `C().method()` will invoke parent's `method` <br>
    Same goes for magic-methods (dunder methods) as well. 

- super() : 
    super simply allows an object to access method from it's `parent` class. [Good explanation by Raymond Hettinger](https://rhettinger.wordpress.com/2011/05/26/super-considered-super/).

    In example below, after creating the instance of `B`, 
    the assignment statement : `self.dummy = 5` will invoke 
    B's `__setattr__` magic method. (which is how setattr works !).

    Inside B's __setattr__ , it's further calling super().__setattr__, which invokes the same method as defined in parent A. From here, we again invoke super(), and that calls the __setattr__ of Base `object` class. 

    As before, when a class has multiple parents, The super() figures out which parent class to use, based on `method_order_resolution`. 

    To find the order : `print(B.__mro__)`
    ```
    class A:
        def __init__(self):
            print('parent : init')
        def __setattr__(self, key, val):
            print('this is self from B now inside A: ',self)
            print('parent : assigning ...')
            super().__setattr__(key, val)
            print('parent : completed')


    class B(A):
        def __init__(self):
            print('This is instance of B aka self : ',  self )
            self.dummy = 5
        def __setattr__(self, key, val):
            print('child : assigning ...')
            super().__setattr__(key, val)
            print('child : completed')
    
    b = B()
    ```