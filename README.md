# N26 copy paste
Easily copy paste N26 records for easier bookkeeping.

## How to use it
1. Copy the code down below
2. Open the consol in the N26 website
3. Paste the code in the console and hit enter

**hint:** don't hesitate on refreshing and starting over if it doesn't work.

**hint2:** it will only work with the records currently loaded in the screen, so before pasting you could load more records 


### Before Script:
![image before the script](https://github.com/Antoine-lb/N26-bank-copy-paste/blob/main/before.jpg)
### After Script:
![image after the script](https://github.com/Antoine-lb/N26-bank-copy-paste/blob/main/after.jpg)

```js
const DATE_YEAR = "2022";
let total = "";

function makeCopiable(element) {
  element.onclick = function () {
    document.execCommand("copy");
  };

  element.addEventListener("copy", function (event) {
    event.preventDefault();
    if (event.clipboardData) {
        event.clipboardData.setData("text/plain", element.textContent);
      // event.clipboardData.setData("text/plain", total);
      element.style.backgroundColor = "#36a18b66";
      //   console.log("copied: ", event.clipboardData.getData("text"));
    }
  });
}

function formatDate(str) {
  let splitedText = str.split(" ");
  //   console.log("splitedText", splitedText);

  return `${splitedText[0]} ${splitedText[1]} ${DATE_YEAR}`;
}

function formatPrice(str) {
  //   console.log("str before", str);
  str = str.replace("−", " ");
  str = str.replace(".", ",");
  //   console.log("str after", str);
  return str;
}

function createParagraph(parrentElement, text) {
  const para = document.createElement("p");
  para.innerHTML = `${text}`;
  para.classList.add("customParagraph");
  //   para.innerHTML = `<p class="customParagraph">${text}</p>`;
  makeCopiable(para);
  parrentElement.appendChild(para);
}

function eachElement(elem) {
  //   console.log("elem.innerText", elem.innerText.split("\n"));
  let splitedText = elem.innerText.split("\n");
  const divContainer = document.createElement("div");

  total += splitedText[0] + "\t";
  total += formatDate(splitedText[1]) + "\t";
  total += formatPrice(splitedText.at(-1)) + "\t";
  total += "\n";

  createParagraph(divContainer, splitedText[0]);
  createParagraph(divContainer, formatDate(splitedText[1]));
  createParagraph(divContainer, formatPrice(splitedText.at(-1)));

  elem.after(divContainer);
}

var style = document.createElement("style");
style.type = "text/css";
style.innerHTML = `
.customParagraph { 
    background-color: #36a18b;
    display: inline-block;
    padding: 10px;
    border-radius: 50px;
    color: white;
    margin-right: 10px;
    cursor: pointer;
}

.customParagraph:hover { 
    background-color: #36a18b99;
}
`;
document.getElementsByTagName("head")[0].appendChild(style);

const target = document.getElementsByTagName("li");

Object.keys(target).forEach((e) => {
  eachElement(target[e]);
});

```
