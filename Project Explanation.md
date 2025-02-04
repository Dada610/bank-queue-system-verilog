1.`UpDown_counter` module is a digital logic component implemented in Verilog, which serves as a bidirectional counter with an alarm feature. Its primary function is to keep track of the number of people entering or leaving bank, adjusting a count in response to these movements and providing an alarm under specific conditions.

[[UpDown Counter]]

**Key Features:**

1. **Bidirectional Counting:** The counter can increment or decrement based on the input signal, allowing it to adapt to people entering or leaving.
2. **Alarm Mechanism:** It includes an alarm system that activates when predefined conditions are met, such as reaching maximum capacity or dropping to zero, ensuring immediate alerts for critical situations.
3. **Reset Capability:** At any point, the counter can be reset to zero through an external signal, allowing for quick reinitialization without manual intervention.



2.`clock_divider` module is designed to generate a slower clock signal (`slow_clk`) from a faster clock signal (`clk`) using clock division. The slower clock signal is generated by counting up to a certain threshold (1,000,000 in this case) and then toggling the `slow_clk` signal.

[[Clock divider code]]

### Logic

1. `input clk, reset;`: These are the input signals. `clk` represents the fast clock signal, and `reset` is the reset signal.
   
2. `output reg slow_clk;`: This declares `slow_clk` as an output signal. The `reg` keyword indicates that `slow_clk` is a register, meaning it can retain its value between clock cycles.

3. `reg [24:0] count;`: This declares a 25-bit register called `count`, which is used to count the number of clock cycles.

4. `always @ (posedge clk, posedge reset)`: This is a combinational block triggered on the positive edge of `clk` or `reset`.

5. `begin`: This marks the beginning of the procedural block inside the `always` block.

6. `if(reset)`: This condition checks if the `reset` signal is active.

7. `count <= 0; slow_clk <= 0;`: If `reset` is active, both the `count` and `slow_clk` signals are reset to zero.

8. `if (count < 1_000_000)`: If `reset` is not active, this condition checks if the `count` is less than 1,000,000.

9. `count <= count + 1;`: If the count is less than 1,000,000, it increments the `count` register by 1 on each clock cycle.

10. `slow_clk = ~slow_clk; count <= 0;`: When the count reaches 1,000,000, it toggles the `slow_clk` signal and resets the `count` to zero, effectively dividing the frequency of `clk` by 1,000,000 and generating a slower clock signal.

This module essentially creates a clock divider that generates a slower clock signal (`slow_clk`) by toggling it after counting a certain number of clock cycles (`1_000_000` in this case). This slower clock signal can be useful for synchronizing operations or for generating signals at a lower frequency than the main clock.

3.`QueueFSM` is designed to for managing Bank Queue

[[QueueFSM code]]

### Logic

1. `input clk, reset, photocellBack, photocellFront;`: These are input signals. `clk` represents the clock signal, `reset` is the reset signal, `photocellBack` indicates the presence of a person at the back of the queue, and `photocellFront` indicates the presence of a person at the front of the queue.

2. `input [1:0] Tcount;`: This input represents the number of tellers in service.

3. `output reg [3:0] nextState, currentState;`: These are output signals representing the next state and the current state of the FSM.

4. `output reg queueOpen, queueClosed, queueFull, queueEmpty;`: These are output signals indicating whether the queue is open, closed, full, or empty.

5. `parameter IDLE = 4'b0000;`, `QUEUE_OPEN = 4'b0001;`, `QUEUE_CLOSED = 4'b0010;`, `QUEUE_FULL = 4'b0011;`, `QUEUE_EMPTY = 4'b0100;`: These are parameterized values representing the states of the FSM.

6. The `always @(posedge clk or posedge reset)` block defines the behavior of the FSM based on clock edges and the reset signal.

7. Within the `always` block, there's a `case` statement that defines the behavior of the FSM based on its current state (`currentState`) and the input signals.

8. The FSM transitions between states based on the current state and the input signals. For example, if the FSM is in the `IDLE` state and `photocellBack` is active while `photocellFront` is inactive, it transitions to the `QUEUE_OPEN` state.

9. The `always @*` block defines the output logic based on the current state of the FSM. Each output signal is assigned based on the current state.

This FSM manages the queue based on the presence of people at the front and back of the queue and the number of tellers in service. Depending on these conditions, it transitions between different states and sets output signals accordingly to indicate the status of the queue.

4`Flags` This module is designed to determine whether a queue (most likely representing the number of people waiting) is either empty or full based on the count of people in the queue, represented by the input `pcount`. Let's break down both the `flags` module and the accompanying test module `flags_t`.

[[Flags code]]

**Logic:**  
- The `always` block triggers on any change to `pcount`.  
- Inside the `always` block, different conditions check the value of `pcount`:  
  - **Full Queue**: If `pcount` equals 7, it sets `full` to 1 (true) and `empty` to 0 (false). This means the queue can hold up to 7 people before it's considered full.  
  - **Empty Queue**: If `pcount` is 0, it sets `empty` to 1 (true) and `full` to 0 (false), indicating there are no people in the queue.  
  - **Neither Full Nor Empty**: For any other value of `pcount`, both `full` and `empty` are set to 0, meaning the queue has people but is neither full nor empty.

5.`ROM` is a piece of Verilog code designed to simulate a read-only memory (ROM) that outputs a predetermined waiting time (`wtime`) based on the number of people currently in the queue (`pcount`) and another parameter,  representing the number of tellers  (`tcount`).

[[ROM code]]

### Logic:  
- **ROM Simulation**: The `always @(*)` block uses a `case` statement to simulate the behavior of a ROM. This block is sensitive to any changes in the inputs (because of `*`), which means it will re-evaluate whenever `pcount` or `tcount` changes.  
- **Case Statement**: The `case` statement maps different combinations of `tcount` and `pcount` to specific waiting times:  
  - **`6'b010000` to `6'b010111`**: These cases handle when `tcount` is 1 (binary `01`), and `pcount` ranges from 0 to 7. For each combination, a specific waiting time is set.  
  - **`6'b100000` to `6'b100111`**: These handle when `tcount` is 2 (binary `10`), mapping different values of `pcount` to waiting times.  
  - **`6'b110000` to `6'b110111`**: These address the situation when `tcount` is 3 (binary `11`), again assigning waiting times based on `pcount`.  
  - **`default`**: Any other combination not explicitly mentioned (like when `tcount` is 0) results in a waiting time of 0.

6.`D_FlipFlop` is employed to solve debounced clock problems

[[FlipFlop code]]

 Practical Usage in project:  
  
- **Debouncing**:  is a process used to ensure that erratic signals from mechanical switches or noisy environments are smoothed out before being used in logic circuits.

7.`Tellers` is designed to encode the status of three tellers into a 2-bit output using the logic of a full adder circuit. Each teller's status is represented by a bit in a 3-bit input teller, and the output tcount provides a 2-bit encoded value.

[[Tellers code]]

 Encoding Logic:  
- **`assign tcount[0] = teller[0] ^ teller[1] ^ teller[2];`** - This line computes the sum (S) of the full adder for three inputs. It uses the XOR (^) operation, which results in `1` if an odd number of inputs are `1`, and `0` otherwise. This bit effectively counts the parity of the number of `1`s in the input, used commonly to check odd or even counts.  
    
- **`assign tcount[1] = (teller[0] & teller[1]) + (teller[2] & teller[1]) + (teller[0] & teller[2]);`** - This line computes the carry (C) output of the full adder. The carry bit is set if any two of the three inputs are `1`, representing a binary addition where the sum exceeds the maximum that can be represented by one bit. This line effectively counts scenarios where at least two tellers are busy.


8.`Bank_System` this module encapsulates a comprehensive bank queue management system, designed to be implemented on an FPGA (Field-Programmable Gate Array). This system integrates various components to handle input signals from the environment, process the number of people in a queue, manage teller statuses, compute waiting times, and display relevant information.

[[Top level Module]]

### Key Components and Functionality:  
- **Clock Divider (`slowClock`)**:  
  - Reduces the frequency of the main clock signal for stable operations and handling debouncing of the push button input.  
    
- **Debouncing Flip-Flop (`FF`)**:  
  - Stabilizes the push button signal to avoid multiple triggers due to mechanical bouncing.  
    
- **UpDown Counter (`UpDown_counter`)**:  
  - Counts the number of people in the queue, increasing or decreasing based on the `updown` signal. It can also trigger an `alarm` under certain conditions (like exceeding capacity).  
    
- **Queue Status Flags (`flags`)**:  
  - Determines if the queue is `full` or `empty` based on the person count (`pcount`).  
    
- **Teller Status Encoder (`Teller`)**:  
  - Condenses the 3-bit teller status into a 2-bit value for further processing or simplification.  
    
- **Waiting Time Calculation (`waiting_time`)**:  
  - Computes the waiting time based on the number of people (`pcount`) and teller status (`tcount`). This likely uses predefined values stored similar to a ROM.  
    
- **Digit Separation**:  
  - Extracts individual digits from the computed waiting time (`wtime`) to be displayed on separate seven-segment displays.  
    
- **Seven-Segment Display Drivers (`seg`)**:  
  - Converts numerical values into signals that can control seven-segment displays, showing the queue count (`pcount`) and the two digits of the waiting time.