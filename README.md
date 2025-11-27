module binary_counter(
    input wire clk,
    input wire reset,
    output wire [3:0] count_bin
    );
    
    reg [3:0] counter = 0;      //counter register to hold the count value
        
    always @(posedge clk)
    begin
        if(reset) counter <= 0;     //synchronous reset
        
        else
        begin
            counter <= counter + 1;     //counter increment logic
        end
    end
    
    assign count_bin = counter;         //assign the register value to the output wire
    
endmodule
