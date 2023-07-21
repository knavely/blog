---
layout: default
---
I have always felt that **Generating Functions(GF)** are like a programming language. Except instead of being about transistors and turning bits on and off, they are hacking the invisible and formless Mathematical Mechanics of Algebra and Analysis that exists formlessly in the ether. So, I thought I'd write a little tutorial about GF as if it were about learning a new programming language. Rather than breathlessly trying to motivate the reader with snazzy applications (a hard thing to keep myself from doing): 
> My goal is to explain the magic from first principles. 

**image ?**

**Disclaimer** _I know next to nothing about Dynamical Systems, and precise definitions of what entails an actual form of computation or for that matter Programming Language (PL). This blog is just purely my pontificating about the connections with PL based on my experience as a programmer and with GF_   

# Why are they called Generating Functions?
This is the first aspect that I think a lot of people skip over. In fact, I am not sure I have ever had it explained to me. Once you get it, the name makes sense, but before then one's imagination can easily get the better of them. If you are not familiar with these examples, then actually that's a benefit. 

*  ~~Generators of algebraic group~~
*  ~~Actually Generate Some type of programming object~~ We will at least mention how they can be used in this context though
*  It's called this because: We write down some algebra that corresponds to an identity we are interested in, so that when we group like terms, (just like in high-school) the basic mechanics of arithmetic **generate** coefficients which tell us valuable information about this identity.    


# Time to Roll the Dice
``Write some code in the programming language of your choice, or calculate otherwise, the expected sum of rolling two dice in an Mystical RPG. One is 3 sided and the other is 4 sided, except that it is magic, and sometimes(with uniform probability) it dissapears when rolled.``

We need to figure out two things: the total number of outcomes (denominator), and the total number of outcomes for each particular sum (numerator).

Watch carefully, this might seem like magic, but I will go back and explain. 
1.  Allocate Dice one: $$D_1 := x + x^2 + x^3$$. 
   *  The first die has 3 sides   
2.  Allocate Dice two: $$D_2 := x^0 + x + x^2 + x^3 + x^4$$
   * The second has 4 sides, but it also disappears so we introduce $$x^0$$
3.  Express: $$\text{Roll the Dice}:= D_1 \cdot D_2$$
   * Rolling both dice corresponds to multiplying them together!
4.  Compile: $$D_1 \cdot D_2 = (x + x^2 + x^3) \cdot (1 + x + x^2 + x^3 + x^4)$$
	$$=(x\cdot1 + xx + xx^2 + xx^3 + xx^4) + (x^2 + x^2x + x^2x^2 + x^2x^3 + x^2x^4) + (x^3 + x^3x + x^3x^2 + x^3x^3 + x^3x^4)$$. Why am I doing this? No, I didn't forget how to multiply polynomials, but its here in this step that the important mechanics happen, and I want to proceed in slow motion so we don't miss it. Pay attention to what happens in the exponent here
	$$=(x^{1+0} + x^{1+1} + x^{1+2} + x^{1+3} + x^{1+4}) + (x^{2+0} + x^{2+1} + x^{2+2} + x^{2+3} + x^{2+4}) + (x^{3+0} + x^{3+1} + x^{3+2} + x^{3+3} + x^{3+4})$$
	
	Thats right **each exponent corresponds to the sum of the two dice. When we combine like terms, the coefficients give the corresponding number of outcomes**
	
	

With that out of the way, lets just jump in. I'm sure we are all familiar with **Vectors** or **Arrays**.  The following line of **C** allocates an array of 10 integers on the stack and initializes them all to 1.
```c
int an_array[10];
for(int i = 0; i < 10; ++i)
  an_array[i] = 1;
 ```
The reason I start here is because I suspect that right off the bat many people get confused when they see a **formal power series** like  
## $$c_0x^0+c_1x^1+c_2x^2+c_3x^3+c_4x^4 + \ldots $$
. Their mind jumps to High School trauma, _yuk SOLVE FOR x!!_ But that is not what we are doing here. $$x$$ is just an indeterminate, which is just a fancy term for a  **black box placeholder** that takes no value. Again, we are not trying to solve for $$x$$ but 
# It's an Indexable Data Structure
Which allows us to store things (the $$c_i$$) at it's _err memory-less_ locations ***just like an array, or linked list***:

*   Its potentially infinite in size. In fact its often easier/less messy to just deal with infinite series up front, and cherry pick the finite parts we care about later
*   There is no actual storage cost, because uhhhh its math not computers!
*   The indexes are the $$x^k$$. 
* Let $$G = 10 + x^0 + x^1 + x^2 + 5x^3$$ 
    * $$G[x^0] = 10$$ 
	* $$G[x^1] = 1$$
	* $$G[x^2] = 1$$
	* $$G[x^3] = 5$$
	* Guess: What do you think is $$G[x^6] = ??$$ 
	* Hint: We consider $$G$$ to be infinite even though it only has as few terms
	
The [] style indexes aren't something I can take credit for here _even though they fit so perfectly with this yarn I'm spinning_. Mathematicians actually write it like this. It's standard notation, though they think of it as denoting the "coefficient of $$x^k$$ in $$G$$" rather than the number that is stored at that spot in a data-structure, but my point is that its really the same thing. (btw $$G[x^6] = 0$$ is the answer to the above. ***There are no Segfaults in Math!!***)
## More Examples

# Compilation Step
Most programmers are aware that when we compile our code, there is an intermediary step where what we wrote gets compressed/expanded into machine code, the 1s and 0s that ultimately translate into the instructions read by the CPU (or GPU) when the thing is actually run. You might find it surprising however that working with Generating Functions also involves a sort of similar step!




This is my first real attempt at any sort of blog. Gonna keep these entries short, so that it's possible for me to actually complete them, and for you to read them. Speaking of the reader, I am assuming you are an enthusiastic programmer, familiar with things like trendy PL features, but I assume close to 0 mathematical background, other than perhaps the vague interest which has you here in the first place, which I feel is fair game.


## Why am I writing a blog about Generating Functions for Programmers ?
I have been completely captivated since I first laid eyes on them. I find the whole concept spectacularly mind blowing:
*   You take a summation (it can be infinite), like, a reduction that sums together a bunch of terms,  hack it in some sort of time suspended mid execution to intercept coefficients, which contain incredibly valuable information. Then in the next moment say _haha its actually just a summation after all, run it!_ Set $$x \leftarrow 1$$, and _let er rip_ to make other sorts of conclusions about the universe. 
For example lets compute the total number of subsets of $$\{1,2,3,\ldots,n\}$$ in C:

```c
// assume n < 64, notice each bit sequence maps to a subset: 00110 <--> {2,3} 
uint64 enumerate_subsets(uint64 n){
	uint64 count = 0;
	for(uint64 i = 0; i < (1 << n); ++i)
		++count;
	return count;
}	
 ```
 Now lets "program" it with generating functions on the machinery of math.
 
 We want to compute how many subsets there are of size 0,1,2,etc, then add them all together. We use $$x$$ to ``suspend the computation'':
 $$\binom{n}{0}x^0 + \binom{n}{1}x^1 + \ldots + \binom{n}{n}x^n = \sum_{k=0}^{n}\binom{n}{k}x^k$$
 Notice that if we plug in $$x \leftarrow 1$$, the above gives us exactly the expression we are trying to compute. But we don't do that quite yet. First we make use of the algebraic identity (OK its the binomial theorem, if you don't know how this library works, just bootstrap accept it right now): 
 
 **Import Binomial Theorem** $$\sum_{k=0}^{n}\binom{n}{k}x^k = (1+x)^n$$.
 
Now we can set $$x = 1$$, to actually run the evaluation:
The number of subsets of $$\{1,2,\ldots,n\} = \sum_{k=0}^{n}\binom{n}{k} = 2^n$$.
But lets go back to the suspended computation, and notice that we are storing all of these binomial coefficients implicitly with just a few characters: $$(1+x)^n$$. We can think of this like a vector, where the kth index gives us the coefficient of $$x^k$$, namely, in this case $$\binom{n}{k}$$. In fact we use [ indexes ] to refer to the coefficients, just like we do with an array in C or numpy: $$(1+x)^n[x^k] = \binom{n}{k}$$.

* Just as there are way more uses for the flexibility of for-loops than this contrived C example can provide, the same is true for Generating Functions. In fact, this "water into snake oil" carves out a deep vertical right through various branches of Mathematics and CS. Probability, Statistics, Combinatorics, Analysis of Algorithms, Random object Generation, etc.. So its a useful tool.   

## Why are you reading a blog about Generating Functions for Programmers ?

## Programming Infinite Vectors


[back](./)
