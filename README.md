[Link to codecademy lesson](https://www.codecademy.com/courses/react-101/lessons/the-effect-hook/exercises/separate-hooks-for-separate-effects)


### The Effect Hook

**Separate Hooks for Separate Effects**

When multiple values are closely related and change at the same time, it can make sense to group these values in a collection like an object or array. Packaging data together can also add complexity to the code responsible for managing that data. Therefore, it is a good idea to separate concerns by managing different data with different Hooks.

Compare the complexity here, where data is bundled up into a single object:
```
// Handle both position and menuItems with one useEffect hook.
const [data, setData] = useState({ position: { x: 0, y: 0 } });
useEffect(() => {
  get('/menu').then((response) => {
    setData((prev) => ({ ...prev, menuItems: response.data }));
  });
  const handleMove = (event) =>
    setData((prev) => ({
      ...prev,
      position: { x: event.clientX, y: event.clientY }
    }));
  window.addEventListener('mousemove', handleMove);
  return () => window.removeEventListener('mousemove', handleMove);
}, []);
```
To the simplicity here, where we have separated concerns:
```
// Handle menuItems with one useEffect hook.
const [menuItems, setMenuItems] = useState(null);
useEffect(() => {
  get('/menu').then((response) => setMenuItems(response.data));
}, []);
```

```
// Handle position with a separate useEffect hook.
const [position, setPosition] = useState({ x: 0, y: 0 });
useEffect(() => {
  const handleMove = (event) =>
    setPosition({ x: event.clientX, y: event.clientY });
  window.addEventListener('mousemove', handleMove);
  return () => window.removeEventListener('mousemove', handleMove);
}, []);
```
It is not always obvious whether to bundle data together or separate it, but with practice, we get better at organizing our code so that it is easier to understand, add to, reuse, and test!

### Instructions
Checkpoint 1 Passed
1. At the moment, this code seems to work just fine. There are three different network requests being made in a single effect and lots of different, unrelated data being managed in a single state variable. Let’s get to work breaking these single useState() and useEffect() calls into separate, simpler hooks. Doing so will make this code easier to understand, build on top of, and reuse as we continue to improve our application!

Begin refactoring this component:

- Use a separate State Hook for menu, newsFeed, and friends.
- Use these new state setters instead of setData() in the effect.
- Simplify our JSX to use these new state variables instead of data.

Replace this:
```
const [data, setData] = useState(null);
```
With this:
```
const [menu, setMenu] = useState(null);
const [newsFeed, setNewsFeed] = useState(null);
const [friends, setFriends] = useState(null);
```
Replace the setData() call in the effect with this:
```
setMenu(menuResponse.data);
setNewsFeed(newsFeedResponse.data);
setFriends(friendsResponse.data);
```
And switch to using each of our new state variables in the JSX, for example:
```
- replace !data || !data.menu with !menu
- replace data.menu.map() with menu.map()
```
2. Promise.all() was helpful to us when we had all of our data bundled up in a single object. It called all backend services, and when they all sent back responses, we could then call our state setters with the responses.

Because we are now managing these values separately, we can remove this complexity! In our effect, call the get() function three times for the three different data collections that our component wants to render, without using Promise.all() any more.

These three endpoints are unrelated, so breaking them apart will help simplify our code!

Replace the Promise.all() function call with a separate get() and then() for each endpoint.

For example, our fetch of the menu data looks like this:
```
get('/menu').then((response) => {
  setMenu(response.data);
});
```
Use this same pattern to fetch data from the other two endpoints and call the appropriate state setter functions to store the response data in the local state.


3. Now that we have three separate backend calls, let’s continue to separate concerns by splitting each of these into three separate Effect Hooks!

Each API call is its own effect, so let’s place each in its own useEffect() call. For example, our Effect Hook for the menu data looks like this:
```
  useEffect(() => {
    get('/menu').then((response) => {
      setMenu(response.data);
    });
  }, []);
```
Use this same pattern to fetch data from the other two endpoints!

4. Each useEffect() call is working with a corresponding useState() call. Let’s reorganize our code to show this relationship more clearly, making the logic easier to read and reuse!

For each of these three data collections, group the related State Hook and the Effect Hook next to one. This will help to make it clear which Hooks are working together to manage each separate data model.

For example, our Hooks for the menu data looks like this:
```
const [menu, setMenu] = useState(null);
useEffect(() => {
  get('/menu').then((response) => {
    setMenu(response.data);
  });
}, []);
```
Use this same pattern to fetch data from the other two endpoints!




