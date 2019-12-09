# keyframe

A simple library for animation in Rust

[![Crate](https://img.shields.io/crates/v/keyframe.svg)](https://crates.io/crates/keyframe)
[![Downloads](https://img.shields.io/crates/d/keyframe.svg)](https://crates.io/crates/keyframe)
[![Documentation](https://docs.rs/keyframe/badge.svg)](https://docs.rs/keyframe)
![License](https://img.shields.io/crates/l/keyframe.svg)

## Features

* Several [easing functions](https://easings.net/en), including user-defined Bézier curves (like CSS [cubic-bezier](https://www.w3.org/TR/css-easing-1/#cubic-bezier-easing-functions)) and keyframable curves
* Animation sequences (like CSS [@keyframes](https://developer.mozilla.org/en-US/docs/Web/CSS/@keyframes))
* [mint](https://github.com/kvark/mint) integration for 2D/3D/4D support (points, rectangles, colors, etc)

## Usage

Tweening between two values is done with `keyframe::ease(function, from, to, time)`. `from` and `to` can be any type that implements `CanTween`, such as `f64` or `mint::Vector2`, while `time` needs to be a floating-point value between zero and one. `function` specifies the transition between `from` and `to` and is any type that implements `EasingFunction`.

`keyframe::AnimationSequence` can be used to create more complex animations that keeps track of keyframes, time, etc. You can create animation sequences with the `keyframes![...]` macro, from an iterator or from a vector.

## Examples

Tweening:

```rust
use keyframe::{ease, functions::*};

fn example() -> f64 {
	let a = 0.0;
	let b = 2.0;
	let time = 0.5;

	ease(EaseInOut, a, b, time)
}
```

Animation sequences:

```rust 
#[macro_use]
extern crate keyframe;

use keyframe::{Keyframe, AnimationSequence};

fn example() {
	// (value, time) or (value, time, function)
	let sequence = keyframes![
		(0.5, 0.0), 
		(1.5, 0.3, EaseIn), // <-- EaseIn used from 0.0 to 0.3
		(2.5, 1.0, Linear) // <-- Linear used from 0.3 to 1.0
	];

	sequence.advance_by(0.65);

	assert_eq!(sequence.now(), 2.0);
	assert_eq!(sequence.duration(), 1.0);
}
```