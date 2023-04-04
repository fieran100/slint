# Builtin Callbacks

## `init()`

Every element implicitly declares an `init` callback. You can assign a code block to the callback
that is invoked when the element is instantiated and after all properties are initialized with the value of their final binding. The invocation order is from inside to outside. The following example illustrates this by printing "first", "second", and then "third":

```slint,no-preview
component MyButton inherits Rectangle {
    in-out property <string> text: "Initial";
    init => {
        // If `text` is queried here, it will have the value "Hello".
        debug("first");
    }
}

component MyCheckBox inherits Rectangle {
    init => { debug("second"); }
}

export component MyWindow inherits Window {
    MyButton {
        text: "Hello";
        init => { debug("third"); }
    }
    MyCheckBox {
    }
}
```

To initialize properties, you should use bindings instead of this callback. Avoid using this
callback for initialization because it violates the declarative principle.

One use case where you might need this callback is, for example, to notify some native code:

```slint,no-preview
global SystemService  {
    // This callback can be implemented in native code using the Slint API
    callback ensure_service_running();
}

export component MySystemButton inherits Rectangle {
    init => {
        SystemService.ensure_service_running();
    }
    // ...
}
```
