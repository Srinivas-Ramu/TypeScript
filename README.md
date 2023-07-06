
# TypeScript common linq command equivalents / CheatSheet

### 📌 Intro
Linq in c# is a great abstraction, it massively reduces the amount of code to do fairly basic operations, using TypeScript doesn’t mean you lose this functionality, its just a little different to achieve many of the operations. In this article I’ll run through the basic Linq-To-Object operations, and how you can achieve similar results in TypeScript.

For these examples, I’ll keep it simple with a list of Person objects that look like this:

```csharp
public class Person
{
    public string Name {get;set;}
    public string Title {get;set;}
}
```
```csharp
class Person
{
    constructor(public Name: string, public Title: string)
    {        
    }
}
```

(How good is that typescript constructor/field syntax!).

With the data:

- Chandler - Mr
- Monica - Mrs
- Rachel - Miss
- Joey - Mr
- Ross - Dr


------------

### 📌 Select
To select the names of each person with their title:

##### CSharp
```csharp
people.Select(x => new { FullTitle = $"{x.Title} {x.Name}"});
```
##### Typescript
In TypeScript, the equivalent to select is the map function:
```Typescript
people.map(x => ({ FullTitle: x.Title + ' ' + x.Name }));
```

------------
### 📌 Where
To filter the list to only the people with the title “Mr”:

##### CSharp
```
people.Where(x => x.Title == "Mr");
```
##### Typescript
```
people.filter(x => x.Title == "Mr");
```
------------
### 📌 OrderBy
To order the list by Name:

##### CSharp
```
people.OrderBy(x => x.Name);
```
##### Typescript
In TypeScript, the equivalent to order by is the sort function, but you do have to give it a sort function that returns 1 or 0:
```
people.sort((x,y) => x.Name > y.Name ? 1 : -1);
```
------------

### 📌 GroupBy
To group the list into the various titles:

##### CSharp
```
people.GroupBy(x => x.Title);
```
##### Typescript

There isn’t a simple equivalent to this, the closest thing is probably the reduce function:
```
var grouped = people.reduce((g : any, person : Person) => {
    g[person.Title] = g[person.Title] || []; //Check the value exists, if not assign a new array
    g[person.Title].push(person); //Push the new value to the array
    return g; //Very important! you need to return the value of g or it will become undefined on the next pass
}, {});
```
------------
### 📌 FirstOrDefault
To select the first person with the title Mr, or null if none exist:

##### CSharp
```
people.FirstOrDefault(x => x.Title == "Mr");
```
##### Typescript
```
people.find(x => x.Title == "Mr");
```
------------
### 📌 Aggregate
Lets concatenate all the names together:

##### CSharp
```
people.Select(x => x.Name).Aggregate((x,y) => x = x + "" + y).Dump();
```
##### Typescript
This time, reduce is almost like for like with the c# equivalent
```
var concat = people.map(x => x.Name).reduce((g : any, name: string) => {
    g += name;
    return g;
}, "");
```


