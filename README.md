# EXPERIMENT 3B: Simulation of All Flip-Flops using Non Blocking Statement

## AIM
To design and simulate basic flip-flops (SR, D, JK, and T) using **Non blocking statements** in Verilog HDL, and verify their functionality through simulation in Vivado 2023.1.

## APPARATUS REQUIRED
- Vivado 2023.1
- Computer with HDL Simulator

## DESCRIPTION
Flip-flops are the basic memory elements in sequential circuits.  
In this experiment, different types of flip-flops (SR, D, JK, T) are modeled using **behavioral modeling** with **Non blocking assignment (`<=`)** inside the `always` block.  
Non Blocking assignments execute sequentially in the given order, which makes it easier to describe simple synchronous circuits.

## PROCEDURE
1. Open **Vivado 2023.1**.  
2. Create a **New RTL Project** (e.g., `FlipFlop_Simulation`).  
3. Add Verilog source files for each flip-flop (SR, D, JK, T).  
4. Add a testbench file to verify all flip-flops.  
5. Run **Behavioral Simulation**.  
6. Observe waveforms of inputs and outputs for each flip-flop.  
7. Verify that outputs match the truth table.  
8. Save results and capture simulation screenshots.

---

## VERILOG CODE

### SR Flip-Flop (Non Blocking)
```verilog
module sr_ff (input clk,input S,input R,output reg Q);
always @(posedge clk)
 begin
    case ({S,R})
      2'b00: Q <= Q;    
      2'b01: Q <= 0;    
      2'b10: Q <= 1;    
      2'b11: Q <= 1'bx; 
 endcase
 end
endmodule
```
### SR Flip-Flop Test bench 
```verilog
module sr_ff_tb;
  reg clk, S, R;
  wire Q;
  sr_ff uut (.clk(clk),.S(S),.R(R),.Q(Q));
  initial begin
   clk = 0;
  forever #10 clk = ~clk; 
  end
  initial begin
    S = 0; R = 0;
    #100 S = 1; R = 0;   
    #100 S = 0; R = 0;   
    #100 S = 0; R = 1;   
    #100 S = 1; R = 1;  
    #100 S = 0; R = 0;
 end
endmodule


```
#### SIMULATION OUTPUT

<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/5d46edf8-bfc8-4517-bf7f-5f181825de58" />

---

### JK Flip-Flop (Non Blocking)
```verilog
module jk_ff(input clk,J,K, output reg Q);
always @(posedge clk) begin
case({J,K})
2'b00: Q<=Q;
2'b01: Q<=0;
2'b10: Q<=1;
2'b11: Q<=~Q;
endcase
end
endmodule
```
### JK Flip-Flop Test bench 
```verilog
module tb_jk_ff;
  reg clk;
  reg J, K;
  wire Q;
  jk_ff uut (.clk(clk),.J(J),.K(K),.Q(Q));
initial begin
clk=0;
forever #20 clk=~clk;
end
initial begin
 J = 0; K = 0;
    #100 J=0; K=0;  
    #100 J=0; K=1; 
    #100 J=1; K=0;  
    #100 J=1; K=1;  
    #100 J=0; K=1; 
    #100 J=1; K=0;  
    #100 J=1; K=1;  
end
endmodule


```
#### SIMULATION OUTPUT

<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/44ae56a5-d8a3-43f4-ac9a-d5a85b3406bb" />

---
### D Flip-Flop (Non Blocking)
```verilog
module d_ff (
module dff ( clk, rst,d, q);
input clk,rst,d;
output reg q;
    always @(posedge clk) begin
        if (rst)   
            q <= 1'b0;
        else
            q <= d;  
    end
endmodule
```
### D Flip-Flop Test bench 
```verilog
module dff_tb;
    reg clk_t, rst_t, d_t;
    wire q_t;
    dff dut (.clk(clk_t),.rst(rst_t),.d(d_t),.q(q_t) );
    initial begin
        clk_t = 1'b0;
        rst_t = 1'b1;  
        d_t   = 1'b0;
        #100 rst_t = 1'b0; 
        #100 d_t = 1'b1;
        #100 d_t = 1'b0;
        #100 d_t = 1'b1;
end
     always #10 clk_t = ~clk_t;
endmodule


```

#### SIMULATION OUTPUT

<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/d4e1180d-6dc0-4bb1-9578-e736f41b8d73" />

---
### T Flip-Flop (Non Blocking)
```verilog
module t_ff(clk,rst,Tout,T);
    input clk,rst,T;
    output reg Tout;
    always@ (posedge clk)
     begin
     if(rst)
        Tout = 1'b0;
     else if(T)
        Tout = ~Tout;
     else
        Tout = Tout;
     end
endmodule
```
### T Flip-Flop Test bench 
```verilog
module t_ff_tb;
  reg clk, rst, T;
  wire Tout;
  t_ff uut (.clk(clk),.rst(rst),.T(T),.Tout(Tout));
  initial begin
    clk = 0;
    forever #10 clk = ~clk;  
  end
  initial begin
    
    rst = 1; T = 0;
    #20 rst = 0;    
    #20 T = 1;      
    #20 T = 0;      
    #20 T = 1;      
    #20 T = 1;      
    #20 T = 0;
  end

endmodule


```

#### SIMULATION OUTPUT
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/d1fc49d1-8f1f-4cdb-8b69-0f69abc3fbde" />


---

### RESULT

All flip-flops (SR, D, JK, T) were successfully simulated using Non blocking statements in Verilog HDL.
The outputs matched the expected truth table values, demonstrating correct sequential behavior.
