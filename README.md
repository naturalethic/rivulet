# rivulet
Rivulet provides a state object that is observable and optionally networked via socket.io
Two rivulet objects linked by a socket will keep each other up-to-date.

Documentation is for LiveScript, but the library is provided as JS, so feel free to use it without LS.

# install
```
npm install naturalethic/rivulet
```

# api

## require
```
require! \rivulet
```

## create
```
state = rivulet!
```

## replace
This will 'replace' the state data, however it will fire observers on anything that has changed.
```
state { foo: \bar }
```

## get full state
```
state! # { foo: 'bar' }
```

## set property
Set/get take full dotted paths via [object-path](https://github.com/mariocasciaro/object-path)
```
state.set 'bim.bam', 1
state! # { foo: 'bar', bim: { bam: 1 } }
```

## get property
```
state.get 'bim.bam' # 1
```

## delete property
```
state.del 'bim.bam'
state! # { foo: 'bar', bim: { } }
```

## merge a partial document
```
state.merge { zim: 2 }
state! # { foo: 'bar', bim: { }, zim: 2 }
```

## observe property
By default, objects will be observed at their location and all their immediate properties (called 'flat').
Providing a function is optional and will fire when a change occurs.  The method returns a [kefir](https://github.com/rpominov/kefir) stream.
```
state.observe \foo, (obj) -> # If 'foo' changes, this function is called with a copy of state as a plain object
```
If you would like to control the depth of your observation call the appropriate method.
```
state.observe-atom # When the object reference changes
state.observe-flat # When the object reference, or immediate child references change
state.observe-deep # When the object reference or any descendants change
```

## patch
Rivulet uses [fast-json-patch](https://github.com/Starcounter-Jack/JSON-Patch) internally, so such a patch can be applied here.  Not recommended, its for internal use.
```
state.patch diff
```

## networked construction
Providing a socket-io socket, and the channel name, will link your rivulet to a partner on the other end
```
state = rivulet socket, \session
```
