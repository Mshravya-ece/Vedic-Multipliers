# Vedic-Multipliers

Vedic sutra is used for multiplier design, which provides better performance and consumes less time for computation. The main algorithm of Vedic multiplication is Urdhva Triyakbhyam. It means vertically and crosswise.

Vedic 2X2 Multiplier

Algorithm steps:

<img width="507" height="338" alt="image" src="https://github.com/user-attachments/assets/aee5e67b-ec81-4a53-8f8b-ead8229f2e45" />

Based on the steps, the circuit can be designed as:

<img width="462" height="299" alt="image" src="https://github.com/user-attachments/assets/b77259c1-27be-4bdb-ac9f-2c98d8292afb" />

Verilog Code of Vedic 2x2 multiplier:


module vedic_2x2(a,b,c);

input [1:0]a;

input [1:0]b;

output [3:0]c;

wire [3:0]c;

wire [3:0]temp;

assign c[0] = a[0] & b[0];

assign temp[0] = a[1] & b[0];

assign temp[1] = a[0] & b[1];

assign temp[2] = a[1] & b[1];

half_adder z1 (temp[0], temp[1], c[1], temp[3]);

half_adder z2 (temp[2], temp[3], c[2], c[3]);

endmodule


Verilog code of modules used:


module half_adder(a,b,sum,carry);

input a,b;

output sum, carry;

xor (sum,a,b);

and (carry,a,b);

endmodule


Testbench Code:


module vedic_2x2_tb();

reg [1:0]a;
reg [1:0]b;
wire [3:0]c;

vedic_2x2 uut (.a(a), .b(b), .c(c));

initial begin

a=2'b00;b=2'b00;
#5 a=2'b00; b=2'b01;
#5 a=2'b00; b=2'b10;
#5 a=2'b00; b=2'b11;

#5 a=2'b01;b=2'b00;
#5 a=2'b01; b=2'b01;
#5 a=2'b01; b=2'b10;
#5 a=2'b01; b=2'b11;

#5 a=2'b10;b=2'b00;
#5 a=2'b10; b=2'b01;
#5 a=2'b10; b=2'b10;
#5 a=2'b10; b=2'b11;

#5 a=2'b11;b=2'b00;
#5 a=2'b11; b=2'b01;
#5 a=2'b11; b=2'b10;
#5 a=2'b11; b=2'b11;
#5 $finish;

end

endmodule

Schematic after RTL Analysis:

<img width="940" height="329" alt="image" src="https://github.com/user-attachments/assets/6e9260de-7419-4d29-9d69-eaa65b7da66c" />

Schematic after Synthesis:

<img width="524" height="280" alt="image" src="https://github.com/user-attachments/assets/6fd305f7-dcf1-413b-8a00-4f305515baa6" />

Output after Simulation:

<img width="940" height="193" alt="image" src="https://github.com/user-attachments/assets/f7ecd62a-1cf2-4afc-96a7-526620465bf9" />


Vedic 4x4 multiplier:

Algorithm steps:

<img width="780" height="139" alt="image" src="https://github.com/user-attachments/assets/a6b20cf5-7ca9-4f3e-996f-815230361470" />

<img width="521" height="275" alt="image" src="https://github.com/user-attachments/assets/75159914-8af6-41d5-abd7-f341334a3b01" />

Based on the steps, the circuit can be designed as:

<img width="506" height="346" alt="image" src="https://github.com/user-attachments/assets/344115f3-5571-4bf0-a6df-7eddbb65a4a0" />

Verilog Code of Vedic 4x4 multiplier:

module vedic_4x4(a,b,c);

input [3:0]a;
input [3:0]b;
output [7:0]c;

wire [3:0]q0;
wire [3:0]q1;
wire [3:0]q2;
wire [3:0]q3;
wire [7:0]c;

wire [3:0]temp1;
wire [5:0]temp2;
wire [5:0]temp3;
wire [5:0]temp4;

wire [3:0]q4;
wire [5:0]q5;
wire [5:0]q6;

vedic_2x2 z1 (a[1:0], b[1:0], q0[3:0]);

vedic_2x2 z2 (a[3:2], b[1:0], q1[3:0]);

vedic_2x2 z3 (a[1:0], b[3:2], q2[3:0]);

vedic_2x2 z4 (a[3:2], b[3:2], q3[3:0]);

assign temp1={2'b0,q0[3:2]};

add_4bit z5 (q1[3:0],temp1,q4);

assign temp2={2'b0,q2[3:0]};

assign temp3={q3[3:0],2'b0};

add_6bit z6 (temp2,temp3,q5);

assign temp4={2'b0,q4[3:0]};

add_6bit z7 (temp4,q5,q6);

assign c[1:0]= q0[1:0];

assign c[7:2]= q6[5:0];

endmodule

Modules used in this code:

module add_4bit(a,b,sum);        
input [3:0]a,b;
output [3:0]sum;
assign sum=a+b;
endmodule

module add_6bit(a,b,sum);
input [5:0]a,b;
output [5:0]sum;
assign sum=a+b;
endmodule

Testbench Code:

module vedic_4x4_tb();

reg [3:0]a;
reg [3:0]b;
wire [7:0]c;

vedic_4x4 uut (.a(a), .b(b), .c(c));

initial begin

a=4'b0000;b=4'b0001;
#5 a=4'b0010;b=4'b0001;
#5 a=4'b0101;b=4'b0010;
#5 a=4'b0011;b=4'b0011;
#5 a=4'b0001;b=4'b0100;
#5 a=4'b1000;b=4'b0101;
#5 a=4'b0100;b=4'b0110;
#5 a=4'b0000;b=4'b0111;
#5 a=4'b0010;b=4'b1000;
#5 a=4'b1100;b=4'b1001;
#5 a=4'b1000;b=4'b1010;
#5 a=4'b0011;b=4'b1011;
#5 a=4'b0110;b=4'b1100;
#5 a=4'b0100;b=4'b1101;
#5 a=4'b0001;b=4'b1110;
#5 a=4'b0010;b=4'b1111;

#5 $finish;

end

endmodule

Schematic after RTL Analysis:

<img width="787" height="338" alt="image" src="https://github.com/user-attachments/assets/32d34b88-3777-4af7-917a-7a6352d2cb3a" />

Schematic after Synthesis:

<img width="617" height="428" alt="image" src="https://github.com/user-attachments/assets/6705d0d1-7795-4a0a-8e60-5f82c6aaed43" />

Output after Simulation:

<img width="940" height="192" alt="image" src="https://github.com/user-attachments/assets/26d77eda-567e-4a20-9574-0073cec115e7" />

Vedic 8x8 multiplier

Algorithm steps:

<img width="939" height="88" alt="image" src="https://github.com/user-attachments/assets/4913c6d8-01b4-4abd-90e1-625c5b148d87" />

<img width="664" height="352" alt="image" src="https://github.com/user-attachments/assets/66c5188e-68d3-458c-b395-d9fb7fab3888" />

Based on this steps, the circuit can be designed as:

<img width="767" height="374" alt="image" src="https://github.com/user-attachments/assets/117f332f-2322-40ba-9009-b5a59a4db916" />


Verilog code of 8x8 Vedic Multiplier:

module vedic_8x8(a,b,c);

input [7:0]a;
input [7:0]b;

output [15:0]c;

wire [15:0]q0;
wire [15:0]q1;
wire [15:0]q2;
wire [15:0]q3;

wire [15:0]c;

wire [7:0]temp1;
wire [11:0]temp2;
wire [11:0]temp3;
wire [11:0]temp4;

wire [7:0]q4;
wire [11:0]q5;
wire [11:0]q6;

vedic_4x4 z1 (a[3:0],b[3:0],q0[15:0]);

vedic_4x4 z2 (a[7:4],b[3:0],q1[15:0]);

vedic_4x4 z3 (a[3:0],b[7:4],q2[15:0]);

vedic_4x4 z4 (a[7:4],b[7:4],q3[15:0]);

assign temp1= {4'b0,q0[7:4]};

add_8bit z5 (q1[7:0],temp1,q4);

assign temp2 = {4'b0,q2[7:0]};

assign temp3 = {q3[7:0],4'b0};

add_12bit z6 (temp2,temp3,q5);

assign temp4 = {4'b0,q4[7:0]};

add_12bit z7 (temp4,q5,q6);

assign c[3:0] = q0[3:0];

assign c[15:4] = q6[11:0];

endmodule

Modules used in this code:

module add_8bit(a,b,sum);

input [7:0]a,b;

output[7:0]sum;

assign sum=a+b;

endmodule

module add_12bit(a,b,sum);

input [11:0]a,b;

output [11:0]sum;

assign sum=a+b;

endmodule

Testbench Code:

module vedic_8x8_tb();

reg [7:0]a;
reg [7:0]b;
wire [15:0]c;

vedic_8x8 uut (.a(a), .b(b), .c(c));

initial begin 

$monitor ($time, "a=%b, b=%b, ... c=%b\n", a,b,c);

a=0;
b=0;
#100;

a=8'd255;
b=8'd255;
#100;

a=8'd5;
b=8'd3;
#100;

a=8'd4;
b=8'd2;
#100;

a=8'd6;
b=8'd8;
#100;

a=8'd2;
b=8'd2;
#100;

end

Schematic after RTL Analysis:

<img width="352" height="531" alt="image" src="https://github.com/user-attachments/assets/5ac8f080-b28b-49d7-ad7c-c180f9c601e2" />

Output:

<img width="939" height="183" alt="image" src="https://github.com/user-attachments/assets/0c884d15-cc76-43bf-a1f7-c6e75e3ee4b0" />
