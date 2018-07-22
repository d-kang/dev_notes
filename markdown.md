# Markdown Notes


> [Link to Markdown Cheat-Sheat](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet)

## Funtion to generate a table of contents
```javascript
function tableOfContents(arr) {
  let result = `\n`

  arr.forEach(a => {
	const dashed = a.replace(/ /g, '-')
    result += `- [${a}](#${dashed})\n`

  })
  return result
}
```


## Function to generate a table
```javascript
function table(obj, title='', description='') {
  let result = `
| ${title}  |      ${description}           |
| --------- | :---------------------------: |
`

  for (const key in obj) {
    result += `| ${key} | ${obj[key]} |\n`
  }

  return result
}


const obj = {
  'plot(y)': 'Plots the vector y with the index as x',
  'plot(x, y)': 'Plots vector y in relation to x vector',
  'plot(x, y, fmt': 'modifies how the line/points are drawn. Default is `-` solid line',
};

table(obj)

// |   |                 |
// | --------- | :---------------------------: |
// | plot(y) | Plots the vector y with the index as x |
// | plot(x, y) | Plots vector y in relation to x vector |
// | plot(x, y, fmt | modifies how the line/points are drawn. Default is `-` solid line |


```
> Then use `markdown all in one` VS Code extension for markdown formatting




