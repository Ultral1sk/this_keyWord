# this_keyWord

-- Here are some takeaways from the Javascript courses of Kyle Simpson about the THIS keyword which i think it is worth taking it into consideration.



Definition about the THIS keyword

-Every function, while executing, has a reference to its current execution context, called THIS.
-The THIS mechanism is Dynamic it is a binding mechanism that looks for things at RUNTIME it is entirely based on how we call something that is why we need to alwasys check the callsite and ask these four questions. We can have 4 different functions called in 4 different ways and receive 4 different bindings

There are multiple rules about the THIS keyword in the official Javascript Documentation but we are only gonna focus on 4 right now.
What we need to take into consideration is always the CALLSITE meaning where actually the functions are being called


function foo() {
      console.log(this.bar)
}

var bar = "bar1";
var o2  = { bar : "bar2" , foo : foo};
var o3  = { bar : "bar3" , foo : foo};

foo();                              // "bar1"
o2.foo();                           // "bar2"
o3.foo();                           // "bar3"

4 - DEFAULT Binding rule 
- Here the function foo() in the callsite it is just called normaly it is not attached to anything doesn not REFER to anything else it REFERS just to ITSELF that means that then the  DEFAULT Binding rule applies.
The same applies for the the IFFIE functions they are immediatelly called and they reffer only to themselves.
-In strict mode the this keyword in the default rule refers to UNDEFINED value if not in strict mode default the THIS keyword into the GLOBAL object



3 - IMPLICIT Binding Rule
- In this rule we need to take look at the callsite again but in this case at line 30 or o2.foo() where the function refers to the o2 variable which is a reference to a on object THEN the THIS keyword it's gonna point to o2 and that is why we get "bar2".
THe same thing will happen with o3.foo()



2 - EXPLICIT Binding Rule

function foo() {
      console.log(this.bar)
}

var bar = bar1
var obj = { bar : "bar2"};

foo();                  //"bar1"
foo.call(obj)           //"bar2"


This rule says if we use the methods .call or .apply at the callsite both of those utilities take as the first parameter the THIS keyword binding
In the example above 
foo() is going to accept the DEFAULT binding and the 
foo.call(obj) will say take obj as my THIS as we previously mentioned the call & apply methods take the this keyword as their first parameter
we are EXPLICITLY stating which Binding we want to occur


- Because the THIS Keyword is very flexible in javascript there is a possibility we do a function which is going to be the BINDER to any object we basically want, that function will have the ability to reference any obj and also EXPLICITLY also attach the THIS keyword.


Example of Kyle Simpson but there is also a another Example in MozilaFirefox which eh says is more flexible then this example meaning
if we find ourself in such a situation using this kind of method he preffered their example over his.
a function that takes a function as a param and an object returns a function which is going to bind the THIS keyword to the object
--
// if there isn't a bind to the prototype already add this bind
if( !Function.prototype.bind2 ) {
      Function.prototype.bind2 = 
            function(o) {
                  var fn = this;  // refers to the function which is going to be called

                  return function (){
                        return fn.apply(o, arguments);
                  };

            };
};


function foo(baz){
      console.log(this.bar + "" + baz);
}

var obj = { bar : "bar"};
foo= foo.bind2(obj)

foo("baz");

And here the IMPLICIT rule applies because in the callsite we are calling a function which is refering to an object and at the same time we are using our manually created THIS function so we know where our THIS keyword will be.
And also doing the EXPLICIT binding reduces our code flexibility

But right now in the newer Javascript Version we have the BIND method which is doing the same job.


1 - The New Keyword

there are 4 things that occur when the New keyword comes in front of a function call
1 - a brand new empty object is being created out of a thin air.
2 - The second thing that happes is that object which is created is linked to a DIFFERENT OBJECT.
3 - The same object gets bound as the THIS keyword for the purpouses of the function call.
4 - If that function with the new keyword does not return anything it will implicitly give the THIS keyword to the object. 

In plain english when we use the new keyword it doesn't matter what the function does the new keyword itself will add these 4 stuff we mentioned IMPLICITLY.


Precedence of the 4 Rules we've mentioned

1. Was the function called with the `new` keyword ?If yes take we need to use that object that means 
that the `new` keyword can overwrrite each of the mentioned rules above because it is the most precedent.
We need to take into consideration about the `new` keyword is when we do EXPLICIT binding the one where we create ourself a function to know where our THIS Keyword is the `new` rewrites that also which means itll always come first and the EXPLICIT binding function takes precedence at the second place

2. Was the function called with `call` or `apply` methods is there an explicit binding happening?
If yes we need to use that object.

3. Was the function called with an IMPLICIT binding rule was there an object to the function?
if yes we use that object

4. Default takes precedence
