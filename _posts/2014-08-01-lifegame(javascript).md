---
layout: post
title: "life game in javascript"
description: "some take-away when learning"
category: "javascript"
tags: []
---
{% include JB/setup %}


this post was some take-away when I follow the tutorial in implement life game in javascript

### window.self === window

```
(function(){
    var root = self;
    var _ = root.Life = function(seed) {
        this.seed = seed;
        this.height = seed.length;
        this.width = seed[0].length;
        this.prevBoard = [];
        this.board = cloneArray(seed);
    }
})()
```

### use `slice` to clone the array

```
	 function cloneArray(array) {
        // we can use slice to shallow cope the arry
        return array.slice().map(function(row){
            return row.slice();
        });
    }
```

### convert undefined to number 0

```
	var number = +!!undefined;

	// 1 convert undefined to boolean by !!
	// 2 convert boolean to number by +
```

### convert a multi array to string

```
 toString: function(){
            return this.board.map(function(row){ return row.join(' ');}).join('\n')
        }
```

### create DocumentFragment to reduce dom manipulation on dom tree

```
  createGrid: function() {
            var fragment = document.createDocumentFragment();
            this.grid.innerHTML = '';
            this.checkboxes = [];
            var that = this;
            for(var y = 0; y < this.size; y++) {
                var row = document.createElement('tr');
                this.checkboxes[y] = [];
                for(var x = 0; x < this.size; x++) {
                    var cell = document.createElement('td');
                    var checkbox = document.createElement('input');
                    checkbox.type = 'checkbox';
                    this.checkboxes[y][x] = checkbox;
                    // act like a tuple
                    checkbox.coords = [y, x];

                    cell.appendChild(checkbox);
                    row.appendChild(cell);
                }
                fragment.appendChild(row);
            }
            this.grid.appendChild(fragment);

```

### use `_` to repersent the Object when create prototype
the benefit is ,when the name of the object changed, we don't need to change the name

```
 var root = self;
    var _ = root.LifeView = function(table, size) {
        this.grid = table;
        this.size = size;
        this.started = false;
        this.autoplay = false;
    }

    _.prototype = {...}
```

### use array to act like tuple
```
 checkbox.coords = [y, x];
```

### test if the target is which element when the event is bubble up
```
  this.grid.addEventListener('change', function(event){
                if(event.target.nodeName.toLowerCase() === 'input'){
                    that.started = false;
                }
            });
``` 

### `get` make function as attribute
```
  get boardArray(){
            return this.checkboxes.map(function(row) {
                return row.map(function(checkbox) {
                    return +checkbox.checked;
                });
            });
        },
  play: function(){
            this.game = new Life(this.boardArray);
            this.started = true;
        },
```

### when button was created by button tag, use textContent to change it's value
### when button was created by input type=button or submit, use value to change it's value

```
  $('#autoplay').addEventListener('change', function(event) {
        // when button was created by button tag, use textContent to change it's value
        // when button was created by input type=button or submit, use value to change it's value
        buttons.next.textContent = this.checked ? 'Start' : 'Next';
        buttons.next.disabled = this.checked;
        lifeView.autoplay = this.checked;
        if(this.checked) {
            lifeView.next();
        }else{
            clearTimeout(lifeView.timer);
        }

    });
```

### use event.keyCode to test the keyboard

```

                    switch (event.keyCode) {
                        case 37: //left
                            if(x > 0) {
                                that.checkboxes[y][x-1].focus();
                            }
                            break;
                        case 38: //up
                            if(y > 0) {
                                that.checkboxes[y-1][x].focus();
                            }
                            break;
                        case 39: //right
                            if(x < that.size - 1) {
                                that.checkboxes[y][x+1].focus();
                            }
                            break;
                        case 40: //down
                            if(y < that.size - 1){
                                that.checkboxes[y+1][x].focus();
                            }
                            break;
                    }
```

https://github.com/xujihui1985/lifegame