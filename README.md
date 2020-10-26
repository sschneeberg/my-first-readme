# My First README
Creating a proper README.md

## Steps to Install on local computer
1. Go to [repo](https://github.com/sschneeberg/my-first-readme) on Github profile
2. `fork` and `clone` repo
3. clone to local machine
``` text 
git clone http://github.com/sschneeberg/my-first-readme.git
```
4. Go to `my-first-readme` directory
5. open `README.md` in code editor

# Code snippet examples
Code by Rome Bell
```javascript 
const handleWin = (letter) => {
  gameIsLive = false;
  if (letter === "x") {
    statusDiv.innerHTML = `${letterToSymbol(letter)} has won!`;
  } else {
    statusDiv.innerHTML = `<span>${letterToSymbol(letter)} has won!</span>`;
  }
};
```

```css
.grid {
    background-color: salmon;
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    grid-template-rows: repeat(3, 1fr);
    gap: 15px;
    margin-top: 50px;
}
```
```html
<div class="grid">
    <div class="box" id="box-1"></div>
    <div class="box" id="box-2"></div>
    <div class="box" id="box-3"></div>
    <div class="box" id="box-4"></div>
    <div class="box" id="box-5"></div>
    <div class="box" id="box-6"></div>
    <div class="box" id="box-7"></div>
    <div class="box" id="box-8"></div>
    <div class="box" id="box-9"></div>
</div>
```
# Table Example

| Functions            | Description |
| -----------          | ----------- |
| `handleWin()`        | Handle win of either player |
| `checkGameStatus()`  | Check the status after each turn |