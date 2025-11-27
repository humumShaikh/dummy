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



Q1. 




Design : -

module PIPO(
    input wire clk,
    input wire reset,                   //active high reset
    input wire shift_enable,            //shift enabling signal
    input wire [3:0] parallel_IN,       //4 bit input data     
    output reg [3:0] parallel_OUT       //4 bit output register
    );
    
    
    always @(posedge clk)               //trigger
    begin //
        if(reset)                       //reset on the clock hence synchronous reset
        begin
            parallel_OUT <= 0;          //reset the output register to 0 on reset
        end
        
        else if(shift_enable)           //if shift_enable = 1 , then shifting operation will be performed
        begin
            parallel_OUT <= parallel_IN;
        end
    end //
    
endmodule








Testbench : -

module tb_PIPO();

    reg clk;
    reg reset;
    reg shift_enable;
    reg [3:0] parallel_IN;
    wire [3:0] parallel_OUT;
    
    PIPO Parallel_In_Parallel_Out_Shift_Register(           //instantiating the design in the testbench
        .clk(clk),                                          			//connecting the ports with the testbench ports
        .reset(reset),
        .shift_enable(shift_enable),
        .parallel_IN(parallel_IN),
        .parallel_OUT(parallel_OUT)
    );
    
    always #5 clk <= ~clk;                                  //giving the stimulus to the clock signal  , 10ns time period clock
    
    initial
    begin
        clk <= 0;                                           //setting the clock to 0
        reset <= 1;
        shift_enable <= 0;
        
        @(posedge clk)  reset <= 0;
        
        @(posedge clk)
        begin
        shift_enable <= 1;
        parallel_IN <= 4'hA;
        end
        
        @(posedge clk)  parallel_IN <= 4'hB;
        
        @(posedge clk)  parallel_IN <= 4'hC;
        
        @(posedge clk)  parallel_IN <= 4'hD;
        
        @(posedge clk)
        begin
        shift_enable <= 0;
        parallel_IN <= 4'hE;     
        end
        
        @(posedge clk)  shift_enable <= 1;
        @(posedge clk)  shift_enable <= 0;
        
        @(posedge clk)  parallel_IN <= 4'hF;
        
        @(posedge clk)  shift_enable <= 1;
        @(posedge clk)  shift_enable <= 0;
        
        @(posedge clk)  reset <= 1;
        @(posedge clk)  reset <= 0;
        
        #50;
        $finish;
        
    end

