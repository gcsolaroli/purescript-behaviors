## Module FRP.Behavior

#### `Behavior`

``` purescript
newtype Behavior a
```

A `Behavior` acts like a continuous function of time.

We can construct a sample a `Behavior` from some `Event`, combine `Behavior`s
using `Applicative`, and sample a final `Behavior` on some other `Event`.

##### Instances
``` purescript
Functor Behavior
Apply Behavior
Applicative Behavior
(Semigroup a) => Semigroup (Behavior a)
(Monoid a) => Monoid (Behavior a)
```

#### `behavior`

``` purescript
behavior :: forall a. (forall b. Event (a -> b) -> Event b) -> Behavior a
```

Construct a `Behavior` from its sampling function.

#### `step`

``` purescript
step :: forall a. a -> Event a -> Behavior a
```

Create a `Behavior` which is updated when an `Event` fires, by providing
an initial value.

#### `unfold`

``` purescript
unfold :: forall a b. (a -> b -> b) -> Event a -> b -> Behavior b
```

Create a `Behavior` which is updated when an `Event` fires, by providing
an initial value and a function to combine the current value with a new event
to create a new value.

#### `sample`

``` purescript
sample :: forall a b. Behavior a -> Event (a -> b) -> Event b
```

Sample a `Behavior` on some `Event`.

#### `sampleBy`

``` purescript
sampleBy :: forall a b c. (a -> b -> c) -> Behavior a -> Event b -> Event c
```

Sample a `Behavior` on some `Event` by providing a combining function.

#### `sample_`

``` purescript
sample_ :: forall a b. Behavior a -> Event b -> Event a
```

Sample a `Behavior` on some `Event`, discarding the event's values.

#### `integral`

``` purescript
integral :: forall a t. Field t => Semiring a => (((a -> t) -> t) -> a) -> a -> Behavior t -> Behavior a -> Behavior a
```

Integrate with respect to some measure of time.

This function approximates the integral using the trapezium rule at the
implicit sampling interval.

The `Semiring` `a` should be a vector field over the field `t`. To represent
this, the user should provide a _grate_ which lifts a multiplication
function on `t` to a function on `a`. Simple examples where `t ~ a` can use
the `integral'` function instead.

#### `integral'`

``` purescript
integral' :: forall t. Field t => t -> Behavior t -> Behavior t -> Behavior t
```

Integrate with respect to some measure of time.

This function is a simpler version of `integral` where the function being
integrated takes values in the same field used to represent time.

#### `derivative`

``` purescript
derivative :: forall a t. Field t => Ring a => (((a -> t) -> t) -> a) -> Behavior t -> Behavior a -> Behavior a
```

Differentiate with respect to some measure of time.

This function approximates the derivative using a quotient of differences at the
implicit sampling interval.

The `Semiring` `a` should be a vector field over the field `t`. To represent
this, the user should provide a grate which lifts a division
function on `t` to a function on `a`. Simple examples where `t ~ a` can use
the `derivative'` function.

#### `derivative'`

``` purescript
derivative' :: forall t. Field t => Behavior t -> Behavior t -> Behavior t
```

Differentiate with respect to some measure of time.

This function is a simpler version of `derivative` where the function being
differentiated takes values in the same field used to represent time.

#### `fixB`

``` purescript
fixB :: forall a. a -> (Behavior a -> Behavior a) -> Behavior a
```

Compute a fixed point

#### `animate`

``` purescript
animate :: forall scene eff. Behavior scene -> (scene -> Eff (frp :: FRP | eff) Unit) -> Eff (frp :: FRP | eff) Unit
```

Animate a `Behavior` by providing a rendering function.


