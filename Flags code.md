```verilog
 
module flags (pcount, empty, full);  
  input [3:0] pcount;  
  output reg empty, full;   
  always @ (pcount)  
  begin  
  // If it's 7 it's full and not empty  
  if(pcount == 7)  
  begin  
    full = 1'b1;  
    empty = 1'b0;  
  end  
  // If it's 0 it's empty and not full  
  else if(pcount == 0)  
    begin  
    empty = 1'b1;  
    full = 1'b0;  
    end  
  // Else it's neither  
  else  
    begin  
      full = 1'b0;  
      empty = 1'b0;  
      end  
  end  
endmodule

module flags_t();  
  reg [3:0] pcount;   
  wire empty, full;  
  initial  
  begin   
  pcount = 1;  
  #100; pcount = 7;  
  #100; pcount = 0;  
  #100; pcount = 3;  
  #100; pcount = 7;  
  #100; pcount = 0;  
  end  
  flags t(pcount, empty, full);  
endmodule
```