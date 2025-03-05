# node-red-contrib-reboot-persistent-fsm

A Node-Red node that wraps around the [Javascript State Machine](https://www.npmjs.com/package/javascript-state-machine) to implement a [finite state machine](https://en.wikipedia.org/wiki/Finite-state_machine) for Node-Red.

An extension of [hufftheweevil](https://github.com/hufftheweevil/node-red-contrib-persistent-fsm/)'s persistent FSM, which is a non-reboot persistent combination of [DeanCording](https://github.com/DeanCording/node-red-contrib-state-machine)'s and [lutzer](https://github.com/lutzer/node-red-contrib-finite-statemachine)'s similar libraries with the following notable change:

- Allows picking a custom variable to persist the FSM state with support of [Node-RED contexts](https://nodered.org/docs/user-guide/context) enabling reboot persistence

As this is a simple fork of [hufftheweevil](https://github.com/hufftheweevil/node-red-contrib-persistent-fsm/)'s persistent FSM, the following features were introduced by them:

- State can optionally persist during re-deployment of nodes.
- Deprecated Node-RED events have been replaced.
- Invalid transition error includes details of request.
- Excellent input UI from [DeanCording](https://github.com/DeanCording) combined with simple graphical rendering from [lutzer](https://github.com/lutzer). Best of both worlds.

### Usage

The node is configured with a number of states and triggers that will cause the node to transition from one state to another. At any time, the node can only be in one of the defined states and it will only transition to another state when it receives a trigger defined for the current state. The same trigger can be used to cause transitions from more than one state.

Triggers that are not valid for the current state can either be ignored or cause an error to be throw that can be caught by a Catch node.

The node will always start in the first state on the state list (unless "Persist on re-deploy" is enabled) and will (optionally) emit a message with the initial (or saved) state if the state output is set to a message property.

Global and flow context properties can be used as trigger inputs but state transitions will only occur when the node receives a message.

The current state can be set to a msg property or stored as a flow or global context property. If the state output is set to a msg property, that property is set in the original message and passed through, otherwise no messages are output.

Picking a custom persist cache allows selecting a variable in a reboot persistent [Node-RED context](https://nodered.org/docs/user-guide/context). Reading from this variable should not pose any issues - writing to it may impact the behavior of the state machine in unexpected ways, thus should be avoided in case of uncertainty.