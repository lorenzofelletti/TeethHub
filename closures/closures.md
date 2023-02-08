[Home](../README.md) > [Closures](closures.md)

## Closures Examples
List of examples of closures you can bring at the exam.
The proposed examples are aimed at highlighting the main features of closures, in a simple, yet effective, way.

### Play-Pause Button
Simple example of using a closure to create a "play-pause button". The `playPauseButton` function takes: an initial state, a play function, and a pause function. It returns a function that can be called to toggle between the two states. The `playPause` variable is initialized to a function that can be called to toggle between play and pause.
```javascript
let playPauseButton = function(init, play, pause) {
  let isPlaying = init;
  return function() {
    if (isPlaying) {
      pause();
      isPlaying = false;
    } else {
      play();
      isPlaying = true;
    }
  };
};

let playPause = playPauseButton(
  false,
  function() { console.log("play"); },
  function() { console.log("pause"); }
);

playPause(); // play
playPause(); // pause
playPause(); // play
```

### Dummy Blockchain
Simple example of using a closure to "simulate" a blockchain. The `initBlockchain` function takes a function to be called when a block is added to the blockchain. It returns a function that can be called to add a block to the blockchain. The `blockchain` variable is initialized to a function that can be called to add a block to the blockchain.
```javascript
let initBlockchain = function(doOnAdd) {
    let chain = "";
    // generate a random between 0 and 1
    let challenge = Math.random();

    return function(block, guess) {
        // You could replace the condition with
        // Math.abs(challenge - guess) <= 0.01
        // to make the challenge easier
        if (guess === challenge) {
            // when the guess is correct, the block is added to the chain
            // and a new challenge is generated
            chain += block;
            challenge = Math.random();
            doOnAdd();
        }
        else {
            console.log("Try again, you will be luckier next time.");
        }
    }
}

let added = false;

let blockchain = initBlockchain(function() {
    added = true;
});

// loops until a block is added to the chain
while (!added) {
    blockchain("block", Math.random());
}

console.log("Congrats! You added a block to the blockchain!");
```

### Switch With an Enable
Example of a closure emulating a switch whose state can be changed only when it is enabled. The `switchWithEnable` function takes an initial state for the switch and an initial state for the enable. It returns an object with the following methods:
- `enable`: enables the switch
- `disable`: disables the switch
- `toggle`: toggles the switch if it is enabled
- `isOn`: returns switch's state. 
```javascript
let switchWithEnable = function(enableInit, initState) {
    let isEnabled = enableInit;
    let isOn = initState;
    return {
        enable: function() {
            isEnabled = true;
        },
        disable: function() {
            isEnabled = false;
        },
        toggle: function() {
            if (isEnabled) {
                isOn = !isOn;
            }
        },
        isOn: function() {
            return isOn;
        }
    }
}

let switch1 = switchWithEnable(true, false);
let switch2 = switchWithEnable(false, false);

switch1.toggle(); // switch1 is turned on
console.log(switch1.isOn()); // true

switch2.toggle(); // switch2 is not turned on
console.log(switch2.isOn()); // false

switch1.disable(); // switch1 is disabled
switch2.enable(); // switch2 is enabled
```
