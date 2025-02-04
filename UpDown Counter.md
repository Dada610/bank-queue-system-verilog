```verilog
module UpDown_counter (clk, reset, updown, pcount, alarm);  
  input wire reset, updown, clk;  
  output [3:0] pcount;  
  output reg alarm;   
  reg [3:0] pcount;  
  always @ (posedge clk, posedge reset)  
  begin  
    // If we reset we get the counter to 0 and the line is empty and not full  
   if (reset)  
      begin  
      pcount <= 0;  
      alarm <= 1'b0;  
      end  
   else  
   begin  
    // If we chose up we increase the number of people and if it's full   
    // we light the alarm up and not count up  
    if (updown)  
      begin  
        if(pcount == 7)  
        begin  
          alarm <= 1'b1;  
        end  
        else  
        begin  
          alarm <= 1'b0;  
          pcount <= pcount + 1;  
        end  
      end  
    // If we chose down we decrease the number of people and if it's empty  
    // we light the alarm and stop count down  
    else  
    begin  
        if (pcount == 0)  
        begin  
          alarm  <= 1'b1;  
        end  
      else  
        begin  
          alarm <= 1'b0;  
          pcount <= pcount - 1;  
        end  
    end  
  end  
  end  
endmodule
```
