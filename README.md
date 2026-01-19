# HDLBits-tutorial-practice
회로 설계 디자이너를 꿈꾸는 학생의 베릴로그 입문
-purpose: Verilog 입문부터 sequential logic까지 학습.
-tool: HDLBits, Verilog Tutorial 사이트를 이용.

###2026/01/19 study: vector의 개념과 기초 연산
  bitwise/logical operator의 차이
    assign out_or_bitwise[0] = a[0]|b[0];
    assign out_or_bitwise[1] = a[1]|b[1];
    assign out_or_bitwise[2] = a[2]|b[2];
    
    assign out_or_logical = a||b;
    assign out_not [2:0]= ~a;
    assign out_not [5:3]= ~b;

  Output pins are stuck at VCC or GND
  성공한 것에서 멈추지 않고 warning문구의 내용을 찾아보고 실제 설계에서 이러한 결과가 왜 나오는지 찾아보았다.
