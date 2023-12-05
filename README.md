# Vite Rerunning hooks on edits


## Steps to reproduce
1. run `yarn install`
2. run `yarn run dev`
3. Open Developer console, there should be two logs of "useEffect: 0" since strictmode is enabled.
4. Go to src\App.tsx and edit "Vite + React" to any other text ex. "Vite + React + Typescript"
5. Check the console again and there is a 3rd log of "useEffect: 0"


The hook was reran even though there were no changes inside the hook. 
This is a problem because I have a hook that fetches data from an API on url change and it reruns the hook even though the url did not change.
It wipes out my local state due to the refetch and causes me to have to fill out the state again.

The only way around this is to use a ref and reimplement what a hook already does for us:

```
const countRef = useRef(count);
 
 useEffect(() => {
    if(count !==  countRef.current) {
    {
       countRef.current = count;
       console.log('useEffect: ' + count);
    }
  }, [count]);
```