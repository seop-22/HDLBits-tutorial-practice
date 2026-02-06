# Verilog를 활용한 Digital Clock구현
회로 설계 디자이너를 꿈꾸는 학생의 베릴로그 입문

-purpose: Verilog 입문부터 sequential logic까지 학습.

-tool: HDLBits, Verilog Tutorial 사이트를 이용.

# HDLBits를 이용한 Verilog 입문
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

## 2/3 practice
### serial FSM (UART) 구현

발생한 문제: 코드 간의 combinational logic과 sequential logic을 구분하지 못함.
->state값이 갱신되는 시기와 cnt가 갱신되는 시기가 달라져서 논리에 오류 발생.

state를 두가지가 아닌 세 가지로 구분해서 해결함.


# VS code 모듈 생성
## 2/5 practice
### 클럭 주파수에 맞춰서 1초 신호를 만들어내는 모듈
-problem: 1초마다 신호를 주는 모듈의 output signal을 1) pulse 2) 반짝 신호 둘 중에 선택이 필요함
-만약 pulse로 만들지 않는다면 always(*)문을 사용해야함, 다음 모듈에서 클럭 신호에 동기화시키지 못한다는 문제가 발생할 것 같음
---> 결국에는 clk처럼 pulse 형태를 가지는 신호로 만들기로 결정함.


## 2/6 practice
### gtkwave를 이용한 파형 확인
테스트 벤치 코드와 실제 모듈을 구현한 코드를 이용해서 gtk wave에서 파형을 확인함
<img width="743" height="461" alt="image" src="https://github.com/user-attachments/assets/52a6a8ec-711f-4d13-922c-8df26dd70fb9" />

1초가 지날 때마다 pulse 신호를 잠깐 주는 모듈을 완성

고찰: pulse와 toggle 중에 어떤 것을 사용해야 하는지에 대한 고민

-> 일반적으로 사용하는 클럭 신호는 toggle형태의 사각파, 하지만 클럭 신호에 동기화시키기 위해서 꼭 똑같이 toggle 신호로 만들 필요가 있을까?
1초를 생성하는 모듈의 신호는 sec를 나타내는 카운터가 받음. 클럭에 동기화한다고 해도 posedge (상승엣지)에 맞춰서 동작하기에 pulse 형태로 두어도 된다고 판단함.

다음과 같은 고찰로 인해 module내부에서 data를 저장하는 sequential logic과 data를 계산하는 combinational logic의 차이점과 기능에 대한 이해도를 증진시킴. 이를 바탕으로 다음 모듈로는 초를 세고 분으로 넘어가는 카운터를 구현 예정.

