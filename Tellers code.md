```verilog
module Teller (teller, tcount);  
input [2:0]teller;  
output wire [1:0] tcount;

// Sum is tcount[0]   
// S = A ^ B ^ C  
assign tcount [0] = teller[0] ^ teller[1] ^ teller[2];

// Carry is the tcount[1]  
// Carry = AB + ACin + BCin  
assign tcount [1] = (teller[0] & teller[1]) + (teller[2] & teller[1]) + (teller[0] & teller[2]) ;  
endmodule
```
