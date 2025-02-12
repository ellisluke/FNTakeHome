# FiscalNote Associate Software Engineer Take-Home Assessment
### By Luke Ellis - lklfellis@gmail.com

## Running the project
1. Clone or Download the Project from GitHub
2. Open a terminal in the root directory of the project
3. Change directory to the folder FN-Web using the command `cd FNWeb`
4. Run the command `dotnet run`
5. Go to [http://localhost:8000/](http://localhost:8000/) in your browser

## Initial Thought Process
I have listed below the most natural steps that influenced my approach to this assessment.

1. **Read and Understand the problem** - I started by reading the PDF instructions and email several times, making note of all the requirements. 
2. **Project Infrastructure** - Before learning the framework, I set up the GitHub Repository and this documentation page.
3. **Learn .NET** - I decided to challenge myself and learn the C# / .NET framework for this assessment. My past experiences with web development and .NET's detailed documentation should make this fairly quick.
4. **GitHub Issues** - After learning the basic structure of .NET, I will create GitHub Issues to serve as milestones for development, and these will also be basis for my branches.
5. **Display Data** - Next, I aim to display a basic form of all the data. Doing this first will allow me to work on the styling with the actual data.
6. **Hand-draw Front-end Designs** - I will draw several options for the front-end and decide on one to implement.
7. **Accessible Implementation** - Last, I will build one of my hand-drawn designs with accessibility (color contrast, screen size, alt-text, etc.) in mind.

## Data Decisions
Below, I will discuss several decisions I made while creating the functionality of the Employees page.

### Using JS Array Data
`/wwwroot/employees.js` contains `function getEmployeeData()` that simply returns `const employees = [ ... ]`  
The class `Employee` mirrors the structure of an element in `const employees = [ ... ]`  
After render, `/Components/Pages/Employees.razor` invokes `getEmployeeData()`, converting the response to `List<Employee>`  

I spent some time pondering how to store and access the given data.
I imagine that the goal of this assignment is to simulate the action of querying an API that returns a JavaScript object. Rather than complicating the steps required to run my project, I decided to just store that data statically in `/wwwroot/employees.js` and pull the data from there using C#. Eventually, these steps would get replaced with an actual API call, but for the purpose of this front-end focused project, that would suffice. No database is needed as the only action required is a **READ** (no Create, Update, or Delete actions are necessary).  

### C# Array vs. List
Initially, I used a C# array to store the data pulled from `employees.js`. After some further research, I decided to convert my arrays to lists due to the dynamic sizing that a list allows. Storing `employeeData` (which does not change after load) as an array and `filteredData` as a list may be an optimal solution, but for simplicity right now, both are a list.

### Language-Integrated Query (LINQ)
```
filteredData = employeeData.Where(employee =>  
                    employee.name.ToLower().Contains(searchText.ToLower()) ||  
                    employee.department.ToLower().Contains(searchText.ToLower()))  
                    .ToList(); 
```
At first, I used a `foreach` loop to attempt search functionality, but this ran into some errors I could not solve quickly. While looking through the [C# List Class Documentation](https://learn.microsoft.com/en-us/dotnet/api/system.collections.generic.list-1?view=net-9.0), I noticed the `Where<TSource>()` method that "Filters a sequence of values based on a predicate." This seemed like a useful and more elegant alternative to a `foreach` loop. After some further reading, I had a working query that had an OR operator `||` in use, too! Creating this filtering function had a similar feel to an SQL query. 