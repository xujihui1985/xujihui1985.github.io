---
layout: post
title: "how react setState work"
description: ""
category: "javascript"
tags: [react]
---
{% include JB/setup %}


#### setState

```
class App extends Component {

    constructor() {
        super();
        this.count = 0;
        this.handleClick = this.handleClick.bind(this);
    }

    handleClick() {
        this.setState({
            state1: 'hello'
        });
        this.setState({
            state2: 'world'
        });
    }
    
    render() {
        this.count++;
        return (
            <botton onClick={this.handleClick}>click me to render</button>
        );
    }
}
```

when I click the button, the total count of render method called is one, but if the handleClick method call an ajax method, and setState in the callback like this

```
class App extends Component {

    constructor() {
        super();
        this.count = 0;
        this.handleClick = this.handleClick.bind(this);
    }

    handleClick() {
        callRemoteApi()
            .then(()=>{
                this.setState({
                    state1: 'hello'
                });
                this.setState({
                    state2: 'world'
                });
            })
    }
    
    render() {
        this.count++;
        return (
            <botton onClick={this.handleClick}>click me to render</button>
        );
    }
}
```

the total count of render method called will be two.


#### how that happened?

when I dig into the source code of react, I found the reason of this behaviour is because, all the dom event that trigger on virtual dom is handled by react,
when use click the button, react will call it's own dispatch method

```
dispatchEvent: function (topLevelType, nativeEvent) {
    if (!ReactEventListener._enabled) {
        return;
    }

    var bookKeeping = TopLevelCallbackBookKeeping.getPooled(topLevelType, nativeEvent);
    try {
        // Event queue being processed in the same cycle allows
        // `preventDefault`.
        ReactUpdates.batchedUpdates(handleTopLevelImpl, bookKeeping);
    } finally {
        TopLevelCallbackBookKeeping.release(bookKeeping);
    }
}
```

when this method called, it will set the `ReactDefaultBatchingStrategy.isBatchingUpdates = true;`

when setState called, it will enqueue the update, first check if `ReactDefaultBatchingStrategy.isBatchingUpdates` is true, 

```
function enqueueUpdate(component) {
    ensureInjected();

    // Various parts of our code (such as ReactCompositeComponent's
    // _renderValidatedComponent) assume that calls to render aren't nested;
    // verify that that's the case. (This is called by each top-level update
    // function, like setProps, setState, forceUpdate, etc.; creation and
    // destruction of top-level components is guarded in ReactMount.)

    if (!batchingStrategy.isBatchingUpdates) {
        batchingStrategy.batchedUpdates(enqueueUpdate, component);
        return;
    }

    dirtyComponents.push(component);
}
```

if this is true, that means react is handling the lifecycle, just put the component to the dirtyComponents, react will batch update it later.

now it's quite easy to understand why it will render twice if setState is called from nexteventloop, that's because at that time, the dispatchEvent has finished,
the `batchingStrategy.isBatchingUpdates` has finished, so there is no way to know when to flush the state, so react has no way but render the component everytime 
setState called.

so be careful to avoid call multiple setState after some ajax call.



