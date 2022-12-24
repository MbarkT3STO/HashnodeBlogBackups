# Back to basics: Pre-Increment and Post-Increment in C#

In C#, you can put the increment ( ) and decrement ( — ) operators either before or after a variable. Placing an operator before a variable is called a prefix (pre-increment) and using an operator after a variable is called a post-increment (post-increment). Below are  examples:


```cs
i++; // post-increment (postfix)
++i; // pre-increment (prefix)
i--; // post-increment (postfix)
--i; // pre-increment (prefix)
``` 

## Difference Between Pre-Increment and Post-Increment in C-Sharp

There is no difference whether you use prefix or postfix form; the variable value will increase by 1. Then you must be wondering why there are two ways to do the same thing. Here is the answer, the value returned by i++ is the value of (i) before the increment takes place, whereas the value returned by ++i is the value of (i) after the increment takes place. The following is an example:

```cs
using System;

class Hello
{
    static void Main(string[] args)
    {
        int i, j;
        // post-increment example
        i = 0;
        j = i++; 
        Console.WriteLine("The value of j (i++): " + j);
        Console.WriteLine("The value of i (i++): " + i);
        // pre-increment example
        i = 0;
        j = ++i; 
        Console.WriteLine("The value of j (++i): " + j);
        Console.WriteLine("The value of i (++i): " + i);
    }
}
```

**Output**
- The value of j (i++): 0
- The value of i (i++): 1
- The value of j (++i): 1
- The value of i (++i): 1
- Press any key to continue . . .

You may also remember that in expression i, variable (i) comes first, so its value is used as the value of the expression before (i) is incremented. In the expression i, the operator comes first, so its operation is performed before the value of (i).