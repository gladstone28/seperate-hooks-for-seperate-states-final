[Link to codecademy lesson](https://www.codecademy.com/courses/react-101/lessons/the-effect-hook/exercises/separate-hooks-for-separate-effects)


The Effect Hook
Separate Hooks for Separate Effects
20 min
When multiple values are closely related and change at the same time, it can make sense to group these values in a collection like an object or array. Packaging data together can also add complexity to the code responsible for managing that data. Therefore, it is a good idea to separate concerns by managing different data with different Hooks.

Compare the complexity here, where data is bundled up into a single object:

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

To the simplicity here, where we have separated concerns:

// Handle menuItems with one useEffect hook.
const [menuItems, setMenuItems] = useState(null);
useEffect(() => {
  get('/menu').then((response) => setMenuItems(response.data));
}, []);

// Handle position with a separate useEffect hook.
const [position, setPosition] = useState({ x: 0, y: 0 });
useEffect(() => {
  const handleMove = (event) =>
    setPosition({ x: event.clientX, y: event.clientY });
  window.addEventListener('mousemove', handleMove);
  return () => window.removeEventListener('mousemove', handleMove);
}, []);

It is not always obvious whether to bundle data together or separate it, but with practice, we get better at organizing our code so that it is easier to understand, add to, reuse, and test!

Instructions
Checkpoint 1 Passed
1.
At the moment, this code seems to work just fine. There are three different network requests being made in a single effect and lots of different, unrelated data being managed in a single state variable. Let’s get to work breaking these single useState() and useEffect() calls into separate, simpler hooks. Doing so will make this code easier to understand, build on top of, and reuse as we continue to improve our application!

Begin refactoring this component:

Use a separate State Hook for menu, newsFeed, and friends.
Use these new state setters instead of setData() in the effect.
Simplify our JSX to use these new state variables instead of data.
