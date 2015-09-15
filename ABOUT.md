# Janus
## Intent: 
  In order to further the development of a home use level of robotics and automation design, we must first enable the layman, with only basic computer skills, to create the robotics they need in a hobby type time-frame. It is these laymen with hobbies throughout history that have devised ingenious ways of instituting new technologies, and handing them a tool to abstract away all of the computing issues, and to let them just "do the thing," is the first step towards innumberable technological, and quality of life advances. The intent of Jane is to be the tool that allows this to happen.


# Architectural naming conventions:

### Bot-piece - Any computer.
  - In context, think of it as any micro-computer, but really though, any computer with wifi access.

### Janus - The whole damned thing
- The name of the entire suite including front facing user api, as well as the individual bot-pieces, and everything in between, in short, Janus would be the 'system name' as Janus is a system of systems.

These 3 are all just different instances of the same "Jane", they simply represent the specific instances roll within an architecture

### Jane - The bot-piece node.js software.
- Connects to other Janes using websockets in a lan
- Read/writes pin inputs/outputs from the jane++ software
- Passes command to sub level Janett devices, and may coordinate between multiple Janetts in a JaneBrain type fashion. 

 * This is also used as a reference to a single bot-piece with configured software, when used in this way it implies that the configured bot-piece is NOT a JaneBrain and NOT just a Janett

### JaneBrain - The controlling node in the bot-piece architecture.
  - Stores and forwards code-blocks (CB's) for child Janes
  - If a task requires coordination of components operated by seperate Janes, the JaneBrain would perform the coordination logic
        
        ie. 
        A drone uses 4 fans to manuever. Each may be controlled by a single Jane, 
        which all report to either a 5th JaneBrain device, or one of the 4 Janes 
        (which would be the acting JaneBrain). To manuever the drone, at least 
        one of the 4 fans must change its speed/direction (assuming mounted rotor 
        ducted fans). The JaneBrain would contain the CB that manages which fan(s) 
        to change, and in what way, then sends those commands along as a JSON 
        singleton object to be written to output, or as a stored CB.

### Janett - An "end node" bot-piece, 
- Controls only self pin-writes/self outputs utilizing the jane++ software
- Ss sent a JSON object, and writes it to the pins or other output. 
- It performs no logic. It only receives JSON orders, and returns JSON snapshots of itself
- The primary difference in a Janett configured device vs a Jane is that the Janett cannot pass information forward to another node, this is meant to be used on devices with weaker computing power, ie, the arduino. Great at writing to gpio, terrible at processing.

- Thus named because the pin-write code is in C++ therefore Jane++ -> Janett, maybe that will help with the visual


# Primary Usage Goal

Think app modularization, with stacking capability.
Crowd source coding meets The Android marketplace, meets complex application stacking, meets the Node.js "require" library (the concept of the library that is)...all in javascript...and maybe some c++

Users have a profile, and can publish "code-blocks", these may be either public or private - private accounts should pay ie. Github
- These code-blocks can have other code blocks dropped in them, this actually makes a new code-block. The new code-block executes both blocks of code, either synchronously or asynchronously, dependant on the function, or even as independant, non-interacting pieces of code.

if CB's are published public
  - Everyone can see them; They don't look like code. They look like color coded buttons, or bricks, or symbols... you get the point, something small, drag and droppable, and looks nothing like a piece of code.

if CB's are published private 
  - Only allowable entities may access them ie. team pages, company pages, etc

Users can take any code blocks they have access to, and combine them with other code blocks in a stacking manor.
Users can register "bots", which are entities that are JaneEnabled, and may receive CB's.

    boiled down to a simple example: 
    CB1 is a function to turn a GPIO pin on at x time. 
    CB2 is a function to turn a GPIO pin off at y time. 

    User takes CB1 and sticks it in a new code-block, with CB2, producing CB3.
    User sends code-block (CB3) to their bot

    The Jane server inserts the received CB and calls it as middleware

      at x time, the function configures a JSON representation of the GPIO pin being on, 
      and passes to the janett c++ GPIO reader/writer, which takes a JSON object, and
      returns a JSON object

      at y time, the function configures a JSON representation of the GPIO pin being off, 
      and passes to the janett c++ GPIO reader/writer


Well what about that bad code that technically works, but is really buggy, and no-one likes? This is were we start to look at the concept of crowd-sourcing at its finest. Public codes are all free. Good CB's get voted up. Bad CB's get voted down. If you don't like a CB, write one yourself, or combine until you get what you need.































