# Makepad

- [Official Website](https://makepad.nl/)
- [Official Repository](https://github.com/makepad/makepad)

```rust,noplayground
// app.rs
use makepad_widgets::*;

live_design! {
    use link::widgets::*;

    App = {{App}} {
        ui: <Root> {
            <Window> {
                body = <View> {}
            }
        }
    }
}

#[derive(Live, LiveHook)]
pub struct App {
    #[live]
    ui: WidgetRef,
}

impl LiveRegister for App {
    fn live_register(cx: &mut Cx) {
        makepad_widgets::live_design(cx);
    }
}

impl AppMain for App {
    fn handle_event(&mut self, cx: &mut Cx, event: &Event) {
        self.ui.handle_event(cx, event, &mut Scope::empty());
    }
}

app_main!(App);
```

## Live Design

The `live_design!{}` macro is used to define Makepad DSL code. DSL code is used
in Makepad to define the layout and styling of our app, similar to CSS.

- `App = {{App}} { ... }` defines an `App`. The `{{App}}` syntax links the
  definition of `App` to an `App` struct in the Rust code. Within it, we
  define the `ui` field as the root of our widget tree:
  - `<Root> { ... }` is our top-level container.
  - `<Window> { ... }` is a window on the screen.
  - `body = <View> {}` is an empty view named `body`.

The live design system enables Makepad's powerful runtime styling capabilities,
allowing us to use Makepad Studio to tweak the app's design without recompiling.

## `Live`, `LiveHook`, and `LiveRegister`

**The `Live` trait** enables a struct to interact with the live design system.
When deriving `Live`, each field needs to be marked with one and only one of the
following attributes:

- **`#[live]`**, which marks the field as part of the live design system. It would
  be initialized with the values defined in the `live_design!{}`, or
  `Default::default()` if failed to find one.
- **`#[rust]`**, which marks the field as an ordinary Rust field. It would be initialized
  with `Default::default()`.

**The `LiveHook` trait** provides several overrideable methods that will be called
at various points during our app's lifetime.

**The `LiveRegister` trait** is used to register DSL code. By registering DSL code,
we make it available to the rest of our app. In our implementation, we call the
`makepad_widgets::live_design` function to register the DSL code in the `makepad_widgets`
crate. Without this call, we would not be able to use any of the built-in widgets
such as `Root`, `Window`, and `View` that we saw earlier.

## The `AppMain` Trait

The `AppMain` trait is used to hook an instance of the `App` struct into the main
event loop.

A `Scope` in Makepad is a container that is used to pass both app-wide data and
widget-specific props along with each event.

## The `Widget` Trait

The `Widget` trait allows us to override the behaviour of a widget. However, deriving
the `Widget` trait does not generate an implementation of it. Instead, it generates
implementations of several helper traits that the Widget trait needs.
