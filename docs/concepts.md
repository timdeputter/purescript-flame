---
layout: default
title: Main concepts
permalink: /concepts
---

## Main concepts

A Flame application consists of the following record
```haskell
{
        init :: model,
        view :: model -> Html model,
        update :: model -> message -> model,
        inputs :: Array Signal
}
```
The type variable `model` refers to the **state** of the application. `message`, on the other hand, describe the kind of **events** the application handles.

1. Application state

In the counter example we set our state as a simple type alias
```haskell
type Model = Int
```
that is, the state of our application is a single integer. In a real world application, the state will probably be something more interesting -- Flame makes no assumption about how it is structured.

With our model type declared, we can define the initial state of the application
```haskell
init :: Model
init = 0
```
The first time the application is rendered, Flame will call the view function with `init`.

2. Application markup

The `view` function maps the current state to markup. Whenever the model is updated, flame will patch the DOM by calling `view` with the new state.

In the counter example, the model is defined as
```haskell
view :: Model -> Html Message
view model = HE.main "main" [
        HE.button [HA.onClick Decrement] "-",
        HE.text $ show model,
        HE.button [HA.onClick Increment] "+"
]
```
The `Message`s raised as events will be used to signal how the application state should be updated.

See [defining views](views) for an in depth look at views.

3. State updating

The `update` function handles events, returning an updated model. In a Flame application, we reify native events as a custom data type. In the counter example, we are interested in the following events:
```haskell
data Message = Increment | Decrement
```
and thus our update function looks like
```haskell
update :: Model -> Message -> Model
update model = case _ of
        Increment -> model + 1
        Decrement -> model - 1
```

See [Handling events](events) for an in depth look at update strategies.

4. External event handling

Finally, the last field in a Flame application record is `inputs`. It contains a list of [signals](https://pursuit.purescript.org/packages/purescript-signal/), which can be used to raise messages from events outside of our view. This includes `window` or `document` events, such as resize or websockets, and custom events, which will then be handled as usual by the application `update` function.

See [Handling external events](events#external) for an in depth look at input signals.

<a href="/index" class="direction previous">Previous: Getting started</a>
<a href="/views" class="direction">Next: Defining views</a>