# Chrome DevTools

## Remix this
* [Remix in Glitch](https://glitch.com/edit/#!/tony)
* compression code
```
const x = require('compression')
app.use(x());
```

## Fetch code

```
fetch('index.html').then(response => {
  return response.text();
}).then(text => {
  console.log(text);
});

```
