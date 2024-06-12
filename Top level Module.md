```verilog
module Bank_System (clk, updown, reset, push, teller,   
full, empty, alarm, display1, display2, display3);

// We have five inputs to get from the user  
input wire clk, reset, push, updown;  
input [2:0] teller;

 
output wire full, empty, alarm;  
wire [3:0] pcount;  
output [6:0] display1, display2, display3;  
wire [1:0] tcount;  
wire [4:0] wtime;  
wire [3:0] first_digit, second_digit;
wire debounced_clk;

 
slowClock divi(clk, reset, slow_clk);  
FF ff (slow_clk, push, debounced_clk);
UpDown_counter counter (debounced_clk, reset, updown, pcount, alarm);
flags f (pcount, empty, full);
Teller t (teller,tcount);
waiting_time wt(pcount, tcount, wtime);

// Then we get the two digits separately  
assign first_digit = wtime % 10;  
assign second_digit = wtime / 10;

// It's time to display the results above   
seg d1 (pcount, display1);  
seg d2 (first_digit, display2);      
seg d3 (second_digit, display3);  
endmodule
```