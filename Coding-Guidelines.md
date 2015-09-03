#### Coding Guidelines Rules
All changes should follow our adopted coding guidelines. A detailed description of our coding guidelines can be found here.
##### DO: Specify access modifiers on everything 
In TypeScript, class members are public by default. However, to improve readability (at the cost of conciseness) we will put accessibility modifiers on all members regardless of whether it’s strictly required. 
 
**Example**
```typescript  
class Example { 
    private a: string; 
    public b: number; 
   
    public doSomething(a: number): bool {  
        return a === 5; 
    } 
}
```

##### DO NOT: Prefix private field names with "_" or other decorations 
Field names should be simple camelCase names, and should not be prefixed with "_" or any other prefix. 
 
**Example** 
```typescript  
class Example { 
    private foo: string; 
    private myVariable: string; 
} 
```
##### DO: Use camelCase names for the newly created files


Same rule as for field names - do not use prefixes and use camelCase style of naming.

**Example** 
```
animatedNumber.ts
```
##### DO: Use the correct case for classes, modules, functions, variables, statics, etc.  
Follow these rules when specifying the case of the names of classes, modules, functions, and variables: 
 
1. Use PascalCase for class names 
2. Use camelCase for functions, modules, and variable names 
3. Use PascalCase for constants 
 
**Example**
```typescript   
module SomeModule { 
    class SomeClass { 
        private someVariable: string; 
        static someStaticVariable: string; 
        static SomeConstant = "SomeConstant"; 
    } 
    export someFunction() { 
    } 
} 
``` 
##### DO: Use type-safe variables 
One of the major benefits of TypeScript over plain JavaScript is its type system. You should take advantage of this by always specifying variable types. The only exceptions are if you're initializing to a literal value or a new instance of a type (in which case TypeScript automatically infers the type). Variables should be explicitly typed as any if not possible to be more specific. 
  
**Good**
```typescript   
var area: number = circle.area(); 
public bar(arg0: string): number { 
} 
```   
**Bad** 
```typescript  
var area = circle.area(); 
public bar(arg0 :string):  number { 
} 
```  
**Allowable Exceptions** 
```typescript   
var document = new Document(); 
var name = "since this is a literal assignment, we don't require name to be explicitly typed"; 
Var aNumber = 10; 
``` 
##### DO: Indent using 4 spaces rather than tabs 
NOTE: The easiest way to ensure your code meets the code formatting guidelines is to choose "Format Document" from Edit -> Advanced in Visual Studio. 
  
**Example** 
```typescript  
module Foo { 
    export var version = 10; 
} 
``` 
##### DO NOT: Use array.forEach() 
For performance, always use a **for** loop. 
 
 
##### DO NOT: Wrap single-line statements in {} 
 
**Good**
```typescript  
if (condition)  
    doAction(); 
```  
**Bad** 
```typescript 
if (condition) { 
    doAction(); 
} 
``` 
##### DO NOT: Use the "this" keyword in static functions 
This applies to both static functions on a class, or function on a module (which are also effectively static). 
  
**Good** 
```typescript 
module Foo { 
    export function parse(): object { } 
    export function callParse(): object { 
        return parse(); 
  } 
} 
```  
**Bad** 
```typescript 
module Foo { 
    export function parse(): object { } 
    export function callParse(): object { 
        return this.parse(); 
  } 
} 
```  
Even though this might seem obvious, TypeScript actually allows the “this” keyword in static functions. If it’s in a function on a module, the type of this is any and thus anything is allowed. Runtime this will resolve, but what it resolves to depends on from where the function was called and can lead to unpleasant surprises. 
 
  
##### DO: When using modules as namespaces, write multiple hierarchy modules using the dotted syntax 
 
**Good** 
```typescript  
module Foo.Bar.Zeta { 
    export var version = 10; 
} 
``` 
**Bad** 
```typescript 
module { 
    module Bar { 
        module Zeta { 
            export var version = 10; 
        } 
    } 
} 
``` 
##### DO: Always specify a return type for functions 
If a function doesn't return anything, explicitly specify that it returns **void**. 
 
**Good** 
```typescript  
public foo(): void { 
} 
``` 
**Bad** 
```typescript  
public foo() { 
} 
``` 
##### DO: Use modules for types with only static methods 
If you are creating a type that contains only static methods, define it in terms of a module and not a class. 
 
**Good** 
```typescript  
module Math { 
    export function round(x: number): number { // Effectively static, since it's accessible without an object instance 
} 
``` 
**Bad** 
```typescript  
class Math { 
    static round(x: number): number { 
} 
``` 
##### DO: Use interfaces for types with only data 
If you have a type that has only data and no methods, use an interface instead of a class. This helps avoid the overhead of TypeScript classes, while still ensuring type-safety. 
 
**Good** 
```typescript  
interface RGB { 
    r: number; 
    g: number; 
    b: number; 
}; 
 
public setRGB(rgb: RGB): void { 
    ... 
} 
 
setRGB({r: 255, g: 255, b: 255}); 
``` 
**Bad** 
```typescript  
class rgb { 
    public r: number; 
    public g: number; 
    public b: number; 
} 
 
var color = new rgb(255, 255, 255); 
``` 
 
##### DO: Alias namespaces 
Scope resolution in JavaScript is slow, especially for highly-populated namespaces. Therefore, you should always alias a namespace if you repeatedly reference its members. 
  
**Good** 
```typescript   
import algorithms = BI.Common.Algorithms; 
var find_if = algorithms.find_if; 
  
export function foo() { 
    var scopeValues1 = find_if(values, filter, i, n); 
    var scopeValues2 = find_if(values, filter, i, m);        
} 
```  
**Bad** 
```typescript   
export function foo() { 
    var scopeValues1 = BI.Common.Algorithms.find_if(values, filter, i, n); 
    var scopeValues2 = BI.Common.Algorithms.find_if(values, filter, i, m);   
} 
``` 
 
 
##### DO: Add a copyright declaration at the top of all files 
The Copyright declaration in code is a Microsoft requirement. 
 
**[Example](https://github.com/Microsoft/PowerBI-visuals/blob/master/LICENSE)** 
```typescript  
 /*
 *  Power BI Visualizations
 *
 *  Copyright (c) Microsoft Corporation
 *  All rights reserved. 
 *  MIT License
 *
 *  Permission is hereby granted, free of charge, to any person obtaining a copy
 *  of this software and associated documentation files (the ""Software""), to deal
 *  in the Software without restriction, including without limitation the rights
 *  to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
 *  copies of the Software, and to permit persons to whom the Software is
 *  furnished to do so, subject to the following conditions:
 *   
 *  The above copyright notice and this permission notice shall be included in 
 *  all copies or substantial portions of the Software.
 *   
 *  THE SOFTWARE IS PROVIDED *AS IS*, WITHOUT WARRANTY OF ANY KIND, EXPRESS OR 
 *  IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, 
 *  FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE 
 *  AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER 
 *  LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
 *  OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
 *  THE SOFTWARE.
 */
``` 
##### DO: Use \ instead of / in reference declarations 
Using \ in reference declarations makes it easier to determine relative paths to files in the file system. 
 
**Good** 
```typescript  
/// <reference path="..\..\Common\Contracts.str" /> 
```  
**Bad** 
```typescript 
/// <reference path="../../Common/Contracts.str" /> 
``` 
##### DO: Use import for aliasing a module. 
If you want to use a shorter alias for a module, use the import TypeScript keyword. 
 
**Bad** 
```typescript
module BI.PV.DP.Edm { 
var Xml = BI.Common.Xml; 
export function Test(document: Xml.IXMLDOMNode) {} // This will fail to compile since TypeScript doesn't recognize Xml as an alias. 
} 
``` 
**Good** 
```typescript
module BI.PV.DP.Edm { 
import Xml = BI.Common.Xml; 
export function Test(document: Xml.IXMLDOMNode) {} // This works just fine. 
} 
``` 
 
##### Consider: Using type-safe function definitions  
1. Communication between functions/objects becomes easier with using delegates.  
2. With TypeScript, you can provide an interface for a function.  
  
**Good** 
```typescript 
interface IStringComparer { 
(a: string, b: string): number; 
} 
 
function Comparer(a : string, b:string) : number {  
    return 1; 
}  
 
function foo(comparer: IStringComparer) { 
    comparer("a", "b"); 
} 
``` 
or 
```typescript 
function foo(comparer: (a: string, b: string) => number) { 
    comparer("a", "b"); 
} 
 
foo(Comparer); 
```
**Bad** 
```typescript  
function foo(comparer: Function) { 
    comparer("a", "b"); 
} 
```
##### Consider: Using type-safe dictionary definitions  
With TypeScript, you can provide an interface for the dictionary. Here's the syntax: 

```typescript
dictionaryName: { [index: keyType]: valueType; }
```
  
**Good** 
```typescript 
var someDict: { [index: string]: SomeObject; }; 
someDict["key1"] = new SomeObject("firstOb"); 
someDict["key2"] = new SomeObject("secondOb"); 
someDict[0] = new SomeObject("thirdOb"); //will throw a compiler error for the key 
someDict["key3"] = 4; //will throw a compiler error for the value 
``` 
 
**Bad** 
```typescript 
var someDict: any; 
someDict["key1"] = new SomeObject("firstOb"); 
someDict["key2"] = new SomeObject("secondOb"); 
someDict[0] = new SomeObject("thirdOb"); //no compiler error 
someDict["key3"] = 4; //no compiler error 
``` 
Dictionaries can be used only when the keys are strings or numbers. For any other type use Map class built by the team. 
 
 
##### Consider: In-place documentation explaining purpose, usage and examples 
This is important when defining something that will be used by others. 
All comments should be done in 'Markdown' format:

**Example** 
(Router) 
```typescript 
/** - Why do we need it 
* 
* Single page Web App runs in browser, so users expect common user experience  
* in Web browsers, that means the app should enable the user to: 
* 1) see address information from browser's URL input bar. 
* 2) see history of navigation from the browser's history dropdown list and 
* jump to anywhere. 
* 3) use browser back, forward button to travel navigation history. 
* 4) use browser’s native keyboard accessibility to travel in the history. 
* 5) bookmark a certain view with browser. 
* 6) share a certain view by send a URL with hash. 
* 7) fresh page (for any reasons, speeding up browser for instance) without  
* having to leave the view. 
*  
* Hash-based routing can also simplify UI navigation architecture and provide 
* a loose-coupled way of hook component together for navigation handling. 
*
* - How it works 
* 
* When hash got changed in browser's URL input bar, a `hashchange` event be 
* fired by the browser on global object. Router will capture the event and 
* compare it with registered routes to find the match handler and invoke it 
* by passing the params (hashtable), query (hashtable) and the URL string. 
* 
* - How do you use it 
* 
* Example: 
* 
* BI.PV.Services.addRoutes([ 
* ['/document/{docId}/page/{pageNamber}', function (params, query, url) { 
* ... 
* }] 
* ]); 
*/
``` 
##### Consider: Inline comments 
 
When the logic is not obvious, add comments to help explain its purpose. 
 
**Example**  
```typescript 
/** If the focus does not match the native focus, blur the native focus. 
* NOTE: 
* When focus blur away from body element, the IE browser lost the  
* focus if there is only one tab in the browser window. 
*/
if (activeElement !== document.activeElement && 
    document.activeElement !== document.body) { 
        $(document.activeElement).blur(); 
} 
``` 
##### Consider: Specifying TODO comments when needed 
 
Consider adding TODO comments on top of the blocks you want to refactor later or in areas that are not complete. In most cases, it will probably not be obvious what needs to be done. In those cases, add additional information about what needs to be done. 
 
```typescript
/** TODO: This code needs to be refactored */
```