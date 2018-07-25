

### How to update npm?
```
npm install -g npm
```
a
Attempt to upgrade to the latest working npm on the current node version
```
nvm install-latest-npm
```

### How to install LTS for NVM?
```
nvm install --lts
```


### to temp use
```
nvm use version
```

### to change default (persists)
```
nvm alias default version/8.11.3
```


```javascript
app.listen(8080, 'localhost', function () {
  console.log('Express started on http://localhost:' + 8080 + '; press Ctrl-C to terminate.');
});

app.listen(8080, function () {
  console.log('Express started on http://localhost:' + 8080 + '; press Ctrl-C to terminate.');
});
```