# docs
Any configs, setup requirements, how-to's or otherwise should find itself in here.

# Janus
## Intent: 
  In order to further the development of a home use level of robotics and automation design, we must first enable the layman, with only basic computer skills, to create the robotics they need in a hobby type time-frame. It is these laymen with hobbies throughout history that have devised ingenious ways of instituting new technologies, and handing them a tool to abstract away all of the computing issues, and to let the just "do the thing," is the first step towards innumberable technological, and quality of life advances. The intent of Jane is to be the tool that allows this to happen.


# Architectural naming conventions:

### bot-piece - any computer.
  - in context, think of it as any micro-computer, but really though, any computer with wifi access.

### Janus 
- the name of the entire suite including front facing user api, as well as the individual bot-pieces, and everything in between, in short, Janus would be the 'system name' as Janus is a system of systems.

These 3 are all just different instances of the same "Jane", they simply represent the specific instances roll within an architecture

### Jane - the bot-piece node software
- connects to other Janes using websockets in a lan
- Read/wrties pin inputs/outputs from the jane++ software
- Passes command to sub level Janett devices, in a JaneBrain type fashion. 

   * this is also used as a reference to a single bot-piece with configured software, when used in this way it implies that the configured bot-piece is NOT a JaneBrain and NOT just a Janett

### JaneBrain - The controlling node in the bot-piece architecture.
  - stores and forwards code-blocks (CB's) for child Janes
  - if a task requires coordination of components operated by seperate Janes, the JaneBrain would perform the coordination logic
        
        ie. 
        A drone uses 4 fans to manuever. Each may be controlled by a single Jane, 
        which all report to either a 5th JaneBrain device, or one of the 4 Janes 
        (which would be the acting JaneBrain). To manuever the drone, at least 
        one of the 4 fans must change its speed/direction (assuming mounted rotor 
        ducted fans). The JaneBrain would contain the CB that manages which fan(s) 
        to change, and in what way, then sends those commands along as a JSON 
        singleton object to be written to output, or as a stored CB.

### Janett - a "end node" bot-piece, 
- controls only self pin-writes/self outputs utilizing the jane++ software
- is sent a JSON object, and writes it to the pins or other output. 
- it performs no logic. It only receives JSON orders, and returns JSON snapshots of itself
- the primary difference in a Janett configured device vs a Jane is that the Janett cannot pass information forward to another node, this is meant to be used on devices with weaker computing power, ie, the arduino. Great at writing to gpio, terrible at processing.

- Thus named because the pin-write code is in C++ therefore Jane++ -> Janett, maybe that will help with the visual


# Primary Usage Goal

  Think app modularization, with stacking capability
  Github meets The android marketplace meets complex modularized application stacking meets the Node "require" library
  ...all in javascript...and maybe some c++

  Users have a profile, and can publish "code-blocks", these may be either public or private - private accounts should pay ie. Github

  if CB's are published public
    - everyone can see them; They don't look like code. They look like color coded buttons, or bricks, or symbols... you get the point, something small, drag and droppable, and looks nothing like a piece of code.
    - theses code-blocks can have other code blocks dropped in them, this actually makes a new code-block. The new code-block executes both blocks of code, either synchronously or asynchronously, dependant on the function

  if CB's are published private 
    - only allowable entities may access them ie. team pages, company pages, etc

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












