npm create-react-app app_name => .js
npm create vite => .jsx
component should be capital, for small letters it will search for built-in component
all custom components should be inside the one root built-in component or use <> </>  ( React Fragment )
props

styling
    inline styling in Component files  style={ {styles} }
    you can create global styling ( className ) => but naming conflicts
    Create a separate Post.module.css file and add styles there and import them 

event listening:
    onChange={function_name}
    
    whenever we call setFunction()  then it re-triggers the function again and update the state value.

    Lifting State Up

Children

Ternary Operator / Short Circuit approach to show html code

(currentData) => [newData, ...currentData]

handling side effect of infinite loop using useEffect
useEffect won't be executed always when component executed, only executed when the dependent list of values got changed

We can't use async as a component function
    trick: create another async function and call it from normal function

fetch

<Link> -> instead of <a> , to not load  == Html code
useNavigate -> to navigate to other pages == Js code
<Form>

Layout

routing , child routing ( Outlet )
useLoaderData() + loader => to load the data before loading the component // request as argument
action => to submit the form  // params as argument

redirect('')



Hooks :
    must be the top level of function.
    can't use inside the class
    can't use inside the conditions

    useState :
        set(count - 1) 
        set(count - 1)
        Both are overriden to one so use prevCount instead
        set(prevCount => prevCount -1)

        useState(expression)  =>   useState( () => anonymous )
        In each time you run the funciton the expression also be evaluted, so time consumes.
        use anonymous function, it will execute only first time.
    
    useEffect :
        return statement will execute first then the body will be executed.
        return statement will be used to clean up what ever we did last time.

    useMemo :
        1. slowfunction  ->  doesn't recompute a function every single time you use component. Only computer whenever the input 
                             got changed
        2. reference     ->  useEffect will compare the last and current references not the values , if we use the useMemo instead then 
                             it will not create new object since we have already have one in the last rendering.


React Testing :
    Applications :
        Bugs
        Confidence on Application
        Speed up QA time
    Types:
        Unit
        Integration
        End to End
    Process:
        Find component
        find element
        react with element
        assert