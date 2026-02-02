# HDLBits-tutorial-practice
회로 설계 디자이너를 꿈꾸는 학생의 베릴로그 입문

-purpose: Verilog 입문부터 sequential logic까지 학습.

-tool: HDLBits, Verilog Tutorial 사이트를 이용.

2026/01/19 study: vector의 개념과 기초 연산
  bitwise/logical operator의 차이
  
    assign out_or_bitwise[0] = a[0]|b[0];
    assign out_or_bitwise[1] = a[1]|b[1];
    assign out_or_bitwise[2] = a[2]|b[2];
    
    assign out_or_logical = a||b;
    assign out_not [2:0]= ~a;
    assign out_not [5:3]= ~b;

  Output pins are stuck at VCC or GND
  성공한 것에서 멈추지 않고 위의 warning문구의 내용을 찾아보고 실제 설계에서 이러한 결과가 왜 나오는지 찾아보았다.
  ->특정 bit를 상수값으로 지정해서 경고 문구가 발생. 현업에서 자주 발생하는 실수이므로 가볍게 여기지 말 것.

2026/02/02 practice: FSM 기능의 이해 및 구현

    module top_module(
    input clk,
    input areset,   
    input in,
    output out);
    
    parameter A=0, B=1; 
    reg state, next_state;

    always @(*) begin    // This is a combinational always block
        case (state)
            A: begin 
                if (in) next_state = A;
                else next_state = B;
            end
            B: begin
                if (in) next_state = B;
                else next_state = A;
            end
        endcase
        // State transition logic
    end

    always @(posedge clk, posedge areset) begin    // This is a sequential always block
        if (areset) state <= B;
        else begin 
            state <= next_state;
        end
        // State flip-flops with asynchronous reset
    end

    // Output logic
    // assign out = (state == ...);
    assign out = (state == B);

endmodule

위의 코드는 HDLBits를 이용해서 구현한 FSM.
다음 클럭 엣지에 할당할 data를 저장하기 위해 next_state를 reg type으로 지정.
always 블록을 sequential logic과 combinational logic으로 나눠서 data를 계산하는 블록과 data를 전송하는 블록으로 구분.
