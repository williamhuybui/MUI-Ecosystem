# React Hook Form

## 1. React Router v6
### Turn single-page to multi-page application

[React Router](https://reactrouter.com/en/main)
### Get Started
1. `npm install react-router-dom@6`

2. In `index.js`
```javascript
import {BrowserRouter} from "react-router-dom"
ReactDom.render(
<BrowserRouter>
<App/>
</BrowserRouter>
);
```

### Rounting
Inside index
```javascript
ReactDOM.render(
    <BrowserRouter> <App /> </BrowserRouter>
)
```
```javascript
function App(){
    return (
        <div className = "App">
            <Routes> 
                <Route path = "/" element = {<Home>}/>
                <Route path = "/about" element = {<About>}/>
            </Routes>
        </div>
    );
}
```
**Routing pages** use <Routes>. (This goes in `index.js`). Has `path` attribute, can either be the static path or dynamic (i.e. `:invoiceId` dynamic segment). `element`: what going to be rendered.
**Layout (sharing)** Use `<Outlet>`
**Hyperlink** Use `Link` or `NavLink`. This has `to` attribute
**useParams** method to get the id of clickable item (similar to **ctx.triggered** id in Python)

_Note:_ Route is nested.

**useSearchParams:** React Hook. Use to make search filter in input. Can put this directly on html atribute `onChange`

**useLocation:** A hook that can return a dictionary that has your current url and separate them in to different children.
**useNavigate** A hook use (on onChange) to automatically change route on event. Ex:

```javascript
navigate = useNavigate();
<Card onClick={() => navigate(`/book/${book.id}`)} />;
```

**Summary** To make multi-page you need to first

1. Use Browser Router to wrap around the application in `index.js`.
2. Define the `path` for each Route using <Routes> and define the event when click using `element` attributes.
3. Routing can also be done using `Link` or `Navlink`
4. Other: useParams, searchParams, useLocation

## 2. React Router - Main Concepts

**Vocab:**

- Search param: anything after ? in route
- Location: browser object represent the url
- Location state: states inside location object.
- History Stack: locations in a stack (Pop, push, replace)
- Segment (url params): path pattern "/user/123" has 2 _segments_
- Router: top-level component, relative with parents.
- Outlet: A component that renders the next match in a set of matches
- Index Route: A child route with no path that renders in the parent's outlet at the parents URL. `<Route index element = ... \> `
- Route config: route matching order, usually represent as a tree

## 3. [Material UI v5](https://mui.com)

**Installation**
`npm install @mui/material @emotion/react @emotion/styled`

```javascript
import {Button}
 from "@mui/material"

 <Button variant = "contained> Click me <\Button>
```

App Bar (Nav bar),
**Note:**: Structure:

- src > components > SearchAppBar.js for all components
- src > pages > DetailPage.js for all pages

**Grid:** Flex box container. Use xs, md, lg for responsive.

**sx**: A dict element of Material UI to quickly style (Use like `style`). Ex: mt = margin-top.

**Note**: Dont forget `key` whenever using loop. This will help when want to get id of clicked button (Using `useParams`)

**Other components**: Typography (h1-h5), Box (similar to container), QuiltedImageList (Collage), Icon (Similar to Iconify)

**Theme:** Similar to manipulate root in CSS

- createTheme: a dict object to pass in `theme` element in ThemeProvider. Use to change theme
- ThemeProvider: wrap around the app

## Method 1: sx prop
sx = system
sx = {
flexDirection: {'xs': 'column', 'md' : 'row'};
mt: 1;
}

## Method 2:
styled-component: css in javascript. Use to create a React component wrapper. When wrap around component it with has the style 
```javascript
const some_style = styled('div')({
    color: 'darkslategray',
    backgroundColor: 'aliceBlue',
    padding: 8,
    borderRadius: 4,
}
)
```
## Theme
Theme is like a class, or a pallete where you can control the primary property globally
```javascript
theme = createTheme({
    palette:{
        text:{
            primary:blue,
            secondary:green,
        },
        action: {
            active:black,
        }
    }
})
<ThemeProvider theme = {theme}>
<Box sx = {{color: text.primary}}>
<\ThemeProvider>
```
## Grid
```javascript
<Grid item xs={2} md ={3}>Something</Grid>
```

## React Hook Form
### useForm and other MUI

register: automatically set State for each component. Can use one register for many component, it will automatically saved in a meta dict
handleSubmit: use to control onSubmit
```javascript
import {useForm} from "react-hook-form"
const {register, handleSubmit, formState: {errors, isSubmitting}} = useForm({username: "", password : ""});

const onSubmit = (data) => console.log(data)

<form onSubmit={handleSubmit(onSubmit)}>
<input type = "text" {...register("username")}></input>
<input type = "text" {...register("password", {required: true})}></input>
</form>
errors ? errors.text : None
```

Other MUI: Stack, Controller, LoadingButton (use for isSubmitting), Alert, FormProvider, useFormContext, InputAdornment, FSwitch,

**Yup:** A React library to define input condition. Use to validate input. Ex:
```javascript
import * as yup from "yup"
const schema = yup.object().shape({
    username: yup.string().required(),
    password: yup.string().email().required(),
})
```
```javascript
const {register, handleSubmit, formState: {errors, isSubmitting}} = useForm({resolver: yupResolver(schema)});
```
In this case yup would replace regex

## HTTP Requests with Axios
```
npm install axios
```
Use **reqres.in** to test API
### Get request
```javascript
import axios from "axios"
axios.get("https://reqres.in/api/users?page=2").then((res) => console.log(res.data))
```
### Post request
```javascript
axios.post("https://reqres.in/api/users", {name: "morpheus", job: "leader"}).then((res) => console.log(res.data))
```

*Note:* Use .env to store base url
In env
```
REACT_APP_BASE_URL = "https://reqres.in/api"
```
In js
```javascript
axios.get(`${process.env.REACT_APP_BASE_URL}/users?page=2`).then((res) => console.log(res.data))
```
### Create Method
```javascript
const apiService = axios.create({
    baseURL : BASE_URL
})
apiService.get("api/users?page=2")
```






